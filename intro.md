% Rust 的 30 分钟介绍

Rust 是一门系统编程语言，结合了强编译时（strong compile-time）正确性保证和快速高效。
通过提供内存安全保证和内存生命周期的完整控制改善了像 C++ 一样的其它系统编程语言的思想。
强内存安全保证使得用 Rust 编写正确的并发程序比其它语言更容易。
这份介绍会在大约 30 分钟内告诉你 Rust 是什么。
它期望你至少依稀熟悉以前的’花括号‘语言，但并不要求先前有系统编程经验。
概念比语法更重要，所以如果无法了解每一个最终的细节也不要着急：[指南](guide.html)稍后会帮你掌握这些。

让我们来谈谈 Rust 最重要的概念，“所有权（ownership）” 及其在程序员经常觉得很难的任务——并发上的意义。

# 所有权的威力（The power of ownership）

所有权是 Rust 的核心，并且许多 Rust 强大能力都是从这个特性得到的。
“所有权” 指的是你程序的哪部分允许读、写并且最终释放内存。
让我们以阅读一些 C++ 程序开始：

```cpp
int* dangling(void)
{
    int i = 1234;
    return &i;
}

int add_one(void)
{
    int* num = dangling();
    return *num + 1;
}
```

**注意: 上面 C++ 程序是为了演示而故意比较简单并且是非惯用的手法，并不是生产质量的 C++ 代表程序。**

这个函数在栈上为一个整数分配了存储空间，并且将其存到变量 `i` 上。然后返回变量 `i` 的引用。这里只有一个问题：当函数返回时栈内存就无效了。这表明 `add_one` 第二行 `num` 指向了一些垃圾值，并且我们得不到想要的效果。尽管这是很平凡的一个示例，但却在 C++ 程序中很常见。当堆上的内存通过 `malloc` （或 `new`）分配，然后由 `free` （或 `delete`）释放，但你的程序试图对这个指针指向的内存做一些操作时会遇到相似的问题。这个被称为“悬垂指针(dangling pointer)“问题，但用 Rust 不可能写出有这样问题的程序。
让我们尝试用 Rust 写这段程序：

```ignore
fn dangling() -> &int {
    let i = 1234;
    return &i;
}

fn add_one() -> int {
    let num = dangling();
    return *num + 1;
}

fn main() {
    add_one();
}
```

将这个程序保存为 `dangling.rs`。当你试图通过 `rustc dangling.rs` 来编译这个程序时，你会得到一个有意思（并且冗长）的错误信息：

```text
dangling.rs:3:12: 3:14 error: `i` does not live long enough
dangling.rs:3     return &i;
                         ^~
dangling.rs:1:23: 4:2 note: reference must be valid for the anonymous lifetime #1 defined on the block at 1:22...
dangling.rs:1 fn dangling() -> &int {
dangling.rs:2     let i = 1234;
dangling.rs:3     return &i;
dangling.rs:4 }
dangling.rs:1:23: 4:2 note: ...but borrowed value is only valid for the block at 1:22
dangling.rs:1 fn dangling() -> &int {
dangling.rs:2     let i = 1234;
dangling.rs:3     return &i;
dangling.rs:4 }
error: aborting due to previous error
```

为了充分理解这个错误信息，我们需要讨论”拥有（own）“是什么意思。到目前为止，我们只需要接受：Rust 不允许写带有悬垂指针的程序，当我们理解了”所有权“后会再回到这段代码。

让我们暂时忘掉编程，来谈谈书籍。我喜欢阅读物理书，有时我真的喜欢某本书并且告诉朋友应该读一读。当我读我的书时，我拥有它：这本书归我所有。当我把书借给其它人一段时间时，他们从我这借书。当你借一本书时，它就会在一段时间内是你的，然后当你还给我时，我就再次拥有这本书，对吧？

这个概念也能直接应用到 Rust 程序：一些程序拥有指向内存的一个特定的指针。它是那个指针的唯一拥有者。它也可以将这片内存借给其它程序一段时间：其它程序”借走（borrow）“这片内存，并且确切地借一段时间，这个时间被称为”生命期（lifetime）“。

这就是它所有内容。这看起来不难，对吧？让我们回到那段错误信息：
`error: 'i' does not live long enough`。
我们试图通过引用（`&` 操作符）借走一个特定的变量 `i`，但 Rust 知道这个变量会在函数返回时失效，所以它告诉我们：`reference must be valid for the anonymous lifetime #1...`。干得漂亮！

这是关于栈内存很棒的一个例子，但堆内存呢？ Rust 有另一种类型的指针：”拥有的盒子（owned box）“，你可以用 `box` 操作符来创建。
看看这个：

```
fn dangling() -> Box<int> {
    let i = box 1234i;
    return i;
}

fn add_one() -> int {
    let num = dangling();
    return *num + 1;
}
```

现在 `1234i` 不是在栈上分配的，而是有一个在堆上分配的 `box 1234i`。 `&` 借走一个指向已有内存的指针，创建了一个”拥有的盒子“，在堆上分配内存并将值放在里面，给你一个唯一的指针指向那段内存。
你可以粗略地比较这两行：

```
// Rust
let i = box 1234i;
```

```cpp
// C++
int *i = new int;
*i = 1234;
```

Rust 推断正确的类型，分配正确数量的内存空间，将其设为你要求的值。这意味着不能分配未初始化的内存：
*Rust 没有 null 的概念*.
好极了！
Rust 和 C++ 这行程序还有另一个不同的地方：
Rust 编译器也会算出 `i` 的生命期，然后在它失效时插入一个相应的 `free` 调用，像 C++ 中的析构函数一样。
你得到了手动分配堆内存的所有好处，而不用自己记下所有帐。此外所有检查都是在编译时完成的，所以没有运行时开销。你会（基本上）得到和你正确编写的 C++ 程序几乎一样的代码，但是不可能写出错误的版本，这多亏有了（Rust）编译器。

你已经见识到了“所有权”和“借出”对于阻止正常情况下在不严格语言中比较危险的程序很有用，但让我们来看看另一个：并发。

# 拥有并发（owning concurrency）

并发在当前软件行业是一个炙手可热的话题。它一直是计算机科学家感兴趣的研究领域，随着互联网爆炸式的应用，人们正指望改善一个给定服务能处理的用户数。并发是实现这个目标的一条途径。但并发程序有一个相当大的缺点：很难进行推理，因为它是非确定性的。写好的并发程序有一些不同的方法，但让我们来讨论 Rust 的所有权和生命期的概念是如何有助于写正确并发程序的。

首先，让我们回顾一个简单的并发示例。Rust 使得创建”任务（tasks）“非常容易，与之不同的称为”线程（threads）“。通常任务并不共享内存，而是彼此间通过 "信道（channels）" 通信，如下：

```
fn main() {
    let numbers = vec![1i, 2i, 3i];

    let (tx, rx)  = channel();
    tx.send(numbers);

    spawn(proc() {
        let numbers = rx.recv();
        println!("{}", numbers[0]);
    })
}
```

在这个例子中，我们创建了一个盒子数组。然后创建了一个 "信道"：Rust 在任务间传递消息的主要途径。`channel` 函数返回信道的两个不同端： `Sender` 和 `Receiver` （通常简写为 `tx` 和 `rx`）。`spawn` 函数创建了一个新任务，给一个*在堆上分配的闭包*运行。正如在程序中看到的那样，我们从原来的任务调用 `tx.send()`，传递盒子数组，我们在新任务里面调用 `rx.recv()`： `Sender` 通过 `send` 方法传递的值通过 `recv` 方法到达 `Receiver`。

现在到了激动人心的部分：由于 `numbers` 是拥有的（owned）类型，当它发送经过信道时，实际上是*移动*了，在任务间转移了 `numbers` 的所有权。这个所有权的转移*非常快*——这种情况下只是简单地拷贝一个指针——同时也确保原来拥有 `numbers` 的任务不能通过继续并行地读写 `numbers` 与新拥有者创建数据竞争。

为了证明 Rust 执行了所有权转移，尝试修改前面的例子继续使用变量 `numbers`：

```ignore
fn main() {
    let numbers = vec![1i, 2i, 3i];

    let (tx, rx)  = channel();
    tx.send(numbers);

    spawn(proc() {
        let numbers = rx.recv();
        println!("{}", numbers[0]);
    });

    // Try to print a number from the original task
    println!("{}", numbers[0]);
}
```

编译器会产生一个错误，提示值不在作用域：

```text
concurrency.rs:12:20: 12:27 error: use of moved value: 'numbers'
concurrency.rs:12     println!("{}", numbers[0]);
                                     ^~~~~~~
```

由于一次只能有一个任务拥有盒子数组，如果不是将 `numbers` 数组分发给单一任务而是想分发给多个任务，我们需要给每一个任务拷贝这个数组。让我们来看一个用 `clone` 方法创建数据拷贝的例子：

```
fn main() {
    let numbers = vec![1i, 2i, 3i];

    for num in range(0u, 3) {
        let (tx, rx)  = channel();
        // Use `clone` to send a *copy* of the array
        tx.send(numbers.clone());

        spawn(proc() {
            let numbers = rx.recv();
            println!("{:d}", numbers[num]);
        })
    }
}
```

这与我们之前的程序相似，除了现在循环了3次，产生了3个任务并且在发送前*克隆*了 `numbers`。

然而如果我们要生成很多任务，或者我们的数据非常大，为每一个任务创建一份拷贝需要许多工作和许多额外的内存但只得到一点点好处。在实践中，因为开销我们可能不会这样做。进入 `Arc`，一个自动计算引用的盒子（"A.R.C" == "原子性地引用计数（atomically reference counted）"）。
`Arc` 是任务间*共享*数据最常用的方法。
下面是一些代码：

```
use std::sync::Arc;

fn main() {
    let numbers = vec![1i, 2i, 3i];
    let numbers = Arc::new(numbers);

    for num in range(0u, 3) {
        let (tx, rx)  = channel();
        tx.send(numbers.clone());

        spawn(proc() {
            let numbers = rx.recv();
            println!("{:d}", (*numbers)[num as uint]);
        })
    }
}
```

这几乎和前面的是一样的，除了这次 `numbers` 是首先放在 `Arc` 里面的。`Arc:new` 创建 `Arc`，`.clone()` 产生另一个 `Arc` 指向相同的内容。所以我们为每个任务克隆了 `Arc`，传递克隆到信道，然后用它打印一个数字。现在不是拷贝整个数组传递给我们的多任务，而仅仅只是拷贝了一个指针（`Arc`）并*共享*这个数组。

但这是如何工作的呢？
确实，如果我们共享数据，一个任务往数组里写而其它任务在读，那么难道没有引起数据竞争吗？

好吧，Rust 超级聪明，只会让你在 `Arc` 中存放可证明是安全的数据。
这种情况，*只要数据是不可变的*，即许多任务可以并行地读数据只要没有任务能写数据，共享数组就是安全的。到目前为止，这种类型和许多其它 `Arc` 只会给你提供数据的一个不可变的视图（immutable view）。

Arc 对于不可变数据很棒，但对于可变数据呢？共享可变状态是并发程序员的祸根：你可以使用互斥来保护共享的可变状态，但是如果你忘了获取互斥，糟糕的事情就会发生，包括崩溃。Rust 提供互斥但用它们破坏内存安全是不可能的。

让我们再来看看相同的示例，并将其修改为共享状态可变：

```
use std::sync::{Arc, Mutex};

fn main() {
    let numbers = vec![1i, 2i, 3i];
    let numbers_lock = Arc::new(Mutex::new(numbers));

    for num in range(0u, 3) {
        let (tx, rx)  = channel();
        tx.send(numbers_lock.clone());

        spawn(proc() {
            let numbers_lock = rx.recv();

            // Take the lock, along with exclusive access to the underlying array
            let mut numbers = numbers_lock.lock();

            // This is ugly for now because of the need for `get_mut`, but
            // will be replaced by `numbers[num as uint] += 1`
            // in the near future.
            // See: https://github.com/rust-lang/rust/issues/6515
            *numbers.get_mut(num as uint) += 1;

            println!("{}", (*numbers)[num as uint]);

            // When `numbers` goes out of scope the lock is dropped
        })
    }
}
```

这个示例开始变得巧妙了，但它暗示了 Rust 并发类型的强大的可组合性。这次我们将数组存放到 `Mutex` 中然后将*这个*放到 `Arc` 里面。像不可变数据，`Mutex` 可共享，但不像不可变数据，`Mutex` 里的数据可以是可变的只要互斥被锁住。

这里的 `lock` 方法返回的不是你原始的数组或指针，而是 `MutexGuard`——一种在锁离开作用域时负责释放锁的数据类型。相同的 `MutexGuard` 能被透明地处理就好像它是 `Mutex` 包含的数据一样，正如你看到的后续执行的可变索引操作那样。

OK，在走得太深入之前我们在这里结束。

# 脚注：不安全（unsafe）

Rust 编译器和库全部是用 Rust 写的；我们称 Rust 是“自托管”的。
如果 Rust 不能在线程间安全地共享数据，而 Rust 又是用 Rust 写的，那么它是如何实现 `Arc` 和 `Mutex` 这样的并发类型的？
答案是 `unsafe`。

如你所见，尽管 Rust 编译器非常聪明，并能避免你通常会犯的错误，但它不是人工智能。
由于我们比编译器聪明——有时——我们需要覆盖这个安全特性。对于这个， Rust 有一个 `unsafe` 关键词。在 `unsafe` 块中， Rust 会关闭许多安全检查。如果你的程序发生了一些糟糕的事情，你只需要审查你在 `unsafe` 块中干了什么，而不是检查整个程序。


如果 Rust 的一个主要目标是安全性，为什么允许关闭安全性呢？
嗯，的确只有三个理由这么做：
外部程序的接口，例如在 C 库里做 FFI（译注：Foreign Function Interface）；
效率（在某些情况下）；
为通常不安全的操作提供一个安全抽象。

我们的 `Arc` 就是最后一个目的的例子，我们能够安全地分发多个指向 `Arc` 内容的指针，因为我们确定数据共享是安全的。但 Rust 编译器不知道我们做了这些选择，所以在 Arc 实现的_内部_，我们用 `unsafe` 块（正常地）做这些危险的事情。但我们暴露一个安全的接口，即意味着 `Arc` 不会被错误地使用。

这就是 Rust 的类型系统如何阻止你犯这些导致并发编程困难的错误，并且得到类似 C++ 语言的高效性。

# 猿们，这就是全部的

我希望这个 Rust 的品尝已经告诉你 Rust 是否适合你。如果是的，那么我建议你查看[指南](guide.html)来深入全面的探索 Rust 的语法和概念。

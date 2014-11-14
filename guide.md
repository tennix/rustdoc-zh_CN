% Rust 指南

嗨，大家好！欢迎来到 Rust 指南。如果你想学习用 Rust 编程，就来对地方了。Rust 是一门系统编程语言，关注“高层次裸机（bare-metal）编程”：编程语言能给你最低层次的控制、零开销、高层次抽象，因为人不是计算机。我们真的认为 Rust 很特别，而且希望你也这样认为。

为了给你展示如何使用 Rust，我们会编写经典的 "Hello, World!" 程序，然后我们会给你介绍一个编写真实世界程序和库的工具："Cargo"。之后我们会讨论 Rust 的基本概念，写一个小程序并试一试，然后学习更高级的东西。

听起来很棒吧？让我们开始吧！

# 安装 Rust

使用 Rust 的第一步是安装它！有许多安装 Rust 的方法，但最容易的莫过于使用 `rustup` 脚本。如果你是在 Linux 或 Mac 系统下，所有你需要做的是下面的（注意你不用输入 `$`，它们只是每个命令的提示符）：

```{ignore}
$ curl -s https://static.rust-lang.org/rustup.sh | sudo sh
```

（如果你担心 `curl | sudo sh`，请继续往下读，下面是免责声明。）

如果你在 Windows 系统下，请下载[32位安装包](https://static.rust-lang.org/dist/rust-nightly-i686-w64-mingw32.exe)或者[64位安装包](https://static.rust-lang.org/dist/rust-nightly-x86_64-w64-mingw32.exe)并运行之。

如果你决定不再使用 Rust，会让我们有点桑心，但还 OK 吧，毕竟一门语言不是对所有人都合适的。只需要给这个脚本一个参数（缷载）就行：

```{ignore}
$ curl -s https://static.rust-lang.org/rustup.sh | sudo sh -s -- --uninstall
```

如果你使用的是 Windows 安装包，只需要再次运行 `.exe`，就会有个缷载选项。

想要升级 Rust，随时再次运行这个脚本，当前这是很常见的。Rust 仍然处于 1.0 版本前，所以人们会假设你使用的是最新版的 Rust。

这提示我另一点：一些人，当我们告诉他运行 `curl | sudo sh` 时很不爽。的确应该这样，基本上当你这样做的时候，你就得信任维护 Rust 的那邦好人不会黑掉你的电脑并干坏事。这是良好的本能！如果你就是这样的人，请查看文档[从源代码构建 Rust](https://github.com/rust-lang/rust#building-from-source)或者[官方二进制程序下载](http://www.rust-lang.org/install.html)。我们保证这不会一直是 Rust 的安装方法：这只是在 Rust 还是 alpha 版时保持更新的最简单方法。

哦，我们也该提一提官方支持的平台：

* Windows (7, 8, Server 2008 R2), 只支持 x86
* Linux (2.6.18 或更高版本，各种发行版)， x86 和 x86-64
* OSX 10.7 (Lion) 或更高版本， x86 和 x86-64

我们在这些平台上进行了大量的测试，也在其它一些平台上做了些测试，如 Android。但上面这些平台是最稳定的，因为进行了最多的测试。

最后有一点关于 Windows 平台的说明。Rust 在发布时将 Windows 作为第一级考虑的平台，但老实说，Windows 下的体验不如 Linux/OS X 平台完整。我们正在这上面努力！如果上面有什么东西不能干活，就是一个 bug，如果发生了这种情况请告知我们。Windows 平台上的每一个提交都像其它平台上一样做了测试。

如果你已经安装了 Rust，打开一个 shell，输入：

```{ignore}
$ rustc --version
```

你应当看到类似下面的输出：

```{ignore}
rustc 0.12.0-pre (443a1cd 2014-06-08 14:56:52 -0700)
```

如果的确是这样的，说明 Rust 已经成功安装了，恭喜！

如果不是，可以从许多地方获得帮助。最简单的是 [irc.mozilla.org 上的 #Rust 频道](irc://irc.mozilla.org/#rust)，你可以通过 [Mibbit](http://chat.mibbit.com/?server=irc.mozilla.org&channel=%23rust) 访问。点击上面的那个链接，你就可以和其它 Rustacean（我们自封的很傻的昵称）聊天了，然后我们就可以帮助你了。其它很棒的资源包括 [我们的邮件列表](https://mail.mozilla.org/listinfo/rust-dev)， [reddit 子栏目 /r/rust](http://www.reddit.com/r/rust) 和 [Stack Overflow](http://stackoverflow.com/questions/tagged/rust)。

# Hello, World!

现在你已经安装了 Rust，让我们来写你的第一个 Rust 程序。在任何新语言的第一个程序里打印 "Hello, world!" 到屏幕是一个传统。以这个简单的程序开始比较好的地方是你可以确认编译器不仅安装了，而且能正确地干活。打印消息到屏幕是很常见的事情。

要做的第一件事情是创建一个文件，将我们的程序写进去。我喜欢在家目录里创建一个 `projects` 目录，并将所有的项目放在里面。Rust 并不关心你的程序在哪。

这实际上导致另一个需要关注的：这份指南假设你熟悉基本的命令行，Rust 并不要求你知道大量的命令行。但直到 Rust 完成度更高前，没有 IDE 支持是一个小瑕疵。Rust 对你的编辑工具或程序保存在哪没有特殊的需求。

有了上面的说明，让我们在项目目录里创建一个子目录。

```{bash}
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

如果你在 Windows 平台而且没有使用 Powershell，`～` 可能不起作用，查阅你的 shell 了解更多细节。

接下来让我们创建一个新的程序源文件。在这些例子中我会使用 `editor filename` 来代表编辑一个文件，但你需要用你自己想用的方法。我们将这个文件称为 `main.rs`：

```{bash}
$ editor main.rs
```

Rust 文件总是以 `.rs` 作为后缀。如果你在文件名中使用不止一个单词，请用下划线。`hello_world.rs` 而不是 `helloworld.rs`。

现在你已经打开了这个文件，那么输入下面这些：

```{rust}
fn main() {
    println!("Hello, world!");
}
```

保存这个文件，然后在你的命令行中输入：

```{bash}
$ rustc main.rs
$ ./main # 或者 main.exe 如果在 Windows 平台
Hello, world!
```

成功鸟！让我们详细地回顾一下刚才发生了什么。

```{rust}
fn main() {

}
```

在 Rust 中这些行定义了一个**函数**。 `main` 函数比较特殊：它是每个 Rust 程序的起点。第一行是说”我定义了一个名叫 `main` 的函数，不接受参数也不返回什么。“ 如果有参数，它们会在圆括弧（`(` 和 `)`）里面，因为我们没有从这个函数中返回任何东西，我们完全丢掉了那个符号，后面会再谈到。

你也许会注意到函数是包在花括弧（`{` 和 `}`）里面的。Rust 要求用花括弧包围所有函数体。将左半花括弧放在函数声明同一行用空格隔开是一个良好的风格。

接下来是这一行：

```{rust}
    println!("Hello, world!");
```

这一行干了我们这个小程序中所有的活。这里有很多重要的细节。第一个是这里用 4 个空格缩进而不是制表符。请将你的编辑器配置成 tab 键插入 4 个空格。我们提供一些[不同编辑器配置示例](https://github.com/rust-lang/rust/tree/master/src/etc)。

第二点是 `println!()` 部分。这被称为 Rust 的**宏**，这是 Rust 元编程的方式。如果是一个函数的话，看起来应该是这样的：`println()`。对于我们的目的而言，我们不需要关心这个不同点。只需要知道有时候，你会看到一个感叹号 `!`，那就表明你在调用宏而不是普通的函数。最后一点需要提到的是：Rust 的宏与 C 语言的宏有很大的不同，如果你用过 C 语言的宏的话。不要害怕使用宏。我们最终会到达这些细节，现在你只需要相信我们就行。

接下来 `"Hello, world!"` 是一个**字符串**。在系统语言中字符串是一个极其复杂的话题，这里是一个**静态分配内存**的字符串。后面我们会谈到不同种类的内存分配。我们将这个字符串作为一个参数传给 `println!`，打印字符串到屏幕。够简单的吧！

最后，这行以分号（`;`）结束。Rust 是一个**面向表达式**的语言，即意味着大多数东西是表达式。`;` 是用来指示当前表达式的结束和下一个表达式的开始。许多 Rust 程序大多数行是以 `;` 结束，这个指南后面我们会深入涉及到这个话题。

最后，实际上是**编译**和**运行**我们的程序。通过将源文件名传递给我们的编译器 `rustc` 来编译它。

```{bash}
$ rustc main.rs
```

如果你有 C 或 C++ 背景的话，你会发现这与 `gcc` 或 `clang` 相似。Rust 会生成一个二进制可执行文件。你可以用 `ls` 命令查看：

```{bash}
$ ls
main  main.rs
```

或者在 Windows 系统下：

```{bash}
$ dir
main.exe  main.rs
```

现在有两个文件：我们的源程序，以 `.rs` 为后缀，和可执行文件（Windows 上是`main.exe`，其它系统上是 `main`）

```{bash}
$ ./main  # or main.exe on Windows
```

这会打印文本 `Hello, world!` 到我们的终端。

如果你是从动态语言如 Ruby、Python 或 JavaScript 过来的，你可能不习惯这两步分开。Rust 是**提前编译的语言**，意味着你可以编译程序，将其给其他人，而别人不需要有安装好的 Rust。而如果你给别人一个 `.rb` 或 `.py` 或 `.js` 文件，则他们必须已经安装了 Ruby/Python/JavaScript，但你只需要一个命令就可以同时编译并运行程序。语言设计中每一点都是折衷，而 Rust 选择了自己的方式。

恭喜！你已经正式地写了一个 Rust 程序。这使你成为了一个 Rust 程序员，欢迎！

接下来我想给你介绍另一个工具：Cargo，用来写真实世界的 Rust 程序。对于简单的事情，只用 `rustc` 就很棒了，但随着你项目的增长，你需要一些工具来管理所有的选项，并使分享你的程序给其它人和项目更容易。

# Hello, Cargo!

[Cargo](http://crates.io) 是用来帮助 Rustaceans 管理项目的工具。Cargo 当前处于 alpha 状态，像 Rust 一样仍然是在进行中的项目。然而对于许多 Rust 项目来说它已经足够好了，因而假定 Rust 项目从一开始就会使用 Cargo。

Cargo 负责三件事情：构建你的程序，下载程序的依赖，构建程序的依赖。刚开始你的程序没有任何依赖，所以我们只会使用第一个功能。最终我们会添加更多。由于我们是从使用 Cargo 开始的，后面添加依赖会非常容易。

让我们将 Hello World 程序转换到 Cargo。使用 Cargo 第一步要做的是安装 Cargo。幸运的是，我们用来安装 Rust 的脚本默认包含了 Cargo。如果你通过其它方式安装的 Rust，你可能需要[查看 Cargo 的 README](https://github.com/rust-lang/cargo#installing-cargo-from-nightlies) 中关于安装 Cargo 的特定的说明。

为了将我们的项目 Cargo 化，我们需要做下面两件事情：创建一个 `Cargo.toml` 配置文件，将我们的源程序放在正确位置。让我们先做这部分：

```{bash}
$ mkdir src
$ mv main.rs src/main.rs
```

Cargo 希望你的源程序文件在 `src` 目录下，这可以让顶层目录放其它东西，如 README，许可证信息以及其它与程序无关的东西。Cargo 帮助我们保持项目干净。项目中所有东西各归其位。

下一步，我们的配置文件：

```{bash}
$ editor Cargo.toml
```

确保名字是对的：你需要大写的 `C`！

将下面这些写到这个文件里面：

```{ignore}
[package]

name = "hello_world"
version = "0.0.1"
authors = [ "Your name <you@example.com>" ]

[[bin]]

name = "hello_world"
```

这个文件是 [TOML](https://github.com/toml-lang/toml) 格式的，让这个文件自己解释自己：

> TOML 旨在成为一种最小的配置文件格式，由于有明显的语义，因而能够很容易地阅读。
> TOML 设计将歧义映射到一个 Hash 表上。
> TOML 应当很容易地解析成各种语言的数据结构。

TOML 与 INI 很像，但有一些额外的好东西。

不管怎样，这个文件中有两个**表**：`package` 和 `bin`。第一个表告诉 Cargo 你的包的元数据，第二个告诉 Cargo 我们对构建一个二进制而不是库（尽管我们可以同时构建！）感兴趣，同时还有二进制程序的名字。

一旦将这个文件放到了正确的位置，我们就准备好构建了！试一试下面的命令：

```{bash}
$ cargo build
   Compiling hello_world v0.0.1 (file:///home/yourname/projects/hello_world)
$ ./target/hello_world
Hello, world!
```

邦邦邦！我们用 `cargo build` 构建了我们的项目，并且用 `./target/hello_world` 来运行。这并没有比简单地用 `rustc` 带来大量的好处，但是想想将来：当我们的项目不止有一个文件，我们可能需要调用 `rustc` 两次，并传递大量选项告诉编译器将所有东西都编译在一起。用 Cargo 的话，随着项目的增长，我们只需要用 `cargo build`，它就会正确地干活。

你也会注意到 Cargo 创建了一个新文件： `Cargo.lock`。

```{ignore,notrust}
[root]
name = "hello_world"
version = "0.0.1"
```

这个文件是 Cargo 用来跟踪你应用的依赖关系。现在我们还没有任何依赖，所以还是一个有点少的文件。从来不需要你自己接触这个文件，让 Cargo 处理它就行。

就这样！我们已经成功地用 Cargo 构建了 `hello_world`。即使我们的程序很简单，也使用了你今后 Rust 生涯中会使用的许多真正的工具。

现在已经搞定了工具，让我们真正地学习更多关于语言本身的东西。这些是基本的东西，将会在余下的时间里一直为很好地伺候你。

# 变量绑定

我们首先要学习的是变量绑定。它们看起来是这样的：

```{rust}
let x = 5i;
```

在许多语言中，这被称为变量。但 Rust 的变量绑定在它们之上有一些其它的梗。Rust 有一个非常强大的功能叫”模式匹配“，我们会在之后详细学习。`let` 表达式左边是一个完整的模式，而不仅仅是一个变量名。
这意味着我们可以像下面这样做：

```{rust}
let (x, y) = (1i, 2i);
```

这个表达式被求值之后，`x` 会是 1，而 `y` 会是 2。模式真的非常强大，但目前我们能对它们做的只有这些，所以在我们继续向下学习时只需要将这些记在大脑中。

顺便说一下，在这些例子中，`i` 表明这个数字是整数。

Rust 是静态类型的语言，意味着需要在前面说明类型。那么我们第一个示例是如何编译的呢？Rust 有所谓的类型推断，如果 Rust 能够推断出类型，就不需要你实际写出来。

不过如果我们想写也可以加上，类型跟在冒号（`:`）后面：

```{rust}
let x: int = 5;
```

如果我叫你在班上其它同学前大声朗读它，你会说 ”`x` 是 `int` 类型的绑定，其值为 5“。默认的绑定是**不可变的**，下面这个程序编译不会通过：

```{ignore}
let x = 5i;
x = 10i;
```

编译器会给出下面的错误：

```{ignore,notrust}
error: re-assignment of immutable variable `x`
     x = 10i;
     ^~~~~~~
```

如果你希望绑定是可变的，可以用 `mut`：

```{rust}
let mut x = 5i;
x = 10i;
```

默认绑定不可变不单单只有一个理由，但我们可以从 Rust 的一个主要关注点——安全性——思考一下，如果你忘了写 `mut`，编译器会捕捉到并告诉你，你已经使一些你可能不想让其可变的成了可变的了。如果默认绑定是可变的，编译器就不会告诉你这些。如果你_确实_想要可变的，那么解决方法非常简单：加上 `mut`。

有一些其它原因尽可能避免可变状态，但超出了本指南的讨论范围。一般来说常常可以避免显式可变，因而在 Rust 中更可取。这就是说有时你需要可变，所以它不是禁止的。

让我们回到变量绑定上。Rust 的变量绑定还有一个方面与其它语言不同：绑定在允许使用前需要初始化。如果我们尝试：

```{ignore}
let x;
```

会得到一个错误：

```{ignore}
src/main.rs:2:9: 2:10 error: cannot determine a type for this local variable: unconstrained type
src/main.rs:2     let x;
                      ^
```

而给它一个类型就可以编译通过：

```{ignore}
let x: int;
```

让我们试一试，将 `src/main.rs` 改成下面这样：

```{rust}
fn main() {
    let x: int;

    println!("Hello world!");
}
```

你可以在命令行中用 `cargo build` 来构建。你会得到一个警告，不过仍然能够打印 "Hello, world!"。

```{ignore,notrust}
   Compiling hello_world v0.0.1 (file:///home/you/projects/hello_world)
src/main.rs:2:9: 2:10 warning: unused variable: `x`, #[warn(unused_variable)] on by default
src/main.rs:2     let x: int;
                             ^
```

Rust警告我们没有用这个变量绑定，但由于我们没用它，所以无毒无害。然而如果我们尝试实际使用 `x`，就会发生变化。将程序改成下面这样：

```{rust,ignore}
fn main() {
    let x: int;

    println!("The value of x is: {}", x);
}
```

试着构建它，你会得到一个错误：

```{bash}
$ cargo build
   Compiling hello_world v0.0.1 (file:///home/you/projects/hello_world)
src/main.rs:4:39: 4:40 error: use of possibly uninitialized variable: `x`
src/main.rs:4     println!("The value of x is: {}", x);
                                                    ^
note: in expansion of format_args!
<std macros>:2:23: 2:77 note: expansion site
<std macros>:1:1: 3:2 note: in expansion of println!
src/main.rs:4:5: 4:42 note: expansion site
error: aborting due to previous error
Could not compile `hello_world`.
```

Rust 不让我们使用没有初始化的值，接下来我们来谈谈加到 `println!` 中的东西。

如果你在要打印的字符串中包括两个花括弧（`{}`，有时称它们为小胡子），Rust 将其解释为插入一些值的请求。**字符串插值**是计算机科学里的一个术语，意思是在字符串中间插入。我们加了一个逗号，然后是 `x`，表明我们想将 `x` 作为插入的值。逗号用来分隔传给函数或宏的参数，如果传递的参数不止一个的话。

当你只用花括弧的话，Rust 会尝试通过检查其类型来以有意义的方式显示这个值。如果你想更详细地指定其格式，有[大量可用选项](std/fmt/index.html)。现在我们只用默认的就行：整数类型打印并不复杂。

# If

Rust 的 `if` 并不是特别复杂，但你会发现它更像动态语言而不是传统的系统语言中的 `if`。所以让我们谈谈它，以确保你了解之间细微的差别。

`if` 是更一般的概念——分支——的特殊形式。名字来源于树的分支：一个决策点，根据选择，可以走多条路径。

对于 `if` 的情况,有一个选择可以导致两条路径：

```rust
let x = 5i;

if x == 5i {
    println!("x is five!");
}
```

如果我们将 `x` 改成其它的值，这行就不会打印。更具体地说，如果 `if` 后面的表达式求值为 `true`，这一块就会被执行；如果求值为 `false`，就不会执行。

如果你想在 `false` 情况下做些事情，使用 `else`：

```{rust}
let x = 5i;

if x == 5i {
    println!("x is five!");
} else {
    println!("x is not five :(");
}
```

这非常标准，不过你也可以这样做：

```{rust}
let x = 5i;

let y = if x == 5i {
    10i
} else {
    15i
};
```

我们可以（或者说应该）将其写成这样：

```{rust}
let x = 5i;

let y = if x == 5i { 10i } else { 15i };
```

这显示了 Rust 的两个有意思的东西：Rust 是面向表达式的语言；分号与其它基于花括弧和分号的语言不同，这两个东西是相关的。

## 表达式 v.s. 陈述

Rust 主要是一门基于表达式的语言，它只有两种陈述，其它的都是表达式。

那么这两者有什么不同呢？表达式返回一个结果，而陈述则不会。在许多语言中，`if` 是一个陈述，因而 `let x = if ...` 没有意义。但是在 Rust 中，`if` 是一个表达式，意味着返回一个值。然后我们可以用这个值来初始化绑定。

说到这，绑定是 Rust 两种陈述中的第一个，真正的名字是**陈述声明**。到目前为止，`let` 是我们见到的唯一一个陈述声明。让我们谈谈更多的，在其它语言中，变量绑定可以写成表达式而不仅仅是陈述，像在 Ruby 中：

```{ruby}
x = y = 5
```

然而在 Rust 中用 `let` 引入的绑定不是表达式，下面这行会产生编译时错误：

```{ignore}
let x = (let y = 5i); // expected identifier, found keyword `let`
```

编译器告诉我们它期望看到表达式的开头，`let` 只能开始一个陈述而不能开始一个表达式。

注意到赋值给一个已经绑定的变量（例如 `y = 5i`）仍然是表达式，尽管它的值不是特别有用。不像 C 中赋值是对所赋的值进行求值（例如前面例子中的 `5i`），在 Rust 中赋值表达式的值是单元(unit)类型 `()`，后面会谈到。

Rust 中的第二种陈述是**表达式陈述**，其目的是将任何表达式转换为陈述。在实践方面，Rust 的语法要求陈述跟着其它陈述。这意味着你用分号将表达式彼此分开，Rust 与其它要求在每行以分号结尾的语言非常相似，你会看到程序中几乎每行你能看到的都以分号结尾。

我们说“几乎”的例外是什么呢？你已经在这个程序中看到了：

```{rust}
let x = 5i;

let y: int = if x == 5i { 10i } else { 15i };
```

注意到我给 `y` 加了一个类型注解，明确说明我想让 `y` 是一个整数。

这与下面程序的不同，下面这个编译不能通过：

```{ignore}
let x = 5i;

let y: int = if x == 5i { 10i; } else { 15i; };
```

注意到 10 和 15 后面的分号。Rust 会给出下面的错误：

```{ignore,notrust}
error: mismatched types: expected `int` but found `()` (expected int but found ())
```

我们期望一个整数，但却得到了一个 `()`。`()` 读作“单元”，是 Rust 类型系统中的一个特殊类型。与其它语言中的 `null` 不同，因为 `()` 与其它类型不同。例如在 C 中，`null` 对于 `int` 类型变量是一个有效的值。而在 Rust 中，`()` _不是_ `int` 型变量的有效值，它只是 `()` 类型变量的有效值，这没多大用处。还记得我们说过陈述不返回值吗？这就是这种情况下单元类型的目的，分号将表达式变成陈述，扔掉它的值并取而代之返回单元。

你会看到 Rust 程序中还有一种情况不以分号结尾。对那种情况，我们需要另一个概念：函数。

# 函数

到目前为止已经见到了一个函数，`main` 函数：

```{rust}
fn main() {
}
```

这可能是最简单的函数声明。如我们之前提到的，`fn` 表明这是一个函数，后面跟着函数名字，一些（空）括号因为这个函数没有参数，然后是花括弧指示函数体。这里有一个名为 `foo` 的函数：

```{rust}
fn foo() {
}
```

那么接受参数呢？这里有一个函数打印一个数字：

```{rust}
fn print_number(x: int) {
    println!("x is: {}", x);
}
```

下面是一个使用 `print_number` 函数的完整程序：

```{rust}
fn main() {
    print_number(5);
}

fn print_number(x: int) {
    println!("x is: {}", x);
}
```

如你所见，函数参数工作方式与 `let` 声明非常相似：你需要在分号后面添加参数名的类型。这里有一个完整的程序，将两个数字加到一起并打印出来：

```{rust}
fn main() {
    print_sum(5, 6);
}

fn print_sum(x: int, y: int) {
    println!("sum is: {}", x + y);
}
```

调用和声明函数时，你都要用逗号分隔参数。

与 `let` 不同的是，你_必须_声明函数参数类型，下面这个不能干活：

```{ignore}
fn print_number(x, y) {
    println!("x is: {}", x + y);
}
```

你会得到下面这个错误：

```{ignore,notrust}
hello.rs:5:18: 5:19 error: expected `:` but found `,`
hello.rs:5 fn print_number(x, y) {
```

这是有意设计的，尽管完整程序推断是可能的，像 Haskell 这样的语言就有这个功能，但显式地说明类型是一个最佳实践。我们赞同强制函数声明类型同时允许函数体中类型推断是完整类型推断和没有类型推断之间的一个最佳点（sweet spot）。

返回一个值呢？下面是一个函数，将一个数加到一个整数上：

```{rust}
fn add_one(x: int) -> int {
    x + 1
}
```

Rust 函数只返回一个值，在“箭头”后面声明类型，“箭头”是短线（`-`）后面跟一个大于号（`>`）。

你会注意到这里缺少一个分号。如果我们在下面加上它：

```{ignore}
fn add_one(x: int) -> int {
    x + 1;
}
```

就会得到一个错误：

```{ignore,notrust}
error: not all control paths return a value
fn add_one(x: int) -> int {
     x + 1;
}

note: consider removing this semicolon:
     x + 1;
          ^
```

还记得我们之前讨论的分号和 `()` 吗？我们的函数要求返回一个 `int` 类型的值，但是用分号却会返回 `()`。Rust 意识到这可能不是我们想要的，因而建议移除分号。

这与我们之前的 `if` 陈述很相似：代码块（`{}`）的结果是表达式的值。其它面向表达式的语言如 Ruby 也这样工作，但在系统编程语言中有一点不一样。当人们第一次学到这点，通常以为引入了 bug。但由于 Rust 的类型系统非常强壮并且单元类型是其仅有的类型，我们还从来没有发现添加或移除返回位置的分号会导致任何问题。

但提前返回呢？对于这个，Rust 的确有一个关键词, `return`：

```{rust}
fn foo(x: int) -> int {
    if x < 5 { return x; }

    x + 1
}
```

以 `return` 作为函数最后一行仍然能干活，但却是一个糟糕的风格：

```{rust}
fn foo(x: int) -> int {
    if x < 5 { return x; }

    return x + 1;
}
```

有一些其它方法定义函数，但会涉及到我们还没学习到的特性，所以目前让我们放一放。

# 注释

现在我们有一些函数了，学习一下注释是个好主意。注释是你给其他程序员留下的注解，用来解释你的代码，编译器大多数情况忽略它们。Rust 有两种你需要注意的注释：**行注释**和**文档注释**。

```{rust}
// Line comments are anything after '//' and extend to the end of the line.

let x = 5i; // this is also a line comment.

// If you have a long explanation for something, you can put line comments next
// to each other. Put a space between the // and your comment so that it's
// more readable.
```

另一种注释是文档注释，文档注释用 `///` 而不是 `//`，并且内部支持 Markdown 标记：

```{rust}
/// `hello` is a function that prints a greeting that is personalized based on
/// the name given.
///
/// # Arguments
///
/// * `name` - The name of the person you'd like to greet.
///
/// # Example
///
/// ```rust
/// let name = "Steve";
/// hello(name); // prints "Hello, Steve!"
/// ```
fn hello(name: &str) {
    println!("Hello, {}!", name);
}
```

写文档注释时，为每个参数、返回值添加小节并且提供一些用法示例非常非常有用。

你可以用 `rustdoc` 工具从文档注释中生成 HTML 格式的文档。当我们讨论模块时会详细讨论 `rustdoc`，因为一般你想导出整个模块的文档。

# 复合数据类型

与许多其它语言相似，Rust 有许多不同的内置数据类型。你已经用整数和字符串做了一些简单工作了，接下来让我们讨论一些更复杂的存储数据方式。

## 元组（Tuples）

我们第一个要讨论的复合数据类型叫做**元组**，元组是固定大小的有序列表，像下面这样：

```rust
let x = (1i, "hello");
```

圆括号和逗号组成长度为 2 的元组，下面是相同的程序，但加上了类型注记：

```rust
let x: (int, &str) = (1, "hello");
```

如你所见，元组的类型与元组很像，但每个位置上是类型名而不是值。细心的读者会注意到元组是异构的：这个元组中有一个 `int` 和一个 `&str`。你之前还没见过 `&str` 作为一种类型，后面我们会详细讨论字符串。在系统编程语言中，字符串比其它语言里的更复杂一些。现在只需要将 `&str` 读作”字符串切片“，马上我们就会学到更多。

你可以通过**析构 let**来访问元组的域，下面是一个示例：

```rust
let (x, y, z) = (1i, 2i, 3i);

println!("x is {}", x);
```

还记得我之前说过 `let` 陈述左边的功能远不止给绑定赋值吗？这就是了，我们可以在 `let` 左边放一个模式，如果与右边的相匹配，则可以一次给多个绑定赋值。这种情况，`let` 析构或分解元组然后将这些位赋值给 3 个绑定。

这个模式非常强大，后面我们会反复地看到这点。

关于元组最后一点要说的是当且仅当元数、类型和值都相等时，元组才相等。

```rust
let x = (1i, 2i, 3i);
let y = (2i, 3i, 4i);

if x == y {
    println!("yes");
} else {
    println!("no");
}
```

这会打印 `no`，因为值不相等。

元组另一个用途是从函数返回多个值：

```rust
fn next_two(x: int) -> (int, int) { (x + 1i, x + 2i) }

fn main() {
    let (x, y) = next_two(5i);
    println!("x, y = {}, {}", x, y);
}
```

尽管 Rust 函数只能返回一个值，元组的确_是_一个值，但恰好由两个元素组成。从这个例子中也可以看到如何析构从函数返回的模式。元组是一种非常简单的数据结构，所以通常不是你想要的。让我们转换到它们更大的兄弟——结构（structs）。

## 结构（structs）

结构是与元组一样的'记录类型(record type)'的另一种形式。有一点区别：结构每个元素都有一个名字，称为域或成员，试一试下面的：

```rust
struct Point {
    x: int,
    y: int,
}

fn main() {
    let origin = Point { x: 0i, y:  0i };

    println!("The origin is at ({}, {})", origin.x, origin.y);
}
```

这里发生了很多事情，让我们拆开来看。我们用 `struct` 关键词声明一个结构，后面跟一个名字。按照惯例，结构以大写字母开头并且是驼峰式的：`PointInSpace`，而不是 `Point_In_Space`。

像往常一样，我们可以通过 `let` 创建一个结构的实例，但用 `key:value` 格式的语法设置每一个域，（域的）顺序可以不用和最开始声明时一样。

最后，因为域有名字，所以我们可以用点记号：`origin.x` 来访问域。

像 Rust 中其它绑定一样，结构的值不可变。不过可以用 `mut` 使其成为可变的：

```{rust}
struct Point {
    x: int,
    y: int,
}

fn main() {
    let mut point = Point { x: 0i, y:  0i };

    point.x = 5;

    println!("The point is at ({}, {})", point.x, point.y);
}
```

这会打印 `The point is at (5, 0)`。

## 元组结构和新类型（tuple structs and Newtypes）

Rust 有另一种数据类型像是元组和结构的混合，称为**元组结构**。元组结构的确有一个名字，但其域没有：

```{rust}
struct Color(int, int, int);
struct Point(int, int, int);
```

这两个不相等，即使两者有相同的值：

```{rust,ignore}
let black  = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

使用结构几乎总是比元组结构好，我们会将 `Color` 和 `Point` 写成这样的：

```{rust}
struct Color {
    red: int,
    blue: int,
    green: int,
}

struct Point {
    x: int,
    y: int,
    z: int,
}
```

现在我们有实际的名字，而不是位置。好名字很重要，使用结构我们有实际的名字。

的确_有_一种情况元组结构非常有用，就是只含一个元素的元组结构，我们称之为”新类型“，因为它可以让你创建一个新类型，这个新类型是另一个类型的同义：

```{rust}
struct Inches(int);

let length = Inches(10);

let Inches(integer_length) = length;
println!("length is {} inches", integer_length);
```

正如你在这里看到的，你可以通过一个析构 `let` 来分解内部的整数类型。

## 枚举（enums）

最后，Rust 有一个”和类型(sum type)“，称为**枚举**。枚举是 Rust 的一种极为有用的特性，在标准库中到处可以看到。下面是一个 Rust 标准库提供的枚举：

```{rust}
enum Ordering {
    Less,
    Equal,
    Greater,
}
```

在任何时间里，`Ordering` 只能是 `Less`、 `Equal` 和 `Greater` 中的_一个_，这里有一个例子：

```{rust}
fn cmp(a: int, b: int) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}

fn main() {
    let x = 5i;
    let y = 10i;

    let ordering = cmp(x, y);

    if ordering == Less {
        println!("less");
    } else if ordering == Greater {
        println!("greater");
    } else if ordering == Equal {
        println!("equal");
    }
}
```

`cmp` 函数比较两个东西，并返回一个 `Ordering`。根据两个值是否大于、小于或等于，返回 `Less`、 `Greater` 或 `Equal`。

变量 `ordering` 类型是 `Ordering`，所以含有三个值中的一个。然后我们可以进行一系列的 `if`/`else` 比较来检查到底是哪一个。

然而重复 `if`/`else` 来比较很无聊，Rust 有一个特性不仅使程序更容易阅读，而且保证你决不会遗漏任何一种情况。但在那样做之前，让我们讨论另一种枚举：带值的枚举。

这个枚举有两个变量，其中一个带有值：

```{rust}
enum OptionalInt {
    Value(int),
    Missing,
}

fn main() {
    let x = Value(5);
    let y = Missing;

    match x {
        Value(n) => println!("x is {:d}", n),
        Missing  => println!("x is missing!"),
    }

    match y {
        Value(n) => println!("y is {:d}", n),
        Missing  => println!("y is missing!"),
    }
}
```

这个枚举代表我们可能有也可能没有的 `int`，在 `Missing` 情况下，我们没有值，但在 `Value` 情况下有值。不过这个枚举特定于 `int` 类型。我们可以使其对任何类型都可以用，但到那步还早着呢！

你可以在枚举类型中有任意数量的值：

```{rust}
enum OptionalColor {
    Color(int, int, int),
    Missing
}
```

带值的枚举相当有用，但像我提到的那样当它们是泛型时会更有用。不过在到达泛型时，让我们讨论一下如何修复之前写的那个大大的 `if`/`else` 陈述，我们会用 `match` 来做这件事情。

# 匹配（match）

通常一个简单的 `if`/`else` 还不够，因为有不止一个可能的选项，并且 `else` 条件可能会极其复杂，那么解决方法是什么呢？

Rust 有一个关键词 `match`，允许你用更强大的东西替换复杂的 `if`/`else` 块，试试下面的：

```{rust}
let x = 5i;

match x {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    4 => println!("four"),
    5 => println!("five"),
    _ => println!("something else"),
}
```

`match` 接收一个表达式，然后根据其值选择分支。分支的每一个臂形式为 `val => expression`。当匹配上一个值时，对应的擘的表达式就会求值。称为 `match` 是因为项和模式匹配，`match` 是其实现。

那么有什么很大的优点呢？答案是有一些（不止一个），首先 `match` 执行详尽的检查，看到最后一个带有下划线 `_` 的擘没？如果我们移除这个擘，Rust 会给出一个错误：

```{ignore,notrust}
error: non-exhaustive patterns: `_` not covered
```

换句话说，Rust 尝试告诉我们忘记了一个值。因为 `x` 是一个整数，Rust 知道它有许多不同的值，例如 `6i`。但是没有 `_`，就没有擘可以匹配，因此 Rust 拒绝编译。`_` 是一种能够捕获所有值的擘。如果没有其它擘可以匹配，`_` 擘就会匹配。因为有了这个能捕获所有值的擘，对于每个可能的 `x` 我们都有一个对应的擘，所以程序现在就可以编译了。

`match` 陈述也可以解析枚举类型，还记得枚举那节中的这个程序吗？

```{rust}
fn cmp(a: int, b: int) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}

fn main() {
    let x = 5i;
    let y = 10i;

    let ordering = cmp(x, y);

    if ordering == Less {
        println!("less");
    } else if ordering == Greater {
        println!("greater");
    } else if ordering == Equal {
        println!("equal");
    }
}
```

我们可以将其重写为一个 `match`：

```{rust}
fn cmp(a: int, b: int) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}

fn main() {
    let x = 5i;
    let y = 10i;

    match cmp(x, y) {
        Less    => println!("less"),
        Greater => println!("greater"),
        Equal   => println!("equal"),
    }
}
```

这个版本有更少的噪音，并且也详尽地检查确保了涵盖 `Ordering` 所有可能的值。在 `if`/`else` 版本中，例如如果忘了 `Greater` 这种情况，编译器也会很嗨皮地编译，但是如果在 `match` 中忘了的话就不会，Rust 帮助我们确保涵盖了每一个基。

`match` 也是一个表达式，意味着我们可以在 `let` 绑定的右边使用。我们也可以像下面这样实现前面的程序：

```{rust}
fn cmp(a: int, b: int) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}

fn main() {
    let x = 5i;
    let y = 10i;

    let result = match cmp(x, y) {
        Less    => "less",
        Greater => "greater",
        Equal   => "equal",
    };

    println!("{}", result);
}
```

这种情况这样做没有多大意义，因为我们只是创建了一个不需要的临时字符串，但有时这是很好的模式。

# 循环（looping）

循环是我们尚未学到的最后一种基本概念，Rust 有两个主要循环：`for` 和 `while`。

## `for` 循环

`for` 循环是用来循环给定次数，Rust 的 `for` 循环与其它系统语言中的 `for` 循环有一点不一样。不过 Rust 的 `for` 循环与下面这种 "C 风格" 的 `for` 循环很不一样：

```{c}
for (x = 0; x < 10; x++) {
    printf( "%d\n", x );
}
```

而 Rust 里的是这样的：

```{rust}
for x in range(0i, 10i) {
    println!("{:d}", x);
}
```

写成稍微更抽象的形式：

```{ignore,notrust}
for var in expression {
    code
}
```

这个表达式是个迭代器，这个在指南后面会详细讨论。迭代器回馈一系列元素。每个元素是循环的一个迭代。然后值被绑定到名为 `var` 的变量中，这个变量在循环体中有效，一旦循环体结束，就从迭代器中取出下一个值再进行一次循环。当没有更多值时，`for` 循环就结束了。

在我们的例子中，`range` 是一个函数，接受起始位置和结束位置，给出一个遍历这些值的迭代器。不包含上界，所以我们的循环会打印 `0` 到 `9`，而没有 `10`。

Rust 是故意没有 C 风格的 `for` 循环的，因为即使是对于有经验的 C 开发者，手动控制循环中的每一个元素也是很复杂且容易出错。

在这个指南后面，当我们涉及到**迭代器**之后会更详细地讨论 `for` 循环。

## `while` 循环

Rust 中的另一种循环结构是 `while` 循环，它长这样：

```{rust}
let mut x = 5u;
let mut done = false;

while !done {
    x += x - 3;
    println!("{}", x);
    if x % 5 == 0 { done = true; }
}
```

当你不确定需要循环多少次时，`while` 循环是一个正确的选择。

如果需要无限循环，你可能会这样写

```{rust,ignore}
while true {
```

不过 Rust 中有一个专门的关键词 `loop` 用来处理这种情况：

```{rust,ignore}
loop {
```

Rust 的控制流分析将这个结构与 `while true` 区别对待，因为我们知道它会一直循环。理解这意味着什么样的细节对当前阶段来说不是特别重要，但是一般而言我们给编译器的信息越多，编译器就越能更好地安全处理并产生代码，所以当需要无限循环时，你应该总是倾向使用 `loop`。

## 提前结束循环

让我们再来看看之前的 `while` 循环：

```{rust}
let mut x = 5u;
let mut done = false;

while !done {
    x += x - 3;
    println!("{}", x);
    if x % 5 == 0 { done = true; }
}
```

我们需要保持一个专门的 `mut` 布尔变量绑定 `done`，用来判断何时从循环中跳出。Rust 有两个关键词来帮助我们修改迭代：`break` 和 `continue`。

这种情况我们可以将循环用 `break` 写成更好的形式：

```{rust}
let mut x = 5u;

loop {
    x += x - 3;
    println!("{}", x);
    if x % 5 == 0 { break; }
}
```

我们现在可以用 `loop` 永远循环，并用 `break` 提前终止循环。

`continue` 与之相似，但不是终止循环而是跳到下一个循环，下面只会打印奇数：

```{rust}
for x in range(0i, 10i) {
    if x % 2 == 0 { continue; }

    println!("{:d}", x);
}
```

`continue` 和 `break` 都在两种循环（for 循环和 while 循环）中有效。

# 字符串（strings）

字符串是每一个程序员都应该掌握的一个重要概念。由于 Rust 关注点是系统，Rust 的字符串处理系统与其它语言有点不同。任何时候你的数据结构是可变大小时，事情就变得棘手，而字符串正是一种可以改变大小的数据结构。这就是说，Rust 的字符串与其它系统语言如 C 工作机制不一样。

让我们挖掘一下细节，一个**字符串**是编码为 UTF-8 字节流的 unicode 标量值序列。所有字符串都被确保为有效编码的 UTF-8 序列，另外字符串不是 null-terminated 并且可以含有空字节。

Rust 有两种主要的字符串类型：`&str` 和 `String`。

第一种是 `&str`，读作字符串切片，字符串文本的类型是 `&str`：

```{rust}
let string = "Hello there.";
```

这个字符串是静态分配的，意味着它是保存在编译的程序中的，并且在程序运行中一直都存在。`string` 绑定是这个静态分配字符串的引用，字符串切片有固定的大小并且不可变。

相反 `String` 则是内存中的字符串，这种字符串是可增长的，并且保证是 UTF-8 编码的。

```{rust}
let mut s = "Hello".to_string();
println!("{}", s);

s.push_str(", world.");
println!("{}", s);
```

用 `as_slice()` 方法可以将 `String` 强制类型转换为 `&str`：

```{rust}
fn takes_slice(slice: &str) {
    println!("Got: {}", slice);
}

fn main() {
    let s = "Hello".to_string();
    takes_slice(s.as_slice());
}
```

比较 `String` 和常量字符串，使用 `as_slice()`...

```{rust}
fn compare(string: String) {
    if string.as_slice() == "Hello" {
        println!("yes");
    }
}
```

而不是 `to_string()`：

```{rust}
fn compare(string: String) {
    if string == "Hello".to_string() {
        println!("yes");
    }
}
```

将 `String` 转换为 `&str` 很廉价，但将 `&str` 转换为 `String` 则涉及到分配内存。除非必须得这样做，否则没有理由这样做！

这就是 Rust 字符串的基础知识！很可能比你习惯的要稍微复杂一点，如果你是从脚本语言中过来的，当底层细节很重要时，它们才真正地重要。只需要记住 `String` 分配内存并且控制其数据，而 `&str` 是其它字符串的引用就搞定了。

# 向量（vectors）

像许多其它语言一样，当你需要一个东西的列表时，Rust 提供一个列表类型。Rust 有不同的类型表示这一想法：`Vec<T>` （一个向量），`[T, .. N]` （一个数组）和 `&[T]` （一个切片），噢！

向量与 `String` 类似：它们有动态的长度并且分配合适的内存。你可以用 `vec!` 宏创建一个向量：

```{rust}
let nums = vec![1i, 2i, 3i];
```

注意到与之前使用的 `println!` 宏不一样的是，我们用方括号（`[]`）和 `vec！`。Rust 允许在任何情况使用任意一种方式，这只是一个惯例。

你可以只用方括号创建一个数组：

```{rust}
let nums = [1i, 2i, 3i];
```

那么区别是什么呢？数组有固定的大小，所以你不能加或减元素：

```{rust,ignore}
let mut nums = vec![1i, 2i, 3i];
nums.push(4i); // works

let mut nums = [1i, 2i, 3i];
nums.push(4i); //  error: type `[int, .. 3]` does not implement any method
               // in scope named `push`
```

`push()` 方法让你在向量末尾附加一个值，但由于数组有固定大小，添加一个元素没有意义。你可以在错误信息 `[int, .. 3]` 中看到准确的类型：`int` 型的数组，长度为 3。

与 `&str` 相似的是，切片是另一个数组的引用，我们可以用 `as_slice()` 方法从向量中得到一个切片：

```{rust}
let vec = vec![1i, 2i, 3i];
let slice = vec.as_slice();
```

所有的三个类型都实现了 `iter()` 方法，这个方法返回一个迭代器。后面我们会更详细地讨论迭代器，现在 `iter()` 方法允许你写一个 `for` 循环来打印向量、数组或切片的内容：

```{rust}
let vec = vec![1i, 2i, 3i];

for i in vec.iter() {
    println!("{}", i);
}
```

这段程序会按顺序在各自的行上打印每一个数。

你可以用**下标记号**访问向量、数组或切片的特定元素：

```{rust}
let names = ["Graydon", "Brian", "Niko"];

println!("The second name is: {}", names[1]);
```

像大多数编程语言一样，这些下标从 0 开始，所以第一个的名字是 `names[0]` 而第二个的名字是 `names[1]`。上面的例子打印 `The second name is Brian`。

向量尚有许多东西，但对于上手已经足够了。我们现在已经学到了 Rust 所有最基本的概念，准备好了开始构建我们的猜谜游戏，但首先还需要知道如何做另一件最后的事情：从键盘获取输入。如果不能猜的话，那么显然不可能有猜谜游戏！

# 标准输入

从键盘获取输入非常简单，但用到了一些我们之前没见过的东西。这儿有一个简单的程序，读取输入然后在后面打印出来：

```{rust,ignore}
use std::io;

fn main() {
    println!("Type something!");

    let input = std::io::stdin().read_line().ok().expect("Failed to read line");

    println!("{}", input);
}
```

让我们挨个的过一遍这些块：

```{rust,ignore}
std::io::stdin();
```

这个调用标准库 `std::io` 里的函数 `stdin()`，如你所想，`std` 中所有的都是由 Rust 的标准库提供，后面我们会详细讨论模块系统。

由于总是写出完整合格的名字很烦人，我们可以用 `use` 声明将其导入：

```{rust}
use std::io::stdin;

stdin();
```

然而导入模块并只用一层限定而不导入个别的函数是一个好的实践：

```{rust}
use std::io;

io::stdin();
```

让我们用这种风格更新一下示例：

```{rust,ignore}
use std::io;

fn main() {
    println!("Type something!");

    let input = io::stdin().read_line().ok().expect("Failed to read line");

    println!("{}", input);
}
```

下一步：

```{rust,ignore}
.read_line()
```

`read_line()` 方法可以在 `stdin()` 函数返回值上调用，来返回完整的一行输入，简单漂亮。

```{rust,ignore}
.ok().expect("Failed to read line");
```

还记得这段代码吗？

```{rust}
enum OptionalInt {
    Value(int),
    Missing,
}

fn main() {
    let x = Value(5);
    let y = Missing;

    match x {
        Value(n) => println!("x is {:d}", n),
        Missing  => println!("x is missing!"),
    }

    match y {
        Value(n) => println!("y is {:d}", n),
        Missing  => println!("y is missing!"),
    }
}
```

我们需要每次都匹配来看是否有一个值，尽管这种情况我们_知道_ `x` 有一个 `Value`，但 `match` 强制我们处理 `missing` 情况。99% 的情况下这是我们想要的，但有时我们比编译器知道的更多。

同样地， `read_line()` 并不一定返回一行输入，因为它可能会返回一行输入也可能失败。这种情况会发生在如果程序不是在终端中运行，而是以定时任务（cron job）的一部分运行或其它没有标准输入的上下文中。因此 `read_line` 返回一个与 `OptionalInt` 非常相似的类型：`IoResult<T>`。我们还没有谈到 `IoResult<T>`，因为它是 `OptionalInt` 的泛型。直到那时，你可以认为对于任何类型而不仅仅是 `int` 类型，它们都是一样的。

Rust 为这些 `IoResult<T>` 提供一种称为 `ok()` 的方法，与我们的 `match` 陈述做一样的事情，但这是假设我们有一个有效的值，如果没有就会终止程序。这种情况，如果没有得到输入，我们的程序就不干活，所以这样很 OK。大多数情况，我们想要显示地处理这个错误，`ok()` 的结果有一个方法 `expect()`，允许我们在崩溃发生时给出出错信息。

稍后在指南中我们会包括所有这些是如何工作的准确的细节，目前有了基本的理解足够你干活了。

回到我们正在处理的代码上，下面是更新的版本：

```{rust,ignore}
use std::io;

fn main() {
    println!("Type something!");

    let input = io::stdin().read_line().ok().expect("Failed to read line");

    println!("{}", input);
}
```

对于这样很长的一行，Rust 用空格给予你一定的灵活性。我们_可以_将示例写成这样的：

```{rust,ignore}
use std::io;

fn main() {
    println!("Type something!");

    let input = io::stdin()
                  .read_line()
                  .ok()
                  .expect("Failed to read line");

    println!("{}", input);
}
```

有时这会更容易阅读，有时会更难阅读，具体你自己决定。

这就是你从标准输入获得基本输入所需要的，不太难但有许多细节部分。

# 猜谜游戏

OK，我们已经掌握了 Rust 的基础，让我们写一个大点的程序。对于第一个项目，我们将实现一个初学者经典的编程问题：猜谜游戏。它是这样玩的：程序会产生一个 1 到 100 之间的随机数，然后提示我们输入一个猜测值，它会告诉我们猜的数是太大了还是太小了。一旦我们猜对了，它会祝贺我们并且在屏幕上打印出我们猜的次数，听起来不错吧？

## 开始

让我们建立一个新项目，到项目目录下，还记得我们创建的目录结构和为 `hello_world` 创建的 `Cargo.toml` 吧？Cargo 有一个命令帮我们干这些事情，我们来试试：

```{bash}
$ cd ~/projects
$ cargo new guessing_game --bin
$ cd guessing_game
```

我们将项目名字传给 `cargo new`，然后是 `--bin` 标志，因为我们要生成一个二进制（可执行程序）而不是库。

检查一下生成的 `Cargo.toml`：

```{ignore}
[package]

name = "guessing_game"
version = "0.0.1"
authors = ["Your Name <you@example.com>"]
```

Cargo 从环境变量中得到这些信息，如果不对的话，改过来就是了。

最后 Cargo 为我们生成了一个 hello, world。查看一下 `src/main.rs`：

```{rust}
fn main() {
    println!("Hello, world!");
}
```

让我们编译一下 Cargo 给我们的东西：

```{bash}
$ cargo build
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
```

棒极了！再次打开 `src/main.rs`，我们会将所有代码写在这个文件中。指南后面会讲多文件项目。

继续之前，让我再给你演示一个 Cargo 命令 `run`，`cargo run` 与 `cargo build` 相似，但后面还会运行生成的可执行程序。试一试：

```{notrust,ignore}
$ cargo run
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
     Running `target/guessing_game`
Hello, world!
```

好极了！当你需要在项目上快速迭代时，`run` 命令非常方便，我们的游戏正是这样的一个（需要快速迭代的）工程，我们需要在迭代的下个版本之前快速测试一下当前的版本。

## 处理猜测值

让我们实现它！我们需要为游戏做的第一件事是允许玩家输入一个猜测值，将下面的程序写到 `src/main.rs`：

```{rust,no_run}
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let input = io::stdin().read_line()
                           .ok()
                           .expect("Failed to read line");

    println!("You guessed: {}", input);
}
```

在我们讨论标准输入时，你已经看到过这些代码了。我们用 `use` 来导入 `std::io` 模块，然后 `main` 函数包含程序的逻辑：打印一些信息宣布游戏开始，让用户输入一个猜测值，获取他们的输入然后打印出来。

因为在标准 I/O 小节中已经讨论过这些，在这里我就不再细说了，如果你需要复习，回到之前的那一节。

## 生成一个秘密的数字

下一步我们需要生成一个秘密的数字。为了这样做，我们需要使用 Rust 的随机数生成（函数），我们还没有谈到这点。Rust 标准库中包含一堆有意思的函数。如果你需要一点程序，可能已经为你写好了！对于我们这种情况，我们的确知道 Rust 有随机数生成（函数），但我们不知道如何使用它。

进入文档，Rust 标准库有一个专门的文档页面。你可以在[这里](std/index.html)找到这个页面。那个页面有许多信息，但最棒的部分是搜索栏。在页面最顶上有一个框，你可以输入搜索词。目前搜索还相当原始，但一直在往更好发展。如果在框中输入 'random'，页面会更新成[这个](http://doc.rust-lang.org/std/index.html?search=random)。第一个结果是一个到[std::rand::random](http://doc.rust-lang.org/std/rand/fn.random.html)的链接。如果点击那个链接，我们会被带到它的文档页面。

这个页面为我们显示了一些东西：函数的类型签名，一些解释文字，然后是一个例子。修改我们的程序添加 `random` 函数：

```{rust,ignore}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random() % 100i) + 1i;

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let input = io::stdin().read_line()
                           .ok()
                           .expect("Failed to read line");


    println!("You guessed: {}", input);
}
```

我们改变的第一件事是 `use std::rand`，正如文档中说明的那样。然后添加 `let` 表达式创建一个变量绑定到 `secret_number`，然后打印这个结果。

你可能也会想知道为什么在结果 `rand::random()` 上用 `%`。这个运算符称为 "取模"，它会返回除法的余数。通过对结果 `rand::random()` 取模，我们将其值限制在0到99之间。然后将结果加1,使其从1到100。使用取模可以给你一个非常非常小偏置的结果，但对于这个例子无所谓。

让我们试试用 `cargo build` 编译：

```{notrust,no_run}
$ cargo build
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
src/main.rs:7:26: 7:34 error: the type of this value must be known in this context
src/main.rs:7     let secret_number = (rand::random() % 100i) + 1i;
                                       ^~~~~~~~
error: aborting due to previous error
```

不行啊！Rust 说 "这个值的类型在上下文中必须已知"。这有什么问题呢？实际上 `rand::random()` 可以生成许多类型的随机数，不仅仅是整数。这种情况 Rust 不知道 `random()` 应该生成哪种类型的值，所以我们需要帮助它。利用数字字面量（number literals），我们在末尾添加一个 `i` 告诉 Rust 它们是整数，但这在函数上不起作用。（对于函数）有不同的语法，是下面这样：

```{rust,ignore}
rand::random::<int>();
```

这句话是说"请给我一个随机整数值"。我们可以将程序改成用这种提示...

```{rust,no_run}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<int>() % 100i) + 1i;

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let input = io::stdin().read_line()
                           .ok()
                           .expect("Failed to read line");


    println!("You guessed: {}", input);
}
```

试着将新程序跑几次：

```{notrust,ignore}
$ cargo run
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
     Running `target/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4
$ ./target/guessing_game
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
$ ./target/guessing_game
Guess the number!
The secret number is: -29
Please input your guess.
42
You guessed: 42
```

等等，为什么是负29呢？我们可是想要一个1到100之间的数啊！这里我们有两种选择：可以要求 `random()` 生成一个无符号整数，即只能是正数，或者可以使用 `abs` （绝对值）函数。我们采用无符号整数的方法。如果想要一个随机正数，我们应该请求一个随机正数。程序看起来像下面这样：

```{rust,no_run}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<uint>() % 100u) + 1u;

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let input = io::stdin().read_line()
                           .ok()
                           .expect("Failed to read line");


    println!("You guessed: {}", input);
}
```

然后再试试：

```{notrust,ignore}
$ cargo run
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
     Running `target/guessing_game`
Guess the number!
The secret number is: 57
Please input your guess.
3
You guessed: 3
```

好极了！下一步：比较猜测值和秘密值。

## 比较猜测的值

如果你记得的话，在这个指南前面，我们创建了一个 `cmp` 函数：比较两个数。让我们将其添加进来，和 `match` 陈述一起比较猜测值和秘密值：

```{rust,ignore}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<uint>() % 100u) + 1u;

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let input = io::stdin().read_line()
                           .ok()
                           .expect("Failed to read line");


    println!("You guessed: {}", input);

    match cmp(input, secret_number) {
        Less    => println!("Too small!"),
        Greater => println!("Too big!"),
        Equal   => { println!("You win!"); },
    }
}

fn cmp(a: int, b: int) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}
```

如果我们试着编译的话，会得到一些错误：

```{notrust,ignore}
$ cargo build
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
src/main.rs:20:15: 20:20 error: mismatched types: expected `int` but found `collections::string::String` (expected int but found struct collections::string::String)
src/main.rs:20     match cmp(input, secret_number) {
                             ^~~~~
src/main.rs:20:22: 20:35 error: mismatched types: expected `int` but found `uint` (expected int but found uint)
src/main.rs:20     match cmp(input, secret_number) {
                                    ^~~~~~~~~~~~~
error: aborting due to 2 previous errors
```

写 Rust 程序时上面这个经常会发生，这是 Rust 最大的优点之一。你尝试一些代码，看看能否编译，Rust 告诉你某些地方做错了。这种情况，`cmp` 函数作用在整数上，但我们传给它的是无符号整数。这种错误修复很简单，因为 `cmp` 函数是我们写的！让我们把它改成接收 `uint` 类型：

```{rust,ignore}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<uint>() % 100u) + 1u;

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let input = io::stdin().read_line()
                           .ok()
                           .expect("Failed to read line");


    println!("You guessed: {}", input);

    match cmp(input, secret_number) {
        Less    => println!("Too small!"),
        Greater => println!("Too big!"),
        Equal   => { println!("You win!"); },
    }
}

fn cmp(a: uint, b: uint) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}
```

然后再编译试试：

```{notrust,ignore}
$ cargo build
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
src/main.rs:20:15: 20:20 error: mismatched types: expected `uint` but found `collections::string::String` (expected uint but found struct collections::string::String)
src/main.rs:20     match cmp(input, secret_number) {
                             ^~~~~
error: aborting due to previous error
```

这个错误与前一个相似：我们希望得到一个 `uint`，但却得到了一个 `String`！因为 `input` 变量来自标准输入，你可以猜测任何值，不信试试：

```{notrust,ignore}
$ ./target/guessing_game
Guess the number!
The secret number is: 73
Please input your guess.
hello
You guessed: hello
```

噢！同时你会注意到即使没有编译通过我们仍然能够运行程序，这是因为成功编译的老版本程序还在那儿，所以你要小心点！

不管怎么说，我们有一个 `String`，但需要一个 `uint`。怎么办呢？嗯，对此这里有一个函数：

```{rust,ignore}
let input = io::stdin().read_line()
                       .ok()
                       .expect("Failed to read line");
let input_num: Option<uint> = from_str(input.as_slice());
```

`from_str` 函数接收一个 `&str` 将其转换成别的。我们用类型提示告诉它别的东西是什么类型。记得 `random()` 时的类型提示吗？它是这样的：

```{rust,ignore}
rand::random::<uint>();
```

还有另外一种方法提供类型提示，就是在 `let` 中声明类型：

```{rust,ignore}
let x: uint = rand::random();
```

这种情况，我们显式地说 `x` 是 `uint`，所以 Rust 能够恰当地告诉 `random()` 生成什么类型的。类似的形式，下面两种都可以：

```{rust,ignore}
let input_num = from_str::<Option<uint>>("5");
let input_num: Option<uint> = from_str("5");
```

这种情况碰巧我喜欢后者，而在 `random()` 的情况我喜欢前者。我认为嵌套的 `<>` 使第一个选择看起来很丑而且也更难阅读。

不管怎么说，将输入转换成数字，我们的程序看起来是这样的：

```{rust,ignore}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<uint>() % 100u) + 1u;

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let input = io::stdin().read_line()
                           .ok()
                           .expect("Failed to read line");
    let input_num: Option<uint> = from_str(input.as_slice());



    println!("You guessed: {}", input_num);

    match cmp(input_num, secret_number) {
        Less    => println!("Too small!"),
        Greater => println!("Too big!"),
        Equal   => { println!("You win!"); },
    }
}

fn cmp(a: uint, b: uint) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}
```

让我们试验一下：

```{notrust,ignore}
$ cargo build
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
src/main.rs:22:15: 22:24 error: mismatched types: expected `uint` but found `core::option::Option<uint>` (expected uint but found enum core::option::Option)
src/main.rs:22     match cmp(input_num, secret_number) {
                             ^~~~~~~~~
error: aborting due to previous error
```

哦，对了，我们的 `input_num` 类型是 `Option<uint>`，而不是 `uint`。我们需要拆开 Option。如果你还记得前面提到的，`match` 是做这件事非常棒的一种方法。试试下面的代码：

```{rust,no_run}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<uint>() % 100u) + 1u;

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let input = io::stdin().read_line()
                           .ok()
                           .expect("Failed to read line");
    let input_num: Option<uint> = from_str(input.as_slice());

    let num = match input_num {
        Some(num) => num,
        None      => {
            println!("Please input a number!");
            return;
        }
    };


    println!("You guessed: {}", num);

    match cmp(num, secret_number) {
        Less    => println!("Too small!"),
        Greater => println!("Too big!"),
        Equal   => { println!("You win!"); },
    }
}

fn cmp(a: uint, b: uint) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}
```

使用 `match` 把 `Option` 中的 `uint` 传给我们或者打印错误消息并返回。让我们试一试：

```{notrust,ignore}
$ cargo run
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
     Running `target/guessing_game`
Guess the number!
The secret number is: 17
Please input your guess.
5
Please input a number!
```

啊，什么？成功了！但实际并没有。看，当从 `stdin()` 获取一行输入，你得到所有的输入，包括按回车得到的 `\n` 字符。所以 `from_str()` 看到了字符串 `"5\n"` 说”不，这不是一个数字，这里面有非数字“。幸运的是 `&str` 上面定义的有一个简单的方法我们可以使用：`trim()`。一个小改动，程序看起来是下面这样：

```{rust,no_run}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<uint>() % 100u) + 1u;

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let input = io::stdin().read_line()
                           .ok()
                           .expect("Failed to read line");
    let input_num: Option<uint> = from_str(input.as_slice().trim());

    let num = match input_num {
        Some(num) => num,
        None      => {
            println!("Please input a number!");
            return;
        }
    };


    println!("You guessed: {}", num);

    match cmp(num, secret_number) {
        Less    => println!("Too small!"),
        Greater => println!("Too big!"),
        Equal   => { println!("You win!"); },
    }
}

fn cmp(a: uint, b: uint) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}
```

让我们试试：

```{notrust,ignore}
$ cargo run
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
     Running `target/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

好极了！可以看到我甚至在猜测值前添加了空格，但它仍然能够知道我猜测的是76。将程序跑几遍，确认猜测数字可以干活，也猜猜一个很小的数字。

Rust 编译器帮了我们许多忙，这个技术称为”依靠编译器（lean on the compiler）“，在写某些程序时通常很有用。让错误消息帮你引向正确的类型。

现在我们已经使游戏的大部分能够工作了，但只能猜测一次。让我们添加循环来修改它。

## 循环

正如我们已经讨论的，`loop` 关键词给我们一个无穷循环。所以让我们把它加入进去：

```{rust,no_run}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<uint>() % 100u) + 1u;

    println!("The secret number is: {}", secret_number);

    loop {

        println!("Please input your guess.");

        let input = io::stdin().read_line()
                               .ok()
                               .expect("Failed to read line");
        let input_num: Option<uint> = from_str(input.as_slice().trim());

        let num = match input_num {
            Some(num) => num,
            None      => {
                println!("Please input a number!");
                return;
            }
        };


        println!("You guessed: {}", num);

        match cmp(num, secret_number) {
            Less    => println!("Too small!"),
            Greater => println!("Too big!"),
            Equal   => { println!("You win!"); },
        }
    }
}

fn cmp(a: uint, b: uint) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}
```

然后试试。但等等，难道我们刚才加入了一个无穷循环？对头，记得那个 `return` 吗？如果给一个非数字答案，我们会返回然后退出。观察一下：

```{notrust,ignore}
$ cargo run
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
     Running `target/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit
Please input a number!
```

哈，`quit` 真的退出了。输入其它非数字也一样，至少可以说这是次优的。首先当你赢了让其真正退出。

```{rust,no_run}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<uint>() % 100u) + 1u;

    println!("The secret number is: {}", secret_number);

    loop {

        println!("Please input your guess.");

        let input = io::stdin().read_line()
                               .ok()
                               .expect("Failed to read line");
        let input_num: Option<uint> = from_str(input.as_slice().trim());

        let num = match input_num {
            Some(num) => num,
            None      => {
                println!("Please input a number!");
                return;
            }
        };


        println!("You guessed: {}", num);

        match cmp(num, secret_number) {
            Less    => println!("Too small!"),
            Greater => println!("Too big!"),
            Equal   => {
                println!("You win!");
                return;
            },
        }
    }
}

fn cmp(a: uint, b: uint) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}
```

在 `You win!` 后面加上 `return`，当我们赢了会退出。还有一个地方需要调整：当某人输入一个非数字，我们不希望退出，只希望忽略它。将那个 `return` 改成 `continue`：

```{rust,no_run}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<uint>() % 100u) + 1u;

    println!("The secret number is: {}", secret_number);

    loop {

        println!("Please input your guess.");

        let input = io::stdin().read_line()
                               .ok()
                               .expect("Failed to read line");
        let input_num: Option<uint> = from_str(input.as_slice().trim());

        let num = match input_num {
            Some(num) => num,
            None      => {
                println!("Please input a number!");
                continue;
            }
        };


        println!("You guessed: {}", num);

        match cmp(num, secret_number) {
            Less    => println!("Too small!"),
            Greater => println!("Too big!"),
            Equal   => {
                println!("You win!");
                return;
            },
        }
    }
}

fn cmp(a: uint, b: uint) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}
```

现在应该可以了，让我们试试：

```{notrust,ignore}
$ cargo run
   Compiling guessing_game v0.0.1 (file:///home/you/projects/guessing_game)
     Running `target/guessing_game`
Guess the number!
The secret number is: 61
Please input your guess.
10
You guessed: 10
Too small!
Please input your guess.
99
You guessed: 99
Too big!
Please input your guess.
foo
Please input a number!
Please input your guess.
61
You guessed: 61
You win!
```

好极了！最后一点小调整，就可以完成这个猜谜游戏了。你能想到是什么吗？对，我们不想打印这个秘密的数字。它用来测试很好，但会毁了这个游戏（直接打印出来还猜个毛啊）。下面是最终的程序：

```{rust,no_run}
use std::io;
use std::rand;

fn main() {
    println!("Guess the number!");

    let secret_number = (rand::random::<uint>() % 100u) + 1u;

    loop {

        println!("Please input your guess.");

        let input = io::stdin().read_line()
                               .ok()
                               .expect("Failed to read line");
        let input_num: Option<uint> = from_str(input.as_slice().trim());

        let num = match input_num {
            Some(num) => num,
            None      => {
                println!("Please input a number!");
                continue;
            }
        };


        println!("You guessed: {}", num);

        match cmp(num, secret_number) {
            Less    => println!("Too small!"),
            Greater => println!("Too big!"),
            Equal   => {
                println!("You win!");
                return;
            },
        }
    }
}

fn cmp(a: uint, b: uint) -> Ordering {
    if a < b { Less }
    else if a > b { Greater }
    else { Equal }
}
```

## 完成！

此时你已经成功构建了一个猜谜游戏，恭喜你！

现在你已经学习了 Rust 的基本语法。所有这些与你以前用过的其它许多编程语言相对比较接近。这些基本语法和语义元素将组成了接下来 Rust 学习的基础。

现在在基本概念方面你已经是一个专家了，是时候学习一些 Rust 更独特的特性了。

# 箱子（crate）和模块（module）

Rust 以强健的模块系统为特性，但与其它编程语言工作方式有点不同。Rust 的模块系统有两种主要部分：**箱子（crate）**和**模块(module)**。

箱子是 Rust 的独立编译单元。Rust 每次总是一次只编译一个箱子，生成库或者可执行文件。然而可执行文件通常依赖于库，而库又依赖于其它库，为了支持这点，箱子可以依赖其它箱子。

每个箱子包含分层次结构的模块。这个树从称为**箱子根**的单个模块开始，有箱子根我们就可以定义其它模块，而这些模块又可以含有其它模块，树的深度随你喜欢多深都可以。

注意到我们还没有提到文件，Rust 不会在你的文件系统结构和模块之间强加一个特别的关系，意思是 Rust 如何在文件系统上寻找模块是一个惯例方法，但这个方法可以被覆盖。

啰嗦够多了，让我们真正构建一些东西！新建一个名为 `modules` 的项目。

```{bash,ignore}
$ cd ~/projects
$ cargo new modules --bin
```

通过编译再次检查我们的工作：

```{bash,notrust}
$ cargo run
   Compiling modules v0.0.1 (file:///home/you/projects/modules)
     Running `target/modules`
Hello, world!
```

太棒了！那么我们已经有了单个箱子：`src/main.rs` 就是一个箱子。这个文件中的一切都在箱子根部。一个箱子在其根部定义 `main` 函数，像我们这里做的一样生成可执行文件。

让我们在箱子里面定义一个新模块，编辑 `src/main.rs` 让它看起来是这样的：


```
fn main() {
    println!("Hello, world!");
}

mod hello {
    fn print_hello() {
        println!("Hello, world!");
    }
}
```

现在在箱子根的内部我们有了一个 `hello` 的模块，模块像函数和变量绑定一样采用 `snake_case` 命名方式。

在 `hello` 模块里我们定义了 `print_hello` 函数，这也会打印出我们的 hello world 消息。模块允许你将程序分割成功能干净的盒子，将公共的东西组合在一起，不同的东西分开。就像有一套货架一样：每样东西各归其位。

为调用 `print_hello` 函数，我们用双冒号（`::`）：

```{rust,ignore}
hello::print_hello();
```

你已经在 `io::stdin()` 和 `rand::random()` 中见过这个了。现在你已经知道如何创建你自己的（模块），然而箱子和模块有**可见性**的规则，控制到底谁可以使用给定模块中定义的函数。默认模块中所有的函数都是私有的，意思是只能在同一个模块中的其它函数中使用，下面的程序无法编译：

```{rust,ignore}
fn main() {
    hello::print_hello();
}

mod hello {
    fn print_hello() {
        println!("Hello, world!");
    }
}
```

它会报错：

```{notrust,ignore}
   Compiling modules v0.0.1 (file:///home/you/projects/modules)
src/main.rs:2:5: 2:23 error: function `print_hello` is private
src/main.rs:2     hello::print_hello();
                  ^~~~~~~~~~~~~~~~~~
```

为使它成为公共的，我们使用 `pub` 关键词：

```{rust}
fn main() {
    hello::print_hello();
}

mod hello {
    pub fn print_hello() {
        println!("Hello, world!");
    }
}
```

这就可以编译：

```{notrust,ignore}
$ cargo run
   Compiling modules v0.0.1 (file:///home/you/projects/modules)
     Running `target/modules`
Hello, world!
```

好极了！你还可以对模块干很多事情，包括将其移到它们自己的文件中，不过对目前来说这些细节已经足够了。

# 测试（testing）

传统上，测试没有成为大多数系统编程语言的强力套件，但 Rust 在语言中内置了非常基本扔测试。尽管自动测试不能证明代码没有 bug，但仍然有助于确认某些行为与预期一样工作。

这儿是一个非常基本的测试：

```{rust}
#[test]
fn is_one_equal_to_one() {
    assert_eq!(1i, 1i);
}
```

你可能注意到一些新的东西：那个 `#[test]` 行。但在我们进入测试机制前，先谈谈属性（attribute）。

## 属性（attribute）

Rust 的测试系统使用**属性**来标记哪个函数是测试，属性可以放到任何 Rust **条目** 上。还记得 Rust 的大部分东西都是表达式，而 `let` 不是吗？条目声明也不是表达式，这里有一个合格条目的列表：

* 函数（function）
* 模块（module）
* 类型定义（type definition）
* 结构（struct）
* 枚举（enumeration）
* 静态条目（static item）
* 特征（trait）
* 实现（implementation）

我们还没有学到所有的这些，但这是一个列表。如你所见，函数在最顶层。

属性可以以 3 种方式出现：

1. 一个单独的标识符，属性名字。`#[test]` 是这样的一个例子
2. 一个标识符后面跟一个等号（`=`）和文本。`#[cfg=test]` 是这样的一个例子
3. 一个标识符后面跟一个圆括号里面是子属性参数列表。`#[cfg(unix, target_word_size = "32")]` 是这样的一个例子，其中一个子参数是上面的第二种。

有许多不同种类的属性，足够多以致我们在这里不会都讲到。在讨论测试特定的属性前，我想讲讲其中一种最重要的属性：稳定性标记。

## 稳定性属性

Rust 提供六个属性来指示你的库不同部分的稳定性等级，这六个等级是：

* 过时的（deprecated）：这个条目不应该再使用，不保证向后兼容性
* 实验性的（experimental）：这个条目只是最近才引入的或者否则是处于不断变化的状态，可能会显著地变化甚至被移除，不保证向后兼容性。
* 不稳定的（unstable）：这个条目仍正在开发中，但需要更多测试才被认为是稳定的，不保证后向兼容性。
* 稳定的（stable）：这个条目是稳定的，不会显著地改动，保证后向兼容性
* 冻结的（frozen）：这个条目非常稳定，不太可能会改动，保证后向兼容性
* 锁定的（locked）：这个条目永远不会改动除非发现一个严重的 bug，保证后向兼容性

Rust 所有标准库都是用这些属性标记来传达它们的相对稳定性的，你也应该在程序中用这些属性标记。有个相关的属性 `warn` 允许你在导入被特定等级标记的条目（过时的、实验性的和不稳定的）时发出警告。目前默认的只有过时的会出现警告，但一旦标准库稳定后这个就会变化。

你可以像下面这样用 `warn` 属性：

```{rust,ignore}
#![warn(unstable)]
```

之后当你导入一个箱子(crate)时：

```{rust,ignore}
extern crate some_crate;
```

如果使用被标记为不稳定的东西，你会得到一个警告。

你可能注意到了在 `warn` 属性声明中有一个感叹号。这个感叹号意思是这个属性用于封装的条目而不是属性后的条目。所以这个 `warn` 属性申明用于封装的箱子本身，而不是后面跟的陈述：

```{rust,ignore}
// applies to the crate we're in
#![warn(unstable)]

extern crate some_crate;

// applies to the following `fn`.
#[test]
fn a_test() {
  // ...
}
```

## 编写测试（writing tests）

让我们以测试驱动的的方式写一个非常简单的箱子(crate)。现在你已经知道常规操作了，创建一个新工程：

```{bash,ignore}
$ cd ~/projects
$ cargo new testing --bin
$ cd testing
```

然后试试下面的：

```{notrust,ignore}
$ cargo run
   Compiling testing v0.0.1 (file:///home/you/projects/testing)
     Running `target/testing`
Hello, world!
```

很好，Rust 的基础设施支持两种地方的测试，它们用于两种测试：在箱子内部包含**单元测试**，在 `tests` 目录内放**集成测试**。单元测试是小的测试用来测试关注的单元。集成测试用来集成地测试多个单元。就是说，这是社会上的惯例，语法上并没有什么不同。让我们创建一个 `tests` 目录：

```{bash,ignore}
$ mkdir tests
```

然后在 `tests/lib.rs` 中创建一个集成测试：

```{rust,no_run}
#[test]
fn foo() {
    assert!(false);
}
```

尽管起一个描述型的名字很好，但如何命名测试函数并不重要，稍后你就会看到为什么。然后用一个宏 `assert!`，断言一些东西是真的。这种情况，我们使它为 `false`，所以测试会失败。让我们试一试！

```{notrust,ignore}
$ cargo test
   Compiling testing v0.0.1 (file:///home/you/projects/testing)
/home/you/projects/testing/src/main.rs:1:1: 3:2 warning: code is never used: `main`, #[warn(dead_code)] on by default
/home/you/projects/testing/src/main.rs:1 fn main() {
/home/you/projects/testing/src/main.rs:2     println!("Hello, world");
/home/you/projects/testing/src/main.rs:3 }

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured


running 1 test
test foo ... FAILED

failures:

---- foo stdout ----
        task 'foo' failed at 'assertion failed: false', /home/you/projects/testing/tests/lib.rs:3



failures:
    foo

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured

task '<main>' failed at 'Some tests failed', /home/you/src/rust/src/libtest/lib.rs:242
```

太多输出信息啦！让我们拆开慢慢看：

```{notrust,ignore}
$ cargo test
   Compiling testing v0.0.1 (file:///home/you/projects/testing)
```

你可以通过 `cargo test` 来运行所有测试。这会运行 `tests` 中的测试和你放在箱子中的测试。

```{notrust,ignore}
/home/you/projects/testing/src/main.rs:1:1: 3:2 warning: code is never used: `main`, #[warn(dead_code)] on by default
/home/you/projects/testing/src/main.rs:1 fn main() {
/home/you/projects/testing/src/main.rs:2     println!("Hello, world");
/home/you/projects/testing/src/main.rs:3 }
```

Rust 有一个默认会使用的 **lint**， 称为“在程序死掉时的警告（warn on dead code）”。lint 是一段代码，它检查你的程序，并且告诉你关于程序的一些东西。这种情况，Rust 警告我们写了一些永远不会用到的代码：我们的 `main` 函数。当然了，因为我们在跑测试，所以不会用 `main`。我们马上会只对这个函数关闭这个 lint。现在只需要忽略这个输出。

```{notrust,ignore}
running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured
```

等等，零个测试？我们一个测试都没有定义吗？对滴，这个是试图运行箱子中的测试时的输出，而在箱子中没有测试。你会注意到 Rust 报告几种类型的测试：通过的、失败的、忽略的和度量的。度量的测试指的是基准测试，我们马上就会涉及到它！

```{notrust,ignore}
running 1 test
test foo ... FAILED
```

现在我们到达某个地方了，记得前面说到的给测试起一个好名字吗？这就是为什么。这里说“测试 foo”，因为我们将测试命名为 foo。如果我们起了一个好的名字，会更加清楚哪个测试失败了，尤其是随着我们添加更多的测试。

```{notrust,ignore}
failures:

---- foo stdout ----
        task 'foo' failed at 'assertion failed: false', /home/you/projects/testing/tests/lib.rs:3



failures:
    foo

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured

task '<main>' failed at 'Some tests failed', /home/you/src/rust/src/libtest/lib.rs:242
```

所有的测试跑过之后，Rust 会从失败的测试中显示所有输出。在这个例子中，Rust 告诉我们用假断言失败了，这是我们预期的结果。

嚄！让我们来修复测试：

```{rust}
#[test]
fn foo() {
    assert!(true);
}
```

然后再次运行测试：

```{notrust,ignore}
$ cargo test
   Compiling testing v0.0.1 (file:///home/you/projects/testing)
/home/you/projects/testing/src/main.rs:1:1: 3:2 warning: code is never used: `main`, #[warn(dead_code)] on by default
/home/you/projects/testing/src/main.rs:1 fn main() {
/home/you/projects/testing/src/main.rs:2     println!("Hello, world");
/home/you/projects/testing/src/main.rs:3 }

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured


running 1 test
test foo ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
```

漂亮！我们的测试如预期一样通过了。我们来去掉 `main` 函数的警告，将 `src/main.rs` 改成下面这样：

```{rust}
#[cfg(not(test))]
fn main() {
    println!("Hello, world");
}
```

这个属性结合了两个东西：`cfg` 和 `not`。`cfg` 属性允许基于某个东西进行条件编译。下面的条目只会在将其配置为真时编译。当 Cargo 编译我们的测试时，会设定一些东西使 `cfg(test)` 为真。但当不为真时我们只想包括 `main`，所以用 `not` 来否定：`cfg(not(test))` 只会在 `cfg(test)` 为假时编译。

有了这个属性，就不会有那个警告了：

```{notrust,ignore}
$ cargo test
   Compiling testing v0.0.1 (file:///home/you/projects/testing)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured


running 1 test
test foo ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
```

好，现在让我们写一个实际的测试。将 `tests/lib.rs` 改成下面这样：

```{rust,ignore}
#[test]
fn math_checks_out() {
    let result = add_three_times_four(5i);

    assert_eq!(32i, result);
}
```

然后试着跑一下测试：

```{notrust,ignore}
$ cargo test
   Compiling testing v0.0.1 (file:///home/youg/projects/testing)
/home/youg/projects/testing/tests/lib.rs:3:18: 3:38 error: unresolved name `add_three_times_four`.
/home/youg/projects/testing/tests/lib.rs:3     let result = add_three_times_four(5i);
                                                            ^~~~~~~~~~~~~~~~~~~~
error: aborting due to previous error
Build failed, waiting for other jobs to finish...
Could not compile `testing`.

To learn more, run the command again with --verbose.
```

Rust 找不到这个函数，这是有道理的，因为我们还没有写这个函数呢！

为了让我们的测试共享这段程序，我们需要创建一个库的箱子。这也是一个好的软件设计：正如前面提到的，将大多数功能放到库的箱子中让可执行的箱子使用这个库是一个好主意，这可以使程序重用。

为了这样做，我们需要创建一个新模块。新建一个文件 `src/lib.rs`，然后将下面的程序写入里面：

```{rust}
# fn main() {}
pub fn add_three_times_four(x: int) -> int {
    (x + 3) * 4
}
```

我们将这个文件命名为 `lib.rs` 因为它与我们的工程有相同的名字，所以按惯例这样命名。

然后我们在 `src/main.rs` 中使用这个箱子：

```{rust,ignore}
extern crate testing;

#[cfg(not(test))]
fn main() {
    println!("Hello, world");
}
```

最后，从 `tests/lib.rs` 中导入这个函数：

```{rust,ignore}
extern crate testing;
use testing::add_three_times_four;

#[test]
fn math_checks_out() {
    let result = add_three_times_four(5i);

    assert_eq!(32i, result);
}
```

让我们跑一跑：

```{ignore,notrust}
$ cargo test
   Compiling testing v0.0.1 (file:///home/you/projects/testing)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured


running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured


running 1 test
test math_checks_out ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
```

好极了！通过了一个测试。我们已经有一个集成测试显示公共方法可以干活，但是也许我们也想测试一些内部逻辑。尽管这个函数很简单，要是它更加复杂的话，可以想象我们需要更多的测试。所以让我们将其拆成两个辅助函数，然后写一些单元测试来测试这些。

将 `src/lib.rs` 改成下面这样：

```{rust,ignore}
pub fn add_three_times_four(x: int) -> int {
    times_four(add_three(x))
}

fn add_three(x: int) -> int { x + 3 }

fn times_four(x: int) -> int { x * 4 }
```

如果运行 `cargo test`，你会得到相同的输出：

```{ignore,notrust}
$ cargo test
   Compiling testing v0.0.1 (file:///home/you/projects/testing)

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured


running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured


running 1 test
test math_checks_out ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
```

如果尝试为这两个函数写一个测试，就不起作用，例如：

```{rust,ignore}
extern crate testing;
use testing::add_three_times_four;
use testing::add_three;

#[test]
fn math_checks_out() {
    let result = add_three_times_four(5i);

    assert_eq!(32i, result);
}

#[test]
fn test_add_three() {
    let result = add_three(5i);

    assert_eq!(8i, result);
}
```

我们会得到这个错误：

```{notrust,ignore}
   Compiling testing v0.0.1 (file:///home/you/projects/testing)
/home/you/projects/testing/tests/lib.rs:3:5: 3:24 error: function `add_three` is private
/home/you/projects/testing/tests/lib.rs:3 use testing::add_three;
                                              ^~~~~~~~~~~~~~~~~~~
```

的确，因为它是私有的，所以外部的集成测试不起作用。我们需要一个单元测试。打开 `src/lib.rs` 然后添加下面这些：

```{rust,ignore}
pub fn add_three_times_four(x: int) -> int {
    times_four(add_three(x))
}

fn add_three(x: int) -> int { x + 3 }

fn times_four(x: int) -> int { x * 4 }

#[cfg(test)]
mod test {
    use super::add_three;
    use super::times_four;

    #[test]
    fn test_add_three() {
        let result = add_three(5i);

        assert_eq!(8i, result);
    }

    #[test]
    fn test_times_four() {
        let result = times_four(5i);

        assert_eq!(20i, result);
    }
}
```

让我们试一试：

```{ignore,notrust}
$ cargo test
   Compiling testing v0.0.1 (file:///home/you/projects/testing)

running 1 test
test test::test_times_four ... ok
test test::test_add_three ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured


running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured


running 1 test
test math_checks_out ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
```

帅气！我们内部函数现在有两个测试了。你会注意到现在有三组输出：一个是 `src/main.rs`，一个 `src/lib.rs` 和一个 `tests/lib.rs`。还有一个有意思的我们没有谈到，是下面这几行：

```{rust,ignore}
use super::add_three;
use super::times_four;
```

因为创建了一个嵌套的模块，我们可以用 `super` 从父模块中导入函数。子模块允许看到父模块中的私有函数。我们有时称 `use` 的这种用法为重导入，因为我们在别的地方又一次导入了这个名字。

现在我们已经涵盖了测试的基本概念。Rust 工具很原始，不过在简单情况下工作得很好。一些 rustaceans 正在所有这些之上构建更加复杂的测试框架，但他们才刚刚开始。

# 指针（pointer）

在系统编程中，指针是极其重要的话题。Rust 有一组非常丰富的指针，它们与许多其它语言中的作用不一样。它们足够重要以致于我们有一个专门的[指针指南](guide-pointers.html)更详细地讨论指针。实际上你现在正在阅读的指南，概述了语言各方面，还有许多其它指南详细地讨论特定的话题。你可以在[文档索引](index.html#guides)中找到指南列表。

在这一节中，我们假设你对指针的一般概念比较熟悉，如果不是，请阅读[指针简介](guide-pointers.html#an-introduction)小节的指针指南，然后回到这儿，我们会等你。

掌握了指针的要点了？非常好，让我们来讨论 Rust 中的指针。

## 引用（reference）

Rust 指针最原始的形式称为**引用**，引用由和符号（`&`）创建。这里有一个简单的引用：

```{rust}
let x = 5i;
let y = &x;
```

`y` 是 `x` 的引用，为了解引用（获得引用的值而不是引用本身）`y`，我们使用星号（`*`）：

```{rust}
let x = 5i;
let y = &x;

assert_eq!(5i, *y);
```

像所有的 `let` 绑定一样，引用默认是不可变的。你可以声明一个函数接受引用：

```{rust}
fn add_one(x: &int) -> int { *x + 1 }

fn main() {
    assert_eq!(6, add_one(&5));
}
```

如你所见，我们也可以将 `&` 应用到字面量（literal）上得到一个引用。当然，在这个简单的函数中，没有多少理由需要通过引用获得 `x`，这仅仅是语法的一个示例。

由于引用是不可变的，你可以有多个引用的**别名**（指向同一个地方）：

```{rust}
let x = 5i;
let y = &x;
let z = &x;
```

我们可以通过 `&mut` 而不是 `&` 使引用可变：

```{rust}
let mut x = 5i;
let y = &mut x;
```

注意到 `x` 也是可变的，如果不是这样的，像这样：

```{rust,ignore}
let x = 5i;
let y = &mut x;
```

Rust 会抱怨：

```{ignore,notrust}
6:19 error: cannot borrow immutable local variable `x` as mutable
 let y = &mut x;
              ^
```

我们不想要不可变数据的可变引用！这个错误消息使用了我们还没谈到的术语借出（borrow），我们马上就会说到这个。

这个简单的例子实际上阐明了许多 Rust 的强力：Rust 已经在编译时阻止我们违背我们的规则。因为 Rust 的引用检查在编译时完整地检查这些规则，这种安全没有运行时开销。在运行时，这些像在 C 或 C++ 中一样与原生机器指针相同。我们只是提前再次确认没有干什么危险的事。

Rust 也会阻止我们创建两个可变引用的别名，下面这个是不能干活的：

```{rust,ignore}
let mut x = 5i;
let y = &mut x;
let z = &mut x;
```

它会报下面这个错误：

```{notrust,ignore}
error: cannot borrow `x` as mutable more than once at a time
     let z = &mut x;
                  ^
note: previous borrow of `x` occurs here; the mutable borrow prevents subsequent moves, borrows, or modification of `x` until the borrow ends
     let y = &mut x;
                  ^
note: previous borrow ends here
 fn main() {
     let mut x = 5i;
     let y = &mut x;
     let z = &mut x;
 }
 ^
```

这是一个很长的错误信息，让我们来研究一会儿。错误消息有三部分：错误和两个注释。错误说的是我们所预期的：不能有两个指针指向同一个内存。

两个注释提供一些额外的内容。当错误很复杂时，Rust 错误信息通常包含这种额外的信息。Rust 告诉我们两件事情：第一，我们不能把 `x` 借给 `z` 的原因是我们之前已经把 `x` 借给 `y` 了；第二个注释显示 `x` 借给 `y` 截止到什么时候。

等等，你说外借？

为了真正地理解这个错误，我们需要学习一些新概念：**拥有**，**外借**和**生存期**。

## 拥有权，外借和生存期（ownership, borrowing and lifetimes）

无论何时当某类资源被创建时，必须同时有东西负责销毁该资源。既然我们现在在讨论指针，让我们来在内存分配上下文中讨论这个问题，尽管这也适用于其它资源。

当分配堆内存时，你需要一种机制来释放那块内存。许多语言让程序员控制内存分配，然后由垃圾回收来处理释放内存。这是一个有效的，经过时间检验的策略，但并不是没有缺点的。因为程序员不需要过多地考虑内存释放，分配内存很常见，因为这很容易。如果你需要精确地控制何时释放内存，将其交给运行时会让这个变得很困难。

Rust 选择了不同的途径，这条途径称为**所有权**。任何创建资源的绑定是那个资源的**所有者**。

作为所有者给你提供了一些特权：

1. 你可以控制何时释放资源
2. 随你喜欢，你可以以不可变的形式将那个资源借给任意多的借方
3. 你可以将那个资源以可变的形式借给一个借方

但仍然有一些限制：

1. 如果某人正在借用你的资源（可变或不可变），你不能改变这个资源或以可变的形式借给别人
2. 如果如果某人正以可变的形式借用你的资源，你不能再将其借出（不管是可变还是不可变）或以任何方式访问它

借入和借出中间究竟发生了什么事？当你分配内存时，你获得一个指向那块内存的指针。这个指针允许你维护所说的内存。如果你是指针的所有者，那么你可以让另一个绑定临时的借走指针，然后它们可以维护那块内存。借方从你那儿借出指针的时间长度称为**生存期**。

如果不同的绑定共享一个指针，而且指针指向的内存是不可变的，那么屁事也没有。但如果是可变的话，两个指针可能同时尝试写那块内存，导致**竞争条件**。因而如果有人想改变从你那儿借走的指针，你肯定不能将那个指针借给其他人。

Rust 有一个称为**借出检验器**的复杂系统来确保每个人都按这些规则玩。它在编译时核实没有破坏规则。如果没有问题，程序会顺利编译，这方面没有任何运行时开销。借出检验器只在编译时工作。如果它确实发现了问题，会报告**生存期错误**，程序就会拒绝编译。

需要消化很多知识，这也是 Rust 所有概念中最重要之一。让我们实际看看这个语法：

```{rust}
{
    let x = 5i; // x is the owner of this integer, which is memory on the stack.

    // other code here...

} // privilege 1: when x goes out of scope, this memory is deallocated

/// this function borrows an integer. It's given back automatically when the
/// function returns.
fn foo(x: &int) -> &int { x }

{
    let x = 5i; // x is the owner of this integer, which is memory on the stack.

    // privilege 2: you may lend that resource, to as many borrowers as you'd like
    let y = &x;
    let z = &x;

    foo(&x); // functions can borrow too!

    let a = &x; // we can do this alllllll day!
}

{
    let mut x = 5i; // x is the owner of this integer, which is memory on the stack.

    let y = &mut x; // privilege 3: you may lend that resource to a single borrower,
                    // mutably
}
```

如果你是借方，你也有一些特权，但同时也要遵循一些限制：

1. 如果借的是不可变量，你可以读指针指向的数据
2. 如果借的是可变量，你可以读和写指针指向的数据
3. 你可以以不可变的形式将指针借给别人，**但是**
4. 当你这样干时，在你归还自己所借的之前，他们必须先归还给你

最后一个要求看起来很奇怪，但也有道理。如果你需要归还，而你又将其借给别人了，他们必须归还给你，你再归还。如果没有这样做，指针所有者可能释放了内存，而借我们指针的人的指针可能会指向无效内存。这被称为悬空指针（dangling pointer）。

让我们再查看一下导致我们说这么多的那个错误，它破坏了以可变形式借出资源的所有者上的限制。
程序是：

```{rust,ignore}
let mut x = 5i;
let y = &mut x;
let z = &mut x;
```

错误消息是：

```{notrust,ignore}
error: cannot borrow `x` as mutable more than once at a time
     let z = &mut x;
                  ^
note: previous borrow of `x` occurs here; the mutable borrow prevents subsequent moves, borrows, or modification of `x` until the borrow ends
     let y = &mut x;
                  ^
note: previous borrow ends here
 fn main() {
     let mut x = 5i;
     let y = &mut x;
     let z = &mut x;
 }
 ^
```

这个错误出现在三个部分，让我们挨个过一遍。

```{notrust,ignore}
error: cannot borrow `x` as mutable more than once at a time
     let z = &mut x;
                  ^
```

这条错误陈述了限制：你不能同时以可变形式多次借出某个东西。借出检验器知道规则！

```{notrust,ignore}
note: previous borrow of `x` occurs here; the mutable borrow prevents subsequent moves, borrows, or modification of `x` until the borrow ends
     let y = &mut x;
                  ^
```

一些编译器错误带有注释帮助你修复错误。这个错误有两个注释，这是第一个。这个注释报告第一次可变借出发生在哪里。错误显示的是第二次借出，所以现在我们知道了出问题的两部分。它也提到了规则3，提醒我们在借出结束之前不能改变 `x` 的值。

```{notrust,ignore}
note: previous borrow ends here
 fn main() {
     let mut x = 5i;
     let y = &mut x;
     let z = &mut x;
 }
 ^
```

这是第二条注释，让我们知道第一次借出会在哪里结束。这很有用，因为如果等到这个借出结束后再尝试借 `x`，一切就 OK 了。

更多高级模式，请参考[生存期指南](guide-lifetimes.html)。你也会学到带有 `'a` 的类型签名是什么意思：

```{rust,ignore}
pub fn as_maybe_owned(&self) -> MaybeOwned<'a> { ... }
```

## 盒子（boxes）

到目前为止，我们创建的所有变量的引用都是在栈上。Rust 中分配堆变量最简单的方式是使用**盒子**。为了创建盒子请使用 `box` 关键词：

```{rust}
let x = box 5i;
```

这在堆上分配一个整数 `5`，然后创建一个绑定 `x` 指向这个数。盒子型指针最棒的是我们不需要手动去释放分配的内存！如果我们写下面的程序：

```{rust}
{
    let x = box 5i;
    // do stuff
}
```

那么 Rust 就会在该块结束的地方自动释放 `x`。这并不是因为 Rust 有垃圾回收器，Rust 并没有垃圾回收器。而实际上是当 `x` 离开作用域后，Rust 释放 `x`。这段 Rust 程序与下面这段 C 程序做相同的事情：

```{c,ignore}
{
    int *x = (int *)malloc(sizeof(int));
    // do stuff
    free(x);
}
```

这意味着我们得到了手动管理内存的好处，但编译器保证我们没有做错什么，我们不会忘记释放内存。

盒子是其内容的单独的所有者，所以不能以可变地形式引用它们，然后使用原始的盒子：

```{rust,ignore}
let mut x = box 5i;
let y = &mut x;

*x; // you might expect 5, but this is actually an error
```

这会报下面的错误：

```{notrust,ignore}
8:7 error: cannot use `*x` because it was mutably borrowed
 *x;
 ^~
 6:19 note: borrow of `x` occurs here
 let y = &mut x;
              ^
```

只要 `y` 正在借用 `x` 的内容，我们就不能使用 `x`。当 `y` 借用完这个值之后，我们就又可以用它了。下面的就可以很好地干活：

```{rust}
let mut x = box 5i;

{
    let y = &mut x;
} // y goes out of scope at the end of the block

*x;
```

## Rc 和 Arc

有时，你需要在堆上分配一些东西，但是分派多个（指向）该内存的引用。Rust 的 `Rc<T>`（读作’啊西梯‘）和 `Arc<T>` 类型（同样这里的 `T` 是泛型，后面会学到更多）提供这种能力。**RC** 代表’引用计数(reference counted)‘，而 **Arc** 代表’原子引用计数(atomically reference counted)‘。这就是 Rust 如何跟踪多个所有者的方法：每次创建一个新的指向 `Rc<T>` 的引用，我们就会向内部引用计数加一，而每次一个引用离开作用域后，计数减一。当计数为零时，`Rc<T>` 就可以安全地释放了。`Arc<T>` 几乎和 `Rc<T>` 一样，除了一点：’Arc‘ 中的 ’atomically‘ 意思是引用计数增加或减少用的是线程安全机制。为什么有两种类型呢？因为 `Rc<T>` 更快，所以如果不在多线程情景下，你可以使用这个优势。由于还没有谈到 Rust 中的线程，我们会在这节中剩余部分用 `Rc<T>` 作示例。

用 `Rc::new()` 创建一个 `Rc<T>`：

```{rust}
use std::rc::Rc;

let x = Rc::new(5i);
```

用 `.clone()` 方法创建第二个引用：

```{rust}
use std::rc::Rc;

let x = Rc::new(5i);
let y = x.clone();
```

`Rc<T>` 会与其任意引用存活一样长。所有引用都离开作用域后，内存就会被释放。

如果使用 `Rc<T>` 或 `Arc<T>`，你需要小心引入循环。如果有两个 `Rc<T>` 彼此指向对方，引用计数永远不会减为0，从而就会导致内存泄漏。想了解更多，查看[指针指南中的 `Rc<T>` 和 `Arc<T>` 小节](http://doc.rust-lang.org/guide-pointers.html#rc-and-arc)。

# 模式（patterns）

在这个指南中我们已经用过好几次模式了：第一次是在 `let` 绑定中，第二次是在 `match` 陈述里。让我们来一个旋风之旅：看看所有模式可以做的事情。

快速回顾：你可以直接匹配字面量，并使用 `_` 匹配任何其它情况：

```{rust}
let x = 1i;

match x {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    _ => println!("anything"),
}
```

你可以用 `|` 匹配多个模式：

```{rust}
let x = 1i;

match x {
    1 | 2 => println!("one or two"),
    3 => println!("three"),
    _ => println!("anything"),
}
```

你可以用 `..` 匹配一个范围内的值：

```{rust}
let x = 1i;

match x {
    1 .. 5 => println!("one through five"),
    _ => println!("anything"),
}
```

范围大多数用于整数和单个字符。

如果用 `|` 或 `..` 匹配多个值，你可以用 `@` 将一个值绑定到一个名字：

```{rust}
let x = 1i;

match x {
    x @ 1 .. 5 => println!("got {}", x),
    _ => println!("anything"),
}
```

如果匹配含有变量的枚举，你可以用 `..` 忽略变量中的值：

```{rust}
enum OptionalInt {
    Value(int),
    Missing,
}

let x = Value(5i);

match x {
    Value(..) => println!("Got an int!"),
    Missing   => println!("No such luck."),
}
```

你可以用 `if` 引入**匹配看守（match guards）**：

```{rust}
enum OptionalInt {
    Value(int),
    Missing,
}

let x = Value(5i);

match x {
    Value(x) if x > 5 => println!("Got an int bigger than five!"),
    Value(..) => println!("Got an int!"),
    Missing   => println!("No such luck."),
}
```

如果匹配指针，你可以使用与声明指针相同的语法，在前面加上 `&`：

```{rust}
let x = &5i;

match x {
    &x => println!("Got a value: {}", x),
}
```

这里 `match` 里面的 `x` 类型为 `int`，换句话说，模式左边析构其值。如果我们有 `&5i`，那么在 `&x` 中的 `x` 就会是 `5i`。

如果你想得到引用，请使用 `ref` 关键词：

```{rust}
let x = 5i;

match x {
    ref x => println!("Got a reference to {}", x),
}
```

这里 `match` 里的 `x` 类型为 `&int`，换句话说，`ref` 关键词_创建_一个引用，用于模式中。如果你需要一个可变的引用，`ref mut` 将以相同方式工作：

```{rust}
let mut x = 5i;

match x {
    ref mut x => println!("Got a mutable reference to {}", x),
}
```

如果有一个结构，你可以在模式内部进行析构：

```{rust}
struct Point {
    x: int,
    y: int,
}

let origin = Point { x: 0i, y: 0i };

match origin {
    Point { x: x, y: y } => println!("({},{})", x, y),
}
```

如果只关注其中部分值，我们就可以不用给出所有变量的名字：

```{rust}
struct Point {
    x: int,
    y: int,
}

let origin = Point { x: 0i, y: 0i };

match origin {
    Point { x: x, .. } => println!("x is {}", x),
}
```

嚄！以上是不同方式的匹配，而你可以根据你所做的事情将以上所讲的混合并匹配：

```{rust,ignore}
match x {
    Foo { x: Some(ref name), y: None } => ...
}
```

模式非常强大，好好利用它们。

# 方法语法（method syntax）

函数很棒，但如果你要对一些数据调用一大堆的函数，就很不方便了。考虑下面的程序：

```{rust,ignore}
baz(bar(foo(x)));
```

我们会从左往右读，因此看到的是 'baz bar foo'。但这却不是函数被调用的顺序，而正好是反过来的：'foo bar baz'。按下面这样做是不是更好呢？

```{rust,ignore}
x.foo().bar().baz();
```

幸运的是，正如你可能已经从引导性问题中猜到了，确实可以这样做！Rust 通过 `impl` 关键词提供这种使用**方法调用语法**的能力。

下面说明它是如何工作的：

```{rust}
struct Circle {
    x: f64,
    y: f64,
    radius: f64,
}

impl Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * (self.radius * self.radius)
    }
}

fn main() {
    let c = Circle { x: 0.0, y: 0.0, radius: 2.0 };
    println!("{}", c.area());
}
```

这会打印 `12.566371`。

我们创建了一个代表圆的结构，然后在其内部写了一个 `impl` 块，定义了一个 `area` 方法。方法接受一个特殊的首参 `&self`。它有三个变种：`self`，`&self` 和 `&mut self`。你可以将这个首参看成 `x.foo()` 中的 `x`。这三个变种对应 `x` 三种可能的值：`self`，如果只是栈上的值；`&self`，如果是一个引用；`&mut self`，如果是一个可变的引用。我们应该默认使用 `&self`，因为这是最常见的。

最后，你可能还记得，圆的半径是 `π*r²`（译注：废话，不记得的可以去撞墙了）。因为我们在 `area` 中接受了 `&self` 参数，我们可以像任何其它参数一样使用。因为我们知道它是一个 `Circle`，我们可以像其它结构一样访问 `radius`。导入 π，然后再做一些乘法，就得到了面积。

你也可以定义不使用 `self` 参数的方法。下面是 Rust 程序中非常常见的一种模式：

```{rust}
struct Circle {
    x: f64,
    y: f64,
    radius: f64,
}

impl Circle {
    fn new(x: f64, y: f64, radius: f64) -> Circle {
        Circle {
            x: x,
            y: y,
            radius: radius,
        }
    }
}

fn main() {
    let c = Circle::new(0.0, 0.0, 2.0);
}
```

这个**静态方法**为我们构建了一个新的 `Circle`。注意，静态方法是用 `Struct::method()` 语法调用，而不是 `ref.method()`。

# 闭包（closures）

到目前为止我们已经在 Rust 中写了很多函数了，但每个函数我们都起了名字。Rust 还允许我们创建匿名函数，Rust 的匿名函数称为**闭包**。闭包本身没多大意思，但当你将以闭包为参数的函数与之结合时，就可能变得非常强大。

让我们创建一个闭包：

```{rust}
let add_one = |x| { 1i + x };

println!("The 5 plus 1 is {}.", add_one(5i));
```

我们用 `|...| { ... }` 语法创建了一个闭包，接着创建了一个绑定以便后面可以使用。注意我们用绑定名和两个括弧调用函数，正如我们调用一个有名字的函数一样。

让我们比较一下语法，下面两行非常相近：

```{rust}
let add_one = |x: int| -> int { 1i + x };
fn  add_one   (x: int) -> int { 1i + x }
```

你可能注意到了，闭包推断其参数和返回值，所以你不用声明它们。这与具名函数不一样，它们默认返回单元（`()`）。

闭包和具名函数的一个很大的不同点就在其名字中：闭包在其环境中闭合。这句话什么意思呢？意思是：

```{rust}
fn main() {
    let x = 5i;

    let printer = || { println!("x is: {}", x); };

    printer(); // prints "x is: 5"
}
```

`||` 语法表示这是一个不接受参数的匿名函数，没有它就只有一个 `{}` 代码块了。

换句话说，闭包可以访问其定义范围内的变量，闭包借走了所有它用到的变量，下面这个会报错：

```{rust,ignore}
fn main() {
    let mut x = 5i;

    let printer = || { println!("x is: {}", x); };

    x = 6i; // error: cannot assign to `x` because it is borrowed
}
```

## 过程（proc）

Rust 有第二种闭包，称为 **proc**，过程用 `proc` 关键词创建：

```{rust}
let x = 5i;

let p = proc() { x * x };
println!("{}", p()); // prints 25
```

过程与闭包有很大的区别：它们只能被调用一次。当我们试图编译下面程序时会报错：

```{rust,ignore}
let x = 5i;

let p = proc() { x * x };
println!("{}", p());
println!("{}", p()); // error: use of moved value `p`
```

这个限制很重要，过程允许消费它们捕捉到的值，因此限制其只能调用一次有稳健的理由：任何被消费掉的值在第二次调用时都会变得无效。

过程对于 Rust 的并发特性非常有用，所以我们暂时将其搁置一边，在“任务”一节会再讨论它们。

## 接受闭包作为参数

闭包作为另一个函数的参数非常有用，这里有一个例子：

```{rust}
fn twice(x: int, f: |int| -> int) -> int {
    f(x) + f(x)
}

fn main() {
    let square = |x: int| { x * x };

    twice(5i, square); // evaluates to 50
}
```

让我们从 `main` 来拆解示例：

```{rust}
let square = |x: int| { x * x };
```

我们之前已经见过这个了，我们创建一个闭包，接收一个整数并返回其平方。

```{rust,ignore}
twice(5i, square); // evaluates to 50
```

这一行更有意思，这里我们调用函数 `twice`，传递两个参数：整数 `5` 和闭包 `square`。这与给函数传递任何两个其它变量绑定一样，但是如果我们之前没有用过闭包，这就显得有点小复杂了。只需要想：“我传递了两个变量，一个是整数，一个是函数”。

然后我们看看 `twice` 是怎么定义的：

```{rust,ignore}
fn twice(x: int, f: |int| -> int) -> int {
```

`twice` 接收两个参数：`x` 和 `f`。这就是为什么我们用两个参数调用它。`x` 是一个 `int`，我们已经这样干过很多次了；而 `f` 是一个函数，这个函数接收一个 `int` 并返回一个 `int`。注意 `|int| -> int` 语法与我们上面定义的 `square` 是多么像，如果我们加入返回类型的话：

```{rust}
let square = |x: int| -> int { x * x };
//           |int|    -> int
```

这个函数接收一个 `int` 并返回一个 `int`。这是目前为止我们见过最复杂的函数签名！多读几遍直到你领悟到它是如何工作的。这需要一点点实践，然后就会变得很容易了。

最后，`twice` 也返回一个 `int`。

OK，让我们来看看 `twice` 的函数体：

```{rust}
fn twice(x: int, f: |int| -> int) -> int {
  f(x) + f(x)
}
```

由于我们的闭包名为 `f`，我们可以像之前的闭包一样调用它。传递参数 `x` 给每一个 `f`，因而是两次。

如果做数学计算，`(5 * 5) + (5 * 5) == 50`，所以这就是我们得到的输出。

耍耍这个概念直到你感到舒服为止，Rust 标准库在适当的地方使用了大量的闭包，所以你会经常使用这种技术。

如果不想给 `square` 起一个名字，我们也可以只在行内定义它。下面这个例子与之前的一样：

```{rust}
fn twice(x: int, f: |int| -> int) -> int {
    f(x) + f(x)
}

fn square(x: int) -> int { x * x }

fn main() {
    twice(5i, square); // evaluates to 50
}
```

具名函数的名字可以在任何你用闭包的地方使用，可以用另一种方法写前一个示例：

```{rust}
fn twice(x: int, f: |int| -> int) -> int {
    f(x) + f(x)
}

fn square(x: int) -> int { x * x }

fn main() {
    twice(5i, square); // evaluates to 50
}
```

这样做并不特别常见，但偶尔会很有用。

这就是所有你理解闭包所需要的！闭包刚开始时有点奇怪，但一旦你习惯了，你就会在所有缺失闭包的语言中想念它们。将函数传给其它函数极其强大，接下来让我们看看其中的一个：迭代器。

# 迭代器（iterators）

让我们来谈谈循环。

记得 Rust 的 `for` 循环吗？这里是一个例子：

```{rust}
for x in range(0i, 10i) {
    println!("{:d}", x);
}
```

你现在已经很熟悉 Rust 了，我们可以详细讨论这个是如何工作的了。`range` 函数返回一个**迭代器**，迭代器是可以对其重复调用 `.next()` 方法的东西，而它会返回一系列的值。

像这样：

```{rust}
let mut range = range(0i, 10i);

loop {
    match range.next() {
        Some(x) => {
            println!("{}", x);
        }
        None => { break }
    }
}
```

我们对迭代器 `range` 返回的值创建了一个可变的绑定。然后用内部 `match` 进行 `loop`。这个 `match` 用在 `range.next()` 的结果上，给出迭代器的下一个值的引用。`next` 返回一个 `Option<int>`，在这种情况下，当我们有一个值时返回 `Some(int)`，而值都跑完后返回 `None`。如果得到 `Some(int)`，就打印出来；如果得到 `None`，就从循环中 `break` 出来。

这段程序示例基本上与 `for` 循环的版本一样。`for` 循环只是一种写 `loop`/`match`/`break` 结构的比较方便的方式。

然而 `for` 循环不是唯一一种使用迭代器的，自己写一个迭代器涉及到实现 `Iterator` 特征（trait）。尽管这么干超出了本指南的范围，Rust 提供许多有用的迭代器来完成不同的任务。在谈这些之前，我们应当谈谈 Rust 的反模式（anti-pattern），那就是 `range`。

是的，我们刚才谈到 `range` 是如何的酷，但 `range` 也是非常基本的。例如，如果你需要遍历一个向量，你可能打算这样写：

```{rust}
let nums = vec![1i, 2i, 3i];

for i in range(0u, nums.len()) {
    println!("{}", nums[i]);
}
```

严格来说，这个比用实际的迭代器更糟糕。向量上的 `.iter` 方法返回一个迭代器，按顺序遍历向量每个元素的引用，所以这样写：

```{rust}
let nums = vec![1i, 2i, 3i];

for num in nums.iter() {
    println!("{}", num);
}
```

这样做有两个原因：首先这个更直接的表达了我们的意思，我们是遍历整个向量而不是遍历索引然后再索引这个向量；其次这个版本更高效，第一个版本需要检查额外的绑定因为使用了索引 `nums[i]`。但由于对向量的每个元素，迭代器按顺序产生了一个引用，所以第二个例子没有绑定检查。这对于迭代器非常常见：我们可以忽略不必要的绑定检查，同时仍然是安全的。

还有一点细节没有 100% 搞清楚：`println!` 如何工作的。`num` 的类型实际上是 `&int`，即它是 `int` 的引用而不是 `int` 本身。`println!` 为我们处理解引用，所以我们看不到。下面的程序也可以工作：

```{rust}
let nums = vec![1i, 2i, 3i];

for num in nums.iter() {
    println!("{}", *num);
}
```

这里我们显式地解引用 `num`，为什么 `iter()` 会给我们引用呢？如果给我们的是数据本身，我们必须是它的拥有者，这会涉及到拷贝数据然后给我们数据的拷贝。而给我们引用的话，我们只是借走了数据的引用，因而只传递一个引用，而不需拷贝。

所以现在我们已经证实了 `range` 通常不是你想要的，让我们讨论一下你真正想要的。

这里有三个相关的类：迭代器，**迭代器适配器（iterator adapter）**，**消费者（consumer）**。下面是一些定义：

* 迭代器给出一系列值
* 迭代器适配器对迭代器进行操作，生成一个输出不同序列的新迭代器
* 消费者对迭代器进行操作，生成一些最终值的集合

由于你已经见过一个迭代器 `range` 了，让我们先讨论消费者。

## 消费者（consumer）

消费者对迭代器进行操作，返回某类值。最常见的消费者是 `collect()`，下面的程序不会编译，但显示了我们的意图：

```{rust,ignore}
let one_to_one_hundred = range(0i, 100i).collect();
```

如你所见，我们在迭代器上调用 `collect()`。`collect()` 接收迭代器所给的值，返回结果的集合。那么为什么不能编译呢？Rust 无法判断你想收集什么类型的值，所以你需要告诉它。下面是可以编译的版本：

```{rust}
let one_to_one_hundred = range(0i, 100i).collect::<Vec<int>>();
```

如果你记得的话，`::<>` 语法允许我们给出一个类型提示，所以我们告诉编译器我们想要整型的向量。

`collect()` 是最常见的消费者，但还有其它的消费者，`find()` 是一个：

```{rust}
let one_to_one_hundred = range(0i, 100i);

let greater_than_forty_two = range(0i, 100i)
                             .find(|x| *x >= 42);

match greater_than_forty_two {
    Some(_) => println!("We got some numbers!"),
    None    => println!("No numbers found :("),
}
```

`find` 接收一个闭包，作用于迭代器的每一个元素的引用上。如果元素是我们要找的，闭包就返回 `true`；否则返回 `false`。因为可能找不到匹配的元素，`find` 返回 `Option` 而不是元素本身。另一个重要的消费者是 `fold`，它看起来是这样的：

```{rust}
let sum = range(1i, 100i)
              .fold(0i, |sum, x| sum + x);
```

`fold()` 是一个长这样的消费者：`fold(base, |accumulator, element| ...)`。它接收两个参数：第一个称为基；第二个是闭包，其本身又接收两个参数：第一个称为收集器（accumulator），第二个是一个元素。每次迭代，闭包被调用，其结果是下一次迭代收集器的值。第一次迭代的基是收集器的值。

OK，有点混乱，让我们看看迭代器所有这些东西的值：

| base | accumulator | element | closure result |
|------|-------------|---------|----------------|
| 0i   | 0i          | 1i      | 1i             |
| 0i   | 1i          | 2i      | 3i             |
| 0i   | 3i          | 3i      | 6i             |

我们用这些参数调用 `fold()`：

```{rust}
# range(1i, 5i)
.fold(0i, |sum, x| sum + x);
```

所以 `0i` 是基，`sum` 是收集器，`x` 是元素。第一次迭代，我们将 `sum` 设成 `0i`，`x` 是 `nums` 的第一个元素 `1i`。然后将 `x` 加到 `sum` 上，得到 `0i + 1i = 1i`，第二次迭代，这个值变成收集器 `sum`，元素成了数组的第二个元素 `2i`。`1i + 2i = 3i`，因而成了最后一次迭代收集器的值。在这个迭代中，`x` 是最后一个元素 `3i`，`3i + 3i = 6i` 是求和的最后结果。`1 + 2 + 3 = 6`，这就是我们得到的结果。

嚄，刚开始几次你看到 `fold` 可能有点奇怪，不过一旦你熟悉了，在所有地方都可以用了。任何时候如果有一个列表，而你想得到单一的结果，`fold` 就比较合适。

消费者很重要，是由于我们还没谈到的迭代器的另一个性质：惰性求值。让我们再谈谈迭代器，然后你就会明白为什么消费者很重要。

## 迭代器（iterators）

正如我们前面说的那样，我们可以对迭代器反复调用 `.next()` 方法，然后它给我们返回一个序列。因为你必须调用方法，所以意味着迭代器是**惰性的**。例如下面这段程序并不会实际地生成 `1-100` 的数字，而只是创建了一个值代表这个序列：

```{rust}
let nums = range(1i, 100i);
```

因为我们没有对 range 做任何事情，它并不会生成这个序列。

一旦我们添加消费者：

```{rust}
let nums = range(1i, 100i).collect::<Vec<int>>();
```

现在 `collect()` 会要求 `range()` 给出一些数字，所以它就会干实际产生序列的活。

`range` 是你即将看到的两个基本迭代器中的一个。另一个是 `iter()` ，之前也用到过。`iter()` 可以将一个向量变成一个简单的迭代器，依次给出每个元素：

```{rust}
let nums = [1i, 2i, 3i];

for num in nums.iter() {
   println!("{}", num);
}
```

这两个基本的迭代器应该就能把你服侍地妥妥的。不过还有一些其它的高级迭代器，包括一些无穷迭代器，像 `count`：

```{rust}
std::iter::count(1i, 5i);
```

这个迭代器从 1 开始数，每次增加 5。它会每次给你一个新的整数直到永远。不过技术上是直到 `int` 能表示的最大的数。但是由于迭代器是惰性的，这总是没有问题的！反正你不会想着在其上调用 `collect()`。。。

关于迭代器已经啰唆的足够多了。迭代器适配器是我们讨论的关于迭代器的最后一个概念。让我们来看看迭代器适配器！

## 迭代器适配器（iterator adapter）

迭代器适配器接收一个迭代器，然后以某种方式修改它，产生一个新的迭代器。最简单的一个被称为 `map`：

```{rust,ignore}
range(1i, 100i).map(|x| x + 1i);
```

`map` 在一个迭代器上调用，产生一个新迭代器，其每个元素引用有一个闭包作为调用参数。所以这个会给我们 `2-101` 的数字。然而如果你编译这个例子，你会得到一个警告：

```{notrust,ignore}
2:37 warning: unused result which must be used: iterator adaptors are lazy and
              do nothing unless consumed, #[warn(unused_must_use)] on by default
 range(1i, 100i).map(|x| x + 1i);
 ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

惰性又一次罢工！这个闭包根本不会执行。这个例子不会打印任何数字：

```{rust,ignore}
range(1i, 100i).map(|x| println!("{}", x));
```

如果你尝试用其副作用在迭代器上执行闭包，请用 `for` 替换。

有大量有意思的迭代器适配器。`take(n)` 会从一个迭代器中获取前 `n` 项，并以列表返回。让我们在前面的无穷迭代器 `count()` 上试一试：

```{rust}
for i in std::iter::count(1i, 5i).take(5) {
    println!("{}", i);
}
```

这会打印：

```{notrust,ignore}
1
6
11
16
21
```

`filter()` 是一个适配器，接收一个闭包作为参数。这个闭包返回 `true` 或 `false`，新迭代器只在闭包返回值为 `true` 的元素上产生返回值：

```{rust}
for i in range(1i, 100i).filter(|x| x % 2 == 0) {
    println!("{}", i);
}
```

这会打印所有1到100之间的偶数。

你可以将所有东西链起来：从迭代器开始，调整适配几次，然后消费这些结果，试试下面的：

```{rust}
range(1i, 1000i)
    .filter(|x| x % 2 == 0)
    .filter(|x| x % 3 == 0)
    .take(5)
    .collect::<Vec<int>>();
```

这会给你一个含有 `6`, `12`, `18`, `24` 和 `30` 的向量。

这只是对迭代器、迭代器适配器和消费者的一个小小的尝试。有许多真正有用的迭代器，你也可以自己写迭代器。迭代器提供安全、高效的方式使用各种列表。开始时有点奇怪，但是一旦你开始玩起来，你就会上瘾的。关于不同迭代器和消费者的完整列表，请参考[迭代器模块文档](std/iter/index.html)。


# 泛型（generics）

当写一个函数或数据类型时，我们可能希望对多种类型的参数都能起作用。例如，还记得我们的 `OptionalInt` 类型吗？

```{rust}
enum OptionalInt {
    Value(int),
    Missing,
}
```

如果我们也希望有一个 `OptionalFloat64`，就得需要一个新枚举：

```{rust}
enum OptionalFloat64 {
    Valuef64(f64),
    Missingf64,
}
```

这样真的很不爽，幸运的是 Rust 有一个特性能给我们一个更好的方法：泛型。泛型在类型理论中被称为**参数多态**，意思是对于给定的参数（"parametric"）具有多种形式的类型或函数（"poly" 意思是多，"morph" 意思是形式）。

好了，类型理论的说明已经足够多了，让我们看看 `OptionalInt` 的泛型。实际上它是由 Rust 本身提供的，这货长这样：

```rust
enum Option<T> {
    Some(T),
    None,
}
```

`<T>` 部分你之前已经看到过几次了，它指出了这是一个泛型数据类型。在我们枚举申明中，其中的 `T` 会被泛型中用到的相同的类型替换。这里有一个用 `Option<T>` 的例子和一些额外的类型注记：

```{rust}
let x: Option<int> = Some(5i);
```

在类型声明中用 `Option<int>`，注意到这与 `Option<T>` 非常相似。因此在这个特定的 `Option` 中，`T` 的值是 `int`。在绑定的右边，我们创建了一个 `Some(T)` 其中 `T` 是 `5i`。因为这是一个 `int`，两边都能匹配，所以 Rust 很嗨皮。如果不匹配的话，就会得到一个错误：

```{rust,ignore}
let x: Option<f64> = Some(5i);
// error: mismatched types: expected `core::option::Option<f64>`
// but found `core::option::Option<int>` (expected f64 but found int)
```

这并不是说我们不能创建一个 `Option<T>` 存放 `f64`，而是说必须匹配：

```{rust}
let x: Option<int> = Some(5i);
let y: Option<f64> = Some(5.0f64);
```

这就好了：一次定义，多次使用。

泛型不仅仅可以对一个类型，考虑 Rust 内置的 `Result<T, E>` 类型：

```{rust}
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

这个类型是对两个类型的泛型：`T` 和 `E`。顺便说一下，大写字母可以是任何你喜欢的字母。只要我们喜欢，可以如下定义 `Result<T, E>`：

```{rust}
enum Result<H, N> {
    Ok(H),
    Err(N),
}
```

惯例是第一个泛型参数应该是 `T`，意思是 'type'，用 `E` 表示 'error'，尽管 Rust 不关心这些。

类型 `Result<T, E>` 用来返回计算结果，并且能在无法计算时给出一个错误。下面是一个例子：

```{rust}
let x: Result<f64, String> = Ok(2.3f64);
let y: Result<f64, String> = Err("There was an error.".to_string());
```

这个特定的结果会在成功时返回一个 `f64` 类型的值，失败时返回一个 `String` 类型的值。让我们写一个使用 `Result<T, E>` 的函数：

```{rust}
fn inverse(x: f64) -> Result<f64, String> {
    if x == 0.0f64 { return Err("x cannot be zero!".to_string()); }

    Ok(1.0f64 / x)
}
```

我们不希望求 0 的逆，所以先检查确保传递的不是 0。如果是 0 的话，则返回一个 `Err` 和一个消息。如果成功则返回封装着答案的 `Ok` 。

为什么这一点重要呢？还记得 `match` 是如何详尽地匹配的吗？下面是如何使用这个函数的：

```{rust}
# fn inverse(x: f64) -> Result<f64, String> {
#     if x == 0.0f64 { return Err("x cannot be zero!".to_string()); }
#     Ok(1.0f64 / x)
# }
let x = inverse(25.0f64);

match x {
    Ok(x) => println!("The inverse of 25 is {}", x),
    Err(msg) => println!("Error: {}", msg),
}
```

`match` 强制我们处理 `Err` 的情况，另外由于答案是封装在 `Ok` 中的，我们不能只使用结果而不进行匹配：

```{rust,ignore}
let x = inverse(25.0f64);
println!("{}", x + 2.0f64); // error: binary operation `+` cannot be applied
           // to type `core::result::Result<f64,collections::string::String>`
```

这个函数很棒，但是有一个问题：只在 64 位浮点数时正常工作。如果我们也想处理 32 位浮点数怎么办呢？我们可能会这样写：

```{rust}
fn inverse32(x: f32) -> Result<f32, String> {
    if x == 0.0f32 { return Err("x cannot be zero!".to_string()); }

    Ok(1.0f32 / x)
}
```

真让人失望！我们需要的是一个**泛型函数**，幸运的是我们可以写一个。然而还无法很好的工作。在我们深入这个之前，先讨论一下语法。`inverse` 的泛型版本长下面这样：

```{rust,ignore}
fn inverse<T>(x: T) -> Result<T, String> {
    if x == 0.0 { return Err("x cannot be zero!".to_string()); }

    Ok(1.0 / x)
}
```

正如定义 `Option<T>` 一样，我们使用相似的语法定义 `inverse<T>`。然后就可以在剩余签名中使用 `T`：`x` 类型为 `T`，`Result` 的一半类型是 `T`。不过如果我们试着编译这个例子，会得到一个错误：

```{notrust,ignore}
error: binary operation `==` cannot be applied to type `T`
```

因为 `T` 可以是任意类型，有可能是没有实现 `==` 运算的类型，因而第一行可能出错。那怎么办呢？为了修复这个问题，需要学习 Rust 的另一个特性：特征。

# 特征（traits）

还记得 `impl` 关键字吗？它是用调用方法的语法调用函数。

```{rust}
struct Circle {
    x: f64,
    y: f64,
    radius: f64,
}

impl Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * (self.radius * self.radius)
    }
}
```

特征与之相似，除了我们是用方法的签名定义特征然后为这个结构实现特征，像下面这样：

```{rust}
struct Circle {
    x: f64,
    y: f64,
    radius: f64,
}

trait HasArea {
    fn area(&self) -> f64;
}

impl HasArea for Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * (self.radius * self.radius)
    }
}
```

如你所见，`trait` 块和 `impl` 块非常相似，但没有定义主体，仅仅是类型签名。当实现一个特征时，用 `impl Trait for Item` 语法，而不仅仅是 `impl Item`。

那么有什么很大的区别吗？记得我们的泛型 `inverse` 函数得到的错误吗？

```{notrust,ignore}
error: binary operation `==` cannot be applied to type `T`
```

我们可以用特征来限制泛型。考虑这个函数，它不能编译并且给出一个相似的错误：

```{rust,ignore}
fn print_area<T>(shape: T) {
    println!("This shape has an area of {}", shape.area());
}
```

Rust 报怨：

```{notrust,ignore}
error: type `T` does not implement any method in scope named `area`
```

因为 `T` 可以是任何类型，我们无法保证其实现了 `area` 方法。但可以给泛型 `T` 增加**特征限制**，确保其实现了 `area` 方法：

```{rust}
# trait HasArea {
#     fn area(&self) -> f64;
# }
fn print_area<T: HasArea>(shape: T) {
    println!("This shape has an area of {}", shape.area());
}
```

语法 `<T: HasArea>` 意思是任何实现了 `HasArea` 特征的类型。因为特征定义了函数的类型签名，可以保证任何实现了 `HasArea` 的类型都有 `.area()` 方法。

下面扩展的例子说明了它是如何工作的：

```{rust}
trait HasArea {
    fn area(&self) -> f64;
}

struct Circle {
    x: f64,
    y: f64,
    radius: f64,
}

impl HasArea for Circle {
    fn area(&self) -> f64 {
        std::f64::consts::PI * (self.radius * self.radius)
    }
}

struct Square {
    x: f64,
    y: f64,
    side: f64,
}

impl HasArea for Square {
    fn area(&self) -> f64 {
        self.side * self.side
    }
}

fn print_area<T: HasArea>(shape: T) {
    println!("This shape has an area of {}", shape.area());
}

fn main() {
    let c = Circle {
        x: 0.0f64,
        y: 0.0f64,
        radius: 1.0f64,
    };

    let s = Square {
        x: 0.0f64,
        y: 0.0f64,
        side: 1.0f64,
    };

    print_area(c);
    print_area(s);
}
```

这个程序输出：

```{notrust,ignore}
This shape has an area of 3.141593
This shape has an area of 1
```

如你所见，`print_area` 现在是泛型的，同时也保证传递的是正确的类型。如果传递的是不正确的类型：

```{rust,ignore}
print_area(5i);
```

会得到编译时错误：

```{notrust,ignore}
error: failed to find an implementation of trait main::HasArea for int
```

到目前为止我们只是给结构添加了特征实现，但你可以为任何类型实现特征。因而技术上，我们_可以_为 `int` 类型实现 `HasArea`：

```{rust}
trait HasArea {
    fn area(&self) -> f64;
}

impl HasArea for int {
    fn area(&self) -> f64 {
        println!("this is silly");

        *self as f64
    }
}

5i.area();
```

为基本的类型实现方法是糟糕的设计风格，尽管这是可能的。

这看起来像是狂野西部，但实现特征还有其它两个限制，防止其失去控制。首先特征必须在你希望使用特征的方法的域中使用，所以像下面示例这样的是不能干活的：

```{rust,ignore}
mod shapes {
    use std::f64::consts;

    trait HasArea {
        fn area(&self) -> f64;
    }

    struct Circle {
        x: f64,
        y: f64,
        radius: f64,
    }

    impl HasArea for Circle {
        fn area(&self) -> f64 {
            consts::PI * (self.radius * self.radius)
        }
    }
}

fn main() {
    let c = shapes::Circle {
        x: 0.0f64,
        y: 0.0f64,
        radius: 1.0f64,
    };

    println!("{}", c.area());
}
```

在它们自己的模块中移除结构和特征，我们就会得到一个错误：

```{notrust,ignore}
error: type `shapes::Circle` does not implement any method in scope named `area`
```

如果在 `main` 之上添加 `use` 行，并让合适的东西公开就可以了：

```{rust}
use shapes::HasArea;

mod shapes {
    use std::f64::consts;

    pub trait HasArea {
        fn area(&self) -> f64;
    }

    pub struct Circle {
        pub x: f64,
        pub y: f64,
        pub radius: f64,
    }

    impl HasArea for Circle {
        fn area(&self) -> f64 {
            consts::PI * (self.radius * self.radius)
        }
    }
}


fn main() {
    let c = shapes::Circle {
        x: 0.0f64,
        y: 0.0f64,
        radius: 1.0f64,
    };

    println!("{}", c.area());
}
```

这意味着即使有人干了坏事，例如为 `int` 添加了方法，也影响不到你，除非你用了那个特征。

对于实现特征还有一个限制，你用 `impl` 实现的特征和类型都必须在箱子里面。因而可以为 `int` 类型实现 `HasArea`，因为 `HasArea` 在我们的箱子里面。但如果我们试图为 `int` 实现 Rust 提供的特征 `Float`，则做不到，因为特征和类型都不在我们的箱子里。

最后一点关于特征的：有特征的泛型函数绑定使用**单态(monomorphization)**（"mono"：单一，"morph"：形态），所以它们是静态分派的。什么意思呢？让我们再次看看 `print_area`：

```{rust,ignore}
fn print_area<T: HasArea>(shape: T) {
    println!("This shape has an area of {}", shape.area());
}

fn main() {
    let c = Circle { ... };

    let s = Square { ... };

    print_area(c);
    print_area(s);
}
```

当我们在 `Circle` 和 `Square` 中使用这个特征时，Rust 对具体的类型生成两个不同的函数，并且在调用点用具体实现替换。换句话说，你会得到这样的：

```{rust,ignore}
fn __print_area_circle(shape: Circle) {
    println!("This shape has an area of {}", shape.area());
}

fn __print_area_square(shape: Square) {
    println!("This shape has an area of {}", shape.area());
}

fn main() {
    let c = Circle { ... };

    let s = Square { ... };

    __print_area_circle(c);
    __print_area_square(s);
}
```

这些名字不会真的变成这样，它只是用来说明。但可以看到确定调用哪个版本没有开销，因为这都是静态处理的。缺点是同样的函数拷贝了两份，所以二进制会变得更大一些。

# 任务（tasks）

并发和并行是许多软件开发者日益感兴趣的话题。现代计算机通常是多核的，即使嵌入式设备如手机都有不止一个处理器。Rust 语义上借出自己，非常漂亮地解决了程序员在并发中碰到的许多问题。许多其它语言中的运行时错误在 Rust 中是编译时错误。

Rust 的并发原语被称为**task**。任务是轻量级的，不会以不安全的方式共享内存，倾向于通过消息传递来通信。值得注意的是任务是作为库实现的，而不是语言的一部分。这意味着将来可以为 Rust 写其它并发库在特定场景起到帮助作用。下面是创建一个任务的示例：

```{rust}
spawn(proc() {
    println!("Hello from a task!");
});
```

`spawn` 函数接收一个过程（proc）作为参数，然后在新任务中运行该过程 。过程接管所有环境，所以 proc 内任何变量之后都不可用：

```{rust,ignore}
let mut x = vec![1i, 2i, 3i];

spawn(proc() {
    println!("The value of x[0] is: {}", x[0]);
});

println!("The value of x[0] is: {}", x[0]); // error: use of moved value: `x`
```

`x` 现在被过程接管了，因而不能再使用它了。许多其它语言允许这样做，尽管这样做不安全。Rust 的类型系统会捕捉到这种错误。

如果任务只是捕捉这些值，那么就不是特别有用。幸运的是，任务可以通过**信道（channel）**彼此通信。信道像下面这样工作：

```{rust}
let (tx, rx) = channel();

spawn(proc() {
    tx.send("Hello from a task!".to_string());
});

let message = rx.recv();
println!("{}", message);
```

`channel()` 函数返回两个端点： `Receiver<T>` 和 `Sender<T>`。可以在 `Sender<T>` 端使用 `.send()` 方法，`Receiver<T>` 端用 `recv()` 方法接收消息。这个方法在接收到消息之前是阻塞的。还有一个相似的方法 `.try_recv()` 返回 `Option<T>` 但不会阻塞。

如果你也想给任务发送消息，那么创建两个 channel。

```{rust}
let (tx1, rx1) = channel();
let (tx2, rx2) = channel();

spawn(proc() {
    tx1.send("Hello from a task!".to_string());
    let message = rx2.recv();
    println!("{}", message);
});

let message = rx1.recv();
println!("{}", message);

tx2.send("Goodbye from main!".to_string());
```

这个过程有一个发送端和一个接收端，主任务在两端也各有一个。现在它们就可以以它们希望的方式来回地交流了。

同时注意到因为 `Sender` 和 `Receiver` 是泛型的，尽管你可以在信道间传送任何类型的信息，但端口是强类型的。如果你试着传递一个字符串然后一个整型，Rust 就会报错。

## 未来（Futures）

有了这些基本的原语，就可以开发许多不同的并发模式了。Rust 标准库中包含了其中的一些类型。例如，如果你希望在后台计算一些值，`Future` 是一个有用的东西：

```{rust}
use std::sync::Future;

let mut delayed_value = Future::spawn(proc() {
    // just return anything for examples' sake

    12345i
});
println!("value = {}", delayed_value.get());
```

调用 `Future::spawn` 如 `spawn()` 一样工作：接收一个过程。这种情况下你不用将信道混合：只用让过程返回值就行。

`Future::spawn` 返回一个值，我们可以用 `let` 绑定。这个值需要是可变的，因为一旦计算出了值，就会保存一份拷贝，如果是不可变的话，就不会更新它本身。

过程会一直在后台处理，当我们需要最终的值时，在其上调用 `get()` 就行。这个会阻塞直到结果完成，但如果后台计算完成，我们就可以立即得到其值。

## 成功和失败（Success and failure）

任务并不总是成功的，有时也会失败。预期会失败的任务可以调用 `fail!` 宏，传递一个消息：

```{rust}
spawn(proc() {
    fail!("Nope.");
});
```

如果一个任务失败了，就不可能恢复。然而可以通知其它任务：我失败了。我们可以用 `task::try` 来实现：

```{rust}
use std::task;
use std::rand;

let result = task::try(proc() {
    if rand::random() {
        println!("OK");
    } else {
        fail!("oops!");
    }
});
```

这个任务会随机失败或成功。`task::try` 返回一个 `Result` 类型，因而我们可以像其它会失败的计算一样处理其响应。

# 宏（macros）

Rust 最先进的特性之一是**宏**系统。函数允许你在值和操作上提供抽象，而宏则允许你在语法上提供抽象。你希望 Rust 有能力做它现在做不到的东西吗？你可以写宏来扩展 Rust 的能力。

你已经广泛地使用过一种宏了： `println!`。当我们调用 Rust 的宏时，我们需要使用感叹号（`!`）。有两个理由需要这样做：第一，它让你很清楚地知道自己在使用宏；第二，宏允许灵活的语法，因此 Rust 需要知道宏在哪开始和结束。`!(...)` 可以对此起到帮助作用。

让我们再谈一些关于 `println!`，我们可以将 `println!` 作为函数实现，但更糟糕。为啥涅？宏允许你写代码生成更多的代码。所以当我们像这样调用 `println!` 时：

```{rust}
let x = 5i;
println!("x is: {}", x);
```

`println!` 干了下面几件事：

1. 解析字符串来查找任何 `{}`
2. 检查 `{}` 数与其它参数匹配
3. 产生一大堆 Rust 代码，记住这点

它的意思是在编译时进行类型检查，因为 Rust 会考虑所有类型以生成代码。如果 `println!` 是一个函数的话，也可以进行类型检查，但是在运行时而不是编译时。

我们可以通过使用 `rustc` 的一个特殊标志来验证这点。下面是 `print.rs` 文件中的程序：

```{rust}
fn main() {
    let x = "Hello";
    println!("x is: {:s}", x);
}
```

可以像这样将其宏进行展开： `rustc print.rs --pretty=expanded` ，这会给出下面庞大的结果：

```{rust,ignore}
#![feature(phase)]
#![no_std]
#![feature(globs)]
#[phase(plugin, link)]
extern crate std = "std";
extern crate rt = "native";
use std::prelude::*;
fn main() {
    let x = "Hello";
    match (&x,) {
        (__arg0,) => {
            #[inline]
            #[allow(dead_code)]
            static __STATIC_FMTSTR: [::std::fmt::rt::Piece<'static>, ..2u] =
                [::std::fmt::rt::String("x is: "),
                 ::std::fmt::rt::Argument(::std::fmt::rt::Argument{position:
                                                                       ::std::fmt::rt::ArgumentNext,
                                                                   format:
                                                                       ::std::fmt::rt::FormatSpec{fill:
                                                                                                      ' ',
                                                                                                  align:
                                                                                                      ::std::fmt::rt::AlignUnknown,
                                                                                                  flags:
                                                                                                      0u,
                                                                                                  precision:
                                                                                                      ::std::fmt::rt::CountImplied,
                                                                                                  width:
                                                                                                      ::std::fmt::rt::CountImplied,},})];
            let __args_vec =
                &[::std::fmt::argument(::std::fmt::secret_string, __arg0)];
            let __args =
                unsafe {
                    ::std::fmt::Arguments::new(__STATIC_FMTSTR, __args_vec)
                };
            ::std::io::stdio::println_args(&__args)
        }
    };
}
```

下面是裁剪后易读的版本：

```{rust,ignore}
fn main() {
    let x = 5i;
    match (&x,) {
        (__arg0,) => {
            static __STATIC_FMTSTR:  =
                [String("x is: "),
                 Argument(Argument {
                    position: ArgumentNext,
                    format: FormatSpec {
                        fill: ' ',
                        align: AlignUnknown,
                        flags: 0u,
                        precision: CountImplied,
                        width: CountImplied,
                    },
                },
               ];
            let __args_vec = &[argument(secret_string, __arg0)];
            let __args = unsafe { Arguments::new(__STATIC_FMTSTR, __args_vec) };

            println_args(&__args)
        }
    };
}
```

嚄！不那么恐怖了。可以看到我们仍然有 `let x = 5i`，但是之后事情变得有点让人发毛。接着又设置了三个绑定：静态格式的字符串，参数向量和参数。然后用产生的参数调用 `println_args` 函数。

这就是 Rust 实际编译的（完整版本的）代码。你可以在这儿看到所有额外信息。我们在编译时得到了所有的类型安全和 Rust 提供的选项，而不用将所有这些都输入进去，这就是为什么宏非常强大。没有宏，你需要手动输入所有这些以获得类型检查的 `println`。

更多关于宏，请参考[宏指南](guide-macros.html)。宏是非常高级的，仍然是实验性的特性，但不需要深入的理解就可以调用，因为它们和函数长得很像。如果你想写自己的宏，这个指南可以帮到你。

# 不安全（unsafe）

最后 Rust 还有一个概念你需要知道：`unsafe`。有两种情况下 Rust 的安全条款不适用。第一个是当和 C 程序连接时，第二种是当构建某些抽象时。

Rust 支持 FFI（你可以阅读 [FFI 指南](guide-ffi.html)），但无法保证 C 程序是安全的。因此 Rust 用 `unsafe` 关键词标记这样的函数，表明这个函数表现可能不正常。

其次，如果你想创建一些共享内存的数据结构，Rust 不允许这样干，因为内存只能有一个拥有者。不过，如果你打算使访问共享内存是安全的，例如有互斥时，你知道这是安全的，但 Rust 不知道。写一个 `unsafe` 块让编译器信任你。这种情况，互斥内部实现被认为是不安全的，而我们所呈现的外部接口则是安全的。这允许在正常 Rust 程序中高效地使用，而同时能够实现让编译器不用复查的功能。

开这样的小窗口难道不会破坏整个系统的安全性吗？是的，如果 Rust 程序出现段错误，_肯定_是由于某个地方的不安全代码。通过准确地标注（不安全代码的）位置，你就可以显著减小搜索区域。

这里我们甚至没有讨论任何示例，因为我想强调除非你知道你在干嘛，否则就不应该写不安全的代码。绝大多数 Rust 开发者只会在 FFI 时与其打交道，高级库的作者可能会用其构建某些抽象。

# 总结

我们在这里涵盖了很多基础，当你掌握了这份指南中的所有知识，你就牢固地掌握了 Rust 开发的基础知识。还有很多东西，我们只是触及到了一些皮毛。有许多话题可以深入挖掘，为此我们制作了专门的文档。想学习更多，去探究[完整文档索引](http://doc.rust-lang.org/index.html)吧。

Happy hacking!

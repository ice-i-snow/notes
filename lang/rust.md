# Rust 学习笔记 #
基于windows 7 系统。

## Hello World ##

- 官方网站[下载](http://www.rust-lang.org/install.html)，并进行安装。我的安装目录：“D:\Application\Rust stable 1.0”
- 在安装目录下创建projects文件夹
- 创建main.rs文件，并编写代码
`fn main(){
    println!("Hello, World!");
}`
- 打开“Rust1.0 Shell”，然后进入到刚才创建的“projects”目录
- 编译main.rs文件。输入命令：`d:\Application\Rust stable 1.0\projects>rustc main.rs`，会编译生成一个“main.exe”可执行文件
- 执行main.exe文件。输入命令： `d:\Application\Rust stable 1.0\projects>main.exe`，会在控制台显示“Hello, World!”

## Hello Cargo ##

Cargo是管理Rust工程的一个工具，负责3件事：构建代码，下载代码需要的依赖，构建依赖。使用Cargo创建一个“Hello World”。

- 进入“projects”目录，并创建src文件夹。命令如下：`d:\Application\Rust stable 1.0\projects>mkdir src`
- 拷贝main.rs文件到 src 目录下，命令：`d:\Application\Rust stable 1.0\projects>copy main.rs src/main.rs`
- 在 projects 目录下创建“Cargo.toml”文件，并写入以下内容：

>   
```
    [package]
 	name = "hello_world"
 	version = "0.0.1"
 	authors = "[ice-i-snow <lib.chrome@gmail.com>]"
```


[TOML](https://github.com/toml-lang/toml)格式规范。

- 进行编译，命令如下：`d:\Application\Rust stable 1.0\projects>cargo build`。编译完成后，会创建 target 文件夹和 Cargo.lock 文件
- 进入到 target\debug 文件夹下，执行 main.exe，命令如下：
`d:\Application\Rust stable 1.0\projects>cd target\debug`
`d:\Application\Rust stable 1.0\projects>main.exe`
此时在控制台输出“Hello，World！”

其实我们可以合并最后2步，使用`d:\Application\Rust stable 1.0\projects>cargo run`命令，直接编译代码并执行，同样在控制台输出：“Hello，World!”

我们可以使用 `Cargo new` 命令创建一个新的项目。命令如下:`d:\Application\Rust stable 1.0\projects>cargo new hello_world --bin`


## Rust语法 ##

### 1.变量绑定（Variable Binding） ###

**普通的绑定方式：** `let (x, y) = (1, 2)` 表示将 1 赋值给 x ，将 2 赋值给 y 。

*let* 表达式是一种‘模式’，具有很强大的功能，在后面的‘Pattern’章节会做单独介绍。

**指定数据类型的方式：** `let x: i32 = 5` 表示 x 的类型为 i32 ，数值为 5 。

在 Rust 语言中，使用 i 表示有符号的整数，使用 u 表示无符号的整数。

**默认情况下，变量是不可变的**,下面的代码是无法编译通过的：
``` rust
    let x = 5;
    x = 10;
```

这样会产生一个错误：
>     error: re-assignment of immutable variable `x`
>         x = 10;
>         ^~~~~~~

如果想定义一个可变的变量，应该使用 *mut* :
``` rust
    let mut x = 5;
    x = 10;
```

Rust 这样做主要是为了 **安全**。

Rust 要求**使用变量之前，必须要初始化变量**。

下面代码在编译时，会产生警告：
```rust
     fn main() {
         let x: i32;
         println!("Hello, World!");
     }
```
>     警告信息如下：
>     Compiling hello_world v0.0.1 (file:///home/you/projects/hello_world)
>     src/main.rs:2:9: 2:10 warning: unused variable: `x`, #[warn(unused_variable)]
>        on by default
>     src/main.rs:2 let x: i32;
>                       ^

下面代码在编译时，会产生错误：
```rust
     fn main() {
         let x: i32;
         println!("The value of x is: {}", x);
     }
```
>     错误信息如下：
>     $ cargo build
>     Compiling hello_world v0.0.1 (file:///home/you/projects/hello_world)
>     src/main.rs:4:39: 4:40 error: use of possibly uninitialized variable: `x`
>     src/main.rs:4     println!("The value of x is: {}", x);
>                                                         ^
>     note: in expansion of format_args!
>     <std macros>:2:23: 2:77 note: expansion site
>     <std macros>:1:1: 3:2 note: in expansion of println!
>     src/main.rs:4:5: 4:42 note: expansion site
>     error: aborting due to previous error
>     Could not compile `hello_world`.

### 2.函数（Functions） ###

无参数函数
```rust
    fn main() {
 
    }
```

有参数的函数
```rust
    fn print_number(x: i32) {
        println!("x is: {}", x); 
    }
```

有多个参数的函数，参数之间使用逗号隔开
```rust
    fn main(){
		print_sum(5, 6);
	}
    fn print_sum(x: i32, y: i32) {
        println!("sum is: {}", x + y);
    }
```

**参数必须要声明类型，否则会报错**。下面的代码会产生错误：
```rust
    fn print_sum(x, y) {
        println!("sum is: {}", x + y);
    }
```

错误信息如下：
>     expected one of `!`, `:`, or `@`, found `)`
>     fn print_number(x, y) {

有返回值的函数
```rust
    fn add_one(x: i32) -> i32 {
		x + 1
	}
```

**只能返回一个值**

**函数的最后一行表示返回值，并且不能添加分号，否则会报错。**下面的代码会产生错误：
```rust
    fn add_one(x: i32) -> i32 {
		x + 1;
	}
```

错误信息如下：
>     error: not all control paths return a value
>     fn add_one(x: i32) -i32 {
>          x + 1;
>     }
> 
>     help: consider removing this semicolon:
>          x + 1;
>               ^

Rust中只存在两种语句：声明语句和表达式语句。其它的都是表达式。

下面代码是错误的：
```rust
	let x = (let y = 5);	// expected identifier, found keyword `let`
```

下面的代码是正确的，但是返回值并非我们想要的：
```rust
	let mut y = 5;
	let x = (y = 6);	//x has the value `()`, not `6`
```

**将赋值语句做为值赋给其它变量，会返回空元组 `()`。**

在函数内部，使用 *return* 后，后面的语句不会被执行
```rust
	fn add_one(x: i32) -> i32 {
		return x;

		//此代码永远不会执行
		x + 1
	}
```

使用 *return* 返回值（这是一种糟糕的代码风格）
```rust
	fn add_one(x: i32) -> i32 {
		return x + 1;
	}
```

应该使用下面的代码：
```rust
	fn add_one(x: i32) -> i32 {
		x + 1
	}
```

**分散函数（Diverging Function)：中断当前的执行线程，并返回 `!` 类型的值，我们叫它‘分散’。**
```rust
	fn main(){
		let x: i32 = diverges();

		//线程中断，此代码永远不会执行
		println!("Hello, x is {}", x);
	}
	fn diverges(){
		pantic!("This function never returns!");
	}
```

编译时，信息如下：
>     src\main.rs:3:6: 3:7 warning: unused variable: `x`, #[warn(unused_variables)] on
>     by default
>     src\main.rs:3   let x: i32 = diverges();
>                         ^

执行时，信息如下：
>     thread '<main>' panicked at 'This function never returns!', src\main.rs:8
>     An unknown error occurred
> 
>     To learn more, run the command again with --verbose.

### 3.原生类型（Primitive Types） ###

**布尔类型（Boolean）**
```rust
	let x = true;
	let y: bool = false;
```

**字符类型（char）**
```rust
	let x = 'x';
	let y: char = '*';
```

**数值类型（Numeric）**：有符号和无符号，固定长度和可变长度，浮点型和整型
```rust
	let x = 32;		// x 的类型默认为 i32
	let y = 1.0;	// y 的类型默认为 f64
```

**数组（Array）**：[T，N]
```rust
	let a = [1, 2, 3];	// a: [i32; 3]
	let mut m = [1, 2, 3];	// m: [i32; 3]
```

可以使用下面的方式，快速初始化具有相同数值的数组：
```rust
	// 每个元素的值都是 `0`
	let a = [0; 20];	// a: [i32; 20]
```

使用 `a.len()` 返回数组的长度，例如：
```rust
	let a = [1, 2, 3];
	println!("a has {} elements.", a.len());
```

通过下标获取数组中的某个元素值（下标从 0 开始）：
```rust
	let names = ["Graydon", "Brian", "Niko"]; // names: [&str; 3]
	println!("The second name is: {}", names[1]);
```

**切片（Slices）**
```rust
	let a = [0, 1, 2, 3, 4];
	let middle = &a[1..4];	// 获取数组元素：1, 2, 3 作为 a 的切片
	let complete = &a[..];	// 包含 a 的所有元素的切片
```

**字符串（str）**

`&str` 表示静态的字符串，固定长度，无法修改。

`String` 保存在堆中，是可以变化的。通常使用 `&str` 类型的 `to_string()` 方法创建。使用 `&` 符号可以转换成 `&str` 类型。
```rust
	// 代码片段一
	let string = "Hello World";	// string: &'static str
	
	// 代码片段二
	let mut s = "Hello".to_string();
	println!("{}", s);	// 打印结果： Hello
	s.push_str(", World");
	println!("{}", s);	// 打印结果： Hello,String.

	// 代码片段三
	fn takes_slice(slice: &str){
		println!("Got {}", slice);
	}
	fn main(){
		let s = "Hello".to_string();
		takes_slice(&s);
	}
```

**字符串类型不提供索引**，下面的代码是错误的：
```rust
	let s = "我的学习笔记";
	println!("The first letter of s is {}", s[0]);	// 这会产生错误
```

错误信息：
>     src\main.rs:26:43: 26:49 error: the trait `core::ops::Index<_>` is not implement
>     ed for the type `str` [E0277]
>     src\main.rs:26  println!("The second letter of s is {}", str[1]);       // Error
>                                                              ^~~~~~

通过以下方式，访问字符串中的某个字符：
```rust
	let s = "我的学习笔记";
	let f = s.chars().nth(0);
	println!("The first letter of s is {}", f);
```

此代码在编译过程中出现错误（按照文档编写，竟然报错，这是为什么啊？），信息如下：
>     src\main.rs:32:42: 32:60 error: the trait `core::fmt::Display` is not implemente
>     d for the type `core::option::Option<char>` [E0277]
>     src\main.rs:32  println!("The first letter of s is {}", f);
>                                                             ^

**元组（Tuples)**：定长、有序、异构的列表。
```rust
	let t = (1, "hello");
	let t: (i32, &str) = (1, "hello");
```

如果两个元组的数据类型相同，数量一致，那么可以将一个元组赋值给另一个元组，例如：
```rust
	let mut x = (1, 2);
	let y = (3, 4);
	println!("x = {:?}; y = {:?}", x, y);	// 打印结果：x = (1, 2); y = (3,4)

	x = y;
	println!("x is {:?}", x);	// 打印结果： x is (3, 4)
```

使用元组为多个变量赋值：
```rust
	let (l, m, n) = (9, 8, 7);
	println!("l={}, m = {}, n = {}", l, m, n);	// 打印结果：l = 9, m = 8, n = 7
```

定义只有一个元素的元组：
```rust
	let t = (0,);
	println!("t = {:?}", t);	// 打印结果： t = (0,);
```

访问元组的任意元素：
```rust
	let tuple = ("I", "am", "ice-i-snow");
	let a = tuple.0;
	let b = tuple.1;
	let c = tuple.2;
	println!("a = {}, b = {}, c = {}", a, b, c);	// 打印结果：a = I, b = am, c = ice-i-snow
```

**函数（Functions）**
```rust
	fn foo(x: i32) -> i32{
		x
	}
	let f: fn(i32) -> i32 = foo;
	println!("f(12) is {}", f(12));	// 打印结果： f(12) is 12
```

### 4.注释（Comments） ###

Rust有两种注释：行注释和文档注释

行注释写法，在 ‘//’ 后面写注释内容：

	let x = 5;	// 初始化变量 x ,赋值为5

文档注释写法，在 ‘///’ 后面写注释内容：

	/// 为传入的变量加1
	///
	/// #examples
	///
	/// ```
	///	let five = 5
	/// assert_eq!(6, add_one(5));
	///
	/// ```
	fn add_one(x : i32) -> i32 {
		x + 1
	}

### 5.if 表达式（if） ###

if 的用法：
```rust
	let x = 5;
	if x == 5 {
		println!(" x is five.");
	}
```

可以添加 else：
```rust
	let x = 5;
	if x == 5 {
		println!(" x is five.");
	} else｛
		println!(" x is not five :(");
	｝
```

如果有多个分支，可以添加 else if：
```rust
	let x = 5;
	if x == 5 {
		println!(" x is five.");
	} else if x == 6 {
		println!(" x is six.");
	} else {
		println!(" x is not five or six :(");
	}
```

上面都是 if 表达式的标准用法，我们还可以这样用：
```rust
	// 通过 if 表达式对变量赋值
	let x = 5;
	let y = if x == 5 {
		10
	} else {
		15
	};	// y : i32
```

其实，我们可以这样写：
```rust	
	let x = 5;
	let y = if x == 5 { 10 } else { 15 };	// y : i32
```

### 6.for 循环（for Loops） ###

for 的用法：
```rust
	// 采取前闭后开的原则，只打印到9，不会打印10
	for x in 0..10 {
		println!("{}", x);	// x : i32
	}
```

抽象结构：
```rust
	for var in expression {
		code
	}
```

Rust 中的 **for 循环不允许手动地操作循环的每一个元素**，它认为这样做是复杂的、容易出错的。

### 7.while 循环（while Loops） ###

while 的用法：
```rust
	let mut x = 5;
	let mut done = false;

	while !done {
		x += x - 3;

		println!("{}", x);

		if x % 5 == 0 {
			done = true;
		}
	}
```

while 可以实现无限循环：
```rust
	while true {}
```

Rust中，推荐使用 *loop* 关键字实现无限循环：
```rust	
	loop {}
```

*break* 表示结束并跳出当前循环：
```rust
	let mut x = 5;
	loop {
		x += x - 3;

		println!("{}", x);

		if x % 5 == 0 { break; }		
	}
```

*continue* 表示直接进入下一个循环：
```rust
	for x in 0..10 {
		if x % 2 == 0 { continue; }

		println!("{}", x);
	}
```

### 8.所有权（Ownership） ###

需要开发人员重点掌握和理解的内容。

	1.ownership(所有权)
	2.borrowing(借用)
	3.lifetimes()

**元(Meta)**

Rust 注重安全和速度，通过‘零消耗抽象概念（zero-costabstraction）’实现。这些都是在编译期进行，不会消耗任何运行时间。

尽管如此，Rust 也有一些必要的消耗：学习曲线。初学者认为他们的程序是有效的，但是 Rust 编译器却拒绝编译。程序员认为 Rust 应该运行的方式，实际上，Rust 并没有已有的规则与之匹配。无论怎样，随着使用 Rust 经验的提升，情况会慢慢好转。

**所有权（Ownership）**

在 Rust 中，变量绑定有一个属性：被绑定变量的所有权。这意味着，当绑定离开作用域后，被绑定的资源会被释放。

```rust
	fn foo(){
		var v = vec![1, 2, 3];
	}
```

当 `v` 在作用域里面，会创建一个新的 `Vec<T>` ,向量会在堆中为3个元素分配一块空间。当 `v` 在 `foo()` 函数末尾离开作用域时，Rust 会清除所有和向量相关的内容，甚至是在堆上分配的内存。

**移动语义（Move Semantics）**

Rust 确保每一个资源都是精确的单一绑定。我们可以将一个已有的向量绑定到另外一个变量：

```rust
	let v = vec![1, 2, 3];
	let v2 = v;
```

但是，在这之后我们再访问 `v` 时，会报错：

```rust
	let v = vec![1, 2, 3];
	let v2 = v;
	println!("v[0] is {}", v[0]);
```

错误信息如下：

>     error: use of moved value: `v`
>     println!("v[0] is: {}", v[0]);
>                             ^

类似地事情也会发生在：我们定义了一个拥有所有权的函数，并且将变量传给了参数后访问这个变量。

```rust
	fn take(v: Vec<i32>) {
		// 这里的代码并不重要
	}

	let v = vec![1, 2, 3];

	take(v);

	println!("v[0] is {}", v[0]);
```

这样写，也会报和上面相同的错误
> error: use of moved value: `v`

**细节（The Details）**

移动之后，无法使用绑定的理论虽然微妙，但是很重要。如下代码所示：

	let v = vec![1, 2, 3];
	let v2 = v;

第一行代码为向量对象、变量 `v`、及数据分配了内存。向量对象存储在栈中，在堆中会有一个指针指向内容（`[1, 2, 3]`）。当我们将 `v` 赋值给 `v2`时，会给`v2`创建一个指针的拷贝。这意味着，在堆中会有两个向量内容的指针。这违反了 Rust 对于数据竞争的安全保障。所以，Rust 禁止移动变量 `v` 后继续使用。

还有一点值得注意，最优方式应是在栈中删除目前字节的拷贝并信任环境。所以，它并非像当初那样低效。

**拷贝类型（Copy types）**

我们已经确立当所有权被转移到另一个绑定时，我们无法使用原来的绑定。无论如何，还有一种特性来改变这个行为，我们称之为‘拷贝’。

我们原先从未讨论过特性，但是目前，你可以把它们想象成特殊类型的注释来增加额外的行为。例如：

```rust
	let v = 1;
	let v2 = v;
	println!("v is {}", v);
```

`v` 是 `i32` 类型的，此类型实现了‘拷贝’特性。这意味着，就像我们移动时，将`v`赋值给`v2`，创建了一个数据的拷贝；但是，和移动时不同的是，我们依然可以使用 `v`。这是因为 `i32` 没有指针指向其他地方的数据，是完全的拷贝。

我们会在 ‘特性’ 章节讨论如何创建你自己的类型拷贝。

**更多的所有权（More than ownership）**

如果我们在每一个函数中返回所有权，可以这样写：

```rust
	fn foo(v: Vec<i32>) -> Vec<i32> {
		// 使用 v 做一些工作

		// 返回所有权
		v		
	}
```

这看起来很乏味。还有更糟糕的事情是我们想要取出更多变量的所有权，例如：

```rust
	fn foo(v: Vec<i32>, v2: Vec<i32>) -> (Vec<i32>, Vec<i32>, i32) {
		// 使用 v1和v2 做一些工作

		// 返回所有权和函数的结果
		(v1, v2, 42)
	}

	let v1 = vec![1, 2, 3];
	let v2 = vec![1, 2, 3];

	let (v1, v2, answer) = foo(v1, v2);
```

啊！返回的类型、返回行、和调用函数变得更复杂。

幸运的是，Rust 提供了一个特性 ‘借用（borrowing）’，会帮助我们解决这个问题。这是下一章节的主题。

### 9.引用和借用（Reference and Borrowing） ###

需要开发人员重点掌握和理解的内容。

	1.ownership(所有权)
	2.borrowing(借用)
	3.lifetimes()

**元(Meta)**

Rust 注重安全和速度，通过‘零消耗抽象概念（zero-cost abstraction）’实现。在这个指南中，我们讨论的所有的分析都是在编译期进行，这些特性不会消耗任何运行时间。

尽管如此，Rust 也有一些必要的消耗：学习曲线。初学者认为他们的程序是有效的，但是 Rust 编译器却拒绝编译。程序员认为 Rust 应该运行的方式，实际上，Rust 并没有已有的规则与之匹配。无论怎样，随着使用 Rust 经验的提升，情况会慢慢好转。

**借用（Borrowing）**

在所有权章节的最后，我们编写了一个很糟糕的函数：
```rust
	fn foo(v: Vec<i32>, v2: Vec<i32>) -> (v1: Vec<i32>, v2: Vec<i32>, i32){
		// 使用 v1和v2 做一些工作

		// 返回所有权和函数的结果
		(v1, v2, 42)
	}
	
	let v1 = vec![1, 2, 3];
	let v2 = vec![1, 2, 3];

	let (v1, v2, answer) = foo(v1, v2);
```
这不符合 Rust 的语言习惯，不过，这也没有用到借用。首先看这里的代码：
```rust
	fn foo(v: &Vec<i32>, v2: &Vec<i32>) -> i32 {
		// 使用 v1和v2 做一些工作

		// 返回函数的结果
		42
	}

	let v1 = vec![1, 2, 3];
	let v2 = vec![1, 2, 3];

	let answer = foo(&v1, &v2);

	// 我们可以在这里调用 v1 和 v2
```

我们使用引用类型： `&Vec<i32>` 代替原先的类型 `Vec<T>` 作为参数，并且传入 `&v1` 和 `&v2` 代替原来的 `v1` 和 `v2`。我们把 `&T` 类型称为‘引用’，它不会拥有资源，而是借用所有权。当离开作用域时，借用绑定不会取消分配的资源。这意味着，当我们调用 `foo()`函数后，依然可以再次使用原有的绑定。

**引用类似于绑定，是不可变的**。这意味着在 `foo()` 函数里面，我们不能改变向量：
```rust
	fn foo(v: &Vec<i32>) {
		v.push(5);
	}

	let v = vec![];

	foo(&v);
```

这样写，会报错：

>     error: cannot borrow immutable borrowed content `*v` as mutable
>     v.push(5);
>     ^

我们不允许增加一个值改变向量。

**&mut引用（&mut refernces）**

这里介绍第二种引用类型： `&mut T`。**‘可变引用’允许你修改借用的资源**。例如：
```rust
	let mut x = 5;
	{
		let y = &mut x;
		*y += 1;
	}
	println!("x is {}", x);
```

打印结果为 `6`。我们创建了 `y` ，一个指向 `x` 的可变引用，然后在 `y` 上加1。你注意到 `x` 已经被标注为 `mut`，如果不这样做，就不能为一个不可变的值创建一个可变引用。

另外，`&mut` 引用类似引用。两者之间有很大的区别以及如何交互。在上面的例子中，你会发现一些可疑的情况，因为我们需要额外的作用域以及 `{` 和 `}` 。如果我们删除它们，就会报错：

>     error: cannot borrow `x` as immutable because it is also borrowed as mutable
>     println!("{}", x);
>                    ^
>     note: previous borrow of `x` occurs here; the mutable borrow prevents
>     subsequent moves, borrows, or modification of `x` until the borrow ends
>         let y = &mut x;
>                      ^
>     note: previous borrow ends here
>     fn main() {
> 
>     }
>     ^

事实证明，这是有规则的。

**规则（The Rules）**

Rust 的借用规则。

首先，借用必须持续在一个比所有权更小的作用域内。这两种借用的方式你可以有一个或多个，但是，在同一时间不能同时出现：

- 0 到 N 个对资源的引用（&T）
- 只有一个可变的引用（&mut）

你可能注意到，这和定义数据竞争是非常相似的，尽管不是完全一样：

>     数据竞争：当一个或多个指针在同一时间使用同一个内存地址时，只能有一个进行写操作，
>     这些操作不允许同步。

使用引用，因为不能进行写操作，所以你可能会有很多想法。如果你想进行写操作，对于同一个内存你需要有两个或多个指针，并且在同一时间只能有一个 `&mut`。这就是 Rust 在编译期如何阻止数据竞争的：如果我们打破规则，就会得到错误。

通过这个思路，让我们再次细想一下例子。

**作用域思想（Thinking in scopes）**

这是一段代码：
```rust
	let mut x = 5;
	let y = &mut x;

	*y += 1;

	pringln!("x is {}", x);
```

这段代码会呈献给我们这个错误：

>     error: cannot borrow `x` as immutable because it is also borrowed as mutable
>     println!("{}", x);
>                    ^

这是因为我们违反了规则：我们已经定义了 `&mutT` 指向 `x` ，不能再创建任何的 `&T` 。下面的注释示意我们如何考虑这个问题：

>     note: previous borrow ends here
>     fn main() {
> 
>     }
>     ^

换句话说，可变借用一直持续到例子的结束。我们期望的是，在尝试调用 `println！` 之前，可变借用会结束，并且变成不可变调用。在 Rust 中，借用依赖于自身有效的作用域。我们的作用域是这样的：
```rust
	let mut x = 5;

	let y = &mut x;    	// -+ &mut borrow of x starts here
                   		//  |
	*y += 1;           	//  |
                   		//  |
	println!("{}", x); 	// -+ - try to borrow x here
                   		// -+ &mut borrow of x ends here 
```

这个作用域发生了冲突：当 `y` 在作用域中，我们不能再创建 `&x` 。所以，我们需要增加大括号：
```rust
	let mut x = t;
	{
		let y = &mut x;		// -+ &mut borrow starts here
		*y += 1;			//  |
	}						// -+ ... and ends here
	println!("x is {}", x);	// <- try to borrow x here
```

这是没有问题的。当我们创建一个不可变借用时，可变借用已经离开了它的作用域。作用域的关键是看一个借用能够持续多长时间。

**作用域限制议题（Issues borrowing prevents）**

为什么会有这些约束规则？其实，正如我们所见，这些规则是用来约束数据竞争的。什么样的问题导致数据竞争？这有一些例子。

**迭代器无效（iterator invalidation）**

这是一个“迭代器无效”的例子，当你试图修改正在迭代的集合时，Rust 的借用检查器会阻止这种情况发生：
```rust
	let mut v = vec![1, 2, 3];
	
	for i in &v {
		println!("{}", i); 
	}
```

这回打印出 1 至 3。当我们循环向量时，只能获取元素的引用。`v` 借用它自身为不可变的，这意味着在整个循环过程中，我们无法修改 `v`。
```rust
	let mut v = vec![1, 2, 3];
	
	for i in &v {
		println!("{}", i); 
		v.push(4);
	}
```

这样写，会报错：

>     error: cannot borrow `v` as mutable because it is also borrowed as immutable
>     v.push(4);
>     ^
>     note: previous borrow of `v` occurs here; the immutable borrow prevents
>     subsequent moves or mutable borrows of `v` until the borrow ends
>     for i in &v {
>               ^
>     note: previous borrow ends here
>     for i in &v {
>         println!(“{}”, i);
>         v.push(4);
>     }
>     ^

我们不能修改 `v`，因为它被循环借用了。

**释放内存后使用（use after free)**

引用必须和它引用的资源存活相同的时间。Rust 会检查引用的作用域，确定是否正确。

如果 Rust 没有检查到这个属性，我们可能会不小心使用一个无效的引用。例如：
```rust
	let y: &i32;
	{
		let x = 5;
		y = &x;
	}
	println!("{}", y);
```

我们会得到下面的错误：

>     error: `x` does not live long enough
>         y = &x;
>              ^
>     note: reference must be valid for the block suffix following statement 0 at
>     2:16...
>     let y: &i32;
>     { 
>         let x = 5;
>         y = &x;
>     }
> 
>     note: ...but borrowed value is only valid for the block suffix following
>     statement 0 at 4:18
>         let x = 5;
>         y = &x;
>     }

换句话说，只有 `x` 存在 `y` 才会有有效的作用域。当 `x` 消失后，对它的引用也会变成无效的。所以，错误信息说借用“存活的时间不够长”，因为它不是有效的正确时间。

相同的问题也发生：引用在被引用的变量之前已经声明
```rust
	let y : i32;
	let x = 5;
	y = &x;

	println!("{}", y);
```

我们会得到下面的错误：

>     error: `x` does not live long enough
>     y = &x;
>          ^
>     note: reference must be valid for the block suffix following statement 0 at
>     2:16...
>         let y: &i32;
>         let x = 5;
>         y = &x;
>     
>         println!("{}", y);
>     }
> 
>     note: ...but borrowed value is only valid for the block suffix following
>     statement 1 at 3:14
>         let x = 5;
>         y = &x;
>     
>        println!("{}", y);
>     }

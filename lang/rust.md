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

### 10.借用期（Lifetimes） ###

需要开发人员重点掌握和理解的内容。

	1.ownership(所有权)
	2.borrowing(借用)
	3.lifetimes(借用期)

**元(Meta)**

Rust 注重安全和速度，通过‘零消耗抽象概念（zero-cost abstraction）’实现。在这个指南中，我们讨论的所有的分析都是在编译期进行，这些特性不会消耗任何运行时间。

尽管如此，Rust 也有一些必要的消耗：学习曲线。初学者认为他们的程序是有效的，但是 Rust 编译器却拒绝编译。程序员认为 Rust 应该运行的方式，实际上，Rust 并没有已有的规则与之匹配。无论怎样，随着使用 Rust 经验的提升，情况会慢慢好转。

**借用期（Lifetimes）**

将引用借给其它的资源是复杂的。例如，想象这一系列操作：

- 获取某种资源的句柄
- 将引用借给资源
- 当你一直拥有引用时，你决定完成使用并解除分配。
- 决定使用这些资源

哇哦！你的引用指向了无效的资源。当资源在内存中，我们称之为挂起的指针或“释放内存后使用”。

为了更正这样做，我们不得不确认步骤4永远不会在步骤3之后发生。在 Rust 的所有权系统中，通过一个被称为借用期的概念进行处理，它描述了引用有效的作用域。

当我们有一个带引用参数的函数时，**我们能够隐式的或显式的表示引用的借用期**：
```rust
	// 隐式表示
	fn foo(x: &i32){

	}

	fn bar<'a>(x: &'a i32){

	}
```

`'a` 读作 ‘借用期 a’。技术上，**每一个引用都会有借用期**，但是编译器通常会省略。在我们接触之前，先分解显示表示的例子：

```rust
	fn bar<'a>(...)
```

这部分声明了借用期。表明 `bar` 有一个借用期 `'a`。如果有两个参数都是引用，我们可以这样写：

```rust
	fn bar<'a, 'b>(...)
```

然后，在参数列表，我们使用已经命名过的借用期：

```rust
	...(x: &'a i32)
```

如果我们有一个 `&mut` 引用，可以这样写：

```rust
	...(x: &'a mut i32)
```

如果你对 `&mut i32` 和 `&'a mut i32` 进行比较，它们是一样的，仅是在 `&` 和 `mut i32` 之间插入了借用期 `'a`。我们将 `&mut i32` 读作‘i32的可变引用’，将 `&'a mut i32` 读作‘i32的可变引用，拥有借用期 `'a`’。

当使用 `strut`s 时，你需要显示地表示借用期：

```rust
	strut Foo<'a>{
		x: &'a i32,
	}

	fn main(){
		let y = &5;	//这段代码也可以这样写：'let _y = 5; let y = &_y;'
		let f = Foo{x: y};

		println!("{}", f.x);
	}
```

如你所见，`strut`s也有借用期，和函数很相似。

```rust
	strut Foo<'a>{
```

声明了一个借用期，并使用：

```rust
	x: &'a i32,
```

为什么这里需要借用期？我们要确保任何对 `Foo` 的引用不能比它包含的对 `i32` 的引用持久。


**作用域思想（Thinking in scopes）**

一种方式认为借用期是用来可视化引用有效作用域的。例如：

```rust
	fn main() {
    	let y = &5;     // -+ y 进入作用域
                    	//  |
    	// 其它代码      //  |
                    	//  |
	} 
```

加入 `Foo`：

```rust
	strut Foo<'a>{
		x: &'a i32,
	}

	fn main(){
		let y = &5;           	// -+ y 进入作用域
    	let f = Foo { x: y }; 	// -+ f 进入作用域
    	// 其它代码              //  |
                          		//  |
	}                         	// -+ f 和 y 离开作用域
```
    
`f` 存活在 `y` 的作用域范围内，所以可以正常执行。如果不在作用域，代码就不会执行：

```rust
	struct Foo<'a> {
	    x: &'a i32,
	}
	
	fn main() {
	    let x;                    // -+ x 进入作用域
	                              //  |
	    {                         //  |
	        let y = &5;           // ---+ y 进入作用域
	        let f = Foo { x: y }; // ---+ f 进入作用域
	        x = &f.x;             //  | | 在这里出错
	    }                         // ---+ f 和 y 离开作用域
	                              //  |
	    println!("{}", x);        //  |
	} 
```

哇哦！如你所见，`f` 和 `y` 的作用域都小于 `x` 的作用域。但当我们执行 `x = &f.x` 时，我们创建了一个已经离开作用域的 `x` 的引用。

命名为借用期是给作用域起名的一种方式。首先起个名字，然后才能讨论它。

**静态（'static）**

命名为‘static’的借用期是一种特殊的借用期。这表示它拥有整个程序的借用期。大多数程序员最先想到是处理字符串：

```rust
	let x: &'static str = "Hello, world.";
```

字符文本拥有 `&'static str` 类型，因为引用时一直存在的：它们被整合进最后的二进制数据段。另一个例子是全局的：

```rust
	static FOO: i32 = 5;
	let x: &'static i32 = &FOO;
```

此例将 `i32` 添加到二进制数据段，`x` 对它进行引用。

**借用期省略（Lifetime elision）**

在函数体内，Rust 提供了强大的本地类型推断，但是在项签名上，被禁止允许单独基于项签名推断类型。然而，因为人体工程学的原因，一个被叫做“借用期省略”的受限的二级推理算法应用于函数签名。它仅基于签名组件进行推断，而非基于函数体，仅仅推断借用期参数，它使用3个简单、易记、明白的规则进行推断。这使得借用期省略了项签名的缩写，不会隐藏涉及的实际类型，作为完整的本地推断进行应用。

当我们讨论借用期省略时，会使用术语 *输入借用期（input lifetime）* 和 *输出借用期（output lifetime）*。*input lifetime* 表示函数参数的借用期，*output lifetime* 表示函数返回值的借用期。例如，下面的函数有一个输入借用期：

```rust
	fn foo<'a>(x: &'a str){}
```

这个函数有一个输出借用期：

```rust
	fn foo<'a>() -> &'a str {}
```

这个函数在两个位置都有借用期：

```rust
	fn foo<'a>(x: &'a str) -> &'a str {}
```

这是三条规则：

1. 每一个省略了借用期的函数参数，都是不同的参数。
2. 如果只有一个输入借用期，不管是否省略，这个借用期会被分配给所有省略了借用期的函数返回值。
3. 如果有多个输入借用期，其中一个是 `&self` 或者是 `&mut self`, `self` 的借用期被分配给所有省略的输出借用期。

其他情况下，省略输出借用期都会产生错误。

**示例**

这有许多省略了借用期的函数示例。我们使用展开的方式和这些函数成对显示：

```rust
	fn print(s: &str); // 省略
	fn print<'a>(s: &'a str); // 展开

	fn debug(lvl: u32, s: &str); // 省略
	fn debug<'a>(lvl: u32, s: &'a str); // 展开

	// 上述例子中，`lvl` 不需要借用期，因为它不是一个引用（&）。只有那些关联了引用的
	// （例如一个 `strut` 中含有引用）才需要借用期。
	
	fn substr(s: &str, until: u32) -> &str; // 省略
	fn substr<'a>(s: &'a str, until: u32) -> &'a str; // 展开
	
	fn get_str() -> &str; // 非法的, 没有输入
	
	fn frob(s: &str, t: &str) -> &str; // 非法的, 有两个输入
	fn frob<'a, 'b>(s: &'a str, t: &'b str) -> &str; // 展开: 输入借用期不明确
	
	fn get_mut(&mut self) -> &mut T; // 省略
	fn get_mut<'a>(&'a mut self) -> &'a mut T; // 展开
	
	fn args<T:ToCStr>(&mut self, args: &[T]) -> &mut Command // 省略
	fn args<'a, 'b, T:ToCStr>(&'a mut self, args: &'b [T]) -> &'a mut Command // 展开
	
	fn new(buf: &mut [u8]) -> BufWriter; // 省略
	fn new<'a>(buf: &'a mut [u8]) -> BufWriter<'a> // 展开
```

### 11.可变（Mutability） ###

可变可以改变某些值，在 Rust 中与其他语言略有不同。首先是可变的非默认状态：

```rust	
	let x = 5;
	x = 6;	// 错误
```

我们需要介绍一下可变的关键字 `mut` ：

```rust
	let mut x = 5;
	x = 6;	// 没有问题
```

这是可变的变量绑定。当绑定是可变的，这意味着你被允许修改绑定指向。所以上面这个例子，我们没有修改 x 的值，而是修改绑定指向另一个 `i32` 。

如果你想修改绑定指向，你需要可变引用：

```rust
	let mut x = 5;
	let y = &mut x;
```

`y` 是不可变绑定指向可变引用，这意味着你不能再对 `y` 做其它的绑定（`y = &mut z`）,但是你可以修改已经绑定到 `y` 上的值（*y = 5）。一个细微的差别。

当然，如果你都想修改，可以这样：

```rust
	let mut x = 5;
	let mut y = &mut x;
```

这样，`y` 既可以绑定其他的值，也可以修改它引用的值。

需要重要注意的是 `mut` 是 模式（pattern）的一部分，所以你可以这样做：

```rust
	let (mut x, y) = (5, 6);
	fn foo(mut x : i32){
```

**内部可变与外部可变（Interior VS. Exterior Mutability）**

不管怎样，当你在 Rust 中表达 ‘不可变（immutable）’时，并不意味着是不能改变的：我们称之为‘外部可变（exterior mutability）’。思考一下，举个例子 Arc<T>：

```rust
	use std::sync::Arc;

	let x = Arc::new(5);
	let y = x.clone();
```

当我们调用 `clone()` 时，`Arc<T>` 需要更新引用数量。在这，我们不需要使用 `mut`，`x` 是一个不可变绑定，我们不需要采取 `&mut 5` 或其它方式。

想要理解这些，我们需要回顾 Rust 的核心指导思想--内存安全，Rust 保证它的机制--所有权系统，和更多具体的--借用：

> 你可以拥有这两种借用方式的任何一个，但是，在同一时间两种方式不能同时出现：
> 
> - 0 到 N 个对资源的引用（&T）
> - 只有一个可变的引用（&mut）

所以，这是‘不可变（immutability）’的真正定义：同时存在两个指针是安全的吗？在 `Arc<T>` 示例中，是安全的：可变完全地存在于结构体自身的内部。不需要使用者面对。出于这个原因，使用 `clone()` 提取 `&T`。如果想提取 `&mut T`，会出现问题。

另一种类型，就像 `std::cell` 这种模式，体现了另一面：‘内部可变（Interior mutability）’，例如：

```rust
	use std::cell::RefCell;

	let x = RefCell::new(42);
	
	let y = x.borrow_mut();
```

RefcCell 通过 `borrow_mut` 方法获取 `&mut` 引用，指向自身的内部。这样不危险吗？如果我们这样做：

```rust
	use std::cell::RefCell;

	let x = RefCell::new(42);
	
	let y = x.borrow_mut();
	let z = x.borrow_mut();
```

在运行时，事实上会产生混乱。`RefCell` 会这样做：在运行时，它会执行 Rust 的借用规则，如果违反规则会产生 `panic!`。这是我们能够应对 Rust 可变性规则的另一个方面。先让我们讨论它。

**字段级可变（Field-level mutability）**

无论是借用（`&mut`）还是绑定（`let mut`）都有可变的特性。如示例所示，这意味着你不能有一个既包含可变字段又包含不可变字段的结构体（struct）。

```rust
	struct Point {
	    x: i32,
	    mut y: i32, // nope
	}
```

结构体的可变体现在它的绑定上：

```rust
	struct Point {
	    x: i32,
	    y: i32,
	}
	
	let mut a = Point { x: 5, y: 6 };
	
	a.x = 10;
	
	let b = Point { x: 5, y: 6};
	
	b.x = 10; // 错误: 不能对不可变字段 `b.x` 赋值
```

不管怎么样，使用 `Cell<T>` 可以模仿字段级可变：

```rust
	use std::cell::Cell;

	struct Point {
	    x: i32,
	    y: Cell<i32>,
	}
	
	let point = Point { x: 5, y: Cell::new(6) };
	
	point.y.set(7);
	
	println!("y: {:?}", point.y);
```

将会打印 `y: Cell { value: 7}`,我们成功地更新了 `y`。

### 12.结构体（Structs） ###

结构体是创建复杂数据类型的一种方式。例如，我们在2d空间计算相关的坐标，我们同时需要 `x` 和 `y` 的值：

```rust
	let origin_x = 0;
	let origin_y = 0;
```

结构体然我们将这两项组合成一个独立的、统一的数据类型：

```rust
	struct Point {
		x: i32,
		y: i32,
	}

	fn main {
		let origin = Point {x: 0, y: 0};
		println!("The origin is at ({},{})", origin.x, origin.y);
	}
```

这段代码展示了很多信息，让我们对它进行分解。我们使用 `struct` 关键字声明了一个 `struct`，接着取了一个名字。依照惯例，`struct`s 以大写字母开头并使用驼峰命名：`PointInSpace`，而不是这种方式 `Point_In_Space`。

我们使用 `let` 创建结构体的一个实例，通常，我们使用 `Key: Value` 的语法样式创建每一个字段。字段顺序不需要和声明时一致。

最后，因为字段有名字，所以我们可以使用点符号的方式 `origin.x` 访问字段。

结构体中的值默认是不可变的，就像 Rust 中的其他绑定。使用 `mut` 让它们变成可变的：

```rust
	struct Point {
		x: i32,
		y: i32,
	}

	fn main {
		let mut point = Point{x: 0, y: 0};
		point.x = 5;

		println!("The point is at ({}, {})", point.x, point.y);
	}
```

打印结果是：
> The point is at (5, 0)

在语言级层面上，Rust 不会提供字段的可变性，所以，你不能编写下面的代码：

```rust
	struct Point {
		mut x: i32,
		y: i32
	}
```

可变是绑定的属性，不是结构体自身的属性。如果你想使用字段级可变，这看起来很奇怪，但它确实大大简化了一些事情。它甚至可以让你在短时间内变成可变：

```rust
	struct Point {
	    x: i32,
	    y: i32,
	}
	
	fn main() {
	    let mut point = Point { x: 0, y: 0 };
	
	    point.x = 5;
	
	    let point = point; // 新的绑定不允许修改
	
	    point.y = 6; // 这样做会报错
	}
```

**更新语法（Update Syntax）**

`struct` 含有 `..`，用来表示使用其它结构体副本中的某些值。例如：

```rust
	struct Point3d {
		x: i32,
		y: i32,
		z: i32,
	}

	fn main {
		let mut point = Point3d{x: 0, y: 0, z: 0};
		point = Point3d{y:1, .. point};
	}
```

这段代码为 `point` 重新设置了 `y`，`x` 和 `z` 保留原来的值。它不需要其它相同的 `struct`，当你创建一个新的时可以使用这种语法，它会拷贝你没有明确指明的值：

```rust
	let origin = Point3d {x: 0, y: 0, z: 0};
	let point = Point3d {z: 1, x: 2, .. origin};
```

**元组结构体（Tuple structs）**

Rust 有另外一种数据结构，比较像元组和结构体的混合体，称之为‘元组结构体’。元组结构体有名字，但是它的字段没有名字：

```rust
	struct Color(i32, i32, i32);
	struct Point(i32, i32, i32);
```

虽然它们具有相同的值，但是也不会相等：

```rust
	let color = Color(0, 0, 0);
	let point = Point(0, 0, 0);
```

使用结构体一直都比使用元组结构体要好很多，我们将会编写想下面的 `Color` 和 `Point` 进行替换：
	
```rust
	struct Color {
		red: i32,
		blue: i32,
		green: i32,
	}

	struct Point {
		x: i32,
		y: i32,
		z: i32,
	}
```

现在有了真正的名字，而非位置。对于结构体来说，好的名字很重要。

有一种情况，当是元组结构体时非常有用，尽管那种情况下元组结构体只有一个元素。我们称之为‘新类型’模式，因为它允许你创建一个新的类型，用来区分它包含的值和它自身的语义：

```rust
	struct Inches(i32);

	let length = Inches(10);
	
	let Inches(integer_length) = length;
	println!("length is {} inches", integer_length);
```

如你所见，你可以使用 `let` 重置，获取内在的整数类型，就像使用常规的元组。在此场景中，`let Inches(integer_length)` 将 `10` 赋值给 `integer_length`。

**单元模仿结构体（Unit-like structs）**

你可以定义一个没有任何成员的结构体：

```rust
	struct Electron;
```

这种结构体被称为‘unit-like’，因为它类似于空元组，`()`，有时被称为‘unit’。类似于元组结构体，它定义了一种新类型。

这是很少有用的（尽管它有时作为标记类型），但是综合其他特性，他会非常有用。例如，一个类库请求创建一个结构体，需要实现某些特征来处理事件。如果你不知道在结构体中要存放什么数据，就可以创建一个单元模仿结构体。

### 13.枚举（Enums） ###

在 Rust 中 `enum` 类型是用来描述几个可能的变量数据。

```rust
	enum Message {
		Quit,
		Color(i32, i32, i32),
		Move {x: i32, y: i32},
		Write (String),
	}
```

每一个变量可以有与之关联的数据。定义变量的语法类似于定义结构体：可以定义没有数据的结构体（就像单元模仿结构体‘unit-like struct’），可以定义有命名数据的结构体，可以定义没有命名数据的结构体（就像元组结构体‘tuple struct’）。虽然不像单独的结构体定义，但是，`enum` 是独立的类型。枚举值能够匹配任意一个变量。正因如此，结构体有时候被称为‘类型汇总（sum type）’：枚举可能值的集合是每一个变量可能值的集合的汇总。

我们通过 `::` 语法使用每一个变量的名字：它们的作用域由 `enum` 自身决定。这允许有两个可以同时工作：

```rust
	let x: Message = Message::Move { x: 3, y: 4 };

	enum BoardGameTurn {
	    Move { squares: i32 },
	    Pass,
	}
	
	let y: BoardGameTurn = BoardGameTurn::Move { squares: 1 };
```

两个变量都被命名为 `Move`，由于它们的作用域取决于枚举的命名，所以可以同时使用，不会造成冲突。除了和变量关联的数据外，枚举类型的值还包含变量的信息。由于数据包含指明类型的‘标签（tag）’，有时会被当作‘标签组合（tagged union）’进行引用。编译器会使用这些信息安全地执行从枚举中获取数据。例如：你不能简单地尝试就像可能的变量那样重新设置值：

```rust
	fn process_color_change(msg: Message) {
	    let Message::ChangeColor(r, g, b) = msg; // 编译时报错
	}
```

没有提供这种操作看起来很受限制，但是这样的限制是可以克服的。这有两种方式：自己实现等式或者使用匹配表达式模式匹配变量，匹配会在下一章节学习。现在，我们对 Rust 实现等式知道的很少，但是我们会在特性章节找到答案。

### 14.匹配（Match） ###

通常，简单的 `if/else` 不能满足有更多选项的场景。而且，条件可能会很复杂。Rust 有关键字 `match`，它更加的强大，允许你替换复杂的 `if/else` 分组。来看看:

```rust
	let x = 5;
	match x {
		1 => println!("one"),
		2 => println!("two"),
	    3 => println!("three"),
	    4 => println!("four"),
	    5 => println!("five"),
	    _ => println!("something else"),
	}
```

`match` 有一个表达式，然后是基于值的分支。每一个分支的‘枝干（arm）’都是这种形式 `val => expression`。当值被匹配时，表达式就会被解析。我们称之为 `match` 是因为术语‘模式匹配（pattern match）’，`match` 是它的实现。那有一个关于模式的完整章节，涵盖了所有可能的模式。

所以，匹配最大的优势是什么？当然有一些。首先，`match` 执行 ‘穷尽性检查（exhuastiveness checking）’。正如你所见的最后一个枝干，使用了下滑线(`_`)。如果我们把它删除，Rust 会给我们提示一个错误：

> 	error: non-exhaustive patterns: `_` not covered

换句话说，Rust 尝试提醒我们忘记了一个值。因为 `x` 是整型，Rust 知道 `x` 可以有许多不同的值-例如：`6`。如果没有 `_`，那么就没有枝干和它匹配，Rust 就会拒绝编译这段代码。`_` 扮演类似‘捕获所有枝干（catch-all arm）’的角色。现在，我们拥有一个对应所有可能的 `x` 值的枝干，所以我们的程序可以编译成功。

**`match` 也是一个表达式**，这意味着可以应用于 `let` 绑定的右侧或者直接当表达式使用：

```rust
	let x = 5;

	let number = match x {
	    1 => "one",
	    2 => "two",
	    3 => "three",
	    4 => "four",
	    5 => "five",
	    _ => "something else",
	};
```

有时，这也是类型转换的一种很好的方式。

**枚举中的匹配（Matching on enums）**

`match` 关键字的另一个重要的用法是处理枚举中可能的变量：

```rust
	enum Message {
	    Quit,
	    ChangeColor(i32, i32, i32),
	    Move { x: i32, y: i32 },
	    Write(String),
	}
	
	fn quit() { /* ... */ }
	fn change_color(r: i32, g: i32, b: i32) { /* ... */ }
	fn move_cursor(x: i32, y: i32) { /* ... */ }
	
	fn process_message(msg: Message) {
	    match msg {
	        Message::Quit => quit(),
	        Message::ChangeColor(r, g, b) => change_color(r, g, b),
	        Message::Move { x: x, y: y } => move_cursor(x, y),
	        Message::Write(s) => println!("{}", s),
	    };
	}
```

Rust 再次检查了所有情况，它要求你为每一个枚举的变量都有一个匹配。如果你离开了，它会给你一个编译器错误，除非你使用 `_`。

和之前的 `match` 使用不同，你不需要使用普通的 `if` 描述来实现。你可以使用 `if let` 的描述，它看起来像 `match` 的简写。

### 15.模式（Patterns） ###

模式在 Rust 中很普遍。我们在变量绑定、匹配描述等地方使用。让我们开启一段模式应用的旋风之旅。

快速回忆：你可以直接匹配字母，`_` 扮演匹配‘任何’情况的角色：

```rust	
	let x = 1;
	
	match x {
		1 => println!("one"),
		2 => println!("two"),
		3 => println!("three"),
		_ => println!("anything"),
	}
```

打印结果是：`one`。

**多模式（Multiple patterns）**

你可以使用 `|` 匹配多个模式：

```rust
	let x = 1;

	match x {
		1 | 2 => println!("one or two"),
		3 => println!("three"),
		_ => println!("anything"),
	}
```

打印结果是：`one or two`。

**范围（Ranges）**

你可以使用 `...` 匹配值的范围：

```rust
	let x = 1;
	
	match x {
		1 ... 5 => println!("one through five"),
		_ => println!("anything"),
	}
```

打印结果是：`one or two`。

范围经常应用于整型和字符型：

```rust
	let x = '*';

	match x {
		'a' ... 'j' => println!("early letter"),
		'k' ... 'z' => prinltn!("late letter"),
		_ => prinltln!("something else"),
	}
```

打印结果是：`something else`。

**绑定（Bindings）**

你可以使用 `@` 给值绑定名称：

```rust
	let x = 1;

	match x {
	    e @ 1 ... 5 => println!("got a range element {}", e),
	    _ => println!("anything"),
	}
```

打印结果是：`got a range element 1`。当你想完成一个数据结构一部分的复杂匹配时，它是有用的：

```rust
	#[derive(Debug)]
	struct Person {
	    name: Option<String>,
	}
	
	let name = "Steve".to_string();
	let mut x: Option<Person> = Some(Person { name: Some(name) });
	match x {
	    Some(Person { name: ref a @ Some(_), .. }) => println!("{:?}", a),
	    _ => {}
	}
```

打印结果是 `Some("Steve")`: 我们给内部的 `name` 绑定了 `a`。

如果你同时使用 `@` 和 `|`，你要确定模式的每一部分都要绑定名称：

```rust
	match x {
		e @ 1 ... 5 | e @ 8 ... 10 => println!("got a range element {}", e),
		_ => println!("anything"),
	}
```

**忽略变量（Ignoring variants）**

如果你相匹配枚举变量，你可以使用 `..` 忽略变量的值和类型：

```rust
	enum OptionalInt {
	    Value(i32),
	    Missing,
	}
	
	let x = OptionalInt::Value(5);
	
	match x {
	    OptionalInt::Value(..) => println!("Got an int!"),
	    OptionalInt::Missing => println!("No such luck."),
	}
```

打印结果是 `Got an int!`。

**保护（Guards）**

你可以使用 `if` 介绍 ‘匹配保护（match guards）’：

```rust
	enum OptionalInt {
	    Value(i32),
	    Missing,
	}
	
	let x = OptionalInt::Value(5);
	
	match x {
	    OptionalInt::Value(i) if i > 5 => println!("Got an int bigger than five!"),
	    OptionalInt::Value(..) => println!("Got an int!"),
	    OptionalInt::Missing => println!("No such luck."),
	}
```

打印结果是 `Got an int!`。

**引用和可变引用（ref and ref mut)**

使用 `ref` 关键字获取引用：

```rust
	let x = 5;

	match x {
	    ref r => println!("Got a reference to {}", r),
	}
```

打印结果是 `Got a reference to 5`。

这段代码中，匹配内部的 `r` 的类型是 `i32`。换句话说，关键字 `ref` 创建了一个引用在模式中使用。如果你需要可变的引用，`ref mut` 会使用相同的方式：

```rust
	let x = 5;

	match x {
	    ref mut mr => println!("Got a mutable reference to {}", mr),
	}
```

**调整（Destructuring）**

如果你有类似结构体的复杂的数据类型，你可以在模式内进行调整：

```rust
	struct Point {
		x: i32,
		y: i32,
	}

	fn main {
		let origin = Point {x: 0, y: 0};

		match origin {
			Point {x: x, y: y} => println!("({}, {})", x, y);
		}
	}
```

如果你只关心它们的值，就不需要列出全部的名称：

```rust
	struct Point {
		x: i32,
		y: i32,
	}

	fn main {
		let origin = Point {x: 0, y: 0};

		match origin {
			Point {x: x, ..} => println!("x is {}", x);
		}
	}
```

打印结果：`x is 0`。

你可以将此种方式的匹配应用于任何成员，不仅仅是第一个：

```rust
	struct Point {
		x: i32,
		y: i32,
	}

	fn main {
		let origin = Point {x: 0, y: 0};

		match origin {
			Point {x: x, ..} => println!("x is {}", x);
		}
	}
```

打印结果：`y is 0`。

‘调整’可以应用在类似元组、枚举这种复杂的数据类型。

**混合匹配（Mix and Match）**

哇哦！这是一种非常不同的匹配，我们称之为混合匹配。这取决于你怎么做：

```rust
	match x {
	    Foo { x: Some(ref name), y: None } => ...
	}
```

模式很强大，好好掌握它。

### 16.方法语法（Method Syntax） ###

函数很有用，但是如果你想调用一连串的函数，那会很别扭。思考一下这段代码：

```rust
	baz(bar(foo)));
```

我们从左向右读这段代码，看到了‘baz bar foo’。但是这并非调用函数的顺序，正确的顺序是从内向外的：‘foo bar baz’。有没有更好的替换方式呢？

```rust
	foo.bar().baz();
```

很幸运，你已经猜到了这个关键问题，你可以！Rust 通过 `impl` 关键字提供了以上‘方法调用语法’。

**方法调用（Method calls）**

看看它如何工作：

```rust
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

	fn main {
		let c = Cicle {x: 0.0, y: 0.0, radius: 2.0};

		println!("{}", c.area());
	}
```

打印结果：`12.566371`。

我们有一个描述圆的结构体。然后编写了一个 `impl` 块，里面定义了 `area` 方法。

方法的第一个参数很特殊，有3个变量：`self，&self，&mut self`。你可以认为第一个参数就是 `foo.bar` 中的 `foo`。3个变量对应 `foo` 的3个可能的不同类型：`self` 表示栈上的值，`&self` 表示它的引用，`&mut self` 表示它的可变引用。因为我们将 `&self` 设置成了 `area` 方法的参数，所以我们可以像其他参数一样使用它。因为它表示 `Circle`，所以我们能想使用其它结构体那样调用 `radius`。

我们默认使用 `&self`，如同你喜欢借用胜过获取它的所有权，而且喜欢获取不可变引用胜过可变引用。这个例子展示了这3个变量：

```rust
	struct Circle {
	    x: f64,
	    y: f64,
	    radius: f64,
	}
	
	impl Circle {
	    fn reference(&self) {
	       println!("taking self by reference!");
	    }
	
	    fn mutable_reference(&mut self) {
	       println!("taking self by mutable reference!");
	    }
	
	    fn takes_ownership(self) {
	       println!("taking ownership of self!");
	    }
	}
```

**链式方法调用（Chaining method calls）**

现在，我们已经知道了如何像 `foo.bar()` 这样调用方法。但是最初的例子该怎么办，它可是这样的：`foo.bar().baz()`？我们称之为‘方法链（method chaining）’，我们需要返回 `self`。

```rust
	struct Circle {
		x: f64,
		y: f64,
		radius: f64,
	}

	impl Circle {
		fn area(&self) -> f64 {
			std::f64::consts::PI * (self.radius * self.radius)
		}

		fn grow(&self, increment: f64) -> Circle {
			Circle {x: self.x, y: self.y, radius: self.radius + increment}
		}
	}

	fn main {
		let c = Circle {x: 0.0, y: 0.0, radius: 2.0};
		println!("{}", c.area());

		let d = c.grow(2.0).area();
		println!("{}", d);
	}
```

查看返回类型：

```rust
	fn grow(&self) -> Circle {
```

我们返回了 `Circle`。使用这个方法，我们可以使用任何尺寸扩大圆。

**关联函数（Associated function）**

你可以定义一个关联函数，而不需要使用 `self` 参数。在 Rust 中这是一种很普遍的模式：

```rust
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

‘关联函数’创建了一个新的 `Circle`。注意，关联函数使用 `Struct::function()` 语法进行调用，而不是 `ref.method()` 语法。某些语言称关联函数为‘静态方法（static methods）’。

**构造模式（Builder Pattern）**

我们想创建圆，但是只允许设置关注的圆的属性。另外，`x` 和 `y` 属性为 `0.0`，`radius` 属性为 `1.0`。Rust 没有方法重载、没有命名参数或没有可变参数。我们提供构造模式进行替换，如下：

```rust
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
	
	struct CircleBuilder {
	    x: f64,
	    y: f64,
	    radius: f64,
	}
	
	impl CircleBuilder {
	    fn new() -> CircleBuilder {
	        CircleBuilder { x: 0.0, y: 0.0, radius: 1.0, }
	    }
	
	    fn x(&mut self, coordinate: f64) -> &mut CircleBuilder {
	        self.x = coordinate;
	        self
	    }
	
	    fn y(&mut self, coordinate: f64) -> &mut CircleBuilder {
	        self.y = coordinate;
	        self
	    }
	
	    fn radius(&mut self, radius: f64) -> &mut CircleBuilder {
	        self.radius = radius;
	        self
	    }
	
	    fn finalize(&self) -> Circle {
	        Circle { x: self.x, y: self.y, radius: self.radius }
	    }
	}
	
	fn main() {
	    let c = CircleBuilder::new()
	                .x(1.0)
	                .y(2.0)
	                .radius(2.0)
	                .finalize();
	
	    println!("area: {}", c.area());
	    println!("x: {}", c.x);
	    println!("y: {}", c.y);
	}
```

在这里，我们创建了另外一个结构体：`CircleBuilder`。在它内部我们定义了构造方法。在 `Circle` 中我们也定义了 `area()` 方法。我们还定义了另外一个方法： `CircleBuilder::finalize()`。这个方法通过构造器创建最终的 `Circle`。现在，我们已经使用类型系统执行我们的关注问题：我们使用 `CircleBuilder` 的这些方法约束创建我们想创建的圆。

### 17.向量（Vectors） ###

‘向量（vector）’是动态或‘可增长（growable）’，通过标准的函数类型 `Vec<T>` 实现。`T` 意味着向量可以是任何类型（查看泛型获取更多信息）。向量的数据都被分配到堆中。你可以使用宏 `vec!` 创建向量：

```rust	
	let v = vec![1, 2, 3, 4, 5];	// v: Vec<i32>
```

（注意，这和我们原先用过的宏 `println!` 不太一样，我们宏 `Vec<T>` 使用方括号 `[]` 。Rust 允许在其它场景使用其它的括号，这仅仅就是个约定。）

这是 `vec!` 的另一个方式，重复初始化值：

```rust
	let v = vec![0; 10];	//初始化10个0
```

**获取元素（Accessing Elements）**

在向量中，通过使用 `[]` 获取指定索引的值：

```rust
	let v = vec![1, 2, 3, 4, 5];

	println!("The third element of v is {}", v[2]);
```

索引从 `0` 开始，所以第三个值是 `v[2]`。

**迭代（Interating）**

当你有一个向量，你会使用 `for` 迭代获取它的元素。这有三个版本：

```rust
	let mut v = vec![1, 2, 3, 4, 5];

	for i in &v {
	    println!("A reference to {}", i);
	}
	
	for i in &mut v {
	    println!("A mutable reference to {}", i);
	}
	
	for i in v {
	    println!("Take ownership of the vector and its element {}", i);
	}
```

向量有更多有用的方法，请阅读它们的 API。

### 18.字符串（Strings） ###
	
字符串是个很重要的概念，任何程序员都要掌握。基于 Rust 系统本身的关注点，它的字符串处理方法和其他语言略有不同。任何时候在你拥有可变长度的数据结构时，都是很棘手的，恰恰字符串就是可变的数据结构。话虽如此，Rust 的字符串还是和其它系统有区别，例如 C。

让我们深入细节。‘string’是有序的 Unicode 值编码成 UTF-8 字节流。所有的字符串保证都是合法的 UTF-8 顺序的编码。另外，不像其他语言，字符串不能以空结束，可以包含空字节。

Rust 有两种**字符串类型： `&str` 和 `String`**。首先讨论 **`&str`。它被称作‘字符串切片（string slices）’**。字符串的类型： `&'static str`。

```rust
	let string = "Hello there."; // string: &'static str
```

这个字符串是静态分配的，这意味着它保存在编译系统的内部并且一直存在于整个运行期。`string`为静态分配绑定了引用。字符串切片有固定长度，不能改变。

另一方面，`String` 是分配到内存堆的字符串。此时的字符串是可变的，但仍然保证是 UTF-8 。创建 `String` 的通常做法是使用 `to_string` 方法将字符串切片转换成 `String` 。

```rust
	let mut s = "Hello".to_string(); // mut s: String
	println!("{}", s);
	
	s.push_str(", world.");
	println!("{}", s);
```

使用 `&` 可以将 `String` 强制转换成 `&str`。

```rust
	fn takes_slice(slice: &str) {
	    println!("Got: {}", slice);
	}
	
	fn main() {
	    let s = "Hello".to_string();
	    takes_slice(&s);
	}
```

`String` 转换成 `&str` 代价很小，但是 `&str` 转换成 `String` 牵扯到内存分配。所以，除非你需要，否则不要这么做。

**索引（Indexing）**

因为字符串是合法的 UTF-8，所以不需要提供索引：

```rust
	let s = "hello";

	println!("The first letter of s is {}", s[0]); // 错误!!!
```

通常地，使用 `[]` 可以款速获取向量。但是，由于在 UTF-8 编码的字符串中的每一个字符都是多字节的，所以，你需要处理完整个字符串才能找到具体位置（nᵗʰ）的字母。这种操作显然是非常昂贵的，我们不希望误入歧途。此外，‘字母（letter）’没有在 Unicode 中定义，确实如此。我们可以选择查看字符串中的独立字节或者是代码点：

```rust
	let hachiko = "忠犬ハチ公";
	
	for b in hachiko.as_bytes() {
	    print!("{}, ", b);
	}
	
	println!("");
	
	for c in hachiko.chars() {
	    print!("{}, ", c);
	}
	
	println!("");
```

打印结果：

> 229, 191, 160, 231, 138, 172, 227, 131, 143, 227, 131, 129, 229, 133, 172, 
忠, 犬, ハ, チ, 公, 

如你所见，字节比字符要多。

你可以像索引那样获取值：

```rust
	let dog = hachiko.chars().nth(1); // 有点像 hachiko[1]
```

着重强调一下，我们已经访问了整个 `char` 列表。

**连结（Concatenation）**

如果有一个 `String`，可以在它后面连结 `&str`：

```rust
	let hello = "Hello ".to_string();
	let world = "world!";
	
	let hello_world = hello + world;
```

如果有两个 `String`，你需要使用 `&`：

```rust
	let hello = "Hello ".to_string();
	let world = "world!".to_string();
	
	let hello_world = hello + &world;
```

这是因为 `&String` 能够自动强制转换成 `&str`，这个特性被称为‘强制转换（Deref coercion）’。

### 19.泛型（Generics） ###

有时，当你编写函数或者数据类型时，你希望可以在多种参数类型下都能运行。幸运地是，Rust 的一个特性为我们提供了比较好的方式：泛型。在类型理论中，泛型被称为：‘参数的多态（parametric polymorphism）’，这意味着他们是拥有多种形态（‘poly’表示多个，‘morph’表示形态）参数（‘parametric’）的类型或者函数。

总之，有足够的类型理论，那么让我们看一下通用的代码。Rust 的标准函数库提供了一个类型，`Option<T>`，这是泛型：

```rust
	enum Option<T> {
		Some(T),
		None,
	}
```

刚才看到的 `<T>` 这部分，表示通用的数据类型。枚举的内部声明，也有一个 `T`，我们使用泛型替换相同的类型。这有一个使用 `Option<T>` 的列子，使用了额外的类型注释：

```rust
	let x: Option<i32> = Some(5);
```

类型被声明为 `Option<i32>`。注意，这看起来类似于 `Option<T>`。所以，`Option` 有了详细地描述，`T` 的类型是 `i32`。在绑定的右侧，我们设置了 `Some(T)`，`T` 为 `5`。因为 `5` 的类型也是 i32，所以两边匹配，Rust 顺利运行。如果不匹配，会抛出错误：

>     let x: Option<f64> = Some(5);
>     // error: mismatched types: expected `core::option::Option<f64>`,
>     // found `core::option::Option<_>` (expected f64 but found integral variable)

这并不能意味着我们不能为 `Option<T>` 设置 `f64` 类型！只要匹配就可以：

```rust
	let x: Option<i32> = Some(5);
	let y: Option<f64> = Some(5.0f64);
```

这很好。一次定义，多处使用。

泛型不仅仅应用于一个类型，考虑一下 Rust 标准函数库中的另一种类型,`Result<T, E>` :

```rust
	enum Result<T, E> {
		Ok(T),
		Err(E),
	}
```

两个类型均为泛型，`T` 和 `E`。顺便说一下，大写字母可以是你喜欢的任意字母。我们定义 `Result<T, E>` 像这样：

```rust
	enum Result<A, Z> {
		Ok(A),
		Err(Z>,
	}
```

按照约定，第一个通用参数是 `T` ,对应‘type’，另一个是 `E` ，对应‘error’。Rust 根本就不在乎。

`Result<T, E>` 打算使用返回的计算结果，如果不能正常运行，还会返回一个错误。

**泛型函数（Generic function）**

使用类似语法编写一个拥有泛型的函数：

```rust
	fn take_anything<T>(x: T) {
		// 使用 x 编写代码
	}
```

语法包含两部分：`<T>` 表示函数有一个泛型 `T`，`x: T` 表示 x 的类型是 T。

多个参数可以有相同的泛型：

```rust
	fn takes_two_of_the_same_things<T>(x: T, y: T) {
		//...
	}
```

这是多种类型的版本：

```rust
	fn takes_two_things<T, U>(x: T, y: U){
		//...
	}
```

泛型函数使用‘特性绑定（train bounds)’更有用，我们会在 trait 章节进行介绍。

**泛型结构体（Generic structs）**

你可以像这样在结构体中存放一个泛型：

```rust
	struct Point<T> {
		x: T,
		y: T,
	}

	let int_origin = Point{x: 0, y: 0};
	let float_origin = Point{x: 0.0, y: 0.0};
```

和函数类似，`<T>` 声明了通用参数，通过类型声明使用 `x: T`。

### 20.特性（Traits） ###

还记得 `impl` 关键字吗，使用方法语法调用函数：

```rust
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

特性和此非常类似，只不过定义了一个拥有方法签名的特性，然后实现结构体的特性。就象这样：

```rust
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

如你所见，`trait` 代码块和 `impl` 代码块很相似，但是 `trait` 没有方法体，只有一个类型签名。当我们 `impl` 特性时，使用 `impl Trait for Item`，而不是仅仅使用 `impl Trait`。

我们实用特性来约束泛型。思考一下这个函数，不会被编译，并且会抛出一个类似这样的错误：

```rust
	fn print_area<T>(shape: T) {
	    println!("This shape has an area of {}", shape.area());
	}
```

Rust 反馈为：

> error: type `T` does not implement any method in scope named `area`

因为 `T` 不是任何类型，不能确定它是否实现了 `area` 方法。但是，我们可以给泛型添加‘特性约束（traits constraint）’，确保它可以这样运行：

```rust
	fn print_area<T: HasArea>(shape: T) {
	    println!("This shape has an area of {}", shape.area());
	}
```

语法 `T: HasArea` 表示 `任何实现了 HasArea 特性的类型`。因为特性定义了函数类型签名，所以我们能够确定任何实现 HasArea 的类型都会有 `.area()` 方法。

这是一个扩展后的例子，看看是如何运行的：

```rust
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

这段程序会输出：

>     This shape has an area of 3.141593
>     This shape has an area of 1

如你所见，`print_area` 是一个泛型，我们给它传递了正确的类型。如果给它传递一个错误类型：

```rust
	pring_area(5);
```

我们会得到一个编译器错误：

> error: failed to find an implementation of trait main::HasArea for int

到目前为止，我们只给结构体添加了特性的实现，其实我们可以给任何类型实现特性。从结束角度看，我们可以为 `i32` 实现 `HasArea`：

```rust
	trait HasArea {
	    fn area(&self) -> f64;
	}
	
	impl HasArea for i32 {
	    fn area(&self) -> f64 {
	        println!("this is silly");
	
	        *self as f64
	    }
	}
	
	5.area();
```

在私有类型上实现方法是一种很糟糕的方式，尽管这是可以的。

这看起来可能很疯狂（Wild West）,其实为了防止失控，有另外两个围绕实现特性的限制。第一个是**当特性没有定义在作用域范围内，就不会被应用**。这有一个例子：标准函数库提供了 `Write` 特性，为 `File`s 提供了额外的功能，对文件执行 I/O 操作。默认的，`File` 没有这些方法：

```rust
	let mut f = std::fs::File::open("foo.txt").ok().expect("Couldn’t open foo.txt");
	let result = f.write("whatever".as_bytes());
```

这是错误信息：

>     error: type `std::fs::File` does not implement any method in scope named `write`
> 
>     let result = f.write(b"whatever");
>                    ^~~~~~~~~~~~~~~~~~

首先，我们需要 `use Write` ：

```rust
	use std::io::Write;

	let mut f = std::fs::File::open("foo.txt").ok().expect("Couldn’t open foo.txt");
	let result = f.write("whatever".as_bytes());
```

这样编译没有错误。

这意味着，虽然有些人做了类似给 `int` 添加方法这样糟糕的事情，但是，只要你不 `use` 特性，就不会对你产生影响。

这是另外一个对于实现特性的约束。**任何你编写的 `impl` 特性和类型，必须由你亲自定义。**所以，我们可以为 `i32` 实现 `HasArea` 类型，因为这是我们自己编写的代码。如果我们尝试为 `i32` 实现 Rust 已经提供的特性 `Float`，将会失败，因为无论是特性还是类型都不是我们自己编写的代码。

关于特性的最后一件事：拥有特性绑定的泛型函数使用‘单一形态monomorphization’（mono：单一，morph：形态），所以它们是静态的分配器。这意味着什么？在 `trait objects` 章节获取更多内容。

**多特性绑定（Multiple trait bounds）**

已经知道如何使用特性绑定一个泛型参数：

```rust
	fn foo<T: Clone>(x: T) {
	    x.clone();
	}
```

**如果你需要多个绑定，使用 `+`**：

```rust
	use std::fmt::Debug;
	
	fn foo<T: Clone + Debug>(x: T) {
	    x.clone();
	    println!("{:?}", x);
	}
```

`T` 现在既是 `Clone` 又是 `Debug`。

**where 子句（Where clause）**

编写一个拥有少量的泛型和少数的特性绑定的函数还是挺不错的，但是随着数量的增加，这种语法会逐渐变得不合适：

```rust
	 use std::fmt::Debug;

	fn foo<T: Clone, K: Clone + Debug>(x: T, y: K) {
	    x.clone();
	    y.clone();
	    println!("{:?}", y);
	}
```

函数的名字在最左边，参数列表在最右边。函数范围和原来一致。

Rust 有解决方案，称之为 `Where 子句`。

```rust
	use std::fmt::Debug;
	
	fn foo<T: Clone, K: Clone + Debug>(x: T, y: K) {
	    x.clone();
	    y.clone();
	    println!("{:?}", y);
	}
	
	fn bar<T, K>(x: T, y: K) where T: Clone, K: Clone + Debug {
	    x.clone();
	    y.clone();
	    println!("{:?}", y);
	}
	
	fn main() {
	    foo("Hello", "world");
	    bar("Hello", "workd");
	}
```

`foo()` 使用原先的语法，`bar()` 使用 `where` 子句。你需要做的就是当定义类型参数时，离开函数范围，在参数列表后面添加 `where` 子句。对于很长的列表，可以加入空格：

```rust
	use std::fmt::Debug;

	fn bar<T, K>(x: T, y: K)
	    where T: Clone,
	          K: Clone + Debug {
	
	    x.clone();
	    y.clone();
	    println!("{:?}", y);
	}
```

在复杂的情况下，这种灵活性显得很清晰。


`where` 还有比这更强大的语法，例如：

```rust
	trait ConvertTo<Output> {
	    fn convert(&self) -> Output;
	}
	
	impl ConvertTo<i64> for i32 {
	    fn convert(&self) -> i64 { *self as i64 }
	}
	
	// can be called with T == i32
	fn normal<T: ConvertTo<i64>>(x: &T) -> i64 {
	    x.convert()
	}
	
	// can be called with T == i64
	fn inverse<T>() -> T
	        // this is using ConvertTo as if it were "ConvertFrom<i32>"
	        where i32: ConvertTo<T> {
	    1i32.convert()
	}
```

这展示了 `where` 子句的特性：允许左右边范围是一个任意类型（此例中是 `i32`），不仅仅是一个简单的类型参数（例如 `T`）。

**默认方法（Default methods）**

这是我们学习的最后一个特性：默认方法。这是最简单的，只要展示一个例子：

```rust
	trait Foo {
	    fn bar(&self);
	
	    fn baz(&self) { println!("We called baz."); }
	}
```

`Foo` 的实现者需要实现 `bar()` 方法，但是不需要实现 `baz()` 方法。它们有默认的行为，当然它们可以选择覆盖这些行为：

```rust
	struct UseDefault;

	impl Foo for UseDefault {
	    fn bar(&self) { println!("We called bar."); }
	}
	
	struct OverrideDefault;
	
	impl Foo for OverrideDefault {
	    fn bar(&self) { println!("We called bar."); }
	
	    fn baz(&self) { println!("Override baz!"); }
	}
	
	let default = UseDefault;
	default.baz(); // prints "We called baz."
	
	let over = OverrideDefault;
	over.baz(); // prints "Override baz!"
```

**继承（Inheritance）**

有时，实现特性需要实现另一个特性：

```rust
	trait Foo {
	    fn foo(&self);
	}
	
	trait FooBar : Foo {
	    fn foobar(&self);
	}
```

`FooBar` 的实现者也需要实现 `Foo`，像这样：

```rust
	struct Baz;
	
	impl Foo for Baz {
	    fn foo(&self) { println!("foo"); }
	}
	
	impl FooBar for Baz {
	    fn foobar(&self) { println!("foobar"); }
	}
```

如果忘记实现 `Foo`，Rust 会告诉我们：

> error: the trait `main::Foo` is not implemented for the type `main::Baz` [E0277]

### 21.Drop ###

我们已经讨论过特性了，让我们讨论一个 Rust 标准函数库提供的特别的特性--`Drop`。`Drop` 特性提供了一种方式：**当值离开作用域时，还可以执行某些代码**。例如：

```rust
	struct HasDrop;
	
	impl Drop for HasDrop {
	    fn drop(&mut self) {
	        println!("Dropping!");
	    }
	}
	
	fn main() {
	    let x = HasDrop;
	
	    // do stuff
	
	} // x 离开了作用域
```

当 `x` 在 `main()` 的结尾离开作用域时，`Drop` 代码将会被执行。`Drop` 有一个方法，也被叫做 `drop()`。它需要一个可变的引用 `self`。

这就是 `Drop`，它的结构很简单，但还是有些微妙之处。例如，**值的显示和它们的定义顺序是相反的**。这是另一个例子：

```rust
	struct Firework {
	    strength: i32,
	}
	
	impl Drop for Firework {
	    fn drop(&mut self) {
	        println!("BOOM times {}!!!", self.strength);
	    }
	}
	
	fn main() {
	    let firecracker = Firework { strength: 1 };
	    let tnt = Firework { strength: 100 };
	}
	This will output:
	
	BOOM times 100!!!
	BOOM times 1!!!
```

TNT 在 firecracker 之前显示，因为它是后声明的。后入，先出。

所以 `Drop` 用来做什么好呢？`Drop` 用来清理与 `struct` 关联的所有资源。例如，`Arc<T> type` 是引用计数类型。当 `Drop` 被调用时，会递减引用的数量，当引用数变成零时，将会清空底层的值。

### 22. if let ###

`if let` 允许将 `if` 和 `let` 组合到一起，减少某些模式匹配的开销。

例如，让我们看一下 `Option<T>` 的分类。如果它是 `Some<T>` 则调用一个函数，如果是 `None` 则不做任何事。就象这样：

```rust
	match option {
		Some(x) => { foo(x) },
		None => {},
	}
```

在这里，我们不需要使用 `match`，例如，可以使用 `if`：

```rust
	if option.is_some() {
	    let x = option.unwrap();
	    foo(x);
	} 
```

这两种方式都不是特别吸引人。我们可以使用 `if let` 这种更好的方式去实现它：

```rust
	if let Some(x) = option {
	    foo(x);
	}
```

如果模式匹配成功，它会将任何有价值的值绑定到模式中的标识符，然后评估表达式。

如果在模式不匹配的情况下，你好要做其它的处理，可以使用 `else`：

```rust
	if let Some(x) = option {
	    foo(x);
	} else {
	    bar();
	}
```

**while let**

相类似的，`while let` 可以应用到条件循环，直到值匹配到某个模式。看下面的代码：

```rust
	loop {
	    match option {
	        Some(x) => println!("{}", x),
	        _ => break,
	    }
	}
```

转换后：

```rust
	while let Some(x) = option {
		println!("{}", x);
	}
```


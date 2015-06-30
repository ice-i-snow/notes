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



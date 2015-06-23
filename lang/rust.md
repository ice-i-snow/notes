# Rust 学习笔记 #
基于windows 7 系统。

### Hello World ###

- 官方网站[下载](http://www.rust-lang.org/install.html)，并进行安装。我的安装目录：“D:\Application\Rust stable 1.0”
- 在安装目录下创建projects文件夹
- 创建main.rs文件，并编写代码
`fn main(){
    println!("Hello, World!");
}`
- 打开“Rust1.0 Shell”，然后进入到刚才创建的“projects”目录
- 编译main.rs文件。输入命令：`d:\Application\Rust stable 1.0\projects>rustc main.rs`，会编译生成一个“main.exe”可执行文件
- 执行main.exe文件。输入命令： `d:\Application\Rust stable 1.0\projects>main.exe`，会在控制台显示“Hello, World!”

### Hello Cargo ###

Cargo是管理Rust工程的一个工具，负责3件事：构建代码，下载代码需要的依赖，构建依赖。使用Cargo创建一个“Hello World”。

- 进入“projects”目录，并创建src文件夹。命令如下：`d:\Application\Rust stable 1.0\projects>mkdir src`
- 拷贝main.rs文件到 src 目录下，命令：`d:\Application\Rust stable 1.0\projects>copy main.rs src/main.rs`
- 在 projects 目录下创建“Cargo.toml”文件，并写入以下内容：
>     [package]
> 	name = "hello_world"
> 	version = "0.0.1"
> 	authors = "[ice-i-snow <lib.chrome@gmail.com>]"


[TOML](https://github.com/toml-lang/toml)格式规范。

- 进行编译，命令如下：`d:\Application\Rust stable 1.0\projects>cargo build`。编译完成后，会创建 target 文件夹和 Cargo.lock 文件
- 进入到 target\debug 文件夹下，执行 main.exe，命令如下：
`d:\Application\Rust stable 1.0\projects>cd target\debug`
`d:\Application\Rust stable 1.0\projects>main.exe`
此时在控制台输出“Hello，World！”

其实我们可以合并最后2步，使用`d:\Application\Rust stable 1.0\projects>cargo run`命令，直接编译代码并执行，同样在控制台输出：“Hello，World!”

我们可以使用 `Cargo new` 命令创建一个新的项目。命令如下:`d:\Application\Rust stable 1.0\projects>cargo new hello_world --bin`


# Rust 学习笔记 #
### Hello World ###
基于windows 7 系统。

- 官方网站[下载](http://www.rust-lang.org/install.html)，并进行安装。我的安装目录：“D:\Application\Rust stable 1.0”
- 在安装目录下创建projects文件夹
- 创建main.rs文件，并编写代码
`fn main(){
    println!("Hello, World!");
}`
- 打开“Rust1.0 Shell”，然后进入到刚才创建的“projects”目录
- 编译main.rs文件。输入命令：`d:\Application\Rust stable 1.0\projects>rustc main.rs`，会编译生成一个“main.exe”可执行文件
- 执行main.exe文件。输入命令： `d:\Application\Rust stable 1.0\projects>main.exe`，会在控制台显示“Hello, World!”


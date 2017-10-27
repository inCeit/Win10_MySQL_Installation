# Win10_MySQL_Installation

在Win10下编译调试MySQL5.7.20记录

## 1.Prerequisite
#### 安装环境

![安装环境](https://github.com/inCeit/Win10_MySQL_Installation/blob/master/pictures/clipboard.png)

#### 必须预先安装的软件


1. CMake，本次使用的是CMake3.9x64版，绿色安装，官网zip包(绿色版需要配置环境变量);

```PowerShell
  我的电脑（右键）-> 属性 -> 高级系统设置 ->环境变量，在下面“系统变量”里编辑 Path的值，增加CMake解压的路径。
```

2. 一个make程序，虽然一些平台有自己的make类工具，但强烈建议使用GNU make3.75（或更高版本），可从这里下载：http://www.gnu.org/software/make/

3. ANSI标准C++编译器（注：make文件中要求Windows上最低版本为VS2013）

4. 安装Boost C++ libraries（构建MySQL需要使用，MySQL本身不用），Boost 1.59.0必须安装（下载直接解压缩就可以），安装完成过后必须告诉CMake从哪里找到Boost库。（Boost 1.59.0下载地址：https://sourceforge.net/projects/boost/files/boost/1.59.0/）

```PowerShell
  PS: cmake . -DWITH_BOOST=C:\boost_1_59_0
```
5.bison 2.4.1或更新版本(安装路径不能包含空格如“Program Files”)

6. ncurses库——5.7手册上没要求，官网要求，实测不用安装（部分linux发行版需要安装）

```PowerShell
  ** 注意：以上软件安装要以管理员身份执行安装，不然会出现错误。 **
```

#### 检查下软件安装情况


1. 依次测试每个软件的安装情况，当出现下面的结果时说明安装完成。

![安装图片](https://github.com/inCeit/Win10_MySQL_Installation/blob/master/pictures/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20171020150907.png)


#### 编译前的准备工作

```PowerShell
  注意修改sql\sql_locale.cc编码格式，方法为用EditPlus或其他软件打开保存为Uncode编码，不然编译可能不通过。
  注意修改sql\mysqld.cc中4344行 DBUG_ASSERT(0) 改为 DBUG_ASSERT(1)
```

## 2.开始安装

1. 进入到源码包内，建立bld文件夹，运行以下命令（设置编译器、设置boost库的位置）生成VS2017解决方案文件：

```PowerShell
  PS:cmake .. -G "Visual Studio 15 Win64" -DWITH_BOOST=C:\boost_1_59_0
```

也可以直接生成zip安装文件：

```PowerShell
  PS:cd .\bld
  PS:cmake .. -DWITH_BOOST=C:\boost_1_59_0
  ~
  ~ 输出
  ~
  PS:cmake --build . --target package
```
这样就会在.\bld\_CPack_Packages\win32\ZIP文件夹下应该会生成.zip文件结尾的压缩包。

![安装图片](https://github.com/inCeit/Win10_MySQL_Installation/blob/master/pictures/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20171020151049.png)

执行上命令的结果：

![安装图片](https://github.com/inCeit/Win10_MySQL_Installation/blob/master/pictures/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20171020151348.png)

2. 打开VS2017（最好以管理员身份运行），依次：“文件”->“打开”->“项目/解决方案”，进入刚才生成的.\bld文件夹，打开MySQL.sln。找到mysld工程（MySQL守护程序），右键选择：“属性”->“调试”，在“命令参数”一栏写入：--initialize（命令行方式启动，并初始化）。

```PowerShell
  第一次调试后，也可以用CMD 进入.\bld\sql\Debug\，运行.\mysqld.exe --console直接启动MySQL服务器。
```

![安装图片](https://github.com/inCeit/Win10_MySQL_Installation/blob/master/pictures/initialize.png)

3.设置完毕后，右键mysqld，点击：“调试”->“生成新实例”，开始编译并调试程序，预计20分钟左右。编译完成后如果没有错误，则mysqld会启动完成初始化，并会自动退出，并在.\bld\sql\data文件夹内生产.err的文件。打开.err结尾的文件到最后一行查看为root@localhost生成的密码，并复制。（此密码第一次登陆时记得修改）

4.此时可以进入.\bld\sql\Debug，执行下面的命令启动服务器，以便下面登陆服务器修改root密码。

```PowerShell
  PS: .\mysqld.exe --console
```

![安装图片](https://github.com/inCeit/Win10_MySQL_Installation/blob/master/pictures/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20171020153850.png)

5.选择mysql工程（MySQL客户端），右键选择“属性”->“调试”，“命令参数”写入：-uroot -p ，再右键“调试”->“生成新实例”，在弹出的窗口中把生成的密码粘贴进窗口，如果一切顺利，就应该可以看到MySQL的欢迎界面。

![安装图片](https://github.com/inCeit/Win10_MySQL_Installation/blob/master/pictures/QQ%E6%88%AA%E5%9B%BE20171020211238.png)

6.运行下面命令修改root用户密码，修改完成后就可以正常调试，使用MySQL。(密码要求至少包括大小写字母数字和特殊字符)

```PowerShell
  msyql> alter user 'root'@'localhost' identified by 'Abc123!'
```

***

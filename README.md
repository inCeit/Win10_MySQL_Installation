# Win10_MySQL_Installation

在Win10下编译调试MySQL5.7.20记录

## 1.Prerequisite

#### 必须预先安装的软件

***

1. CMake，本次使用的是CMake3.9x64版，绿色安装，官网zip包;
2. 一个make程序，虽然一些平台有自己的make类工具，但强烈建议使用GNU make3.75（或更高版本），可从这里下载：http://www.gnu.org/software/make/
  3. ANSI标准C++编译器（注：make文件中要求Windows上最低版本为VS2013）
  4. 安装Boost C++ libraries（构建MySQL需要使用，MySQL本身不用），Boost 1.59.0必须安装，安装完成过后必须告诉CMake从哪里找到Boost库。（Boost 1.59.0下载地址：https://sourceforge.net/projects/boost/files/boost/1.59.0/）
  > 例：shell > cmake . -DWITH_BOOST=/usr/local/boost_1_59_0
  5. ncurses库——手册上没要求，官网要求，实测不用安装
#### 检查下软件安装情况
***
1. 依次测试每个软件的安装情况，当出现下面的结果时说明安装完成；另外Boost库已经下载解压至C:\boost_1_59_0。

![安装图片](https://github.com/inCeit/Win10_MySQL_Installation/blob/master/pictures/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20171020150907.png)
2. 进入到源码包内，建立bld文件夹，运行以下命令：
```PowerShell
  PS:cmake .. -G "Visual Studio 15 Win64" -DWITH_BOOST=C:\boost_1_59_0
```
![安装图片](https://github.com/inCeit/Win10_MySQL_Installation/blob/master/pictures/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20171020151020.png)

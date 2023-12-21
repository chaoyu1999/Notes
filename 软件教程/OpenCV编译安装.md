# 1、下载安装OpenCV

- [OpenCV 4.8.0 for Windows安装包下载地址](https://objects.githubusercontent.com/github-production-release-asset-2e65be/5108051/909c7495-778f-473e-986f-15771fd5aba4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20231209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20231209T013243Z&X-Amz-Expires=300&X-Amz-Signature=f5832c8bb45ae1204dde7b06a6359ad69994f8edb5e2d80a27c17062c886b832&X-Amz-SignedHeaders=host&actor_id=46945475&key_id=0&repo_id=5108051&response-content-disposition=attachment%3B%20filename%3Dopencv-4.8.0-windows.exe&response-content-type=application%2Foctet-stream)

![](https://raw.githubusercontent.com/onlyfabin/PB_1/main/markdown/202312091028229.png)

- 选择提取目录

![image-20231209103030406](C:\Users\25351\AppData\Roaming\Typora\typora-user-images\image-20231209103030406.png)

# 2、编译环境

使用`CMake+MinGW`来编译项目

![](https://raw.githubusercontent.com/onlyfabin/PB_1/main/markdown/202312091032470.png)

# 3、编译源码

1. 打开CMD命令行。

2. 切换到OpenCV的源代码目录：

```cmd
cd D:\Program Files (x86)\opencv\sources
```

3. 创建一个新的目录来存放编译生成的文件：

```cmd
mkdir build
cd build
```

4. 运行CMake来配置项目和生成构建系统，使用MinGW Makefiles生成器：

```cmd
cmake -G "MinGW Makefiles" ..
```

这个命令会为MinGW创建Makefile。请确保MinGW安装路径已经添加到系统环境变量中，这样CMake才能找到它。

5. 编译OpenCV：

```cmd
mingw32-make
```

或者如果你的MinGW安装了多线程编译支持，可以使用：

```cmd
mingw32-make -j<number_of_cores>
```

这里`<number_of_cores>`是你想要使用的核心数，例如，如果你有4个核心，可以使用`mingw32-make -j4`来加速编译过程。

> **编译过程报错：**
>
> ![](https://raw.githubusercontent.com/onlyfabin/PB_1/main/markdown/202312091101597.png)
>
> 报错原因：错误信息 `'D:\Program' 不是内部或外部命令，也不是可运行的程序或批处理文件。` 表明命令行在解析包含空格的路径时出现了问题。在Windows中，当路径包含空格时，需要将整个路径用双引号（"）括起来。
>
> 错误发生在尝试编译OpenCV的`vs_version.rc`文件时，这是因为CMake生成的Makefile或其他构建脚本没有正确处理包含空格的路径。
>
> **解决方法：**
>
> 移动OpenCV项目到没有空格的目录下。在CMD里面用`move "D:\Program Files (x86)\opencv" "D:\"`移动到D盘根目录下。
>
> ![](https://raw.githubusercontent.com/onlyfabin/PB_1/main/markdown/202312091107221.png)



6. 安装OpenCV（可选）：

```cmd
mingw32-make install
```

这将会安装OpenCV到默认的安装路径，或者你在CMake配置时指定的路径。

完成这些步骤后，你应该有一个使用MinGW编译的OpenCV版本，可以在CLion中使用。确保在CLion的CMakeLists.txt文件中设置正确的`OpenCV_DIR`变量，指向你的OpenCV构建目录，这样CLion就能找到OpenCV的配置文件并正确链接库文件。
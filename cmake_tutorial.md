# CMakeLists.txt

## cmake一个可执行文件👇

```cmake
cmake_minimum_required(VERSION 3.16.2)

project(project_name（随便写）)

add_executable(executable_file_name [src_files ...])
```

### set

1. 针对[src_files ...]太长，可用`set`存储到`VAR`中：set(字符串)

> 实际上并未解决问题，仍然需要在指定srcs的时候手动输入很多文件名

```cmake
set(VAR [VALUE ...])

eg:
set(srcs a.cpp b.cpp c.cpp) 或者 set(srcs a.cpp;b.cpp;c.cpp)
add_executable(app ${srcs})
```

用以下**两种**方法**搜索某个路径下所有文件**：

> `set/file/aux_source_directory`(存储变量)

```cmake
1.
aux_source_directory(搜索路径 VAR):
    aux_source_directory(${PROJECT_SOURCE_DIR} VAR):  # PROJECT_SOURCE_DIR:CMakeLists.txt所在路径，等于CMAKE_CURRENT_SOURCE_DIR
2.
file(GLOB/GLOB_RECURSE VAR 搜索路径和文件名)：
    file(GLOB/GLOB_RECURSE VAR project/src/*.cpp)
```

1. 指定编译器版本

```cmake
方法1：
set(CMAKE_CXX_STANDARD 11)

方法2：(脚本中)
cmake .. -DCMAKE_CXX_STANDARD=11
```

3. 指定**可执行文件**输出路径

> (产生的`Makefile`等**中间文件**还是将会在你当前所在目录，如`build`)

```cmake
set(HOME /home/xxx)
set(EXECUTABLE_OUTPUT_PATH ${Home}/bin)  # 若路径不存在，会自动创建
```

### 指定头文件查找路径

> 多个路径使用""来组合成为list，使用`\;`进行分割或者**换行**分割

```cmake
include_directories(
    "${PROJECT_SOURCE_DIR}/include"
    "${PROJECT_SOURCE_DIR}/sub_dir/include"
)
```

> 建议使用 target_include_directories() 命令来为特定的目标设置包含路径，而不是全局地设置整个项目的包含路径。

## 制作一个库

```cmake
add_library(库名称 STATIC/SHARED SRCS...)
```

## 指定生成库的位置

> 同样产生的Makefile等中间文件会在当前cmake目录中😃

```cmake
set(LIBRARY_OUPUT_PATH /project/libs)
```

## 连接生成的库

```cmake
link_directories(库所在文件夹)
link_libraries(库名)
```

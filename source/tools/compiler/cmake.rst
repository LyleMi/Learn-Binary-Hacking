CMake
========================================

运行
----------------------------------------
运行CMake最基本的方法是通过CMake命令行程序。调用它的最简单方法是更改到build目录，并将生成器类型和源树位置的选项传递给CMake。例如:

.. code-block:: shell

    mkdir build
    cd build
    cmake -G "Unix Makefiles" ../source

如果省略了-G选项，CMake将根据平台选择默认的生成器类型。

注意这里如果没有新建一个build目录也可以构建，这种方式被称为源内构建。这种方式下编译文件和源文件混在一起，增加了构建的困难，也难以创建干净的源，不推荐在任何情况下使用这种方式。

最小项目
----------------------------------------
构建一个最小CMake项目的CMakeLists.txt文件如下：

.. code-block:: cmake

    cmake_minimum_required(VERSION 3.2)
    project(App)
    add_executable(elf main.cpp)

注意CMake的命令名称也是不区分大小写的。

常用函数
----------------------------------------

cmake_minimum_required
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
函数原型为 ``cmake_minimum_required(VERSION major.minor[.patch[.tweak]])`` ，其中VERSION关键字必须始终出现，提供的版本详细信息必须有major.minor部分。

project
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
每个CMake项目都应该包含一个project()命令，它应该在cmake_minimum_required()调用之后出现。

项目名称是必需的，只能是字母、数字、下划线(_)和连字符(-)，通常只有字母和下划线在实践中使用。

构建目标
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: cmake

    add_executable(targetName [WIN32] [MACOSX_BUNDLE]
                   [EXCLUDE_FROM_ALL]
                   source1 [source2 ...]
    )

    add_library(targetName [STATIC | SHARED | MODULE]
                [EXCLUDE_FROM_ALL]
                source1 [source2 ...]
    )

FLAGS
----------------------------------------
- ``CC`` 编译器
- ``CXX`` C++ 编译器
- ``LD`` 链接器
- ``CFLAGS`` C 编译器的选项
- ``CXXFLAGS``  C++ 编译器的选项
- ``LDFLAGS`` 链接器的选项
- ``LIBS`` 链接器要链接的库

参考链接
----------------------------------------
- `CMake Reference Documentation <https://cmake.org/cmake/help/latest/>`_
- `Professional CMake <https://github.com/xiaoweiChen/Professional-CMake.git>`_

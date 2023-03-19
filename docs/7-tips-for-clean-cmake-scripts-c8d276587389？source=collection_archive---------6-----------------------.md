# 制作优秀 CMake 脚本的 7 个技巧

> 原文：<https://towardsdatascience.com/7-tips-for-clean-cmake-scripts-c8d276587389?source=collection_archive---------6----------------------->

## 如何让你的 DevOps 生活更轻松

![](img/56828e01ddb7d3e796b01f2311d00566.png)

Photo by [贝莉儿 DANIST](https://unsplash.com/@danist07?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

CMake 已经成为 c++构建自动化、测试和打包的事实上的标准工具。它广泛用于多平台开发，并支持为大多数 C++编译器生成构建文件。自从 21 年前推出以来，它走过了漫长的道路，增加了对许多优秀特性的支持，在 C++开发人员中广受欢迎。

在这篇文章中，我想向你介绍一些可能帮助你编写更好的 CMake 脚本的技巧。虽然您可能已经熟悉其中一些，但我相信您也会发现一些有用的。

# 1.始终使用 target_*()命令

由于向后兼容，像 *add_definitions* 、*include _ directory*、 *link_libraries、*等函数在现代 CMake 中仍然存在。但是，只要有可能，您应该更喜欢使用它们的对应项*target _ compile _ definitions*、*target _ include _ directory*、 *target_sources、*或 *target_link_libraries* 来代替。

由于不带 *target_* 的函数没有指定作用域，它们将泄漏到目录中指定的所有目标。例如，如果您想要构建一个可执行目标，它还依赖于子目录中定义的库，您可以这样指定它:

```
project(main_project)
add_executable(main_project main.cpp)
add_subdirectory(dependency_lib)
include_directories(include_files)
```

不幸的是，由于 include _ directories 中指定的目录被附加到当前 *CMakeLists.txt* 文件中所有目标的列表中，它们也将被附加到 *dependency_lib* 。因此，我们最终可能会在 dependency_lib 中使用错误的包含文件。这同样适用于其他命令。尤其是使用 *add_definitions* 或者 *add_compile_options* 这样的命令，这可能会导致一些难以发现的编译错误。

另一方面，使用*target _ include _ directory*或 *target_link_libraries* ，我们显式地指定了我们想要使用的目标，避免了任何泄漏问题。此外，这些命令让我们可以有选择地指定实现所需继承行为的范围(见下一篇技巧)。

# 2.使用目标传递来指定依赖关系层次结构

在现代 CMake 中，目标命令让您使用*接口*、*私有*和*公共*关键字来指定命令范围。如果您想要将依赖从子目标延续到父目标，这是很有用的。

例如，让我们考虑下面的代码:

```
project(main_project)
add_executable(main_project main.cpp)
add_subdirectory(dependency_lib)
target_link_libraries(main_project PRIVATE dependency_lib)
```

在子目录 dependency_lib 中，我们有以下 CMakeLists.txt:

```
project(dependency_lib)
add_library(dependency_lib SHARED library.cpp)
add_subdirectory(sub_dependency_lib)
target_link_libraries(dependency_lib PRIVATE sub_dependency_lib)
```

显然，我们有一个依赖关系的层次结构。 *main_project* 依赖于 *dependency_lib* ，后者又依赖于 *sub_dependency_lib* 。这样写， *sub_dependency_lib* 对 *main_project* 是不可见的。这意味着 *main_project* 将不能直接使用 *sub_dependency_lib* 的任何函数。如果你将关键字 PRIVATE 替换为 PUBLIC，你也可以在 *main_project* 中使用 *sub_dependency_lib* 。如果 *dependency_lib* 没有完全隐藏依赖关系，这是很有用的，这是经常发生的情况。INTERFACE 关键字使用频率较低，它指定一个在“上游”目标中使用的依赖项，而不在声明它的目标中使用(在本例中， *sub_dependency_lib* 对 *main_project* 可见，但不会在 *dependency_lib* 本身中使用)。

# 3.防止源代码内生成

在配置任何 CMake 构建之前，您应该执行的默认步骤是创建一个构建子目录，然后运行 CMake。但是，有时您可能会忘记创建附加目录并在根目录下运行 CMake。这种“源代码内构建”污染了您的项目，并在 git 中产生了许多变化。

如果您想要禁用源代码内编译，请将下面几行放到您的根文件 *CMakeLists.txt* 中:

```
if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
message(FATAL_ERROR “In-source build detected!”)
endif()
```

这个简单的脚本比较了源目录和构建目录，如果它们相等，就会抛出一个错误。

# 4.指定跨平台 C++一致性的语言标准

建议使用以下定义为每个 CMake 项目指定 C++标准:

```
project(main_project LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
```

这确保了在给定编译器支持的情况下，标准将被强制执行，并关闭非标准 C++特性。避免通过添加编译标志来设置 C++标准。这不是跨平台兼容的，因为每个编译器使用不同的标志。

如果需要更好的控制，可以指定编译器支持的具体 C++特性:

```
target_compile_features(dependency_lib
    PUBLIC
        cxx_nullptr
    PRIVATE
        cxx_variadic_templates
        cxx_lambdas
)
```

在这种情况下，我们要求 *dependency_lib* 编译时支持变量模板、nullptr 和 lambdas，而上游目标也应该编译时支持 nullptr。

# 5.如果您的项目包含 CUDA 代码，请将 CUDA 添加为一种语言

有了 modern CMake，您就不仅仅局限于 C 或 C++。CMake 还支持其他语言，如 Objective-C 或 Fortran。C++世界中使用频率较高的语言之一是 CUDA，NVIDIA 的 GPGPU 编程语言。设置 CUDA 传统上是相当困难的。使用 CMake，您可以将 CUDA 作为编程语言添加到 CMake 项目中，如下所示:

```
cmake_minimum_required(VERSION 3.8 FATAL_ERROR)
project(cmake_and_cuda LANGUAGES CXX CUDA)
```

然后，您可以像添加 C++源文件一样轻松地添加新的 CUDA 内核:

```
add_executable(cmake_and_cuda kernel.cu kernel.h)
```

要选择特定的 CUDA 架构，您可以将以下内容添加到 cmake 命令中:

```
cmake -DCMAKE_CUDA_FLAGS=”-arch=sm_75” .
```

# 6.将重复的 CMake 代码放在宏或函数中

与任何其他编程语言一样，CMake 也提倡不重复自己(DRY)原则。将需要两次以上的代码放在单独的函数或宏中总是好的。CMake 中的宏定义如下:

```
macro(foo arg)
  <commands>
endmacro()
```

另一方面，函数被定义为:

```
function(foo arg)
  <commands>
endfunction()
```

尽管这两个概念非常相似，但它们在作用域上有所不同:一个函数定义了它自己的作用域，而一个宏的行为就像你在使用它的地方粘贴了代码一样。例如，宏中的 return()语句不是从宏返回控制，而是返回封闭范围的控制。

# 7.使用 ExternalProject 添加自定义目标

如果您的项目依赖于自定义目标，例如外部存储库，您可以使用 ExternalProject 模块。 *ExternalProject_Add* 命令允许您下载并使用 git repo，如下所示:

```
ExternalProject_Add(
  Eigen
  GIT_REPOSITORY "https://gitlab.com/libeigen/eigen.git"
  GIT_TAG "${EXPECTED_EIGEN_VERSION}"
  SOURCE_DIR eigen
  BINARY_DIR eigen-build
  CMAKE_ARGS
    -DBUILD_TESTING:BOOL=OFF
    -DBUILD_SHARED_LIBS:BOOL=ON
)
```

在这个例子中，我们使用了流行的 Eigen3 矩阵向量库，方法是指定存储库 URL、要检查的特定 git 标记、源目录和普通 CMake 项目中的二进制目录，以及 CMake 参数。除了 git，该命令还支持 Subversion、CVS、Mercurial 和普通下载。默认情况下，添加的项目被假定为 CMake 项目(尽管并不需要如此)。您也可以指定单独的 CMake 生成器和单独的构建命令。

指定外部项目后，它可以用作另一个 CMake 目标，即您的目标可以依赖于它，外部项目本身也可以依赖于其他目标。你可以在 [CMake 文档](https://cmake.org/cmake/help/latest/module/ExternalProject.html)中找到更多关于 ExternalProject 模块的信息。

# 结论

现代 CMake 是一个用于定义整个软件构建的广泛且可扩展的系统。它支持许多不同的平台和语言，并允许您建立一个相互依赖的目标层次结构。最后一个技巧:将 CMake 代码视为代码库的一部分，并好好维护它。这将使构建过程对项目的所有开发人员透明。

# 资源

*   CMake 文档:【https://cmake.org/cmake/help/v3.20/ 
*   CMake 目标说明:[https://lei Mao . github . io/blog/CMake-Public-Private-Interface/](https://leimao.github.io/blog/CMake-Public-Private-Interface/)
*   目标及物性解释:[https://pspdfkit.com/blog/2018/modern-cmake-tips/](https://pspdfkit.com/blog/2018/modern-cmake-tips/)
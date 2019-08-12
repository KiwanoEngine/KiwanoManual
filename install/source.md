# 通过源代码安装

如果您想在使用 Kiwano 的过程中学习源代码，或者希望使用更多的编译选项（例如编译 DLL），则可以使用源代码的方式安装 Kiwano 引擎。

## 安装步骤

1. 打开 [Kiwano 仓库]({{ book.github.repo }})，克隆该仓库或点击 “Download” 按钮下载源代码 ![Step1]({{book.assets.images}}/install/source_01.png)
2. 打开你的 Visual Studio 解决方案, 右键你的解决方案并选择 “添加” => “现有项目” ![Step2]({{book.assets.images}}/install/source_02.png)
3. 选择你下载的源代码中 projects 文件夹下的 `.vcxproj` 文件 ![Step3]({{book.assets.images}}/install/source_03.png)
4. 右键你的项目，打开“属性”页，编辑 “C\C++” => “常规” 的“附加引用目录”选项，输入你下载的源代码中的 src 文件夹 ![Step4]({{book.assets.images}}/install/source_04.png)
5. 右键你的项目的“引用”按钮，选择添加引用，并选中 `kiwano` 项目 ![Step5]({{book.assets.images}}/install/source_05.png)
6. 现在你可以通过源代码构建你自己的程序了！

# 通过源代码安装

1. Clone or download source code from Github repository
2. Open your Visual Studio solution, right-click your solution in Solution Explorer, select `Add` and then `Existing item`
3. Select `.vcxproj` files in /projects folder which you downloaded in 1st step
4. Right-click your project and choose `Properties`, select C\C++ => General, add the root directory of kiwano project to the `Additional include directory` field
5. Right-click `References` and choose `Add Reference`, select `kiwano` project
6. Now you can build your own applications based on Kiwano source code !

将下面的的代码（可以按自己需求更改）保存到一个批处理文件，例如gl.bat中，再将该文件所在的路径添加到path环境变量下。之后就可以通过cmd或运行窗口输入"gl"直接调用该脚本，很方面地提交代码。

```bash
echo 执行cd命令：进入本地指定仓库

cd /d E:\Data_Learning\Notes

echo 执行git命令：添加以N开头的文件或文件夹

git add N* 

echo 执行git命令：添加README

git add README.md

echo 执行git命令：添加描述

git commit -m "⏫ ✍️ 📄 Update notes at %Date:~0,4%/%Date:~5,2%/%Date:~8,2% %Time:~0,2%:%Time:~3,2%"

echo 执行git命令：push到仓库  

git push

```


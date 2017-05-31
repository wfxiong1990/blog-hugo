---
title: 用python来拷贝文件到指定文件夹并启动对应程序
date: 2014-10-25 12:15:16
tags: [python,]

---


## 码代码目的
因为工作需要，经常要把自己项目下的dll从debug版本拷贝到publish版本，然后启动publish版本下面的exe程序，由于重复劳动多次后，决定来造福自己一下。

## 开始码代码
两个技术点，一是用python来复制文件并且覆盖原文件，二是从cmd来启动应用程序。

### python复制文件
有两种方法，一种是用自带的open和write方法来实现文件的打开和写入，上代码：

```python
open(targetFile, "wb").write(open(sourceFile, "rb").read())
```
其中wb和rb分别对应的写二进制文件和读二进制文件。所以代码的意思就是，把sourcefile用二进制的方式都进来，然后再写到targetfile文件中去。

经过测试，我发现把targetfile的属性改成只读的，读写文件的方法也能够把文件从源文件夹复制到目的文件夹，而且复制完以后只读属性还在的。

还有一种就是调用shutil.py模块里面的copyfile的方法，简单暴力。

```python
import shutil

shutil.copyfile(sourcefile,targetfile)
shutil.copy2(sourcefile,targetfile)  #原文件的所有属性状态全部复制
```
shutil里面有很多copy的方法，各自有细微的区别，具体参考<http://www.cnblogs.com/CLTANG/archive/2011/11/15/2249257.html>。需要注意的是，sourcefile和targetfile必须是**文件**的绝对路径，而不是文件名的绝对路径，否则程序会报错。

### 从命令行启动程序
首先，看看你的程序是否可以从cmd启动，也是是`Win+R`打开cmd后输入程序的绝对路径，例如`E:\Program Files (x86)\Tencent\QQ\Bin\QQ.exe`回车，如果能跳出程序的话，好办了~
用subprocess的check_call方法直接调用就可以了

```python
subprocess.check_call([u"E:\\Program Files (x86)\\Tencent\\QQ\\Bin\\QQ.exe"])
```
后面还可以跟好多参数，具体见这里<https://docs.python.org/2.7/library/subprocess.html#subprocess.check_call>
官方的说明如下：

> Run command with arguments. Wait for command to complete. If the return code was zero then return, otherwise raise CalledProcessError. 

好了，大功告成，现在贴一下做所有的代码:

```python
# coding=UTF-8
import os
import subprocess


def copyFiles(sourceDir, targetDir):  # 把某一文件夹下的指定文件复制到目标文件夹中    
    fileList = ['1.dll', '2.dll', '3.dll', '4.dll', '5.dll', '6.dll',
                '7.dll', '8.dll']
    for item in fileList:
        sourceFile = os.path.join(sourceDir, item)
        targetFile = os.path.join(targetDir, item)
        if os.path.isfile(sourceFile):
            try:                
                open(targetFile, "wb").write(open(sourceFile, "rb").read())
                print ("'copy file ",item," successfully!'")
            except StandardError, e:
                print e.message


if __name__ == "__main__":  # 主函数
    sourceDir = 'D:\\my_program\\Debug\\bin'
    targetStr=raw_input('Press r to copy to release or others to copy to publish\n')
    # targetDir
    if(targetStr.lower()=='r'):
        targetDir = 'D:\\my_program\\Release\\bin'
    else:
        targetDir='D:\\my_program\\Publish\\bin'
    copyFiles(sourceDir, targetDir)
    subprocess.check_call([u"D:\\my_program\\target.exe"])//通过命令行启动target.exe
    raw_input('Press any key to exit!')
```

## 总结
如果没有这个程序，那么之前我需要打开sourceDir文件夹，在200多个文件中找到需要的dll，然后再打开targetDir,复制过去，会弹框说该目录下已存在此文件，把 *为之后7个冲突执行此操作* 打上勾，点复制和替换，找到target.exe，双击启动。现在呢，双击这个copydll.py文件，然后dll复制好了，程序也起来了，it works！

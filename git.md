

在当前目录创建一个txt文本,内容是file 1

```
echo "file 1">file1.txt
```

查看文件内容

```
cat file1.txt
```

查看状态或差异

```
git log --oneline  查看提交历史， --oneline表示简介模式
```




文件提交到暂存区

```
 git add file1.txt   git add *.txt(提交所有txt文件到暂存区)
 git add. (当前目录下所有文件)
```





创建.ssh文件夹和.git位于同一目录

```
mkdir .ssh
```



--查看公钥

```
vi test.pub
```

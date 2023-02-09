**linux相关知识点**

https://blog.csdn.net/han_xiaoyang/article/details/11492379

查找文件夹下文件数据大小在一百兆以上的文件

​                find . -type f -size +100000k find . -type f -size +100M              

**ubuntu 添加 github** 

​                192.30.253.112 github.com 151.101.44.249  github.global.ssl.fastly.net              

来源： https://blog.csdn.net/weixin_34245749/article/details/94506724

**ubuntu python 读取doc 和docx文件**

**读取docx文档**

使用的包是python-docx

1. 安装python-docx包

```
sudo pip install python-docx               
```

2. 使用python-docx包读取数据

```
#encoding:utf8  
import docx  
doc = docx.Document('test.docx')  
docText = '\n'.join([paragraph.text for paragraph in doc.paragraphs])
#print(docText)               
```

python-docx这个包是不能处理doc文档的，要读取doc文档内容的话需要使用antiword这个工具。

**读取doc文档**

1. 到网站 http://www.winfield.demon.nl/#Programmer 下载antiword。

2. 下载完毕之后解压，在解压得到的文件夹中依次运行make和make install命令。

3. 使用antiword读取doc文档内容


```
#encoding:utf8  
import subprocess  
word = 'test.doc'  
output = subprocess.check_output(['antiword',word])  
print(output)               
```

**优盘挂载**

有时候优盘不能挂载，或者挂载上去显示被保护

```
mkfs.ext4 /dev/sdd
df -hl
fdisk -l
mount /dev/sdi /mnt/usb/
```

**ubuntu 查看文件是否缓存在内存中**

```
watch -n 0.1 free -h              
```

**deepin  搜索不到wifi**

```
sudo iw dev wlp4s0 set power_save off  sudo iwconfig wlp4s0  txpower fixed              
```

**linux 更改桌面壁纸**

```
gsettings set org.gnome.desktop.background picture-uri "path"
```

**deepin 搜狗输入法待选框乱码**

1、直接卸载搜狗拼音
```
sudo apt-get remove sogoupinyin
```
2、删除~/.config/下搜狗相关的东西
```
rm -r ~/.config/sogou*
rm -r ~/.config/SogouPY*
```
3、官网下载一个最新的安装包再 安装即可
```
dpkg -i 安装包.deb
```
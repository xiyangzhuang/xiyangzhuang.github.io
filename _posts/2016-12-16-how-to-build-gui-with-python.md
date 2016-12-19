---
layout: post
title: How to build gui with python
modified: 2014-12-19
categories: [articles, python]
tags: 
  - python
comments: true
---
为了方便测试我们搭建的界面和构建一下小应用，该应用获取服务器字符串，端口号，文件名（路径），并将它保存在get_filepath_from_server.py,由于本应用仅供测试使用，其代码简单如下所示：

```python
helptxt = """
get_filepath_from_server.py -file fffffff [-port number] [-server server|localhost]
"""
defaultServer = 'localhost'
defaultPort = '80'
def parse_command_line():
    dict = {}                          #put in dict for easy finding
    args = sys.argv[1:]                #skip program name at front of the args
    while len(args) >= 2:
        dict[args[0]] = args[1]         #examle dict['-server']='localhost'
        args = args[2:]
    return dict
  
def get_filepath_from_server(server, port, file):
   return server + ":" + port + ":" + file
     
def main(args):
      
    server = args.get('-server', defaultServer)
    port = args.get('-port', defaultPort)
    filepath = args.get('-file', None)
    if filepath == None:
        print(helptext)
      
if __name__ == '__main__':
    args = parse_command_line()
    main(args)
```

假设大家对Python3界面搭建有一个基本的了解，下面分别介绍使用行框架和命令行，网格，复用表单三种搭建界面的方法。对于追求代码通用性的应该果断舍弃前两种方案。

<!-- more -->

1）使用行框架和命令行：

```python
helptxt = """
get_filepath_from_server.py -file fffffff [-port number] [-server server|localhost]
"""
import sys, os
from tkinter import *
from tkinter.messagebox import showinfo
 
def onReturnKey():
    cmdline = ('python get_filepath_from_server.py -server %s -port -%s -host %s'%
                (content['File'].get(),
                content['Port'].get(),
                content['Server'].get()))
    os.system(cmdline)
    showinfo('getfilegui-1', 'Process complete')
 
box = Tk()
labels = ['Server', 'Port', 'File']
content = {}
for label in labels:
    row = Frame(box)
    row.pack(fill=X)
    Label(row, text=label, width=6).pack(side=LEFT)
    entry = Entry(row)
    entry.pack(side=RIGHT, expand=YES, fill=X)
    content[label] = entry
 
box.title('getfilegui-1')
box.bind('<Return>', (lambda event: onReturnKey()))
mainloop()
```

2）使用网格和函数调用：

```python
helptxt = """
get_filepath_from_server.py -file fffffff [-port number] [-server server|localhost]
"""
import get_filepath_from_server
from tkinter import *
from tkinter.messagebox import showinfo
 
def onSubmit():
    get_filepath_from_server.get_filepath_from_server(content['Server'].get(),
                                    content['Port'].get(),
                                    content['File'].get())
    showinfo('getfilegui-2', "process complete")
 
     
box= Tk()
labels = ['Server', 'Port', 'File']
rownum =0
content = {}
for label in labels:
    Label(box, text=label).grid(column=0, row=rownum)
    entry = Entry(box)
    entry.grid(column=1, row=rownum, sticky=E+W)
    content[label] = entry
    rownum += 1
     
box.columnconfigure(0, weight=0)
box.columnconfigure(1, weight=1)
Button(text='Submit', command=onSubmit).grid(row=rownum, column=0, columnspan=2)
 
box.title('getfilegui-2')
box.bind('<Return>', (lambda event: onSubmit()))
mainloop()
```

3）使用可利用的表单布局类：

示例代码1：

```python
"""
"""
from tkinter import *
entrysize = 40
 
class Form:
    def __init__(self, labels, parent=None):
        labelsize = max(len(x) for x in labels) + 2
        box = Frame(parent)
        box.pack(expand=YES, fill=X)
        rows = Frame(box, bd =2, relief=GROOVE)
        rows.pack(side=TOP, expand=YES, fill=X)
        self.content = {}
        for label in labels:
            row = Frame(rows)
            row.pack(fill=X)
            Label(row, text=label, width=labelsize).pack(side=LEFT)
            entry = Entry(row, width=entrysize)
            entry.pack(side=RIGHT, expand=YES, fill=X)
            self.content[label] = entry
        Button(box, text='Cancel', command=self.onCancel).pack(side=RIGHT)
        Button(box, text='Submit', command=self.onSubmit).pack(side=RIGHT)
        box.master.bind('<Return>', (lambda event: self. onSubmit()))
    def onSubmit(self):
        for key in self.content:
            print(key, '\t=>\t', self.content[key].get())
    def onCancel(self):
        Tk.quit()
 
         
class DynamicForm(Form):    
    def __init__(self, labels=None):
        labels = input('Enter field names:').split()
        Form.__init__(self, labels)
         
    def onSubmit(self):
        print('Field Values...')
        Form.onSubmit(self)
        self.onCancel()
if __name__ == "__main__":
    import sys
    if len(sys.argv) == 1:
        Form(['Name', 'Age', 'Job'])
    else:
        DynamicForm()
    mainloop()
```

示例代码2：

```python
"""
getfilegui.py
"""
from form import Form
from tkinter import Tk, mainloop
from tkinter.messagebox import showinfo
import get_filepath_from_server, os
 
class GetFilePathForm(Form):
    def __init__(self, oneshot=False):
        root = Tk()
        root.title('getfilegui')
        labels = ['Server Name', 'Port Number', 'File']
        Form.__init__(self, labels, root)
        self.oneshot = oneshot
    def onSubmit(self):
        Form.onSubmit(self)
        portnumber = self.content['Port Number'].get()
        servername = self.content['Server Name'].get()
        filename = self.content['File'].get()
        get_filepath_from_server.get_filepath_from_server(servername, portnumber, filename)
        showinfo('getfilegui', 'Process Complete')
        if self.oneshot: Tk().quit()
if __name__ == '__main__':
    GetFilePathForm()
    mainloop()
```

---
title: 'IDEA的使用'
date: 2020-05-06 21:22:10
tags: []
published: true
hideInList: false
feature: 
isTop: false
---

* 构成

  文件从大到小依次是project->module->package，

​       自己的代码放在src文件下

​       package的命名一般由数字、小数点 、小写字母构成(例如 cn.itcast.day04.demo01 每个小数点前都是一个文           

​       件夹，包含小数点后的文件，是一组层级文件)

* 设置和操作

​       导入一个模块：file->project Structure->import module

​       settings->keymap->Mainmenu->code->completion->basic(将其改为Alt+/) ，用于代码提示快捷输入。

* 常用快捷键

  `Alt+/`，快捷输入 ； `Alt+Enter`，导入包，自动修正代码； `Ctrl+Y` 删除光标所在行；

  ` Ctrl+D` 复制光标所在行的内容，插入光标位置下面 ；  `Ctrl+Alt+L` 格式化代码 ；

  `Ctrl+/ `单行注释 ； `Ctrl+Shift+/ `选中代码注释，多行注释，再按取消注释 ；

  `Alt+Ins `自动生成代码，toString，get，set等方法 ；`Alt+Shift+上下箭头` 移动当前代码行

  `n.fori` 快速生成n次循环for语句
  
  psvm快捷生成`public static void main(String[], args)`
  
  sout快速输出
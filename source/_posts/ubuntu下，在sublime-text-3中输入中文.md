---
title: ubuntu下，在sublime text 3中输入中文
date: 2017-07-09 16:25:24
tags:
 - ubuntu
 - sublime text 3
 - 中文
---

### WHAT
解决在ubuntu下，sublime中无法输入中文的问题

### WHY
因为打算把每天遇到的技术问题记录下来，写个博客，以备以后查看。然而，编辑器习惯用sublime，但是，ubuntu上sublime text 3又不支持中文输入，于是便动手找解决方法。

<!-- more -->

### HOW
[百度经验](http://jingyan.baidu.com/article/f3ad7d0ff8731609c3345b3b.html)找到的解决办法，上面写适用于Ubuntu 14.04，经检验，同样适用于Ubuntu 16.04。

#### 工作环境
Ubuntu 16.04，搜狗拼音输入法，sublime text 3

#### 保存下面代码到文件'~/sublime_imfix.c'
```c
#include <gtk/gtkimcontext.h>
void gtk_im_context_set_client_window(GtkIMContext *context, GdkWindow *window)
{
    GtkIMContextClass *klass;
    g_return_if_fail(GTK_IS_IM_CONTEXT(context));
    klass = GTK_IM_CONTEXT_GET_CLASS(context);
    if(klass->set_client_window)
        klass->set_client_window(context, window);
    g_object_set_data(G_OBJECT(context), "window", window);
    if(!GDK_IS_WINDOW(window))
        return;
    int width = gdk_window_get_width(window);
    int height = gdk_window_get_height(window);
    if(width != 0 && height !=0)
        gtk_im_context_focus_in(context);
}
```

#### 编译代码为共享库libsublime-imfix.so
命令：
```shell
cd ~
gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
```

#### 然后将libsublime-imfix.so拷贝到sublime_text所在文件夹
```shell
sudo mv libsublime-imfix.so /opt/sublime_text/
```

#### 修改文件/usr/bin/subl的内容
```shell
sudo vim /usr/bin/subl
```
将

```shell
#!/bin/sh
exec /opt/sublime_text/sublime_text "$@"
```
修改为
```shell
#!/bin/sh
LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text "$@"
```

此时，在终端中执行 **subl** 打开sublime已经可以输入中文了

#### 修改文件sublime_text.desktop的内容
为了使用鼠标右键打开文件时能够使用中文输入，还需要修改文件sublime_text.desktop的内容。命令：
```shell
sudo vim /usr/share/applications/sublime_text.desktop
```

将***[Desktop Entry]***中的字符串：
```shell
Exec=/opt/sublime_text/sublime_text %F
＃修改为：
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text %F"
```
将***[Desktop Action Window]***中的字符串：
```shell
Exec=/opt/sublime_text/sublime_text -n
＃修改为：
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text -n"
```
将***[Desktop Action Document]***中的字符串：
```shell
Exec=/opt/sublime_text/sublime_text --command new_file
＃修改为：
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text --command new_file"
```
**注意**：
修改时请注意双引号"",否则会导致不能打开带有空格文件名的文件。

### END
至此，已经可以在sublime text 3 中输入中文了
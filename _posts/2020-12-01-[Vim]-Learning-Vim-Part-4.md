---
layout: post
title: "[Vim] Learning Vim (Part-4)"
tags: ["Vim","Text Editor"]
comments: true
---
In the previous [blogpost](https://shrishailsgajbhar.github.io/post/Vim-Learning-Vim-Part-3), we learned how to make use of markers in Vim. In this post, we will understand the concept of buffers and read command.

## Buffers in Vim

###### What are the buffers in Vim? What is their use?
Many times we want to open multiple files in Vim. The contents of the file currently opened in vim are stored on the buffer and the changes made to the buffer are only written on the disk if the buffer is saved. In vim terminology, a window is a view to the buffer. Having many windows  pointing to the same buffer is possible in vim and is useful whenever we want to view different parts of the file at the same time.

For example, let us open the file `perfect.txt` using vim which looks like the following
![](/assets/images/20201201/pic1.png)

To open a new window pointing to the same buffer type `:split` which will create two windows horizontally. 
![](/assets/images/20201201/pic2.png)

Any changes made to any window will be reflected in other window also. To switch the windows on can use `Ctrl+w` followed by `action`. For example if we want to switch to the second window and go to the first word then type `Ctrl+w``w`. To close one of the window type `Ctrl+w``c`.

To open two different files `sleep.txt` and `success.txt` in different windows, type `vim sleep.txt` to open it in the first window. Now to open the other file in another window, type `:new success.txt` in `normal` mode. The output looks like the following:
![](/assets/images/20201201/pic3.png)

Here, you can copy contents from one window and paste in another window by switching between the windows.

Suppose you type `vim` at the terminal it will create an empty buffer. If you open a file for example `success.txt` the content of it will be stored in a buffer and window to that buffer will show its contents. If you opened a new file using `:e sleep.txt`, then the previous window will disappear and current window will show contents of new buffer i.e., contents of `sleep.txt`. Where did the first file go? Well it is still in there and you can view its contents by closing the current buffer using `:bd` here `b` stands for current buffer and `d` for delete. Now the previous file contents will be visible in the window.

##### How to know how many buffers are currently available?
Type `:ls` in normal mode.

For example, if two files `sleep.txt` and `success.txt` are currently opened then the output for `:ls` will look like the following:
![](/assets/images/20201201/pic4.png)

You can switch between these buffers using `:bn` to open next buffer while `:bp` to open previous buffer. You can also switch to a particlular buffer by using filename also. For example, typing `:b sleep.txt` will open the buffer for file `sleep.txt`. Since each opened buffer is numbered one we can also use the numbers to swich between buffers. For example, typing `:b 1` will switch to the buffer for `sleep.txt`.

##### Read command in Vim
Many times when you want to include some content regularly in other files such as license text then the `read` command is useful for this purpose. You can create a seperate file for regularly used content for example `license.txt`. Now to include this file in the following program at the top:
![](/assets/images/20201201/pic5.png)

one can first go to the start of the file using `gg` then press `O` to create an empty line above. We create it since the contents of file read using `read` command will be pasted below that line. Now, press `Esc` to go in normal mode then type `:r licence.txt` and press enter. The output will now be as follows:
![](/assets/images/20201201/pic6.png)

 

## References:

1. [Learning Vim](https://www.linkedin.com/learning/learning-vim) by Miki Tebeka hosted on **LinkedIn Learning**.

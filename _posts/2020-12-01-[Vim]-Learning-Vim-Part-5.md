---
layout: post
title: "[Vim] Learning Vim (Part-5)"
tags: ["Vim","Text Editor"]
comments: true
---
In the previous [blogpost](https://shrishailsgajbhar.github.io/post/Vim-Learning-Vim-Part-4), we understood the concept of buffers and use of read command. In this post, we will understand how to customize vim for our needs by editing the `.vimrc` file.

## Editing your `.vimrc` file for customization
Vim editor lets you perform many customizations. If you want your customizations to be functional whenever you open the vim then you need to write them in your `.vimrc` file.

For example, we want line numbers to be on each time we open the vim then we can modify the `.vimrc` file by opening it using `vim ~/.vimrc` at the terminal and then write command `set number` save the file and close it and open vim again to see the changes.
![](/assets/images/20201201/pic7_1.png)

##### Adding key mapping to your .vimrc file
Vim allows us to have the custom key mappings for different actions. One most popular key mapping recommended by programmers is for the `Esc` key which is used a lot for switching to the `normal mode`. Let us map this to `jk`. We can do this customization by editing the`.vimrc` file by adding the command `imap jk <Esc>`.

##### Adding abbriviations to your .vimrc file
While writing many times we would like to abbriviate some long text instead of typing it again. Vim allows you to do that using `abbr` command. For example, if you have to type your email id many times in a file then you can add abbriviation for the same permenantly by adding it to your `.vimrc` as `abbr _me shrishailgajbhar5@gmail.com`. Now whenever you open any file and type `_me` then immediately after pressing the `<SPACE>` key, it will be expanded to your email id. If you do not want to let expand your abbreviation in some cases then after typing the abbreviation press `Ctrl+v`.

##### Adding custom commands to your .vimrc file
If you are a python developer and want to quickly test your program without switching to terminal and using `python <filename.py>` to run your program. You can do this inside the vim by adding `com! Py ! python %`.
Here, `com!` stands for forcing an existing command, `Py` you name for the command, `!` infront of `Py` refers to the external command, while `python` is actual command and `%` refers to the current filename.

Consider the following program:
![](/assets/images/20201201/pic8.png)

When you press enter after typing `:Py`, the following output will be displayed:
![](/assets/images/20201201/pic9.png)

##### Changing settings in vim
You can view your current settings by typing `:set` in normal mode. You can change your settings by modifying your `.vimrc`. For example, you want case insensitive searching turned on then you can add this setting to your `.vimrc` as `set ic`. Also, you can turn the highlighting for search on using `set hls`, you can disable it using `noh`.  

##### Few more things about vim 
* You can open a file in vim with cursor at line number 8 using `vim file1.txt +8 `.
* If you want to compare two versions of the same file e.g., `file_v1.txt` and `file_v2.txt`, then you can use `diff` mode in vim with `vim -d file_v1.txt file_v2.txt`. You can change the windows using `Ctrl+w`. 
* You can open `.zip` or `.tar` files directly in vim. You can open the contents, make some changes and save them also without any need to extract the files (quite handy!).
*  Suppose you have list of files, then you can open the file by going to that particular file name and pressing `gf`.
* You can read the output of an external command also and store write in a file. For example, you want to create a file `index.txt` listing all the files of some folder then you can do it by reading the output of external command `ls` as:
	* Create 'index.txt' using `vim index.txt`
	* Type `:r ! ls` and press enter.
* Learn more about vim from its user manual by typing `:help user`, go to the listed topic names and press `Ctrl+]` to open the help, to close the file press `Ctrl+o`.
* You can learn even more by typing `vimtutor` at terminal which will open the classic vim tutorial.
* You can learn vim through games also! such as [vimadventures](https://vim-adventures.com/)
* You can make use of many vim plugins to make your vim even more powerful such autocompletion, fancy status bars etc. You can find them at https://vimawesome.com/

That's it about the vim series. Thanks to the great course by Miki Tebeka hosted on LinkedIn learning. It was fun.. Please give vim a try.

## References:

1. [Learning Vim](https://www.linkedin.com/learning/learning-vim) by Miki Tebeka hosted on **LinkedIn Learning**.

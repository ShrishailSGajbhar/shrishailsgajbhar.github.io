---
layout: post
title: "[Vim] Learning Vim (Part-1)"
tags: ["Vim","Text Editor"]
comments: true
---
> I've been using Vim for about 2 years now, mostly because I can't figure out how to exit it.
> 
> -- [I Am Devloper](https://twitter.com/iamdevloper), a twitter user.

Vim is a feature rich and hackable text editor which has vertical learning curve however it has been tested through the time (26 years) and according to Lindy effect it will continue to stay for a longer time in future also.

Vim may come preinstalled on Linux and Mac OS, you can check the current vim version using

```bash
vim --version 
```

If it is not present then you can install it on your linux machine using

```bash
sudo apt-get update
sudo apt-get install vim
```

On my Ubuntu 20.04, vim version at the time of writing this blog is `Vi IMproved 8.1`.

## Basics

Vim is a *modal* editor. It has different modes which offer various functionalities to the programmer:

1. Normal Mode (used for text editing, deleting etc.) [A vim user spends most of the time in this mode]

2. Insert Mode (Typing or text insertion mode)

3. Command Line mode (for running commands)

4. Replace Mode (for replacing text)

5. Visual Mode (for selecting blocks of text)
   1. Line
   
   2. Block

You can open a textfile in Vim through terminal using:

```bash
vim xyz.txt
```

Here, vim will be opened in the `normal mode`. You can switch to the `insert mode` by pressing the `i` key on your keyboard. To switch to `normal mode` press `Esc` key. You can switch to `command line mode` from normal mode using `:` operator while use `Esc` to switch from command line to the insert mode. Use `R` to switch from `normal` to `replace` mode, use `Esc` to go in normal mode again. You can use `v` to switch from `normal` to `visual` mode. For switching to specific visual mode from `normal` mode, press `V` to enable visual line mode while `Ctrl+v` for visual block mode. 

If you want to set the line numbers in vim, type the following command in the normal mode
```vim
:set number
```

If you want to set the line numbers permenantly in Vim then edit the `.vimrc` file by first opening it using `vim ~/.vimrc` command at the terminal. Then go to the end of the file by pressing `G`, then switch to the insert mode by pressing `i` then write the above command in this file without `:` operator, go to normal mode again using `Esc` key and finally exit the file by typing `:wq`. 

##### How to quit vim?

Make sure you are in `normal mode` then press `:q` 

##### How to quit all vim editor files?

Type the following in the `normal mode`:

```vim
:qa! 
```

Explanation: (its a command which means "quit all now")

Here, **:** is used as prefix to enter command line mode, **q** stands for shortcut to quit command, **a** stands all buffers and **!** is used for forcing the command.

##### How to save/write the the contents onto the vim file ?

Write the text/code or any content in the `insert mode` then switch to the normal mode by hitting the `Esc` key and write :

```vim
:w
```

**w** here is shortcut for `write` command.

If you want you can also write the contents onto the different file by specifying the filename as an argument to write command. For example,

```vim
:w file1.txt
```

Here, `file1.txt` will be created explicitly but you will still be editing your previously opened file. In order to edit the `file1.txt` you may type

```vim
:e file1.txt
```

in the `normal mode`. You may use `Tab` after **e** to get the desired filename.

Use `Ctrl + g` to know the name of the current filename which you are editing.

##### Generally, `:wq` is used to write and quit from the vim editor.

## Moving Around

When you open a file in vim it opens it in the `normal` mode and to navigate through the file you may tend to use naturally the arrow keys and they perfom as desired however you need to move your fingers from the normal typing position. In Vim editor there are four keys in the home row which you can use to perform the following:

* `h`: to move the cursor in the left direction

* `j`: to move the cursor downward direction

* `k`: to move the cursor in the upward direction

* `l`: to move the cursor in the right direction

In order to move four words in the right direction, type `4l`.  In order to move between the starts of the words in a sentences use `w` whereas use `e` to move between ends of the words. Use `}` to move between the paragraphs in the downward direction while use `{` to move beteen the paragraphs in the upward direction. Use `)` to jump one sentence to the right while use `(` to jump to the left by one sentence. In order to jump 3 sentences to the right, one can type `3)` in the normal mode.

In order to jump to a particular word downward to your current position use `/` followed by the word you want to go. For example to go to word `Shrishail` type `/Shrishail` and hit enter and your cursor will move to the right where first entry of your entered word is present  to go to the next occurance of the word press `n` to go in the backward direction to your word use `N`. 

Let's say you want to search all occurances of a word in the upward direction from your current position type `?<your word>` and press enter to go to the first occurance of that word upwards. Use `n` to continue to visit the next occurances in upward direction whereas use `N` to go in the reverse direction.

To move to the end of the file press `G`. To move to the begining of the file press `g` twice i.e., `gg`. 

One useful command for debugging the program file is by jumping to particular line where the error has occured. You can do it in vim by typing `:` followed by the `line number`. For example, if you want to jump to line number 8 then type `:8` in normal mode. You may use `$` to go to the end of the line or `0` to go the start of the line.

If you want to search for the next instance of the particular word where your cursor currently is without using `/` then use as `*` to go to the next instance of it. To go backwards use `#` symbol.

## References:
1. [Learning Vim](https://www.linkedin.com/learning/learning-vim) by Miki Tebeka hosted on **LinkedIn Learning**.
2. [Lecture notes](https://missing.csail.mit.edu/2020/editors/) and [youtube video](https://youtu.be/a6Q8Na575qc), part of **The Missing Semester of Your CS Education** lecture series (MIT).
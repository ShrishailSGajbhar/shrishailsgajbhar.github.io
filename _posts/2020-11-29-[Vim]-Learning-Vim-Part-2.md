---
layout: post
title: "[Vim] Learning Vim (Part-2)"
tags: ["Vim","Text Editor"]
comments: true
---
In the previous [blogpost](https://shrishailsgajbhar.github.io/post/Vim-Learning-Vim-Part-1), we studied the basics of Vim editor and some essential commands to move freely in the files opened using Vim without using the mouse.

In this post, we will see how to change the text in Vim.

## Changing Text

##### How to delete a word in a file opened in Vim editor?

Make sure you are in the normal mode, then type `dw`, the word where the cursor was currently will be deleted. 

##### How to delete a sentence in a file opened in Vim editor?

In the normal mode, type `d)`

##### How to delete a line in a file opened in Vim editor?

Type `dd` (in normal mode)

##### How to delete everything upto a certain word in a file?

Type `d` first then use regular expression as `/<word>`. All content from your current position upto the first occurance of that word will get deleted.

If your want to **undo** the changes type `u`.

##### How to do copy paste in Vim?

To copy a word in vim use `yw` here, `y` stands for "yank" . To paste this word type `p` this will paste the word next to the character where cursor currently resides. To paste just before the current cursor position use  `P`. To copy the whole line type `yy` and use `p` to paste just below the current line.

  If you want to delete a character under cursor use `x`. Let's say we made a spelling mistake in typing the word `recieve` and we want to swap the positions of characters `i` and `e` to get the correct word. Here, we take our cursor to the character `i` in the word then type `xp` to get the correct word `receive`.

##### Use of the change command `c`

The change command is pretty useful to make changes in the text by deleting either a character, word, sentence or line etc. and then going directly in the `insert` mode thus  saving us typing of one key `i` explicitely.

For example consider the following text

```vim
 Success means you're going to have better problems.
 -- Ari Weinzweig
```

Here, let us say we want to change word *problems* into the *challenges*. Let us do it using the `change` command as follows: 

Assuming normal mode and the cursor is at the start of the line i.e., at `S` then we can go to the word problems by using regular expression as `/pr` and then pressing enter to go the desired word. Now in order to delete this word and going into the `insert` mode, we type `cw` . Now one can observe that word is deleted and we are in the insert mode, now we can type the word *challenges* to complete the task.

##### About visual mode

The visual mode of Vim is very handy to make changes in the text. From normal mode one can switch to visual mode using `v` . One can switch to `visual line` mode using `V` whereas to go into the visual block mode use `Ctrl+v`. Now consider the following text as follows is opened in Vim and we want to edit the same

```vim
    Light thinks it travels faster than anything but it is wrong. No
    matter how fast light travels, it finds the darkness has always got
    there first, and is waiting for it.
    -- Terry Pratchett
```

In the above block one can see that there are two whitespaces on each line which we want to remove. This can be done very efficiently using visual block mode.

Make sure you are in the `normal` mode, then type `Ctrl+v` to go in the visual block mode. Now press `ll` to select the two whitespaces of the first line then type `3j` to select the all 4 lines in the block. Now press `d` to delete the whitespaces. One can observe that the mode is automatically switched to the `normal` mode again.

Now let us say you want to insert the `-` on each of 4 lines of the new block which has no whitespaces. 

We can do the same in two steps:

* first adding a single whitespace at the begining of each line 

* Using change mode, changing these whitespaces to `-` syambol

In first step, press `Ctrl+v` to select the visual block mode and press `4j` to select the first characters of all 4 lines. Now to insert the whitespace we do the following which is bit tricky. We need to type `:` which automatically appends `'<,'>` to its end, now in the same line  type `norm I ` which will add one space to all 4 lines. The overall command in this case should look like `: '<,'> norm I `.  After pressing the `enter` key, you will see that one space is added at the start of each line.

**Explanation:** (taken from this [reference](https://www.fir3net.com/UNIX/Linux/how-do-i-add-a-space-to-selected-lines-within-vim.html))

`:'<,'>` : For the line I selected  

`norm` : Execute the following sequence of keystrokes as if I was in normal mode  

`I`: Insert at the beginning of the line the following characters  

`<SPACE>`: The character(s) you would like to insert

Now, for second step, type `gg` to go to the start of the line. The cursor will be on  character  `L`.  Now press `h` to go the whitespace of first line. Now switch to the  `visual mode` by pressing `Ctrl+v`. Here, you will see that all whitespaces of all 4 lines are selected. Now, press `c` which will delete the whitespaces of all 4 lines. The cursor now will be on the first line with the **insert** mode turned on automatically. Press `-` and then `Esc` key, you will find that all 4 lines now have `-` symbol at the start of each line. The final output will be as follows:

```vim
-Light thinks it travels faster than anything but it is wrong. No
-matter how fast light travels, it finds the darkness has always got
-there first, and is waiting for it.
-    - Terry Pratchett
```

Let us say if we want to delete the line number 2 and 3 of the above block then we can do the same by using following steps:

* Type `:2` to go the 2nd line.

* Type `V` to enable the visual line mode which will select the first line.

* Type `j` to select one more line below i.e., 3rd line.

* Type `d` to delete the lines

##### Use of registers

In Vim there are 52 registers available to the programmer which are useful to store the text data. So basically you have 52 clipboards where you can store the copied data. Each register has an identifier which can be one of the following set: `{"a, "b, ..., "z, "A, "B ,..., "Z}`. 

For example, you want to copy a word and store it in the register `"a` then you have to type `"ayw"` which will tell vim to yank a word and store it in register `"a`. Similarly, to paste the contents of register `"a`, type `"ap` or `"aP` to paste as per the requirement.

When you store the copied content to the register for example `"b` then you can view its contents by typing

```vim
:reg b
```

For more details on Vim registers I recommend you to read this great [article](https://www.brianstorti.com/vim-registers/). 

##### Replacing all occurances of a word

Let us say we want to replace all occurances of the word **you** by **they** in the following text:

```vim
If you want something new, you have to stop doing something old.
 -- Peter Drucker
```

We can achieve it by typing the following command in normal mode as:

```vim
:s/you/they/g
```

Here `:s` stands as short form for *find and replace* command while `g` for global. The resulting output will be:

```vim
If they want something new, they have to stop doing something old.
 -- Peter Drucker
```

If you want to make sure whether to replace or not with confirmation from the Vim then you may use `:s/you/they/gc`. Here, `c` will confirm from user whether to replace the word `you` by `they` or not. If you just used `:s/you/they` then only the first occurance of word **you** will be replaced by **they**.

## References:

1. [Learning Vim](linkedin.com/learning/learning-vim) by Miki Tebeka hosted on **LinkedIn Learning**.

2. A [blogpost](https://www.brianstorti.com/vim-registers/) about vim registers by [Brian Storti]([About Brian Storti](https://www.brianstorti.com/about)).

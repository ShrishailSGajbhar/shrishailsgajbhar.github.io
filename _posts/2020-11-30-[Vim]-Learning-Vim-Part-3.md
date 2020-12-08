---
layout: post
title: "[Vim] Learning Vim (Part-3)"
tags: ["Vim","Text Editor"]
comments: true
---
In the previous [blogpost](https://shrishailsgajbhar.github.io/post/Vim-Learning-Vim-Part-2), we learned how to change the text opened in Vim. In this post, we will understand how to make use of the markers which come very handy whenever we need to jump to a particular entry point of a block of text regularly. Markers are also useful for manipulating the large chunks of data such as for the purpose of copy, paste, delete etc. We will also see code folding using markers.

## Use of Markers in Vim

###### Use of markers to go back and forth between the text

Let us say you are writing a an article in markdown where at the start of the file you have declared your table of contents. For example,

```markdown
# TOC

1. Wake Up
1. Exercise
1. Wash
1. Breakfast & Coffee
1. Reading
1. Work
1. R&R
1. Sleep

## Wake Up
```

Now you want to expand the topics in your file. Here, we need to visit our table of contents for copying the topic names while writing. For this purpose use of `markers` is very useful. The marker in vim can be defined for example as `ma`, here marker `a` will be defined for the line where the cursor currently is. Let us assume marker was at line 1 where content is `# TOC`. You can go on editing your current topic for example `Wake Up`. You can do the same by pressing `G` in the normal mode which will take the cursor to end of the file. The press `o` to go in the `insert mode` with cursor moved on the next line.  After you finish this topic, then in order to copy the name of the next topic you can type in the `normal mode` as `'a` which will transfer the cursor location to the place where marker `a` is defined. Then type `3j` to go to the next topic name. i.e., `1.Exercise`. Here, press `yy` to copy the current line then press `G` to go to the end of the file and press `p` to paste it. After that you need to delete everything upto character `E` which can be done using regular expression so type `c` then `/Ex` and press enter to go in insert mode with cursor at `E`. Now put `##` to complete the topic title line. 

You can define 26 unique markers with name from lowercase alphabets of English language i.e., you can choose from `a...z`.

###### Copy and pasting large chunks of data using markers.

Let us assume we want to copy everything from `# TOC` i.e., from the start to end of the file and paste those many line below the end of file. In this case, markers come very handy. Assuming cursor is at the start of the file, we can go to the end of the file by typing `G` in normal mode. Here, we will make a new marker `b` using command `mb`. Then we use `y'b` to yank (copy) everything upto marker `b` and paste it using `p` to paste below the marker `b` position.

###### Deleting large chunks of data using markers.

Now let us say, we want to delete everything upto maker `a` from our current position. To do this we can type `d'a` which will delete everything upto marker `a`.

**To jump between the markers, one can use `Ctrl+i` to move to next marker and `Ctrl+o` to move previous marker.**

Note: Since, Vim editor maintains a list of previous jumps, one can use `Ctrl+i` and `Ctrl+o` to to go back and forth between previous jump history.

**Vim maintains its own marker `'.` which can help us to jump to the line where last change was made.**

> One can use character 'a' in normal mode to switch to the insert mode after the current position of the cursor.

##### How to fold the code using markers?

Vim can fold the large code into single line which can be a boon to the programmer for editing a large file with many functions, classes etc.

Consider the problem that, we want to fold our `void foo()` function to a single line in the following program.

```cpp
#include <iostream>
using namespace std;

void foo(){
     .
     .
     .
     .
     .
     .
     .
 }
 
int main(){
    foo();
    return 0;
  }

```

In order to fold the `foo()` function, we make a marker first at the closing brace of this function as `mb`. Then we move to the begining of the function and press `zf'b` in normal mode to create a code folding. Here, `zf` told vim to fold everything from the current cursor position upto the marker `b` in one line. The output now looks like the following:

![](/assets/images/20201130/pic1.png)

If you want to expand the folded code then type `zo`. To fold again use `zc`.

For more information on markers one can refer these posts ([post1](https://vim.fandom.com/wiki/Using_marks) and [post2](https://www.marksanborn.net/software/using-markers-in-vim/)).

## References:

1. [Learning Vim](https://www.linkedin.com/learning/learning-vim) by Miki Tebeka hosted on **LinkedIn Learning**.

2. [Using Markers in Vim](https://www.marksanborn.net/software/using-markers-in-vim/)

3. [Vim fandom article on using marks](https://vim.fandom.com/wiki/Using_marks)

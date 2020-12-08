---
layout: post
title: "[Linux] Bash Adventure: Automatically write the copied link to the text file"
tags: ["Linux","Bash"]
comments: true
---

Many times we require to download bunch of things, be it videos, images, audio files or pdfs. For that batch download is the easiest way to avoid the painstaking process of downloading each file individually. For batch downloading, if we have a text file containing list of urls we can automate the downloading process by using command line tools such as curl, wget or powerful opensource softwares such as youtube-dl.

But, how can I create a text file of my desired download urls automatically? Well there is a one solution that I came across thanks to  [askubuntu post](https://askubuntu.com/questions/1209313/automatic-transfer-of-clipboard-copied-text-to-some-text-file), that solves our problem.

## Step-1: Create a folder and empty text file where urls will be written automatically.

```bash
mkdir AutoCopy && cd AutoCopy && touch links.txt
```

![](/assets/images/20201024/pic1.png)

## Make sure you have a copy of following shell script in your folder.

Open your favorite text editor copy following code and save that file as "automatic_transfer_copied_text.sh" in folder created in step-1.

```bash
#!/bin/bash

# name: automatic_transfer_copied_text
# author: Ghost Rider 
# source: Glutanimate and Siddharth (https://askubuntu.com/questions/1167026/detect-clipboard-copy-paste-event-and-modify-clipboard-contents)

# Automatically transfers text copied by the mouse to some text file

while ./clipnotify;
do
  SelectedText="$(xsel)"
  CopiedText="$(xsel -b)"
  if [[ $CopiedText == $SelectedText ]]; then
    echo $CopiedText >> "$HOME/AutoCopy/links.txt"
  fi
done
```

However, we have not made the shell script as executable. Make it executable using

```bash
chmod +x automatic_transfer_copied_text.sh
```

![](/assets/images/20201024/pic2.png)

You can see now that the color of file **automatic_transfer_copied_text.sh** has turned green and can be executed like a binary executable. However,wait...! We need to sort out the dependancies.

There are two dependancies:

1. **xsel** which can be installed as:
   
   ```bash
    sudo apt-get install xsel
   ```

2. **clipnotify** 
   
   Clone the repo from: [https://github.com/cdown/clipnotify](https://github.com/cdown/clipnotify) in the directory of your choice.
   
   **Copy the two files clipnotify.c & Makefile from this folder to your directory where your automatic_transfer_copied_test.sh is available.**
   
   Now, you can sort out the option of creating shared library for this by compiling from source as
   
   ```bash
   sudo apt install git build-essential libx11-dev libxtst-dev
   git clone https://github.com/cdown/clipnotify.git
   cd clipnotify
   sudo make
   ```
   
   Wierdly, I got a compilation error for "sudo make" line as "**Command not found cc, make error 127**".
   
   The problem was solved by simply modifying the final line as:
   
   ```bash
   make CC=gcc
   ```

![](/assets/images/20201024/pic3.png)

Now that, we have all our dependancies available. Run the shell script as follows:

```bash
./automatic_transfer_copied_text.sh
```

The script above will start a session of automatic writing onto the specified text file.  Whatever link or text that you copy will be pasted automatically!!. 

Let's assume the urls you copied are some video files, then you can download all of them by using my personal favourite downloader (**youtube-dl**) as

```bash
youtube-dl -f best -a links.txt 
```

Here, the **-a** flag is used to **specify the file containing URLs** to download. It considers '-' for stdin, one URL per line. Lines starting with '#', ';' or ']' are ignored as comments. The **-f** flag is for **video format selection** which is set in above example to the **best** resolution available.                                

Enjoy the bash automation with a beaming face..! 

This post couldn't have been possible but the following two resources:

1. [scripts - Automatic transfer of clipboard copied text to some text file - Ask Ubuntu](https://askubuntu.com/questions/1209313/automatic-transfer-of-clipboard-copied-text-to-some-text-file)

2. [compiling - Command not found cc, make error 127 - Ask Ubuntu](https://askubuntu.com/questions/1095168/command-not-found-cc-make-error-127)

If you need to copy from the PDF without linebreak and paste elsewhere, you may visit

1. [Scripts/PDF-Copy-without-Linebreaks-Linux at master · SidMan2001/Scripts · GitHub](https://github.com/SidMan2001/Scripts/tree/master/PDF-Copy-without-Linebreaks-Linux)

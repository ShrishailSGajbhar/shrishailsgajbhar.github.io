---

layout: post
title: "[Python] Generate Your Own QR Code using Python" 
tags: ["Python"]
comments: true

---
Use of **Quick Response** (QR) codes is widespread nowadays ranging from money transactions to marketing or sharing your personal/professional contact info or url links etc.[1] Currently, most of the mobile phones can scan and understand the QR code patterns. In this blog post, we will understand how easily we can generate QR code using python package `pyqrcode` and save or show that QR code. 

As a first step towards generating the QR code make sure you have `pyqrcode` available in your Python development environment. To install you may use `pip` as `pip install pyqrcode`. In order to store your QR code as a png file we will also install Python `png` package as `pip install png`.

Now we will import the `QRCode` class of `pyqrcode` package as:

```python
from pyqrcode import QRCode
```
Now in this blog,  we consider to generate the QR code for my GitHub blog on which you are reading this post. We can declare the url of the website as:

```python
site = "https://shrishailsgajbhar.github.io/"
```
Then we call the constructor of the `QRCode` class to create the object of that class with our string variable `site` passed as an argument.

```python
qr_code_site = QRCode(site)
```
Finally, we can save the generated QR code as a `png` file using:

```python
qr_code_site.png("my_blog.png", scale=8)
```
The scale argument can be used as above to set the appropriate size for the generated QR code.

If you want to take a look at your QR code then you can also use `show` method of the `QRCode` class as:

```python
qr_code_site.show()
```
Above command will show the generated QR code in the web browser.

The `png` file created using the above program is as follows:
![](/assets/images/20210213/my_blog.png)

Complete program is as follows:

```python
import pyqrcode  # package to generate the QR code.
import png
from pyqrcode import QRCode

site = "https://shrishailsgajbhar.github.io/"

# generate and save the QR Code.

qr_code_site = QRCode(site)

# save the qr code for our site as png file.
qr_code_site.png("my_blog.png", scale=8)

# If we want to see the qr code, uncomment line below.
# qr_code_site.show()
```

References:
1. https://en.wikipedia.org/wiki/QR_code

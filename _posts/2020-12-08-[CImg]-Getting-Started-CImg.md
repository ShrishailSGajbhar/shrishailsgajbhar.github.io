---
layout: post
title: "[CImg] CImg Library for C++ based Image Processing"
tags: ["CImg","image processing"]
comments: true
---
C++ is undoubtedly the choice of the programmers when they need to create memory  efficient and fast applicatons. Most of the popular computer vision libraries such as OpenCV, TensorFlow, YOLO etc. are written in C++, not only those but many popular  browsers, games and softwares are written in C++. Also, softwares involving self-driving cars are mostly written using C++. 

In the context of Image procesing, there are many open-source c++ libraries. One of such library is [CImg](https://cimg.eu/index.html). As per the documentation, it is a **small** and **open-source** **C++ toolkit** for **image processing** written to have following properties:

* Usefulness

* Genericity

* Portability

* Simplicity

* Extensibility

* Freedom

Use of this library just requires to include single header file [CImg.h](https://github.com/dtschump/CImg/raw/master/CImg.h) !!  The library is maintained by [David Tschumperlé](http://tschumperle.users.greyc.fr/) who is also the main contributor for the same. Let us try this Cool Image (CImg) processing library for fun and understanding the concepts.

**As per the documentation, ImageMagick is required to be installed for dealing with various image formats.**

```cpp
// Filename: cimg_1.cpp
// Very basic usage of CImg library
// Thus program reads and displays a color image using CImg

// Starting with include headers and required namespace
#include "CImg.h"
using namespace cimg_library;

// The CImg makes use of only 4 classes:
// 1) cimg_library::CImg
// 2) cimg_library::CImgList
// 3) cimg_library::CImgDisplay
// 4) cimg_library::CImgException

// Out of the above classes for the classes needed for our purpose are CImg
// and CImgDisplay.

int main() {
  // Let's us read the image using CImg class constructor;
  CImg<unsigned char> image("lena.png");
  // To display the image create an object of CImgDisplay class and pass
  // object of image to be displayed and title of the display window.
  CImgDisplay dispWindow(image, "Lena");
  // By default the window is closed after a fraction of second after
  // displaying, so while loop is used to tell the display window to wait until
  // user explicitely closes the same.

  while (!dispWindow.is_closed()) {
    dispWindow.wait();
  }

  return 0;
}
```

##### How to run??

```bash
c++ cimg_1.cpp -o first -pthread -L/usr/local/lib -lX11              
```

Here, `-pthread` is used to make use of the POSIX multithreading library. Other necessary libraries required such **ImageMagick** are available in `-L/usr/local/lib`. Since, we want to display the image, we require to make use of low-level graphical system library for Unix namely `X11`.

Above step produces executable `first` which when executed using `./first` produces following output:

![](/assets/images/20201208/pic1.png)

# References:

1. [The CImg Library Documentation](http://cimg.eu/reference/)

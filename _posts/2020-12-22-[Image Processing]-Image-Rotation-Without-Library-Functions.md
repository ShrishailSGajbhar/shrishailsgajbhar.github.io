---
layout: post
title: "[Image Processing] Naive image rotation without using library functions in Python"
tags: ["Image Processing","Affine Transform","Python"]
comments: true
---

In this blogpost, we will try to implement the naive image rotation function from scratch to understand the concept of *affine* transformations in image processing. This is for understanding purpose, in case of practical applications use of library functions is suggested which are fully optimized functions. One can use 

1. [warpAffine](https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_geometric_transformations/py_geometric_transformations.html) function from OpenCV library

2. [AffineTransform](https://scikit-image.org/docs/stable/api/skimage.transform.html#skimage.transform.AffineTransform) class from scikit-image library

3. [AffineTransform](https://pillow.readthedocs.io/en/1.7.8/pythondoc-PIL.ImageTransform.html) class from PIL library

python library based *affine* transformations.

For our task, we will write a function which takes two arguments, first argument is  `image` which is to be rotated by amount  of `degrees` provided as second argument.

**Note:** The function will rotate the image around its center by the speccified degrees and we assume the size of the original and rotated image to be same.

The code for the image rotation function and testing is as shown below:

```python
# Written by Dr. S. S. Gajbhar
# Filename: transform1.py

import numpy as np
import cv2 
import math

def naive_image_rotate(image, degree):
    '''
    This function rotates the image around its center by amount of degrees
    provided.
    '''
    # First we will convert the degrees into radians
    rads = math.radians(degree)

    # We consider the rotated image to be of the same size as the original
    rot_img = np.uint8(np.zeros(image.shape))

    # Finding the center point of rotated (or original) image.
    height = rot_img.shape[0]
    width  = rot_img.shape[1]

    midx,midy = (width//2, height//2)

    for i in range(rot_img.shape[0]):
        for j in range(rot_img.shape[1]):
            x= (i-midx)*math.cos(rads)+(j-midy)*math.sin(rads)
            y= -(i-midx)*math.sin(rads)+(j-midy)*math.cos(rads)

            x=round(x)+midx 
            y=round(y)+midy 

            if (x>=0 and y>=0 and x<image.shape[0] and  y<image.shape[1]):
                rot_img[i,j,:] = image[x,y,:]

    return rot_img 

def main():
    image = cv2.imread("lena.png")
    rotated_image = naive_image_rotate(image,45)
    cv2.imshow("original image", image)
    cv2.imshow("rotated image",rotated_image)
    cv2.waitKey(0)

if __name__=='__main__':
    main()
```

**Explanation:** First, we import 3 modules/libraries namely `numpy`, `cv2` and `math`. The opencv-python i.e.,`cv2` library is used here only for image reading and displaying purpose.  While implementing the function, as a first step, we will convert the degrees into radians using `math.radians()` function. Then we declared the ouput image to be of the same size as original (or input) image as a `numpy` array with all values as 0.

Since the output and input image sizes are same, they will have same center point. In the next step, the center point of output image is calculated.

Now for correct rotation without any holes we need to fill the intensity values for output image from the new coordinates obtained using following equation:

$\begin{bmatrix} 
x'\\ 
y' 
\end{bmatrix} = \begin{bmatrix}  
\cos(\theta) & \sin(\theta)\\  
-\sin(\theta) & \cos(\theta)  
\end{bmatrix}$. $\begin{bmatrix}  
x\\  
y  
\end{bmatrix}$

where `x'` and `y'` are the new coordinates after rotation by angle $\theta$ in radians. 

Since we want to rotate the image with respect to its center point, we subtract the center point coordinates from each coordinate location so that center point will have (0,0) value before applying the equations to calculate the new coordinate values. The new coordinates are again added with center point coordinates to get the coordinates as in the original image case. Now for each location in rotated image, we find the correspoding intensity values in original image using new coordinates.

In the last step, due to rotation, the new coordinate values may go beyond the size of the original image thus we only consider those pixels which are inside the size of the original image.

To execute the above python code type the following command at the terminal:

```bash
python3 transform1.py
```

The output of the written function `naive_image_rotate()` for input image `lena.png` wih $45$ degrees of rotation is as follows:

![](/assets/images/20201222/pic1.png)

# References:

[1] R. Szeliski. Computer Vision: Algorithms and Applications. 2010.

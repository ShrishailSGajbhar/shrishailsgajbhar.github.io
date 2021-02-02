---

layout: post
title: "[OpenCV] Coin Detection using OpenCV: Basic Project 1" 
tags: ["OpenCV","Image Processing"]
comments: true

---
From this blog post onwards, I will document my journey of image processing/computer vision related projects of all types: basic, intermediate, advanced, research or innovative using open source tools such as OpenCV, scikit-image etc using either Python or C++. 

In this post, we start our journey by following the master Mr. Adrian Rosebrock's awesome book's (Practical Python and OpenCV, 3rd edition) last chapter project "counting coins". 

> About Adrian's Book: I am big fan of Adrian's blog as well as his books. Adrian's book `Practical Python and OpenCV` in my opinion, is the best way to start for any beginner in image processing area using Python. However, familiarity with basic image processing concepts is necessary to understand it more effectively.

> Problem statement: Given an image of non overlapping circular objects such as coins, count the total objects, separate and display these objects.

**Note:I don't take any credit for the solution to this problem but shout out to Adrian Rosebrock who inspires me in many ways.**

Solution: The solution strategy is simple:
1. Read the color image whose path is given as command line argument
1. Convert this image to the grayscale
1. Apply the Gaussian blur to this image
1. Find the Canny edge detected image using the output image of step 3.
1. Use the binary image as input to function `cv2.findContours` to get the contours of the detected objects 
1. Print the total detected objects using the length of the contour related output variable of the `cv2.findContours` function.
1. Draw these contours on the original image and display the same
1. Using masking technique show the separated objects. 

Please find below the code to obtain the same:

```python
import cv2
import argparse
import numpy as np
import matplotlib.pyplot as plt 
# Counting the coins

ap = argparse.ArgumentParser()
ap.add_argument("-i","--image", required=True, help="Path to input image")
args = vars(ap.parse_args())
image = cv2.imread(args['image'])

img_gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Do Gaussian Blurring and Canny Edge Detection
img_blur = cv2.GaussianBlur(img_gray, (7,7), 0)

img_canny = cv2.Canny(img_blur, 10, 250)

# Now let us find the contours
(cnts,_) = cv2.findContours(img_canny.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
#print(type(cnts))
coins = image.copy()
cv2.drawContours(coins,cnts,-1,(0,0,255),2)
cv2.imshow("Original", image)
print("In the image {} countours are found!".format(len(cnts)))
cv2.imshow("Coins detected", coins)
cv2.imshow("Canny Edge Detection", np.hstack((img_gray, img_canny)))

fig, axes = plt.subplots(nrows=3, ncols=3, figsize=(50,50))

# Now let us separate the coins and show them 

for (i,c) in enumerate(cnts):
    x,y,w,h = cv2.boundingRect(c)
    (Cx,Cy), rad = cv2.minEnclosingCircle(c)
    sub_image = image[y:y+w, x:x+h]
    # Since we want to display using matplotlib we convert the BGR2RGB
    sub_image = cv2.cvtColor(sub_image, cv2.COLOR_BGR2RGB)
    mask = np.zeros(image.shape[:2], dtype = "uint8")
    cv2.circle(mask, (int(Cx),int(Cy)), int(rad), 255, -1)
    mask = mask[y:y+w, x:x+h]

    masked_subimage = cv2.bitwise_and(sub_image,sub_image,mask=mask)
    # Create index for the subplots
    axes[i//3, i%3].imshow(masked_subimage)
    axes[i//3, i%3].set_title("Coin Number {}".format(i+1))

plt.show()
cv2.waitKey(0)

```

**Output images:**

**Original image:**
![](/assets/images/20210202/pic1.png)

**Grayscale image and its Canny edge detected version:**
![](/assets/images/20210202/pic2.png)

**In the image 9 countours are found!**

**Detected coins:**
![](/assets/images/20210202/pic3.png)

**Separated Coins:**

![](/assets/images/20210202/pic4.png)

# References
1) Practical Python and OpenCV: An Introductory, Example Driven Guide to Image Processing and Computer Vision, 3rd Edition by Dr. Adrian Rosebrock.

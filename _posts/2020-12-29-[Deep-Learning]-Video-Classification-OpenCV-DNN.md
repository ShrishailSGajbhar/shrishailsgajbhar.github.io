---
layout: post
title: "[Deep Learning] Video Classification using OpenCV's dnn Module"
tags: ["Deep Learning","Video Classification","OpenCV","Python"]
comments: true
---
# Video classification using OpenCV dnn module

In the previous [blog post](https://shrishailsgajbhar.github.io/post/Deep-Learning-Image-Classification-Opencv-DNN), we understood how can we use OpenCV dnn module as an inference engine with application to image classification. In this post, we will see how can we use OpenCV dnn module for the video classification purpose. 

## Step-1: loading the video

As part of the video classification, we will first see how we can use OpenCV to load video either from the file or directly from the webcam.

The code for loading the video is as follows and is self-explanatory:

```python
# Written by: Dr. S. S. Gajbhar
# Filename: vidclassify.py
import cv2
import numpy
import argparse

# We will use argparse module to load video either from the file or from the webcam

# Create the object of the ArgumentParser class 
ap = argparse.ArgumentParser()

# Add the argument
ap.add_argument('-v','--video', required = False, help = 'Path to the video file/feed')

# Create a dictionary of arguments
args = vars(ap.parse_args())

videopath = args['video']

# if videopath is None then load from the webcam.
if videopath == None:
    videopath=0
#@ Let us load the video 

# First we need to create the object of VideoCapture class
cap = cv2.VideoCapture(videopath)
# Check the video initialization is proper or not
status = cap.isOpened()
if status==False:
    print("Error while reading the video..!")
# If everything is fine then go for frame by frame loading
while(True):
    # Read the video frame by frame
    retVal, frame = cap.read()

    # if retVal of above function call is true then show the frame
    if(retVal):
        cv2.imshow("Video",frame)

        # In order to control the video display
        if(cv2.waitKey(25) and 0xFF==27):
            break
    else:
        break
cap.release()
cv2.destroyAllWindows()
```

#### ## Step-2: Modify above code for the classification task

The above code can be easily modified for the video classification as we are reading the video frame by frame. Here each frame can be considered as an image and an image classification steps as seen in the previous [blog post](https://shrishailsgajbhar.github.io/post/Deep-Learning-Image-Classification-Opencv) can be applied. The only difference we are going to make here is that instead of observing the class probabilities on the terminal window, we will put those on the frame itself. The complete code for video classification is as follows:

```python
# Written by: Dr. S. S. Gajbhar
# Filename: vidclassify.py
import cv2
import numpy as np 
import argparse

# We will use argparse module to load video either from the file or from the webcam

# Create the object of the ArgumentParser class 
ap = argparse.ArgumentParser()

# Add the argument
ap.add_argument('-v','--video', required = False, help = 'Path to the video filefeed')

# Create a dictionary of arguments
args = vars(ap.parse_args())

videopath = args['video']

if videopath == None:
    videopath=0

# --------- code for the inference part ---------
# Read the synset_words.txt file
all_rows = open('./model/synset_words.txt').read().strip().split('\n')
# Using list comprehension to drop ids
classes = [r[r.find(' ')+1:] for r in all_rows]

net = cv2.dnn.readNetFromCaffe('./model/bvlc_googlenet.prototxt','./model/bvlc_googlenet.caffemodel')
#-----------------------------------------------
#@ Let us load the video 

# First we need to create the object of VideoCapture class
cap = cv2.VideoCapture(videopath)
# Check the video initialization is proper or not
status = cap.isOpened()
if status==False:
    print("Error while reading the video..!")
# If everything is fine then go for frame by frame loading
while(True):
    # Read the video frame by frame
    retVal, frame = cap.read()

    #------- Code for inference part ----------
    blob = cv2.dnn.blobFromImage(frame, 1, (224,224))
    net.setInput(blob)
    out = net.forward()
    # Let us grab the top5 probability indices
    idx_asc = out.argsort()
    idx_desc = np.fliplr(idx_asc)
    idx_top5 = idx_desc[0,:5]
    
    r=1
    # Now for each of the index create a text and put on displayed frame
    for id in idx_top5:
        text = "{}: probability {:.3} %".format(classes[id], out[0,id]*100)
        cv2.putText(frame,text,(0,25+40*r), cv2.FONT_HERSHEY_SIMPLEX,1,(255,0,0),2)
        r+=1
    #------------------------------------------

    # if retVal of above function call is true then show the frame
    if(retVal):
        cv2.imshow("Video",frame)
        
        # In order to control the video display
        if(cv2.waitKey(25) and 0xFF==27):
            break
    else:
        break
cap.release()
cv2.destroyAllWindows()

```

#### Example code to run the above program with option of loading the video from the file

```bash
python vidclassify.py --image ../images/shore.mov
```

#### Example code to run the above program with option of loading the video from the live feed from webcam

```bash
python vidclassify.py
```

## Output:

Following demo video shows output of the above program:

<iframe width="560" height="315" src="https://www.youtube.com/embed/jRBqOXwLS7k" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Reference:

* https://www.lynda.com/OpenCV-tutorials/Introduction-Deep-Learning-OpenCV/5034175-2.html 

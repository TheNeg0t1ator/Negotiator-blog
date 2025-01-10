---
title : "OpenCV Video Wall"
date : 2024-11-13
description : "OpenCV Video Wall"

summary : "OpenCV Video Wall"
---
# A video wall made of monitors
This project is built in opencv, using raspberry pi 4's. The pi's are clients that get a video stream, and display it on their monitor.
the monitors are `1280x1024` resolution. The main video stream is started and captured on a server.
> in this case, an old 2009-2012 pc, running ubuntu.

   on this server, the video pipeline is implemented like this:
{{< mermaid >}}
flowchart LR
    VideoIN(Video input)
    VideoFIT(Fit to full</br>Resolution)
    VideoSPLIT(Split</br>into screens)
    VideoCOMPRESS(Compress)
    VideoTX(Stream frames </br>to pi)
    VideoSHOW(receive, decompress</br>and show)
    
    VideoIN-->VideoFIT
    VideoFIT-->VideoSPLIT
    VideoSPLIT-->VideoCOMPRESS
    VideoCOMPRESS-->VideoTX
    VideoTX-->VideoSHOW

{{< /mermaid >}}

So we will run through these steps one at a time now.

## Video input

The video input is currently being handled by opencv, because of the amount of input types. This means videostreams, like http, rtsp, mp4, webcams, etc.. 
it looks like this:
```python
import cv2
cap = cv2.VideoCapture(0, cv2.CAP_DSHOW)
```


## Video fitting

now the video needs to be fit to the max size it can be on the displays. We can do this by checking if it is wider than the aspect ratio or not, like this:
```python
if aspect_ratio > target_width / target_height:
    # pad top and bottom
else:
    # pad right and left
```
Then we call a simple `cv2.resize()`, and a `cv2.copyMakeBorder()`.
## Video Splitting

The video needs to be split into the multiple frames to then send as a whole. So we will look at the splitting of the image now.
first, the wall is 4 displays wide, and 3 displays high.

## Video Compression
Currently video compression is handled by scaling down the frame, as the project is not finished yet, and is easy for a POC.

## Video Streaming
The streaming of the video is currently handled by an http stream, altough rtsp is in the works. this is because http is a tcp stream, and really slows down the system.

## Video displaying 
video displaying is handled by mpv, and really works well, no updates needed at this point in time, altough synchronization might be a problem.

## //TODO

So, it's not all rainbows and sunshine, there are bugs, and things that are not working yet:

- Synchronous operation, every pi is working seperately from the other, and do not sync yet with the server.
- capture problem. due to threading, and them not being in sync, more than one class is accessing the same cap class, wich is corrupting the class and making the server crash.
- rtsp is not working. 
- We're working on dockers for both the raspberry pi and the server, as to easily test and be able to run it on all the pi's.


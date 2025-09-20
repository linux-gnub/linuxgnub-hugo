---
title: How to use a my Canon M50 in Linux
date: 2024-11-06
draft: false
tags:
  - Linux
  - Video
---

Linux is and has been my daily driver for a long time now, but I've only just started using a fancy mirror-less  camera for photos and videos. I snagged a pretty good deal on a Canon M50 off eBay and I've really enjoyed playing around with it, but I'd like to use it for Discord to chat with friends or OBS to make videos for my Linux/Gnub YouTube channel. However, Linux doesn't get the same software or driver efforts as Windows or MacOS does from camera manufactures, so there's no plug and play support for us Linux users. I have a capture device coming in soon to solve this issue, but a couple of weeks back I was getting ready to hop on a call with some friends and wanted to see if I could use the M50. After some googling and digging through reddit, I found a few posts that said it could be done by using gphoto2 and ffmpeg along with v4l2loopback. There is a great video tutorial from Novaspirit Tech, AKA Don Hui, that laid out the steps needed to get setup. I managed to get it working and had a great conversation with my friends! Thanks to this little trick and the hard work of the developers I was able to get a great video feed from my camera!

Now there are some limitations that I'll explain later, but for now lets go through the install, setup and usage so I won't forget for next time I want to capture video this way.

# installation and usage

To install the two libraries run:

```
sudo dnf install gphoto2 v4l2loopback
```

 Then load the kernel module:

```
sudo modprobe v4l2loopback exclusive_caps=1 max_buffer=2 
```

Now, plug in the USB cable from the camera into the computer, then turn on the camera. To check if the machine is able to detect the camera, run:

```
gphoto2 --auto-detect
```

To find out what video source to point ffmpeg to I can run:

```
v4l2-ctl --list-devices

```

I can now use ffmpeg to redirect it to the virtual camera device.

```
gphoto2 --stdout --capture-movie | ffmpeg -i - -vcodec rawvideo -pix_fmt yuv420p  -f v4l2 /dev/video1
```

Voala!

Now I can use my camera to capture video using OBS or as a video source for Zoom and Discord instead of my dinky webcam. This should be a much better picture quality, even if it is at a lower resolution. To see if your camera is compatible be sure to check the gphoto compatibility list in the link below.

References: gPhoto2 http://www.gphoto.org/ Novaspirit Tech https://youtu.be/TsuY4o2zLVQ (Rest in peace)
---
author: "Koushik"
date: 2022-02-08
title: MIPI Driver for sync stereo camera HAT
weight: 10
readingtime: true
tags: ["Stereo Vision", "Computer Vision", "Arducam Stereo HAT"]
categories: ["Stereo Camera"]
---

### Prerequisites

Make sure that OpenCV is installed properly. This MIPI driver is specific to only RPi OS and [will not work on any ARM based Ubuntu OS](https://forum.arducam.com/t/rpi-3b-ubuntu-18-04-stereo-hat-8mp/1265/3). If you are running bullseye, enable the legacy camera support from `raspi-config` and add `start_x=1` to `/boot/config.txt`. Then we are good to go.

#### Possible errors

- `libmmal_core.so.0 not found` after running `make clean && make`. This library can be accessed only after enabling the [legacy camera support](https://picamera.readthedocs.io/en/release-1.13/api_mmalobj.html) as it depends on the library.

Once the driver is installed, test it out by running `1_test.py`. Possible errors could be the orientation error and green tint error which was [already discussed here](https://vkoushik.netlify.app/posts/fixing-green-tint-and-orientation/)

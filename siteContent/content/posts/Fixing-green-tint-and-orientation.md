 ---
author: "Koushik"
date: 2022-02-04
#linktitle: MacOS on Hackintosh PC
title: Fixing green tint and orientation
weight: 10
comments: true
readingtime: true
tags: ["Stereo Vision", "Computer Vision", "Arducam Stereo HAT"]
categories: ["Stereo Camera"]
---

Stereo cameras once connected to the _hat_, have to be calibrated before using it. There were a couple of issues that I encountered while calibrating. First one being the green tint on both sensors, IMX 219, and the second one being the incorrect orientation. However there was an error while running the calibration code, {{< notice error >}}mmal: Failed to fix lens shading, use the default mode!{{< /notice >}} but the code ran just fine. There were several modes to choose from, I went with `mode 7` which gave the resolution of 1280 x 480.

I had my camera setup exactly how it is shown [here](https://forum.arducam.com/t/several-problems-with-depth-mapping-need-to-fix-asap/1371/8).
To fix the orientation, just appended the lines after `fmt = camera.get_format()` in the main function.
```
camera.set_control(0x00980914,1)
camera.set_control(0x00980915,1)
```
This seemed to solve the problem although this was the solution for a [different case](https://forum.arducam.com/t/python3-6-dm-video-py-is-upside-down/1356), that is if the cameras were reversed in the first place, which was not in my case.

The green tint was solved by setting the AWB enable from the API.
```
camera.software_auto_white_balance(enable = True)
```
However, there is a pink tint on the IR camera which seems to be a common issue I'm facing right now. Hopefully that will also get resolved soon.
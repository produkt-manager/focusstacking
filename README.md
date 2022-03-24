# Focusstacking on Xilinx Kria KV 260

This implements a simple focus stacking algorithm for use with Xilinx Kria SOM.

## Why Focus Stacking

Photo or video cameras capture images that are 2-dimensional representations of a 3-dimensional world. In a conventional camera, light rays from the real 3D-scene pass an aperture before they hit the 2D-sensor/ film. At a given shutter speed and ISO-setting, the aperture defines how much light enters the camera, and how large the depth of field of the 2D-photo/frame can finally be. 

For certain imaging applications, a 2 dimensional representation is needed that includes a larger depth of field as it can physically be captured by a given combination of aperture and shutter speed. Examples are Macro Photos, Microscopy, and more. For such a situations a solution is needed. 

In order to create photos that are sharp in every picture plane the photographer normally closes the aperture (high f-stop). However in a lens camera very small apertures lead to light bending and therefore to an overall degradation of sharpness. Therefore this approach is technically limited.

In order to capture photos beyond this technical limits it is also possible to capture different frames of the same object in which the focus settings vary varied. Such images are then computationally combined into one image (focus stack).

## Algorithm

Different algorithms for stacking photos exists. In each case the objective is to create a stack of photos of several individual frames, while only the most sharpest areas of each frame are copied into the final image. This program uses the following logic in order to achieve this:

* Alignment: Camera movements in-between shots, or the change of the focus plane itself, which leads to zooming, requires that the frames are first aligned from one to the other.
* Blurring: In each frame the sharpest areas are the most relevant for the final stack. In order to find them the light density of the pixels are relevant. Therefore the Gausian blur is applied to each photo
* Gradients: The darker the pixels the sharper the particular area of a blurred photo is. In order to find the gradients a gradient map is created, vy means of calculating the laplacian of the blurred photo.
* Creation of the stacked photo: In order to create the final photo, each pixel [x,y] in the output image consists of the pixel [x,y] from all input images that consits of the largest gradient [x,y].

## Concept and Test

The following notebook uses the Wolfram Language, and the documentation of the Wolfram Language in order to test the focus stacking algorithms. This lays the foundation for a concept https://github.com/produkt-manager/focusstacking/blob/main/Focus_Staking%20Test_Contest.pdf. 

As you see in this notebook, the following process is able to produce a sharp stacked image out of a list of images that differ in focus plane.

* Take a focus stack that includes several images with variations in the focus plane.
* Align each image in the stack to one reference image within the stack.
* Calculate which parts of each image are in-focus and assign per-pixel weights that represent these parts.  
* Calculate the image average of these areas and combine these parts info one image.

To identify in-focus pixels in an image, the local sharpness is calculated using the Laplacian operator. The size of the operator directly indicates the sharpness of the pixels (the higher, the sharper). Gaussian blur is then added to make sure that nearby pixels have similar weights. Then each image and pixel the Laplacian is multiplied with the Gaussian blur. The result is added, and finally divided by the sum of weights. 

Depending on the quality of the inbound images, the resulting image may include image errors, and therefore the process requires postprocessing steps. 

# Installation

The XILINX KRIA KV260 is a powerful board and it is able to handle multiple intense image streams in parallel. This beginner project elaborates the range of possibilities, and test one of the so-called focus-stacking techniques using the XILINX KRIA KV260.

## Installation of Xilinx Kria KV260

The XILINX KRIA KV 260 board and accessory pack includes the complete hardware that is needed. In addition you need a host computer in order to recreate this project.

You can provide the inbound photo stream as by the following channels: 

* USB Webcam
* Image Sensor (MIPI Interface)
* SD card 

To keep things simple and focussed, this project works with example images stored on the SD card.

## Installation of Xilinx Toolchain

This section targets developers that start from scratch on MAC OSX.

* Getting started: After you received your development kit, you probably want to get your hands around it, by means of a hello world example. Xilinx offers the following starter guide and introduction video https://www.xilinx.com/video/som/out-of-the-box-with-kria-kv260-up-and-running-in-under-an-hour.html. Run it from start to end. The starter guide requires that you have a separate monitor, mouse and keyboard. In this documentation describes how you can configure and use the VLC player instead of a separate monitor in order to present the hello-world-video stream https://www.hackster.io/whitney-knitter/getting-started-with-the-kria-kv260-ai-vision-kit-c6d6af

* Toolchain: In order to develop Xilinx Kria Apps, Xilinx offers you several tools. To install and use them you either need a system on Microsoft OS or on Linux. On Apple computers (OSX) you would typically  run these tools in a Linux or Windows System that runs within a virtualized box. Some virtualization options are Oracle's Virtual Box (https://www.virtualbox.org), or the open source QEMU (https://www.qemu.org/download/). Install your virtualization framework of choice, and create an empty image. Be aware that Xilinx Tools demand a high amount of disk space and compute power (see next section).

* Start the image, and install a clean target OS (for my project, I use Ubuntu 18.04). The following guide leads you through the process to install the complete Xilinx tool-chain on this clean system: https://www.hackster.io/whitney-knitter/installing-vivado-vitis-petalinux-2021-2-on-ubuntu-18-04-0d0fdf (Remark: As by the author, the resource requirements are "I've found you should have at least 300GB of free space available to install all of the Xilinx tools and 32GB - 64GB of RAM with at least 8 CPU cores you can dedicate to them").

* Develop a Kria App: After your system is set up and running, you can start to develop your project. The following Guide leads you thru the process to develop a Kria App that makes use of the open source GStreamer framework. This framework is often used in video projects https://community.element14.com/technologies/fpga-group/b/blog/posts/kv260-vvas-sms-2021-1-part7. I recommend you to recreate the example within your own hardware.

## Installation of Python Environment

A different, less demanding option, is to install a Python environment within the official Ubuntu SD Card image for the XILINX KRIA KV260 SOM. This extension is quite new. As by their GitHub, "PYNQ is an open-source project from Xilinx that makes it easy to design embedded systems with Zynq All Programmable Systems on Chips (APSoCs)." 

* This GitHub repository describes the installation: https://github.com/Xilinx/Kria-PYNQ.
* This post documents the installation steps https://discuss.pynq.io/t/pynq-now-available-for-the-kria-kv260-vision-ai-starter-kit/3579.

After installation update your installation using the commands "sudo apt-get update",  "sudo apt-get upgrade", and "sudo reboot". After the system is up and running, start the environment using this weblink inside your own network  http://kria:9090, and log In to your system.

You can install the extensions of your needs by using the corresponding area in JupyterLab. In particular implement the assignment to Github using the jupyterlab/jupyterlab-github extension. This extension allows you to open GitHub repositories directly from your XILINX KRIA KV260 board.

The JupyterLab installation for the XILINX KRIA KV260 SOM includes several examples that show you the different options to program and work with the board. This is for example code for a webcam that is able to detect faces using openCV http://kria:9090/lab/workspaces/auto-y/tree/kv260/video/opencv_face_detect_webcam.ipynb. Explore all other options.

# Focus stacking in Python on Ubuntu

After you have installed and tested the standard Xilinx Kria KV260 and PYNQ you can upload the Jupyter Notebook for focus stacking. Use the standard upload features in the Jupyter Lab in order to load it to the board. If you have installed it you can also use the GitHub link from Jupyter lab to my GitHub.

The Jupyter Notebook is self-explanatory. Using example images, it shows you step by step, how to combine these images into one image. 

## Related Information

Find here a list of helpful imaging resources on the internet:

* Extreme Macro http://extreme-macro.co.uk/
* Photographylife - Article about macro photography https://photographylife.com/macro-photography-tutorial
* Blur detection with openCV https://www.pyimagesearch.com/2015/09/07/blur-detection-with-opencv/
* How to focus stack: step by step https://digital-photography-school.com/a-beginners-guide-to-focus-stacking/ 

Disclaimer: I do not take responsibility for the contentof linked websites

Copyright 2022 Andreas Rudolph, and is released under the Apache 2.0 license (see license file).


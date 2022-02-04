# focusstacking
Focusstacking on Xilinx Kria KV 260

This implements a simple focus stacking algorithm for use with Xilinx Kria SOM.

## Focus Stacking

Modern photo or video cameras capture frames or photos on a camera sensor. These frames or photos represent a 2-dimensional representation of a 3-dimensional world. In a conventional optical system, light rays from the real 3D-scene pass an aperture before they hit the 2D-sensor/ film. At a given shutter speed, the aperture defines how much light enters the camera, and how large the depth of field of the 2D-photo/frame can finally be. As all changeable parameters experience limitations by physics the photographic result is somewhat a compromise.

In many photos or video frames it is fine to present important parts of the scene in focus, while areas in front or behind the object of interest are blurred. However for certain applications, a 2 dimensional representation is needed that includes a larger depth of field as it can physically be captured by a given combination of aperture and shutter speed. Examples are Macro Photos, Microscopy, and more.

In order to create photos that are sharp in every picture plane the photographer normally closes the aperture (high f-stop). However in a lens camera very small apertures lead to light bending and therefore to an overall degradation of sharpness. Therefore this approach is technically limited.

In order to capture photos beyond this technical limits that are sharp in every focus plane, photograpers typically capture different frames of the same object in which vary the focus settings are varied. These photos with varying focus planes are then computationally combined into one photo (focus stack).

## Algorithm

Different algorithms for stacking photos exists. In each case the objective is to create a stack of photos of several individual frames, while only the most sharpest areas of each frame are copied into the final photo. This program uses the following logic in order to achieve this:

Alignment: Camera movements in-between shots, or the change of the focus plane itself, which leads to zooming, requires that the frames are first aligned from one to the other.
Blurring: In each frame the sharpest areas are the most relevant for the final stack. In order to find them the light density of the pixels are relevant. Therefore the Gausian blur is applied to each photo
Gradients: The darker the pixels the sharper the particular area of a blurred photo is. In order to find the gradients a gradient map is created, vy means of calculating the laplacian of the blurred photo.
Creation of the stacked photo: In order to create the final photo, each pixel [x,y] in the output image consists of the pixel [x,y] from all input images that consits of the largest gradient [x,y].
Copyright 2022 Andreas Rudolph, and is released under the Apache 2.0 license (see license file).


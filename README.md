# Meteoros

*polished version with more feaures to be released over winter break (before 2nd week of Jan)*

## Overview

This project is a real-time cloudscape renderer in Vulkan that was made as the final project for the University of Pennsylvania course, CIS 565: GPU Programming and Architecture. It is based on the cloud system NUBIS that was implemented for the Decima Engine by Guerrilla Games. The clouds were made for the game 'Horizon Zero Dawn' and were described in the following SIGGRAPH 2015 and 2017 presentations: 

* 2015 [The Real-time Volumetric Cloudscapes of Horizon Zero Dawn](https://www.guerrilla-games.com/read/the-real-time-volumetric-cloudscapes-of-horizon-zero-dawn)

* 2017 [Nubis: Authoring Realtime Volumetric Cloudscapes with the Decima Engine](https://www.guerrilla-games.com/read/nubis-authoring-real-time-volumetric-cloudscapes-with-the-decima-engine)

Contributors:
1. Aman Sachan - M.S.E. Computer Graphics and Game Technology, UPenn
2. Meghana Seshadri - M.S.E. Computer Graphics and Game Technology, UPenn

Skip Forward to:
1. [Instructions](#Instructions)
2. [Features](#Features)
	- [Current](#Current)
	- [Upcoming](#Upcoming)
3. [Pipeline Overview](#Pipeline)
4. [Implementation Overview](#Implementation)
	- [Ray-Marching](#Raymarching)
	- [Modelling](#Modeling)
	- [Lighting](#Lighting)
	- [Rendering](#Rendering)
	- [Post-Processing](#Post)
5. [Optimizations](#Optimizations)
5. [Performance Analysis](#Performance)
6. [Resources](#Resources)
7. [Notes](#Notes)
8. [Bloopers](#Bloopers)

## Instructions

If you wish to run or develop on top of this program, please refer to the [INSTRUCTION.md](https://github.com/Aman-Sachan-asach/Meteoros/blob/master/INSTRUCTION.md) file.

## Features

### Current

### Upcoming
- Fully functional reprojection optimization 

### Pipeline Overview

#### Vulkan

Describe the Vulkan graphics and compute pipeline set up here.

#### Graphics Pipeline
![](/images/SimplifiedPipeline.png)

## Implementation Overview <a name="Implementation"></a>

### Ray-Marching <a name="Raymarching"></a>

### Modeling

Read in 3D textures for low and high frequency noise. The low frequency noise forms the base shape of the clouds, and the high frequency noise erodes the edges of the clouds to form them into finer shapes.

Low frequency texture = perlin-worley + 3 worley's

Perlin-Worley
![](/images/perlinworleyNoise.png)

Worley 1
![](/images/worleyNoiseLayer1.png)

Worley 2
![](/images/worleyNoiseLayer2.png)

Worley 3
![](/images/worleyNoiseLayer3.png)


High frequency = 3 worley's at higher frequencies
![](/images/highFrequencyDetail.png)


Curl Noise is used to simulate wind and other motion effects on the clouds. Sample along with the high frequency clouds.

![](/images/curlNoise.png)


![](/images/cloudmodelling.png)
![](/images/erodeclouds.png)
![](/images/modellingClouds.png)
![](/images/modellingClouds1.png)



### Lighting

The lighting model consists of 3 different probabilities:

* Beer-Lambert
* Henyey-Greenstein
* In-scattering

![](/images/FinalLightingModel.png)


1. Beers Law

![](/images/beerslaw.png)


2. Henyey-Greenstein

![](/images/beerspowderlaw.png)


3. In-Scattering



### Rendering 


### Post-Processing <a name="Post"></a>

## Optimizations


![](/images/sampleoptimisation.png)

## Performance Analysis <a name="Performance"></a>

Performance analysis conducted on: Windows 10, i7-7700HQ @ 2.8GHz 32GB, GTX 1070(laptop GPU) 8074MB (Personal Machine: Customized MSI GT62VR 7RE)


## Resources

#### Texture Resources:
- [Low and High Frequency Noise Textures](https://www.guerrilla-games.com/read/nubis-authoring-real-time-volumetric-cloudscapes-with-the-decima-engine) were made using the 'Nubis Noise Generator' houdini tool that was released along with the 2015 paper. 
- [Curl Noise Textures](http://bitsquid.blogspot.com/2016/07/volumetric-clouds.html)
- Weather Map Texture made by Dan Mccan

#### Libraries:
- [Image Loading Library](https://github.com/nothings/stb)
- [Obj Loading Library](https://github.com/syoyo/tinyobjloader)
- [Why to include stb in .cpp file](https://stackoverflow.com/questions/43348798/double-inclusion-and-headers-only-library-stbi-image)
- [Imgui](https://github.com/ocornut/imgui) for our partially wriiten gui

#### Vulkan
- [Vulkan Tutorial](https://vulkan-tutorial.com/)
- [RenderDoc](https://renderdoc.org/)
- [Setting Up Compute Shader that writes to a texture](https://github.com/SaschaWillems/Vulkan/tree/master/examples/raytracing)
- [3D Textures](https://github.com/SaschaWillems/Vulkan/tree/master/examples/texture3d)
- [Pipeline Caching](https://github.com/SaschaWillems/Vulkan/tree/master/examples/radialblur) was used for post-processing and so it made more sense to see how it is done for post processing
- [Radial Blur](https://github.com/SaschaWillems/Vulkan/tree/master/examples/radialblur)

#### Post-Processing:
- [Uncharted 2 Tone Mapping](http://filmicworlds.com/blog/filmic-tonemapping-operators/)
- [God Rays](https://developer.nvidia.com/gpugems/GPUGems3/gpugems3_ch13.html)

#### Upcoming Feature Set:
- [Off-screen Rendering](https://github.com/SaschaWillems/Vulkan/tree/master/examples/offscreen)
- [Off-screen Rendering](https://github.com/SaschaWillems/Vulkan/tree/master/examples/pushconstants)

#### Other Resources
- FBM Procedural Noise Joe Klinger
- Preetham Sun/Sky model from Project Marshmallow 

## Notes
- We did not add checks (which is highly recommended when developing Vulkan code for other users) to make sure some features are supported by the GPU before using them, such as anisotropic filtering and the image formats that the GPU supports.

## Bloopers

* Tone Mapping Madness

![](/images/READMEImages/meg01.gif)

* Sobel's "edgy" clouds

![](/images/READMEImages/meg02.gif)
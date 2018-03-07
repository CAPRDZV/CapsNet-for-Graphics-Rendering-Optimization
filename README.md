# CapsNet-for-Graphics-Rendering-Optimization

What I'm actually doing:

- Training neural networks on graphical simulations, primarily fluid and lighting physics (since I have lots of references). Writing my own simple C++ API now with OpenCV since my alternatives aren't working. This will be far easier to think with and integrate with Vulkan. Using Tensorflow (and Python in general) and Julia for reference.
- Replacing traditional "linearly iterative" compute models with those trained models to run through GLSL code. The trained models can probably be loaded like any other file, then the weight matrices used for computation.
- Testing different encoder and decoder models for quality, speed, and usability.
- Integrating that with Vulkan in C++. I have a [working](https://github.com/moothyknight/Vulkan-Compute-Example) engine for Windows and Linux (mostly borrowed) with SPIR-V number generators for cross-platform. The engine originally supported Android and I think Mac, so if I prove this works I will restore that functionality.
- End up with an intuitive process for others to use and learn from. This field's advancing fast, and the available tools need to keep evolving so it's not so specialized. This math is too powerful!

Long form:

I'm putting this out now looking for collaborators, since my skill with machine learning code is very limited and this will take me eons otherwise as I am in the process of learning. I am building in [Vulkan](https://github.com/moothyknight/Vulkan-Compute-Example) and OpenCV, then maybe [JuliaGPU](https://github.com/JuliaGPU).

Basically, [Capsule Networks](https://medium.com/ai%C2%B3-theory-practice-business/understanding-hintons-capsule-networks-part-i-intuition-b4b559d1159b) are neural networks that are better generalized for encoding matrix operations, whereas the traditional kind work in scalars. 

![2](https://github.com/moothyknight/CapsNet-Tensorflow/raw/master/imgs/capsuleVSneuron.png)
(from: https://github.com/naturomics/CapsNet-Tensorflow)

This means more complex 3D features and transforms should be able to be encoded, then decoded to a reference map as rendered features on the screen. This could be used with [lighting](https://blogs.nvidia.com/blog/2017/05/10/ai-for-ray-tracing/) and [physics](https://cims.nyu.edu/~schlacht/CNNFluids.htm), and many more things. I'm essentially trying to blend all of these ideas into a neural network-based renderer, where you give it your reference map, like a grid where you mark out the feature types and coordinates, plus constraints, and a neural net will then render those features from its learned sets. In practical terms, the neural net would be the compute shader (just like cuDNN's appropriation of CUDA computes, or the upcoming TensorRT3 for Volta tensor cores), and write to the vertex/pixel shader to be shown on screen. I don't know yet if there's an easier way that doesn't introduce overhead.

The issues obviously are speed and data size. Well newer decoders, like this [Multiscale Entanglement Renormalization Ansatz (MERA)](https://arxiv.org/pdf/1711.03357.pdf) tensor network method are a viable replacement for fully-connected layers (traditionally used to decode convolutions), using thousands of times fewer parameters with similar accuracy and thus far fewer computation resources. Encoders can be [bottlenecked](https://arxiv.org/abs/1705.08086) to extract the the best feature maps (or [symmetries](https://arxiv.org/pdf/1512.06293.pdf)) at the data limit you need, governed by the amount of [information](https://courses.cs.washington.edu/courses/cse528/13sp/lecture-slides/Lecture6.pdf) you need out of it (or resolution in this case). Encoders can also be optimized around the idea that better models will have less loss at lower resource costs, revolving around the fact that correllations approach maximum entropy (or maximal information) to create models - the reason a multiscale entanglement decoder (where a system in a state of maximum entropy is said to be entangled) can be applied to a trained dataset, with the embedded models existing as 'crystallized' correlation networks you can perform faster transforms with than by traditional rendering, though generally at an imperfect precision. Wild! And with so many implications!

I'm attempting to figure out how to encode one graphical feature at a time into a capsule network, then ["style transfer"](https://github.com/moothyknight/UniversalStyleTransfer) those onto the screen in reference to a map + game camera (i.e. the viewing frustum). It'll have to be quite modular through shaders and different datasets, so various features can be toggled and modified. This will start as a normal engine with a few features being generated from a trained net, like particle physics and lighting, since a lot of that has been done already (just not with CapsNet) and can be referenced. What would be really trippy would be handing over control of the reference map and camera in a deep-dream-like recurrent or adversarial way. 

I would love help! I shall dub this the InsanityNet if it works. I need help tweaking a capsnet for different datasets, as well as bottlenecking it to conserve resources. I'm covering the graphics side. This will be open-source for the love of the internet, with open collaboration for the love of my global peeps out there (also my skill limit).
Reach me to with suggestions or to help at: brewster.joshua1@gmail.com

I'm a computer science and psychology student. I've been studying this for a few years now, it's only in 2017 that some of the tools are becoming available to make something like this idea possible. Now I just need a Titan V to get decent framerate... [X_x]( https://i.pinimg.com/736x/f2/fe/7a/f2fe7abeae7cafb5c622b447c4fa294a--high-meme.jpg)

Rough plans

- Reproduce several of the studies highlighted here in C++ based on other examples: the neural-net generated [ray-tracing](https://blogs.nvidia.com/blog/2017/05/10/ai-for-ray-tracing/) (or a more performance-friendly model like PBR or VXGI) and [Eulerian](https://cims.nyu.edu/~schlacht/CNNFluids.htm) (how about [Schrodinger?](https://www.youtube.com/watch?v=5C9BLAXCe1I)) smoke simulations, and [style transfer](https://github.com/moothyknight/UniversalStyleTransfer) algorithm. 
- The style transfer work will be a good place to try to re-create the [MERA]((https://arxiv.org/pdf/1711.03357.pdf)) optimization as it uses the VGG19 net with fully connected layers. It's a perfect example of using correlation matrices with neural networks, thus my use of it here.
- Create CapsNet encoders for these problems and compare performance. Examples: https://github.com/loretoparisi/CapsNet
- Use the understanding gained there to combine the studies and integrate with Vulkan. The cuDNN neural net will be integrated with the compute shader via SPIRV using my engine's SPIR-V number generator, unless there is a simpler way that doesn't introduce overhead. I have a working fully-featured engine for windows and linux [here](https://github.com/moothyknight/Vulkan-Compute-Example) featuring an N-body simulation. I put it together from many examples.
- Photogrammetry? Terrain generation?

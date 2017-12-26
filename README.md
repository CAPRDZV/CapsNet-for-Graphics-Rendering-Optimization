# CapsNet-for-Graphics-Rendering-Optimization

I'm putting this out now looking for collaborators, since my skill with Tensorflow and Keras is very limited and this will take me eons otherwise.

Basically, [Capsule Networks](https://hackernoon.com/what-is-a-capsnet-or-capsule-network-2bfbe48769cc) are neural networks that are better generalized for matrix operations, whereas the traditional kind work in scalars. This means more complex 3D features and transforms should be able to be encoded, then decoded to a reference map as rendered features on the screen. This could be used with [lighting](https://blogs.nvidia.com/blog/2017/05/10/ai-for-ray-tracing/) and [physics](https://cims.nyu.edu/~schlacht/CNNFluids.htm). I'm essentially trying to blend all of these ideas into a neural network-based renderer, where you give it your reference map, like a grid where you mark out the feature types and coordinates, plus constraints, and a neural net will then render those features from its learned sets. In practical terms, the neural net would be the compute shader (just like cuDNN's appropriation of CUDA computes, or the upcoming TensorRT3 for Volta tensor cores), and write to the vertex/pixel shader to be shown on screen.

The issues obviously are speed and data size. Well newer decoders, like this [Multiscale Entanglement Renormalization Ansatz](https://arxiv.org/pdf/1711.03357.pdf) tensor network method are a viable replacement for fully-connected layers (traditionally used to decode convolutions), using thousands of times fewer parameters with similar accuracy and thus far fewer computation resources. Encoders can be [bottlenecked](https://arxiv.org/abs/1705.08086) to extract the the best feature maps (or [symmetries](https://arxiv.org/pdf/1512.06293.pdf)) at the data limit you need, governed by the amount of information you need out of it (or resolution in this case). Encoders themselves can be optimized around the idea that better models will have less loss at lower resource costs, revolving around the fact that correllations approach maximum entropy (or maximal information) to create models - the reason a multiscale entanglement decoder can be applied to a trained dataset, with the embedded models existing as 'crystallized' correlation networks you can perform faster transforms with than by traditional rendering, though generally at an imperfect precision.

Basically I'm gonna figure out how to encode one graphical feature at a time into a capsule network, then ["style transfer"](https://github.com/moothyknight/UniversalStyleTransfer) those onto the screen in reference to a map + game camera (i.e. the viewing frustum). It'll have to be quite modular, so various features can be toggled and modified. This will start as a normal engine with a few features being generated from a trained net, like particle physics and lighting, since a lot of that has been done already (just not in CapsNet) and can be referenced. What would be really trippy would be handing over control of the reference map and camera in a deep-dream-like recurrent way. 

I would love help! I shall dub this the InsanityNet if it works. I need help tweaking a capsnet for different datasets, I'm covering the graphics engine.
Reach me at: brewster.joshua1@gmail.com

I'm a computer science and psychology student. I've been studying this for a few years now, it's only in 2017 that some of the tools are becoming available to make something like my idea possible. Now I just need a Titan V to get decent framerate... X_x https://i.pinimg.com/736x/f2/fe/7a/f2fe7abeae7cafb5c622b447c4fa294a--high-meme.jpg

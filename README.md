# Dream Art

Dream Art combines [inceptionism/DeepDream][inceptionism] with
"[A Neural Algorithm of Artistic Style][neural-art-arxiv]."
The following examples were created with [create-outputs.sh](/create-outputs.sh)
and show an input content image followed by the DeepDream content image,
and then pairs of input artistic styles and
the DeepDream content image with the style applied.
See [the gallery](/gallery.md) for more examples.

# Tubingen
## Original
<img src="https://raw.githubusercontent.com/bamos/dream-art/master/examples/inputs/tubingen.jpg" height="300px">

## DeepDream
<img src="https://raw.githubusercontent.com/bamos/dream-art/master/examples/outputs/tubingen_deepdream.png" height="300px">

## Escher's "Hand with Reflecting Sphere"
<img src="https://raw.githubusercontent.com/bamos/dream-art/master/examples/inputs/escher_sphere.jpg" height="300px">

## DeepDream with Escher's Style
<img src="https://raw.githubusercontent.com/bamos/dream-art/master/examples/outputs/tubingen_escher_deepdream.png" height="300px">

# Golden Gate
## Original
<img src="https://raw.githubusercontent.com/bamos/dream-art/master/examples/inputs/golden_gate.jpg" height="300px">

## DeepDream
<img src="https://raw.githubusercontent.com/bamos/dream-art/master/examples/outputs/golden_gate_deepdream.png" height="300px">

## Kahlo's Self Portrait
<img src="https://raw.githubusercontent.com/bamos/dream-art/master/examples/inputs/frida_kahlo.jpg" height="300px">

## DeepDream with Kahlo's Style
<img src="https://raw.githubusercontent.com/bamos/dream-art/master/examples/outputs/golden_gate_kahlo_deepdream.png" height="300px">

# Brad Pitt
## Original
<img src="https://raw.githubusercontent.com/bamos/dream-art/master/examples/inputs/brad_pitt.jpg" height=300px>

## DeepDream
<img src="https://raw.githubusercontent.com/bamos/dream-art/master/examples/outputs/brad_pitt_deepdream.png" height=300px>

## TODO
TODO

## DeepDream with TODO

# How can I use this on my images without downloading anything?

Tweet the style and source image to
[@brandondamos](https://twitter.com/brandondamos)
with the hashtag `#DreamArtRequest` and I'll manually
generate images for you.
If there's enough interest, I'll create a bot to
automatically process images with `#DreamArtRequest`
on my server.

# Inceptionism (DeepDream)

*Inceptionism* is introduced in
[Google Research's blog post](http://googleresearch.blogspot.ch/2015/06/inceptionism-going-deeper-into-neural.html)
and visualizes an
[artificial neural network](https://en.wikipedia.org/wiki/Artificial_neural_network)
by enhancing input images to elicit an interpretation.
The following example from the blog post.
See [the original gallery](https://photos.google.com/share/AF1QipPX0SCl7OzWilt9LnuQliattX4OUCj_8EP65_cTVnBmS1jnYgsGQAieQUc1VQWdgQ?key=aVBxWjhwSzg2RjJWLWRuVFBBZEN1d205bUdEMnhB)
or [#DeepDream](https://twitter.com/search?q=%23DeepDream) for more examples.

![](http://2.bp.blogspot.com/-nxPKPYA8otk/VYIWRcpjZfI/AAAAAAAAAmE/8dSuxLnSNQ4/s640/image-dream-map.png)

The current implementations are
[google/deepdream][deepdream] which uses [Caffe][caffe] and
[eladhoffer/DeepDream.torch][DeepDream.torch] which uses [Torch][torch].

# A Neural Algorithm of Artistic Style
'[A Neural Algorthm of Artistic Style][neural-art-arxiv]' is a paper by
Leon Gatys, Alexander Ecker, and Matthias Bethge released as
a preprint on August 26, 2015.
The current implementations are
[jcjohnson/neural-style][neural-style] and
[kaishengtai/neuralart][neural-art], which
both use [Torch][torch].

# Can't I just run the implementations separately? What does this repository provide?
Nontrivial toolchains and system configuration make the barrier to
running current implementations high.
This repository lowers the barrier by combining
[DeepDream.torch][DeepDream.torch] with
[neural-style][neural-style] into Torch applications application that share the
same model and run on the CPU and GPU.

Ideally, both of these implementations should ship on [luarocks](https://luarocks.org)
and provide a command-line interface and library that
nicely load the models.
When they do, this repository will be obsolete, but until then,
this repository glues them together.

# Setup
The following is from [jcjohnson/neural-style][neural-style].

Dependencies:
* [torch7](https://github.com/torch/torch7)
* [loadcaffe](https://github.com/szagoruyko/loadcaffe)

Optional dependencies:
* CUDA 6.5+
* [cudnn.torch](https://github.com/soumith/cudnn.torch)

**NOTE**: If your machine does not have CUDA installed, then you may
  need to install loadcaffe manually like this:
```
git clone https://github.com/szagoruyko/loadcaffe.git
# Edit the file loadcaffe/loadcaffe-1.0-0.rockspec
# Delete lines 21 and 22 that mention cunn and inn
luarocks install loadcaffe/loadcaffe-1.0-0.rockspec
```
After installing dependencies, you'll need to run the following script
to download the VGG model:
```
sh models/download_models.sh
```
This will download the original
[VGG-19 model](https://gist.github.com/ksimonyan/3785162f95cd2d5fee77#file-readme-md).
Leon Gatys has graciously provided the modified version of the VGG-19
model that was used in their paper;
this will also be downloaded.
By default the original VGG-19 model is used.

# Usage

See `th deepdream.lua -help` and `th neural-style.lua -help` for the most updated docs.

```
dream-art(master*)$ ./deepdream.lua -help
-content_image Content target image [examples/inputs/tubingen.jpg]
-gpu           Zero-indexed ID of the GPU to use; for CPU mode set -gpu = -1 [0]
-num_iter      [100]
-num_octave    [8]
-octave_scale  [1.4]
-end_layer     [32]
-clip          [true]
-proto_file    [models/VGG_ILSVRC_19_layers_deploy.prototxt]
-model_file    [models/VGG_ILSVRC_19_layers.caffemodel]
-backend       nn|cudnn [nn]
-output_image  [out.png]
```

```
dream-art(master*)$ ./neural-style.lua -help
-style_image    Style target image [examples/inputs/seated-nude.jpg]
-content_image  Content target image [examples/inputs/tubingen.jpg]
-image_size     Maximum height / width of generated image [512]
-gpu            Zero-indexed ID of the GPU to use; for CPU mode set -gpu = -1 [0]
-content_weight [5]
-style_weight   [100]
-tv_weight      [0.001]
-num_iterations [1000]
-init           random|image [random]
-print_iter     [50]
-save_iter      [100]
-output_image   [out.png]
-style_scale    [1]
-pooling        max|avg [max]
-proto_file     [models/VGG_ILSVRC_19_layers_deploy.prototxt]
-model_file     [models/VGG_ILSVRC_19_layers.caffemodel]
-backend        nn|cudnn [nn]
```

# Other Q&A

+ **How long does it take to process a single image?**
  About 20 minutes with a Tesla K40.
+ **How much memory does this use?**
  This depends on the neural network model and image size.
  On average, 4GB.

# Licensing

All portions are
[MIT licensed](/LICENSE.mit)
by Brandon Amos unless otherwise noted.

This project uses and modifies the following open source projects
and resources.
Modifications remain under the original license.

| Project | Modified | License |
|---|---|---|
| [DeepDream.torch] | Yes | MIT
| [jcjohnson/neural-style][neural-style] | Yes | MIT


[inceptionism]: http://googleresearch.blogspot.ch/2015/06/inceptionism-going-deeper-into-neural.html
[deepdream]: https://github.com/google/deepdream
[DeepDream.torch]: https://github.com/eladhoffer/DeepDream.torch

[neural-art-arxiv]: http://arxiv.org/abs/1508.06576)
[neural-style]: https://github.com/jcjohnson/neural-style
[neural-art]: https://github.com/kaishengtai/neuralart

[caffe]: http://caffe.berkeleyvision.org
[torch]: http://torch.ch

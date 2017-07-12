# A step towards procedural terrain generation with GANs

Authors: [Christopher Beckham](https://github.com/christopher-beckham), [Christopher Pal](http://www.professeurs.polymtl.ca/christopher.pal/)

Procedural generation in video games is the algorithmic generation of content intended to increase replay value through interleaving 
the gameplay with elements of unpredictability. This is in contrast to the more traditional, `handcrafty' generation of content, which 
is generally of higher quality but with the added expense of labour. A prominent game whose premise is almost entirely based on procedural
terrain generation is Minecraft, a game where the player can explore a vast open world whose terrain is based entirely on voxels
('volumetric pixels'), allowing the player to manipulate the terrain (i.e. dig tunnels, build walls) and explore interesting landscapes
(i.e. beaches, jungles, caves).

So far, terrains have been procedurally generated through a host of algorithms designed to mimic real-life terrain. Some prominent
examples of this include Perlin noise and diamond square, in which a greyscale image (a heightmap) is generated from a noise source, 
which, when rendered in 3D as a mesh, produces a terrain. While these methods are quite fast, they generate terrains which are quite 
simple in nature. Software such as L3DT employ sophisticated algorithms which let the user control what kind of terrain they desire,
(e.g. mountains, lakes, valleys), and while these can produce very impressive terrains it would still seem like an exciting endeavour
to leverage the power of generative networks in deep learning (such as the GAN) to learn algorithms to automatically generate terrain
directly through learning the raw data without the need to manually write algorithms to generate them.

## Datasets

In this work, we leverage extremely high-resolution terrain and heightmap data provided by the NASA [Visible Earth](https://visibleearth.nasa.gov/) project
in conjunction with generative adversarial networks (GANs) to create a two-stage pipeline in which heightmaps can be randomly generated as
well as a texture map that is inferred from the heightmap. Concretely, we synthesise 512px height and texture maps using random 512px
crops from the original NASA images (of size 21600px x 10800px), as seen in the below images. (Note, per-pixel resolution is 25km, so a
512px crop corresponds to ~13k square km.)

<!-- <p align="center"> -->
<a href="https://eoimages.gsfc.nasa.gov/images/imagerecords/73000/73934/gebco_08_rev_elev_21600x10800.png">
<img src="https://github.com/christopher-beckham/gan-heightmaps/raw/master/md/earth_heightmap.png" />
</a>
<!-- </p> -->

<!-- <p align="center"> -->
<a href="https://eoimages.gsfc.nasa.gov/images/imagerecords/74000/74218/world.200412.3x21600x10800.jpg">
<img src="https://github.com/christopher-beckham/gan-heightmaps/raw/master/md/earth_texture.jpg" />
</a>
<!-- </p> -->

(TODO: upload h5 file so that people can use the dataset.)

## Results

We show some heightmaps and texture maps generated by the model. Note that we refer to the heightmap generation as the 'DCGAN' (since it
is essentially the DCGAN paper), and the texture generation as 'pix2pix' (which is based on the conditional image-to-image translation GAN
paper).

### DCGAN

Here are some heightmaps generated at roughly ~590 epochs for the DCGAN part of the network.

<img src="https://github.com/christopher-beckham/gan-heightmaps/blob/master/output/test1_repeatnod_fixp2p_nobn/dump_a_bakup_593ish/0.png" width="256" height="256" /> <img src="https://github.com/christopher-beckham/gan-heightmaps/blob/master/output/test1_repeatnod_fixp2p_nobn/dump_a_bakup_593ish/3.png" width="256" height="256" /> <img src="https://github.com/christopher-beckham/gan-heightmaps/blob/master/output/test1_repeatnod_fixp2p_nobn/dump_a_bakup_593ish/14.png" width="256" height="256" />

<img src="https://github.com/christopher-beckham/gan-heightmaps/blob/master/output/test1_repeatnod_fixp2p_nobn/dump_a_bakup_593ish/12.png" width="256" height="256" /> <img src="https://github.com/christopher-beckham/gan-heightmaps/blob/master/output/test1_repeatnod_fixp2p_nobn/dump_a_bakup_593ish/17.png" width="256" height="256" /> <img src="https://github.com/christopher-beckham/gan-heightmaps/blob/master/output/test1_repeatnod_fixp2p_nobn/dump_a_bakup_593ish/16.png" width="256" height="256" />

Click [here](http://www.youtube.com/watch?v=CvttVAhhCAs) to see a video showing linear interpolations between the 100 different randomly generated heightmaps (and their corresponding textures):

### pix2pix

Generation of texture maps from ground truth heightmaps ~600 epochs in (generating texture maps from generated heightmaps performed 
similarly):

<img src="https://github.com/christopher-beckham/gan-heightmaps/raw/master/output/test1_repeatnod_fixp2p_nobn/out_600.png" width="800" />

(TODO: texture outputs can be patchy, probably because the discriminator is actually a PatchGAN. I will need to run some experiments using
a 1x1 discriminator instead.)

Here is one of the generated heightmaps + corresponding texture map rendered in Unity:

<img src="https://github.com/christopher-beckham/gan-heightmaps/raw/master/md/unity_render_24.png" width="800" />

## Running the code

This code was inspired by -- and uses code from -- Costa et al's [vess2ret](https://github.com/costapt/vess2ret), which is a pix2pix implementation used for retinal image synthesis. Some code was also used from the [keras-adversarial](https://github.com/bstriner/keras-adversarial) library.

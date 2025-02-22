# PyTorch mip-NeRF 

A reimplementation of mip-NeRF in PyTorch. 

![mipnerf](misc/images/nerfTomipnerf.png)

Not exactly 1-to-1 with the official repo, as we organized the code to out own liking (mostly how the datasets are structued, and hyperparam changes to run the code on a consumer level graphics card), made it more modular, and removed some repetitive code, but it achieves the same results.

## Features

* Can use Spherical, or Spiral poses to generate videos for all 3 datasets
  * Spherical:

https://user-images.githubusercontent.com/42706447/171090423-2cf37b0d-44c9-4394-8c4a-46f19b0eb304.mp4


  * Spiral:

https://user-images.githubusercontent.com/42706447/171099856-f7340263-1d65-4fbe-81e7-3b2dfa9e93b8.mp4

 
* Depth and Normals video renderings:
* Depth:

https://user-images.githubusercontent.com/42706447/171091394-ce73822c-689f-496b-8821-78883e8b90d4.mp4

* Normals:

https://user-images.githubusercontent.com/42706447/171091457-c795855e-f8f8-4515-ae62-7eeb707d17bc.mp4

* Can extract meshes


https://user-images.githubusercontent.com/42706447/171100048-8f57fc9a-4be5-44c2-93dd-ee5f6b54dd6e.mp4


https://user-images.githubusercontent.com/42706447/171092108-b60130b5-297d-4e72-8d3a-3e5a29c83036.mp4


## Future Plans

In the future we plan on implementing/changing:

* Factoring out more repetitive/redundant code, optimize gpu memory and rps
* Clean up and expand mesh extraction code
* Zoomed poses for multicam dataset
* [Mip-NeRF 360: Unbounded Anti-Aliased Neural Radiance Fields](https://jonbarron.info/mipnerf360/) support
* [NeRV: Neural Reflectance and Visibility Fields for Relighting and View Synthesis](https://pratulsrinivasan.github.io/nerv/) support

## Installation/Running

1. Create a conda environment using `mipNeRF.yml`
2. Get the training data
   1. run `bash scripts/download_data.sh` to download all 3 datasets: LLFF, Blender, and Multicam.
   2. Individually run the bash script corresponding to an individual dataset
         * `bash scripts/download_llff.sh` to download LLFF
         * `bash scripts/download_blender.sh` to download Blender
         * `bash scripts/download_multicam.sh` to download Multicam (Note this will also download the blender dataset since it's derived from it)
3. Optionally change config parameters: can change default parameters in `config.py` or specify with command line arguments
    * Default config setup to run on a high-end consumer level graphics card (~8-12GB)
4. Run `python train.py` to train
   * `python -m tensorboard.main --logdir=log` to start the tensorboard
5. Run `python visualize.py` to render a video from the trained model
6. Run `python extract_mesh.py` to render a mesh from the trained model

## Code Structure

I explain the specifics of the code more in detail [here](misc/Code.md) but here is a basic rundown.

* `config.py`: Specifies hyperparameters.
* `datasets.py`: Base generic `Dataset` class + 3 default dataset implementations.
  * `NeRFDataset`: Base class that all datasets should inherent from.
  * `Multicam`: Used for multicam data as in the original mip-NeRF paper.
  * `Blender`: Used for the synthetic dataset as in original NeRF.
  * `LLFF`: Used for the llff dataset as in the original NeRF.
* `loss.py`: mip-NeRF loss, pretty much just MSE, but also calculates psnr.
* `model.py`: mip-NeRF model, not as modular as the way the original authors wrote it, but easier to understand its structure when laid out verbatim like this.
* `pose_utils.py`: Various functions used to generate poses.
* `ray_utils.py`: Various functions related involving rays that the model uses as input, most are used within the forward function of the model.
* `scheduler.py`: mip-NeRF learning rate scheduler.
* `train.py`: Trains a mip-NeRF model.
* `visualize.py`: Creates the videos using a trained mip-NeRF.

## mip-NeRF Summary

Here's a summary on how NeRF and mip-NeRF work that I wrote when writing this originally.

* [Summary](misc/Summary.md)

## Results

<sub><sup>All PSNRs are average PSNR (coarse + fine).</sub></sup>

### LLFF - Trex

<div>
   <img src="misc/results/trex/LR.png" alt="pic0" width="49%">
   <img src="misc/results/trex/Evaluation_PSNR.png" alt="pic1" width="49%">
</div>
<div>
   <img src="misc/results/trex/Train_Loss.png" alt="pic2" width="49%">
   <img src="misc/results/trex/Train_PSNR.png" alt="pic3" width="49%">
</div>

<br>
Video:
<br>


https://user-images.githubusercontent.com/42706447/171100120-0a0c9785-8ee7-4905-a6f6-190269fb24c6.mp4


<br>
Depth:
<br>


https://user-images.githubusercontent.com/42706447/171100098-9735d79a-c22f-4873-bb4b-005eef3bc35a.mp4


<br>
Normals:
<br>


https://user-images.githubusercontent.com/42706447/171100112-4245abd8-bf69-4655-b14c-9703c13c38fb.mp4


### Blender - Lego

<div>
   <img src="misc/results/lego/LR.png" alt="pic0" width="49%">
   <img src="misc/results/lego/Evaluation_PSNR.png" alt="pic1" width="49%">
</div>
<div>
   <img src="misc/results/lego/Train_Loss.png" alt="pic2" width="49%">
   <img src="misc/results/lego/Train_PSNR.png" alt="pic3" width="49%">
</div>
Video:
<br>

https://user-images.githubusercontent.com/42706447/171090423-2cf37b0d-44c9-4394-8c4a-46f19b0eb304.mp4

<br>
Depth:
<br>

https://user-images.githubusercontent.com/42706447/171091394-ce73822c-689f-496b-8821-78883e8b90d4.mp4

<br>
Normals:
<br>

https://user-images.githubusercontent.com/42706447/171091457-c795855e-f8f8-4515-ae62-7eeb707d17bc.mp4

### Multicam - Mic

<div>
   <img src="misc/results/mic/LR.png" alt="pic0" width="49%">
   <img src="misc/results/mic/Evaluation_PSNR.png" alt="pic1" width="49%">
</div>
<div>
   <img src="misc/results/mic/Train_Loss.png" alt="pic2" width="49%">
   <img src="misc/results/mic/Train_PSNR.png" alt="pic3" width="49%">
</div>
Video:
<br>

https://user-images.githubusercontent.com/42706447/171100600-7f3307c7-0ca4-4677-b9b7-180cf27fd175.mp4



<br>
Depth:
<br>


https://user-images.githubusercontent.com/42706447/171100593-e0139375-1ae6-4235-8961-ba3c45f88ead.mp4


<br>
Normals:
<br>


https://user-images.githubusercontent.com/42706447/171092348-9315a897-a6a3-4c49-aedf-3f3331fdfe52.mp4


## References/Contributions

* Thanks to [Nina](https://github.com/ninaahmed) for helping with the code
* [Original NeRF Code in Tensorflow](https://github.com/bmild/nerf)
* [NeRF Project Page](https://www.matthewtancik.com/nerf)
* [NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis](https://arxiv.org/abs/2003.08934)
* [Original mip-NeRF Code in JAX](https://github.com/google/mipnerf)
* [mip-NeRF Project Page](https://jonbarron.info/mipnerf/)
* [Mip-NeRF: A Multiscale Representation for Anti-Aliasing Neural Radiance Fields](https://arxiv.org/abs/2103.13415)
* [nerf_pl](https://github.com/kwea123/nerf_pl)

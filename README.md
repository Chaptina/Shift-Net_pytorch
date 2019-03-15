# Architecutre
<img src="architecture.png" width="1000"/> 

# Shift layer
<img src="shift_layer.png" width="800"/> 

# Shift-Net_pytorch
This repositity is our Pytorch implementation for Shift-Net, it is much faster than the original code is https://github.com/Zhaoyi-Yan/Shift-Net.

## Prerequisites
- Linux or Windows.
- Python 2 or Python 3.
- CPU or NVIDIA GPU + CUDA CuDNN.
- Tested on pytorch >= 0.4.0

## Getting Started
### Installation
- Install PyTorch and dependencies from http://pytorch.org/
- Install python libraries [visdom](https://github.com/facebookresearch/visdom) and [dominate](https://github.com/Knio/dominate).
```bash
pip install visdom
pip install dominate
```
- Clone this repo:
```bash
git clone https://github.com/Zhaoyi-Yan/Shift-Net_pytorch
cd Shift-Net_pytorch

```

### tain and test
- Download your own inpainting datasets.

- Train a model:
Please read this paragraph carefully before running the code.

By now, 5 kinds of shift-nets are proposed, presented to you by \"advanced-descending\" order.

Usually, we train and test a model with `center` mask.

**Mention: For now, the best performance of `center mask inpainiting` can be achieved if you train this line:**.

```bash
python train.py --batchsize=1 --use_spectral_norm_D=1 --which_model_netG='basic' --mask_type='center'
```

For training random mask, you need to train the model by setting
`mask_type='random'` and also `mask_sub_type='rect'` or `mask_sub_type='island'`.


For `navie shift-net`:
```bash
python train.py --which_model_netG='unet_shift_triple' --model='shiftnet' --shift_sz=1 --maskA_thred=1
```

**These 4 models are just experimental**

For `res navie shift-net`:
```bash
python train.py --which_model_netG='res_unet_shift_triple' --model='res_shiftnet' --shift_sz=1 --maskA_thred=1
```

For `pixel soft shift-net`:
```bash
python train.py --which_model_netG='soft_unet_shift_triple' --model='soft_shiftnet' --shift_sz=1 --maskA_thred=1
```

For `patch soft shift-net`:
```bash
python train.py --which_model_netG='patch_soft_unet_shift_triple' --model='patch_soft_shiftnet' --shift_sz=3 --mask_thred=4
```

For `res patch soft shift-net`:
```bash
python train.py --which_model_netG='res_patch_soft_unet_shift_triple' --model='res_patch_soft_shiftnet' --shift_sz=3 --mask_thred=4
```
DO NOT change the shift_sz and mask_thred. Otherwise, it errors with a high probability.

For `patch soft shift-net` or `res patch soft shift-net`. You may set `fuse=1` to see whether it delivers better results.


- To view training results and loss plots, run `python -m visdom.server` and click the URL http://localhost:8097. The checkpoints will be saved in `./log` by default.

- Test the model

**Keep the same settings as those during training phase to avoid errors or bad performance**

For example, if you train `patch soft shift-net`, then the following testing command is appropriate.
```bash
python test.py --fuse=1/0 --which_model_netG='patch_soft_unet_shift_triple' --model='patch_soft_shiftnet' --shift_sz=3 --mask_thred=4 
```
The test results will be saved to a html file here: `./results/`.

If you find this work useful, please cite:
```
@InProceedings{Yan_2018_Shift,
author = {Yan, Zhaoyi and Li, Xiaoming and Li, Mu and Zuo, Wangmeng and Shan, Shiguang},
title = {Shift-Net: Image Inpainting via Deep Feature Rearrangement},
booktitle = {The European Conference on Computer Vision (ECCV)},
month = {September},
year = {2018}
}
```

## Performance degrades when batchsize > 1
-^_^-, trying to fix it...
A very strange thing is that if the `batchSize>1`, then the performance degrades.
I wonder whether it is due to some incompatibility of IN with UNet.


## Kindly remindier
If you find it a little hard to read the code, you may read [Guides](https://github.com/Zhaoyi-Yan/Shift-Net_pytorch/blob/master/guides.md).


## New things that I want to add
- [x] Make U-Net handle with inputs of any sizes. (By resizing the size of features of decoder to fit that of the corresponding features of decoder.
- [x] Update the code for pytorch >= 0.4.
- [x] Clean the code and delete useless comments.
- [x] Guides of our code, we hope it helps you understand our code more easily.
- [x] Add more GANs, like spectural norm and relativelistic GAN.
- [x] Boost the efficiency of shift layer.
- [x] Directly resize the global_mask to get the mask in feature space.
- [x] Visualization of flow. It is still experimental now.
- [x] Extensions of Shift-Net. Still active in absorbing new features.
- [ ] Fix performance degradance when batchsize is larger than 1.
- [ ] Fix bug in guidance loss when adopting it in multi-gpu.
- [ ] Add random batch of masks
- [ ] Add soft transition from centered mask to random ones
- [ ] Add composit L1 loss between mask loss and non-mask loss
- [ ] Finish optimizing soft-shift
- [ ] Evaluate imagenet pre-trained models as discriminator

## Acknowledgments
We benefit a lot from [pytorch-CycleGAN-and-pix2pix](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix)

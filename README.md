We recently release a project focusing on interpolation on cartoon line art: [AnimeInbet](https://github.com/lisiyao21/AnimeInbet) in ICCV23! 

Do not hesitate to give a star! your star is my strongest motivation!

# AnimeInterp

This is the code for paper "Deep Animation Video Interpolation in the Wild" (CVPR21). 

[[Paper]](https://arxiv.org/abs/2104.02495) | [[Data (Google Drive)]](https://drive.google.com/file/d/1XBDuiEgdd6c0S4OXLF4QvgSn_XNPwc-g/view) [[Data (OneDrive)]](https://entuedu-my.sharepoint.com/:u:/g/personal/siyao002_e_ntu_edu_sg/EY3SG0-IajxKj9HMPz__zOMBvyJdrA-SlwpyHYFkDsQtng?e=q7nGlu) | [[Video Demo]](https://www.youtube.com/watch?v=2bbujT-ZXr8)

> Abstract: In the animation industry, cartoon videos are usually produced at low frame rate since hand drawing of such frames is costly and time-consuming. Therefore, it is desirable to develop computational models that can automatically interpolate the in-between animation frames. However, existing video interpolation methods fail to produce satisfying results on animation data. Compared to natural videos, animation videos possess two unique characteristics that make frame interpolation difficult: 1) cartoons comprise lines and smooth color pieces. The smooth areas lack textures and make it difficult to estimate accurate motions on animation videos. 2) cartoons express stories via exaggeration. Some of the motions are non-linear and extremely large. In this work, we formally define and study the animation video interpolation problem for the first time. To address the aforementioned challenges, we propose an effective framework, AnimeInterp, with two dedicated modules in a coarse-to-fine manner. Specifically, 1) Segment-Guided Matching resolves the "lack of textures" challenge by exploiting global matching among color pieces that are piece-wise coherent. 2) Recurrent Flow Refinement resolves the "non-linear and extremely large motion" challenge by recurrent predictions using a transformer-like architecture. To facilitate comprehensive training and evaluations, we build a large-scale animation triplet dataset, ATD-12K, which comprises 12,000 triplets with rich annotations. Extensive experiments demonstrate that our approach outperforms existing state-of-the-art interpolation methods for animation videos. Notably, AnimeInterp shows favorable perceptual quality and robustness for animation scenarios in the wild. 

![image](https://github.com/lisiyao21/AnimeInterp/blob/main/figs/sample0.png)

## Overview

AnimeInterp consists three parts ('segment-guided mathing', 'recurent flow refine' and 'warping & synthesis') to generate the in-between anime frame given two inputs.

![image](https://github.com/lisiyao21/AnimeInterp/blob/main/figs/pipeline.png)

### Environment
* 0.4 <= pytorch <= 1.1

If you have to use higher version, please set argument "align_corner" as True for any "grid_sample" function that appears in this project. 

Ref. [Pytorch Official Doc 1.3.0](https://pytorch.org/docs/1.3.0/nn.functional.html?highlight=grid_sample#torch.nn.functional.grid_sample)
![image](https://github.com/Soooda/AnimeInterp/blob/main/figs/align_corners.png)

#### Commands
```bash
conda create -n AnimeInterp pip python=3.7
conda install pytorch==1.1.0 torchvision==0.3.0 cudatoolkit=10.0 -c pytorch
conda install scipy cupy opencv
pip install easydict
```
`skimage.measure.compare_psnr()` and `skimage.measure.compare_ssim()` was replaced by `skimage.metrics.peak_signal_noise_ratio()` and `skimage.metrics.structural_similarity()`.

Ref. [scikit-image Official Doc 0.16.1](https://scikit-image.org/docs/0.16.x/api/skimage.measure.html?highlight=compare_psnr#skimage.measure.compare_psnr)

Install lower version to avoid modifications.
```bash
conda install scikit-image==0.14.3 -c conda-forge
```

If you got `ImportError: cannot import name 'PILLOW_VERSION' from 'PIL'`, try downgrading Pillow verion.

Ref. [Github/python-pillow Issue #4130](https://github.com/python-pillow/Pillow/issues/4130)

Pillow is by default installed with pytorch so we need to perform a manually downgrade.
```bash
conda install pillow=6.1
```

### Data

To use the data, please first download it from [link](https://drive.google.com/file/d/1XBDuiEgdd6c0S4OXLF4QvgSn_XNPwc-g/view) and uncompress it into this project directory. When uncompressed, the data will look like

        datasets 
             |_train_10k 
             |        |_ train_triplet0 
             |        |             |_ frame1.jpg 
             |        |             |_ frame2.jpg
             |        |             |_ frame3.jpg
             |        |
             |        ...
             |
             |_train_10k 
             |        |_ test_triplet0 
             |        |             |_ frame1.png 
             |        |             |_ frame2.png
             |        |             |_ frame3.png
             |        |
             |        ...
             |
             |_test_2k_ann 
                      |_ test_triplet0 
                                    |_ triplet0.json   


We also provid pre-computed SGM flows in the datasets folder.

### Code

To run the reference code, first download the pre-trained weights from [link](https://www.dropbox.com/s/oc8juclx1775qib/anime_interp_full.ckpt?dl=0)(Dropbox) or [link](https://www.jianguoyun.com/p/DVKXlwIQ6OS4CRixxPQD)(坚果云) and move it to the checkpoints folder. Then, run

``` 
python test_anime_sequence_one_by_one.py configs/config_test_w_sgm.py 
```

The interpolated results will be recorded into the where the "store_path" argument indicates in the config file.

### Citation

    @inproceedings{siyao2021anime,
	    title={Deep Animation Video Interpolation in the Wild},
	    author={Siyao, Li and Zhao, Shiyu and Yu, Weijiang and Sun, Wenxiu and Metaxas, Dimitris and Loy, Chen Change and Liu, Ziwei },
	    booktitle={CVPR},
	    year={2021}
    }

### License

Our code is released under MIT License.

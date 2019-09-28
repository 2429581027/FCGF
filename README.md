# Fully Convolutional Geometric Features, ICCV, 2019

Extracting geometric features from 3D scans or point clouds is the first step in applications such as registration, reconstruction, and tracking. State-of-the-art methods require computing low-level features as input or extracting patch-based features with limited receptive field. In this work, we present fully-convolutional geometric features, computed in a single pass by a 3D fully-convolutional network. We also present new metric learning losses that dramatically improve performance. Fully-convolutional geometric features are compact, capture broad spatial context, and scale to large scenes. We experimentally validate our approach on both indoor and outdoor datasets. Fully-convolutional geometric features achieve state-of-the-art accuracy without requiring prepossessing, are compact (32 dimensions), and are 600 times faster than the most accurate prior method.

[ICCV'19 Paper](https://node1.chrischoy.org/data/publications/fcgf/fcgf.pdf)


## 3D Feature Accuracy vs. Speed

![Accuracy vs. Speed](images/fps_acc.png)
*Feature-match recall and speed in log scale on the 3DMatch benchmark. Our approach is the most accurate and the fastest. The gray region shows the Pareto frontier of the prior methods.*


### Related Works

3DMatch by Zeng et al. uses a siamese convolutional network to learn 3D patch descriptors.
CGF by Khoury et al. maps 3D oriented histograms to a low-dimensional feature space using multi-layer perceptrons. PPFNet and PPF FoldNet by Deng et al. adapts the PointNet architecture for geometric feature description. 3DFeat by Yew and Lee uses a PointNet to extract features in outdoor scenes.

Our work addressed a number of limitations in the prior work. First, all prior approaches extract a small 3D patch or a set of points and map it to a low-dimensional space. This not only limits the receptive field of the network but is also computationally inefficient since all intermediate representations are computed separately even for overlapping 3D regions. Second, using expensive low-level geometric signatures as input can slow down feature computation. Lastly, limiting feature extraction to a subset of interest points results in lower spatial resolution for subsequent matching stages and can thus reduce registration accuracy.


### Visualization of FCGF

We color-coded FCGF features for pairs of 3D scans that are 10m apart for KITTI and a 3DMatch benchmark pair for indoor scans. FCGF features are mapped to a scalar space using t-SNE and colorized with the Spectral color map.

| KITTI LIDAR Scan 1   | KITTI LIDAR Scan 2   |
|:--------------------:|:--------------------:|
| ![0](images/3_1.png) | ![1](images/3_2.png) |

| Indoor Scan 1              | Indoor Scan 2              |
|:--------------------------:|:--------------------------:|
| ![0](images/kitchen_0.png) | ![1](images/kitchen_1.png) |


## Requirements

- Ubuntu 14.04 or higher
- CUDA 10.0 or higher
- Python v3.7 or higher
- Pytorch v1.2 or higher
- [MinkowskiEngine](https://github.com/stanfordvl/MinkowskiEngine) v0.2.7 or higher


## Installation & Dataset Download


We recommend conda for installation. First, follow the [MinkowskiEngine installation instruction](https://github.com/stanfordvl/MinkowskiEngine) to setup the environment and the Minkowski Engine.

Next, download FCGF git repository and install the requirement from the FCGF root directory..

```
git clone https://github.com/chrischoy/FCGF.git
cd FCGF
# Do the following inside the conda environment
pip install -r requirements.txt
```

For training, download the preprocessed 3DMatch benchmark dataset.

```
./scripts/download_datasets.sh /path/to/dataset/download/dir
```

For KITTI training, follow the instruction on [KITTI Odometry website](http://www.cvlibs.net/datasets/kitti/eval_odometry.php) to download the KITTI odometry training set.


## Training and running 3DMatch benchmark

```
python train.py --threed_match_dir /path/to/threedmatch/
```

For benchmarking the trained weights on 3DMatch, download the 3DMatch Geometric Registration Benchmark dataset from [here](http://3dmatch.cs.princeton.edu/) and follow:

```
python benchmark_3dmatch.py --source /path/to/threedmatch --target ./features_tmp/ --voxel_size 0.025 --model ~/outputs/checkpoint.pth --do_generate --do_exp_feature --with_cuda
```

## Model Zoo

| Model       | Normalized Feature  | Dataset | Voxel Size | Feature Dimension | Performance              | Link   |
|:-----------:|:-------------------:|:-------:|:----------:|:-----------------:|:------------------------:|:------:|
| ResUNetBN2C | True                | 3DMatch | 2.5cm      | 32                | FMR: 0.9578 +- 0.0272    | [download](https://node1.chrischoy.org/data/publications/fcgf/2019-08-19_06-17-41.pth) |
| ResUNetBN2C | True                | 3DMatch | 2.5cm      | 16                | FMR: 0.9442 +- 0.0345    | [download](https://node1.chrischoy.org/data/publications/fcgf/2019-09-18_14-15-59.pth) |
| ResUNetBN2C | True                | 3DMatch | 5cm        | 32                | FMR: 0.9372 +- 0.0332    | [download](https://node1.chrischoy.org/data/publications/fcgf/2019-08-16_19-21-47.pth) |
| ResUNetBN2C | False               | KITTI   | 20cm       | 32                | RTE: 0.0534, RRE: 0.1704 | [download](https://node1.chrischoy.org/data/publications/fcgf/2019-07-31_19-30-19.pth) |
| ResUNetBN2C | False               | KITTI   | 30cm       | 32                | RTE: 0.0607, RRE: 0.2280 | [download](https://node1.chrischoy.org/data/publications/fcgf/2019-07-31_19-37-00.pth) |


## Citing FCGF

FCGF will be presented at ICCV'19: Friday, November 1, 2019, 1030–1300 Poster 4.1 (Hall B)

```
@inproceedings{FCGF2019,
    author = {Christopher Choy and Jaesik Park and Vladlen Koltun},
    title = {Fully Convolutional Geometric Features},
    booktitle = {ICCV},
    year = {2019},
}
```

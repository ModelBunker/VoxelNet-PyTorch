
# Stereo R-CNN based 3D Object Detection for Autonomous Driving - PyTorch

PyTorch implementation of the paper [Stereo R-CNN based 3D Object Detection for Autonomous Driving](https://arxiv.org/abs/1902.09738) based on previous work in [this repo](https://github.com/skyhehe123/VoxelNet-pytorch).

The file **todo.sh** contains all build instructions, so either run it with ./todo.sh or copy paste the following into your shell.
```
docker build -t deepsort .

docker volume create --opt type=none \
                     --opt o=bind \
                     --opt device=. \
                     dpsrt-vol

docker run --gpus all -it \
           -p 5001:6006 \
           --name deepsort_cnt \
           -v dpsrt-vol:/home/PyTorch-YOLOv3/:rw \
           deepsort

cd Object-Tracking/deep_sort_pytorch

python demo.py
```



## Introduction

This is an unofficial implementation of [VoxelNet: End-to-End Learning for Point Cloud Based 3D Object Detection](https://arxiv.org/abs/1711.06396) in pytorch. A large part of this project is based on the work [here](https://github.com/jeasinema/VoxelNet-tensorflow)
## Dependencies
- `python3.5+`
- `pytorch` (tested on 0.3.1)
- `opencv`
- `shapely`
- `mayavi`

## Installation
1. Clone this repository.
2. Compile the Cython module for box_overlaps
```bash
$ python3 setup.py build_ext --inplace
```
3. Compile the nms model
```bash
$ python3 nms/build.py
```


## Data Preparation
1. Download the 3D KITTI detection dataset from [here](http://www.cvlibs.net/datasets/kitti/eval_object.php?obj_benchmark=3d). Data to download include:
    * Velodyne point clouds (29 GB): input data to VoxelNet
    * Training labels of object data set (5 MB): input label to VoxelNet
    * Camera calibration matrices of object data set (16 MB): for visualization of predictions
    * Left color images of object data set (12 GB): for visualization of predictions

2. In this project, the cropped point cloud data for training and validation. Point clouds outside the image coordinates are removed.
```bash
$ python3 data/crop.py
```
3. Split the training set into training and validation set according to the protocol [here](https://xiaozhichen.github.io/files/mv3d/imagesets.tar.gz).
```plain
└── DATA_DIR
       ├── training   <-- training data
       |   ├── image_2
       |   ├── label_2
       |   ├── velodyne
       |   └── crop
       └── testing  <--- testing data
       |   ├── image_2
       |   ├── label_2
       |   ├── velodyne
       |   └── crop
```

## Train

```bash
$ python3 train.py
```

## TODO
- [x] training code
- [x] data augmentation
- [ ] validation code
- [ ] reproduce results for `Car`, `Pedestrian` and `Cyclist`
- [ ] multi-gpu support
- [ ] improve the performances




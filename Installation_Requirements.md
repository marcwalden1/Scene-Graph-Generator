# Installation
This project was implemented and tested using an NNVIDIA GeForce GTX 780 Ti with CUDA 10.1 support.

### Requirements

    Python <= 3.8
    PyTorch >= 1.2 (I used version 1.4.0 to make it compatible with my version of CUDA 10.1)
    torchvision >= 0.4 (I used version 0.5.0  to make it compatible with my version of CUDA 10.1)
    cocoapi
    yacs
    matplotlib
    GCC >= 4.9
    OpenCV

### Step by step installation

```bash
# First, make sure that conda is setup properly within the right environment
# For that, check that `which conda`, `which pip` and `which python` points to the
# right path. From a clean conda env, this is what you need to do

conda create --name scene_graph_benchmark
conda activate scene_graph_benchmark

```
Now that our scene_graph_benchmark conda virtual environement is setup, we can move on to installing the correct dependencies inside of the environment. 

```bash
# this installs the right pip and dependencies for the fresh python
conda install ipython
conda install scipy
conda install h5py

# scene_graph_benchmark and coco api dependencies
pip install ninja yacs cython matplotlib tqdm opencv-python overrides

```

Follow PyTorch installation in https://pytorch.org/get-started/locally/ .

These instructions are for CUDA 10.1
```bash
conda install pytorch==1.4.0 torchvision==0.5.0 cudatoolkit=10.1 -c pytorch
```

You can check whether Cuda is supported by your GPU and working as intended by running the following command in python:
```python
import torch
print(torch.cuda.is_available())
# Make sure that your python kernel is setup inside of your scene_graph_benchmark virtual environement. The kernels should match.
```
If you run into compatibility issues with you pytorch version, you should install compatible versions of numpy and matplotlib.
```bash
pip install numpy==1.24.4
pip install matplotlib==3.1.2
```

You can also check that your pytorch, torchvision, python, numpy, and matplotlib versions are correctly setup inside of your virtual environments by running the following commands:
```python
import torchvision
import torch
import numpy
import matplotlib
print(torchvision.__version__)
print(torch.__version__)
print(numpy.__version__)
print(matplotlib.__version__)
```


This project also requires GloVe embeddings for processing text data in scene graph generation. Ensure that you download and correctly set up the GloVe directory before running the model.
GloVe Setup Instructions:

- Download GloVe Embeddings: Visit the official GloVe website and download the embeddings. For this project, I recommended to use the glove.6B.zip file.

- Extract GloVe Files: After downloading the zip file, extract it to a desired location. For example:
```bash
unzip glove.6B.zip -d path/to/glove/
```
This scene graph generator uses 200-dimensional word embeddings, meaning that you will need the glove.6B.200d.txt file for tokenization and embedding purposes. Depending on your PyTorch implementation, you may need to convert this file to glove.6B.200d.pt for compatibility and efficient loading.


Now we proceed to install pycocotools and apex inside of our installing directory. 
```bash
# Keep track of your your working directory. 
export INSTALL_DIR=$PWD

# install pycocotools
cd $INSTALL_DIR
git clone https://github.com/cocodataset/cocoapi.git
cd cocoapi/PythonAPI
python setup.py build_ext install

# install apex
cd $INSTALL_DIR
git clone https://github.com/NVIDIA/apex.git
cd apex

# WARNING if you use older Versions of Pytorch (anything below 1.7), you will need a hard reset,
# as the newer version of apex does require newer pytorch versions. Ignore the hard reset otherwise.
git reset --hard 3fe10b5597ba14a748ebb271a6ab97c09c5701ac
```
We now install this Python package with CUDA and C++ extensions for GPU acceleration and optimization, then clone and build the Scene-Graph-Benchmark repository in development mode. 
```bash

python setup.py install --cuda_ext --cpp_ext

# install PyTorch Detection
cd $INSTALL_DIR
git clone https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch.git
cd scene-graph-benchmark

# the following will install the lib with
# symbolic links, so that you can modify
# the files if you want and won't need to
# re-build it
python setup.py build develop


unset INSTALL_DIR

```


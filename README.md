# ![neurophox](media/logo.png)

The `neurophox` module is an open source machine learning and photonic simulation framework based on unitary mesh networks presented in [arxiv/1808.00458](https://arxiv.org/pdf/1808.00458.pdf) and [arxiv/1903.04579](https://arxiv.org/pdf/1903.04579.pdf).

## Motivation

Orthogonal and unitary neural networks have interesting properties and have been studied for synthetic natural language processing tasks (see [unitary mesh-based RNN](http://proceedings.mlr.press/v70/jing17a/jing17a.pdf), [unitary evolution RNN](https://arxiv.org/pdf/1511.06464.pdf), and [orthogonal evolution RNN](https://arxiv.org/pdf/1602.06662.pdf)). Furthermore, new energy-efficient photonic technologies are being built to realize such neural networks using light as the computing medium as opposed to conventional analog electronics.

## Introduction

`neurophox` provides a robust and general framework for mesh network implementations of orthogonal and unitary neural networks. We use an efficient definition for any feedforward mesh architecture, `neurophox.meshmodel.MeshModel`, to develop mesh layer architectures in Numpy (`neurophox.numpy.layers`), Tensorflow 2 (`neurophox.tensorflow.layers`), and (soon) PyTorch.

Scattering matrix models used in unitary mesh networks for photonics simulations are provided in `neurophox.components`. The models for all layers are fully defined in `neurophox.meshmodel`, which provides a general framework for efficient implementation of any unitary mesh network.

## Getting started

### Installation

There are three options to install `neurophox`:
1. Installation via `conda` (must be in a dedicated conda environment!) for Linux and Mac OS targets.

    GPU version:
    ```bash
    conda install -c sunilpai neurophox-gpu
    ```
    CPU version:
    ```bash
    conda install -c sunilpai neurophox
    ```
2. Installation via `pip` (all other dependencies installed manually).
    ```bash
    pip install neurophox
    ```
3. Installation from source via `pip`.
    ```bash
    git clone https://github.com/solgaardlab/neurophox
    pip install -e .
    pip install -r requirements.txt
    ```
    
If not installing via `conda`, you'll need to install [PyTorch](https://pytorch.org/) (for future version compatibility) and [Tensorflow 2.0](https://www.tensorflow.org/versions/r2.0/api_docs/python/tf).

If using the `conda` package installation, it is much easier to install GPU dependencies using CUDA 10.0 using the following commands:
```bash
conda install pytorch torchvision cudatoolkit=10.0 -c pytorch
pip install tensorflow-gpu==2.0.0-alpha0
```

### Examples

####Imports

```python
from neurophox.numpy import RMNumpy
from neurophox.tensorflow import RM
import matplotlib.pyplot as plt

N = 16

tf_layer = RM(N)
np_layer = RMNumpy(N, phases=tf_layer.phases)
```

#### Inspection

We can inspect the parameters $\theta_{n\ell}, \phi_{n\ell}$ for each layer using `neurophox.control.MeshPhases`:
```python
phases = tf_layer.phases
phases.theta.param  # raw values of theta
phases.theta.checkerboard_arrangement  # values of theta arranged in checkerboard, useful for RM, TM layers only
phases.theta.single_mode_arrangement  # values of theta arranged in striped pattern (single-mode arrangement)
```


We can inspect the matrix elements of $U$ implemented by each layer as follows: 

```python
np.allclose(np_layer.transform(np.eye(N)), np_layer.matrix)  # True
np.allclose(tf_layer.transform(np.eye(N)), tf_layer.matrix)  # True
np.allclose(np_layer.matrix, tf_layer.matrix)  # True
```

#### Visualize



## Contributions

The module is under development and is not yet stable. We welcome pull requests and contributions from the broader community.

If you would like to contribute, please submit a pull request. If you find a bug, please submit an issue on Github.

## Dependencies and requirements

Some important requirements for `neurophox` are:
1. Python >=3.6
2. Tensorflow 2.0
3. PyTorch 1.1

The dependencies for `neurophox` (specified in `requirements.txt`) are:
```text
numpy
scipy
matplotlib
tensorflow==2.0
torch==1.1
tensorboard
```

## Authors
`neurophox` was written by Sunil Pai (email: sunilpai@stanford.edu).
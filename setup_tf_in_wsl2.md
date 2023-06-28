# Install TensorFlow with CUDA support in WSL2

Follow https://www.tensorflow.org/install/pip for the most part.

But then you may run into [two
problems](https://stackoverflow.com/questions/76016645/tensorflow-2-12-could-not-load-library-libcudnn-cnn-infer-so-8-in-wsl2)
-- missing `libcuda.so` and missing device:

To solve the first problem, do
```
ln -s /usr/lib/wsl/lib/libcuda.so.1 /home/yman/miniconda3/envs/tf2/lib/
```

To solve the second problem, do
```
$ mkdir -p $CONDA_PREFIX/lib/nvvm/libdevice/
$ cp -p $CONDA_PREFIX/lib/libdevice.10.bc $CONDA_PREFIX/lib/nvvm/libdevice/
$ export XLA_FLAGS=--xla_gpu_cuda_data_dir=$CONDA_PREFIX/lib
$ conda install -c nvidia cuda-nvcc --yes
```

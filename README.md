# es-dev-stack
An on-premises, bare-metal solution for deploying GPU-powered applications in containers

**Blog Post with deployment details:**

http://www.emergingstack.com/2016/01/10/Nvidia-GPU-plus-CoreOS-plus-Docker-plus-TensorFlow.html 

(pinmarch added: Docker Hub https://hub.docker.com/r/pinmarch/nvidia-install/ )

### Prerequisites

- CoreOS-compatible dedicated machine with vanilla CoreOS installed
- Current-generation Nvidia GPU (tested with TitanX)
- Current-generation Nvidia GPU (tested with GTX970 by pinmarch)
- NVIDIA driver installer file (*.run)

### To Build

**Nvidia Drivers Installation Image**

```
$ cd es-dev-stack/corenvidiadrivers
```

```
$ docker build -t nvidia-install .
```

**GPU-enabled TensorFlow Image**

```
$ cd es-dev-stack/tflowgpu
```

```
$ docker build -t tflowgpu .
```

### To Run

**Stage 1 - Install Nvidia Drivers & Register GPU Devices (One-Time)**

```
# docker run -it --rm -v /dev/:/dev/ --privileged nvidia-install
```

(./mkdevs.sh is executed in the previous command.)

**Stage 2 - TensorFlow Docker Container with mapped GPU devices**

```
$ docker run -v /dev/nvidia0:/dev/nvidia0 -v /dev/nvidia1:/dev/nvidia1 -v /dev/nvidiactl:/dev/nvidiactl -v /dev/nvidia-uvm:/dev/nvidia-uvm -it -p 8888:8888 tflowgpu
```

### To Test

- Open your web browser to http://{host IP}:8888 and launch the CNN.ipynb notebook
- Execute all steps to confirm
- To validate GPU is utilized, watch the statistics produced from the Nvidia-SMI tool;

```
$ docker exec -it {container ID} /bin/bash
```

From within the running container:
```
$ watch nvidia-smi
```

### Credits:
This solution takes inspiration from a few community sources. Thanks to;

[Nvidia driver setup via Docker](https://github.com/StudioPyxis/coreos-nvidia/blob/master/Dockerfile) - Joshua Kolden joshua@studiopyxis.com

[ConvNet demo notebook](https://github.com/ebanner/tensorflow-tutorials/blob/master/mnist/CNN.ipynb) - Edward Banner edward.banner@gmail.com

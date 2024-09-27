# docker

Docker

---

## Podman

```bash

# tests-cpu

podman run -it --rm tensorflow/tensorflow python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
podman run -it --rm tensorflow/tensorflow bash
podman run -it --rm -v $PWD:/tmp -w /tmp tensorflow/tensorflow python ./script.py # run script in current dir
podman run -it --rm -p 8888:8888 tensorflow/tensorflow:latest-jupyter
podman run -it --rm -v $(realpath ~/notebooks):/tf/notebooks -p 8888:8888 tensorflow/tensorflow:latest-jupyter

# tests-gpu

podman run --device nvidia.com/gpu=all -it --rm nvidia/cuda nvidia-smi
podman run --device nvidia.com/gpu=all -it --rm tensorflow/tensorflow:latest-gpu nvidia-smi
podman run --device nvidia.com/gpu=all -it --rm tensorflow/tensorflow:latest-gpu python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
podman run --device nvidia.com/gpu=all -it --rm tensorflow/tensorflow:latest-gpu python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
podman run --device nvidia.com/gpu=all -it --rm tensorflow/tensorflow:latest-gpu bash
podman run --device nvidia.com/gpu=all -it --rm -p 8888:8888 tensorflow/tensorflow:latest-gpu-jupyter
podman run --device nvidia.com/gpu=all -it --rm -v $(realpath ~/notebooks):/tf/notebooks -p 8888:8888 tensorflow/tensorflow:latest-gpu-jupyter

# build

podman build --target="diffusers-dreambooth-dev" -t gabr1elt/diffusers:dreambooth-dev -f "${HOME}/Development/diffusers/examples/dreambooth/docker/Dockerfile" "${HOME}/Development/diffusers/"

# run-cpu

# podman run -it --rm -v "${HOME}/Development/diffusers:/root/Development:cached" -h "difsrs_drmbth-dev" gabr1elt/diffusers:dreambooth-dev
podman run -it --rm -v "${HOME}/Development/diffusers:/root/Development:cached" -p 127.0.0.2:3000:3000 -h "difsrs_drmbth-dev" gabr1elt/diffusers:dreambooth-dev
podman run -it --rm -v "${HOME}/Development/diffusers:/root/Development:cached" -p 127.0.0.2:3000:3000 -p 127.0.0.1:6006:6006 -h "difsrs_drmbth-dev" gabr1elt/diffusers:dreambooth-dev

# run-gpu

# podman run --device nvidia.com/gpu=all -it --rm -v "${HOME}/Development/diffusers:/root/Development:cached" -h "difsrs_drmbth-dev" gabr1elt/diffusers:dreambooth-dev
podman run --device nvidia.com/gpu=all -it --rm -v "${HOME}/Development/diffusers:/root/Development:cached" -p 127.0.0.2:3000:3000 -h "difsrs_drmbth-dev" gabr1elt/diffusers:dreambooth-dev
podman run --device nvidia.com/gpu=all -it --rm -v "${HOME}/Development/diffusers:/root/Development:cached" -p 127.0.0.2:3000:3000 -p 127.0.0.2:6006:6006 -h "difsrs_drmbth-dev" gabr1elt/diffusers:dreambooth-dev


```

# dreambooth-training

DreamBooth training

--------

## Podman

``` bash

podman build -f docker/Dockerfile -t gabr1elt/dreambooth-training-dev

podman run --rm -v "${HOME}/Development/diffusers:/root/Development:cached" -p 127.0.0.1:3000:3000 -it -h "drmbth-training-dev" gabr1elt/dreambooth-training-dev
podman run --rm -v "${HOME}/Development/diffusers:/root/Development:cached" -p 127.0.0.1:3000:3000 -it -h "drmbth-training-dev" gabr1elt/dreambooth-training-dev /bin/bash

```

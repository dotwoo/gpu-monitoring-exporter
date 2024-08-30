# gpu-monitoring-exporter

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

Prometheus exporter for GPU process metrics.

## Metrics

### nvidia_smi_process

Currently running process information on the GPU, including Docker container informations.

#### labels

| name           | description                                                                    | data source |
| -------------- | ------------------------------------------------------------------------------ | ----------- |
| command        | command with all its arguments as a string                                     | ps          |
| container_name | container name of the compute application                                      | docker      |
| cpu            | CPU utilization of the process                                                 | ps          |
| gpu            | index of the GPU                                                               | nvidia_smi  |
| image_name     | image name of the compute application                                          | docker      |
| mem            | ratio of the process's resident set size to the physical memory on the machine | ps          |
| pid            | process id of the compute application                                          | nvidia_smi  |
| process_name   | process Name                                                                   | nvidia_smi  |
| user           | effective user name                                                            | ps          |
| uuid           | globally unique immutable alphanumeric identifier of the GPU                   | nvidia_smi  |

### Output example

```
# HELP nvidia_smi_process Process Info
# TYPE nvidia_smi_process gauge
nvidia_smi_process{command="python /work/cnn_mnist.py",container_name="boring_spence",cpu="133",gpu="0",image_name="tensorflow/tensorflow:latest-gpu",mem="1.0",pid="36301",process_name="python",user="root",uuid="GPU-74c4d80d-b8ae-d50a-d48d-4d7fe273c206"} 1
nvidia_smi_process{command="python /work/cnn_mnist.py",container_name="boring_spence",cpu="134",gpu="1",image_name="tensorflow/tensorflow:latest-gpu",mem="1.0",pid="36301",process_name="python",user="root",uuid="GPU-4c7d1647-2a51-77df-c347-a152b885e29d"} 1
```

## Prerequisites

Use the output of [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface).

- NVIDIA GPU drivers Installed
- nvidia-docker version > 2.0 (see how to [install](https://github.com/NVIDIA/nvidia-docker) and it's [prerequisites](<https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0)#prerequisites>))

## Installation

Clone gpu-monitoring-exporter from the master branch of GitHub repository.

```
git clone https://github.com/yahoojapan/gpu-monitoring-exporter.git
```

## Using Docker

```
cd docker
docker-compose up
```

Bundled docker-compose starts [node_exporter](https://github.com/prometheus/node_exporter).
Metrics can be confirmed as follows:

```
% curl -s localhost:9100/metrics |grep nvidia_smi_

# HELP nvidia_smi_cpu Process Info
# TYPE nvidia_smi_cpu gauge
nvidia_smi_cpu{command="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server --model ",container_name="BMS",gpu="1",image_name="BMS",pid="757516",process_name="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server",user="997",uuid="GPU-305e55aa-680a-a1dd-96f9-0a3ab516e0a8"} 0
nvidia_smi_cpu{command="python3 ./ComfyUI/main.py ",container_name="comfyui",gpu="0",image_name="comfyui-docker:latest",pid="3922759",process_name="python3",user="1000",uuid="GPU-a0029be2-eb4d-2fcf-047c-1279d3a58352"} 0.3
# HELP nvidia_smi_gmem_used Process Info
# TYPE nvidia_smi_gmem_used gauge
nvidia_smi_gmem_used{command="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server --model ",container_name="BMS",gpu="1",image_name="BMS",pid="757516",process_name="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server",user="997",uuid="GPU-305e55aa-680a-a1dd-96f9-0a3ab516e0a8"} 6504
nvidia_smi_gmem_used{command="python3 ./ComfyUI/main.py ",container_name="comfyui",gpu="0",image_name="comfyui-docker:latest",pid="3922759",process_name="python3",user="1000",uuid="GPU-a0029be2-eb4d-2fcf-047c-1279d3a58352"} 384
# HELP nvidia_smi_gmem_util Process Info
# TYPE nvidia_smi_gmem_util gauge
nvidia_smi_gmem_util{command="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server --model ",container_name="BMS",gpu="1",image_name="BMS",pid="757516",process_name="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server",user="997",uuid="GPU-305e55aa-680a-a1dd-96f9-0a3ab516e0a8"} 0
nvidia_smi_gmem_util{command="python3 ./ComfyUI/main.py ",container_name="comfyui",gpu="0",image_name="comfyui-docker:latest",pid="3922759",process_name="python3",user="1000",uuid="GPU-a0029be2-eb4d-2fcf-047c-1279d3a58352"} 0
# HELP nvidia_smi_gpu_util Process Info
# TYPE nvidia_smi_gpu_util gauge
nvidia_smi_gpu_util{command="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server --model ",container_name="BMS",gpu="1",image_name="BMS",pid="757516",process_name="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server",user="997",uuid="GPU-305e55aa-680a-a1dd-96f9-0a3ab516e0a8"} 0
nvidia_smi_gpu_util{command="python3 ./ComfyUI/main.py ",container_name="comfyui",gpu="0",image_name="comfyui-docker:latest",pid="3922759",process_name="python3",user="1000",uuid="GPU-a0029be2-eb4d-2fcf-047c-1279d3a58352"} 0
# HELP nvidia_smi_max_memory_usage Process Info
# TYPE nvidia_smi_max_memory_usage gauge
nvidia_smi_max_memory_usage{command="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server --model ",container_name="BMS",gpu="1",image_name="BMS",pid="757516",process_name="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server",user="997",uuid="GPU-305e55aa-680a-a1dd-96f9-0a3ab516e0a8"} 6504
nvidia_smi_max_memory_usage{command="python3 ./ComfyUI/main.py ",container_name="comfyui",gpu="0",image_name="comfyui-docker:latest",pid="3922759",process_name="python3",user="1000",uuid="GPU-a0029be2-eb4d-2fcf-047c-1279d3a58352"} 384
# HELP nvidia_smi_mem Process Info
# TYPE nvidia_smi_mem gauge
nvidia_smi_mem{command="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server --model ",container_name="BMS",gpu="1",image_name="BMS",pid="757516",process_name="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server",user="997",uuid="GPU-305e55aa-680a-a1dd-96f9-0a3ab516e0a8"} 0.5
nvidia_smi_mem{command="python3 ./ComfyUI/main.py ",container_name="comfyui",gpu="0",image_name="comfyui-docker:latest",pid="3922759",process_name="python3",user="1000",uuid="GPU-a0029be2-eb4d-2fcf-047c-1279d3a58352"} 0.4
# HELP nvidia_smi_process Process Info
# TYPE nvidia_smi_process gauge
nvidia_smi_process{command="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server --model ",container_name="BMS",gpu="1",image_name="BMS",pid="757516",process_name="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server",user="997",uuid="GPU-305e55aa-680a-a1dd-96f9-0a3ab516e0a8"} 1
nvidia_smi_process{command="python3 ./ComfyUI/main.py ",container_name="comfyui",gpu="0",image_name="comfyui-docker:latest",pid="3922759",process_name="python3",user="1000",uuid="GPU-a0029be2-eb4d-2fcf-047c-1279d3a58352"} 1
# HELP nvidia_smi_run_time Process Info
# TYPE nvidia_smi_run_time gauge
nvidia_smi_run_time{command="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server --model ",container_name="BMS",gpu="1",image_name="BMS",pid="757516",process_name="/tmp/ollama1896338632/runners/cuda_v11/ollama_llama_server",user="997",uuid="GPU-305e55aa-680a-a1dd-96f9-0a3ab516e0a8"} 0
nvidia_smi_run_time{command="python3 ./ComfyUI/main.py ",container_name="comfyui",gpu="0",image_name="comfyui-docker:latest",pid="3922759",process_name="python3",user="1000",uuid="GPU-a0029be2-eb4d-2fcf-047c-1279d3a58352"} 0
```

```
# the label values BMS means running on the Bare Metal Server.
container_name="BMS",image_name="BMS"
```

Output only when the process is running on the GPU.

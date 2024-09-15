ARG PYTORCH="2.0.0"
ARG CUDA="11.7"
ARG CUDNN="8"
ARG USER="hannan"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

# To fix GPG key error when running apt-get update
#RUN rm /etc/apt/sources.list.d/cuda.list \
#    && rm /etc/apt/sources.list.d/nvidia-ml.list \
#    && apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub \
#    && apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub \
    && apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

# Install system dependencies for opencv-python
RUN apt-get update && apt-get install -y libgl1 libglib2.0-0 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install mmcv
ARG MMCV="2.1.0"
RUN if [ "${MMCV}" = "" ]; then pip install -U openmim && mim install 'mmcv>=2.0.0rc1'; else pip install -U openmim && mim install mmcv==${MMCV}; fi

# Verify the installation
RUN python -c 'import mmcv;print(mmcv.__version__)'

RUN useradd -ms /bin/bash ${USER}
WORKDIR /home/${USER}

RUN apt-get update \
    && apt-get install -y wget \
    sudo \
    gnupg \
    git vim

RUN git config --global user.email "github-email-id"
RUN git config --global user.name "github-usernam"

RUN git clone https://github.com/HannanEjazKeen/mmdetection.git
RUN git clone https://github.com/HannanEjazKeen/mmsegmentation.git

RUN pip install ftfy regex future tensorboard

WORKDIR /home/${USER}/mmdetection

RUN pip install -v -e .
RUN python -m torch.utils.collect_env

WORKDIR /home/${USER}/mmsegmentation

RUN pip install -v -e .
RUN python -m torch.utils.collect_env

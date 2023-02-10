FROM nvcr.io/nvidia/pytorch:22.08-py3

ENV DEBIAN_FRONTEND=noninteractive
ENV PIP_ROOT_USER_ACTION=ignore
ENV FORCE_CUDA=1

RUN apt-get update && apt install -y ffmpeg curl 
RUN pip install --upgrade pip
RUN pip install pip install torch torchvision torchaudio
RUN pip install "git+https://github.com/facebookresearch/pytorch3d.git"


RUN pip install "mmcv-full>=1.3.17,<=1.5.3" -f https://download.openmmlab.com/mmcv/dist/cu117/torch1.13.1+cu117/index.html 
RUN git clone https://github.com/open-mmlab/mmhuman3d.git 

RUN pip install -r /workspace/mmhuman3d/requirements/runtime.txt

RUN sed -i "s/SyncBN/BN/g" /workspace/mmhuman3d/configs/hmr/resnet50_hmr_pw3d.py

RUN pip install vedo --no-dependencies 
RUN pip install mmdet mmpose mmcls mmtrack     
RUN pip install -v -e /workspace/mmhuman3d/ 
RUN pip uninstall -y opencv-contrib-python opencv-contrib-python-headless opencv-python
RUN pip install opencv-contrib-python==4.5.5.62


RUN mkdir -p /workspace/mmhuman3d/data \
    /workspace/mmhuman3d/data/body_models \
    /workspace/mmhuman3d/data/body_models/smpl \
    /workspace/mmhuman3d/data/checkpoints \
    /workspace/mmhuman3d/output \
    /workspace/mmhuman3d/vis_results

RUN wget -O /workspace/mmhuman3d/data/checkpoints/resnet50_hmr_pw3d.pth https://openmmlab-share.oss-cn-hangzhou.aliyuncs.com/mmhuman3d/models/hmr/resnet50_hmr_pw3d-04f40f58_20211201.pth?versionId=CAEQHhiBgMD6zJfR6xciIDE0ODQ3OGM2OWJjMTRlNmQ5Y2ZjMWZhMzRkOTFiZDFm  
    
COPY basicModel_f_lbs_10_207_0_v1.0.0.pkl /workspace/mmhuman3d/data/body_models/smpl/SMPL_FEMALE.pkl 
COPY basicmodel_m_lbs_10_207_0_v1.0.0.pkl /workspace/mmhuman3d/data/body_models/smpl/SMPL_MALE.pkl 
COPY basicModel_neutral_lbs_10_207_0_v1.0.0.pkl /workspace/mmhuman3d/data/body_models/smpl/SMPL_NEUTRAL.pkl
COPY smpl_mean_params.npz /workspace/mmhuman3d/data/body_models/
COPY J_regressor_extra.npy /workspace/mmhuman3d/data/body_models/
COPY J_regressor_h36m.npy /workspace/mmhuman3d/data/body_models/
 
WORKDIR /workspace/mmhuman3d



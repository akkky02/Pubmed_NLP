# hash:sha256:4daf4f4a2b5c154f9b46d78fcb22261adacce09ffbde1f263e8f10fdb148f8af
FROM registry.hackathon2023.radr-ai.com/codeocean/tensorflow:2.11-python3.10-cuda11.7.0-runtime-ubuntu22.04

ARG DEBIAN_FRONTEND=noninteractive

RUN conda install -y \
        jupyter==1.0.0 \
        jupyterlab==4.0.8 \
        matplotlib==3.7.1 \
        numpy==1.26.0 \
        pandas==2.1.3 \
        scikit-learn==1.3.2 \
        seaborn==0.12.2 \
    && conda clean -ya

RUN pip install -U --no-cache-dir \
    drug-named-entity-recognition==1.0.2 \
    spacy==3.7.2

RUN pip3 install -U --no-cache-dir \
    sacremoses==0.1.1 \
    tenacity==8.2.3 \
    torch==2.1.0 \
    tqdm==4.66.1 \
    transformers==4.35.1

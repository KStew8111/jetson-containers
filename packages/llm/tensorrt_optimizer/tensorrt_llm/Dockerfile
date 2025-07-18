#---
# name: tensorrt_llm
# group: llm
# config: config.py
# depends: [tensorrt, pytorch, transformers, cuda-python, nixl]
# test: [test.py]
# requires: '>=35'
# notes: The `tensorrt-llm:builder` container includes the C++ binaries under `/opt`
#---

ARG BASE_IMAGE
FROM ${BASE_IMAGE}

ARG TRT_LLM_VERSION \
    TRT_LLM_BRANCH \
    TRT_LLM_SOURCE \
    TRT_LLM_PATCH \
    CUDA_ARCHS \
    CUDA_VERSION \
    FORCE_BUILD="off" \
    BUILD_DIR="/opt/TensorRT-LLM/cpp/build" \
    SOURCE_DIR="/opt/TensorRT-LLM" \
    SOURCE_TAR="/tmp/TensorRT-LLM/source.tar.gz" \
    GIT_PATCHES="/tmp/TensorRT-LLM/patch.diff" \
    TMP_DIR="/tmp/TensorRT-LLM/"

COPY ${TRT_LLM_PATCH} ${GIT_PATCHES}
COPY sources ${TMP_DIR}
COPY build.sh install.sh /tmp/setup/

RUN /tmp/setup/install.sh || /tmp/setup/build.sh #|| echo "BUILD FAILED!"

RUN pip3 install --upgrade transformers && \
    pip3 install uvicorn fastapi

COPY llama.sh ${SOURCE_DIR}/

# check python version and save variable PYTHON_VERSION
RUN PYTHON_TAG=$(python3 -c "import sys; print('{}.{}'.format(sys.version_info.major, sys.version_info.minor))") \
    && echo "PYTHON_TAG=${PYTHON_TAG}"
ARG PYTHON_VERSION=${PYTHON_TAG}

# name '_device_get_memory_info_fn' is not defined
COPY patches/profiler.py /usr/local/lib/python${PYTHON_VERSION}/dist-packages/tensorrt_llm/profiler.py
COPY patches/convert_utils.py /usr/local/lib/python${PYTHON_VERSION}/dist-packages/tensorrt_llm/models/convert_utils.py

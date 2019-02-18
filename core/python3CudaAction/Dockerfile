#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

# Dockerfile for python AI actions, overrides and extends ActionRunner from actionProxy
# FROM tensorflow/tensorflow:1.11.0-py3
# FROM tensorflow/tensorflow:1.12.0-gpu-py3
# FROM nvidia/cuda:9.0-cudnn7-runtime
FROM nvidia/cuda:9.0-base

RUN apt-get update && apt-get install -y --no-install-recommends \
        curl \
        rsync \
        software-properties-common \
        unzip
        # && \
    # apt-get clean && \
    # rm -rf /var/lib/apt/lists/*

RUN add-apt-repository -y ppa:deadsnakes/ppa
RUN apt-get update && apt-get install -y --no-install-recommends \
        python3.6 \
        # python3-dev \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN curl -O https://bootstrap.pypa.io/get-pip.py && \
    python3.6 get-pip.py && \
    rm get-pip.py

COPY requirements.txt requirements.txt
RUN pip install --upgrade pip six && pip install --no-cache-dir -r requirements.txt
RUN pip install torch torchvision

RUN mkdir -p /actionProxy
ADD https://raw.githubusercontent.com/apache/incubator-openwhisk-runtime-docker/dockerskeleton%401.3.3/core/actionProxy/actionproxy.py /actionProxy/actionproxy.py

RUN mkdir -p /pythonAction
ADD https://raw.githubusercontent.com/apache/incubator-openwhisk-runtime-python/3%401.0.3/core/pythonAction/pythonrunner.py /pythonAction/pythonrunner.py

CMD ["/bin/bash", "-c", "cd /pythonAction && python3.6 -u pythonrunner.py"]

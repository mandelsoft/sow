# Copyright 2019 Copyright (c) 2019 SAP SE or an SAP affiliate company. All rights reserved. This file is licensed under the Apache Software License, v. 2 except as noted otherwise in the LICENSE file.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM alpine:3.13
ARG ARCH=amd64
ARG GITHUB_PROFILE=ezeeyahoo

RUN apk update && apk add --no-cache bash curl libc6-compat findutils apache2-utils
RUN apk add git~=2
RUN apk add terraform~=0.14
RUN apk add jq~=1.6
RUN curl -L -o /usr/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/v1.18.18/bin/linux/${ARCH}/kubectl \
    && chmod +x /usr/bin/kubectl
RUN if [ "$ARCH" = "amd64" ] ; then curl -L -o kustomize-archive.tar.gz https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2Fv3.5.4/kustomize_v3.5.4_linux_${ARCH}.tar.gz; \
    else curl -L -o kustomize-archive.tar.gz https://github.com/${GITHUB_PROFILE}/kustomize/releases/download/kustomize%2Fv3.5.4/kustomize_v3.5.4_linux_${ARCH}.tar.gz; fi \
    && mkdir kustomize-extract \
    && tar -xzf kustomize-archive.tar.gz -C kustomize-extract \
    && cp kustomize-extract/kustomize /usr/bin/kustomize \
    && rm -rf kustomize-archive.tar.gz kustomize-extract \
    && chmod +x /usr/bin/kustomize
RUN curl -L -o helm-archive.tar.gz https://get.helm.sh/helm-v3.2.4-linux-${ARCH}.tar.gz \
    && mkdir helm-extract \
    && tar -xzf helm-archive.tar.gz -C helm-extract \
    && cp helm-extract/linux-${ARCH}/helm /usr/bin/helm \
    && rm -rf helm-archive.tar.gz helm-extract \
    && chmod +x /usr/bin/helm
RUN if [ "$ARCH" = "amd64" ] ; then curl -L -o spiff-archive.zip https://github.com/mandelsoft/spiff/releases/download/v1.6.1/spiff_linux_${ARCH}.zip; \
    else curl -L -o spiff-archive.zip https://github.com/${GITHUB_PROFILE}/spiff/releases/download/v1.6.1/spiff_linux_${ARCH}.zip; fi \
    && mkdir spiff-extract \
    && unzip -d spiff-extract spiff-archive.zip \
    && cp "spiff-extract/spiff++" /usr/bin/spiff \
    && rm -rf spiff-archive.zip spiff-extract \
    && chmod +x /usr/bin/spiff
COPY . /sow
ENV PATH=$PATH:/sow/bin
ENTRYPOINT ["sow"]

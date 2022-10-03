# Copyright 2021 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM ubuntu as builder
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt install -y autoconf automake libssl-dev git libcunit1 libcunit1-dev bison libbison-dev cmake clang

RUN git clone https://github.com/eclipse-cyclonedds/cyclonedds.git
WORKDIR /cyclonedds
RUN mkdir build
WORKDIR build
RUN CC=clang CXX=clang++ cmake -DCMAKE_INSTALL_PREFIX=/cyclonedds/install -DBUILD_IDLC=ON ..
RUN cmake --build .
RUN cmake --build . --target install

COPY . /cyclonedds-cxx

WORKDIR /cyclonedds-cxx
RUN mkdir build
WORKDIR build
RUN CC=clang CXX=clang++ cmake -DCMAKE_PREFIX_PATH=/cyclonedds/install -DCMAKE_INSTALL_PREFIX=/cyclonedds-cxx/install -DBUILD_FUZZING=ON ..
RUN cmake --build .

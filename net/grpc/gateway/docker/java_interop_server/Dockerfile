# Copyright 2018 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM maven:3.6.1-jdk-8

RUN apt-get -qq update && apt-get -qq install -y \
  unzip

WORKDIR /tmp

RUN curl -sSL https://github.com/protocolbuffers/protobuf/releases/download/v3.14.0/\
protoc-3.14.0-linux-x86_64.zip -o protoc.zip && \
  unzip -qq protoc.zip && \
  cp ./bin/protoc /usr/local/bin/protoc

RUN curl -sSL https://github.com/grpc/grpc-web/releases/download/1.4.1/\
protoc-gen-grpc-web-1.4.1-linux-x86_64 -o /usr/local/bin/protoc-gen-grpc-web && \
  chmod +x /usr/local/bin/protoc-gen-grpc-web

WORKDIR /var/www/html/dist

WORKDIR /github/grpc-web

RUN git clone https://github.com/grpc/grpc-web . && \
  cd src/connector && \
  mvn install --no-transfer-progress && \
  cd ../../net/grpc/gateway/examples/grpc-web-java/interop-test-service && \
  mvn package --no-transfer-progress

ENTRYPOINT ["java", "-cp", "net/grpc/gateway/examples/grpc-web-java/interop-test-service/target/interop-test-0.1-jar-with-dependencies.jar", "grpcweb.examples.StartServiceAndGrpcwebProxy"]

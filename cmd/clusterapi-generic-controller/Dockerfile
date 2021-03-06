# Copyright 2018 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Reproducible builder image
FROM golang:1.10.0 as builder
ENV GOPATH /go
WORKDIR /go/src/sigs.k8s.io/cluster-api-provider-tencent

# This expects that the context passed to the docker build command is
# the cluster-api directory.
# e.g. docker build -t <tag> -f <this_Dockerfile> <path_to_cluster-api>
COPY . .

#RUN CGO_ENABLED=0 GOOS=linux go install -a -ldflags '-extldflags "-static"' clusterapi-tke-controller/cmd/manager
#RUN CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-extldflags "-static"' -x -gcflags="-N -l" -o /go/bin/clusterapi-generic-controller sigs.k8s.io/cluster-api-provider-tencent/cmd/clusterapi-generic-controller
RUN CGO_ENABLED=0 GOOS=linux make clusterapi-generic-controller
# Final container
#FROM debian:stretch-slim
FROM scratch
ADD config/cert/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /go/src/sigs.k8s.io/cluster-api-provider-tencent/output/bin/clusterapi-generic-controller /
CMD ["/clusterapi-generic-controller", "--logtostderr"]

#  Fire-Flyer File System

[![Build](https://github.com/deepseek-ai/3fs/actions/workflows/build.yml/badge.svg)](https://github.com/deepseek-ai/3fs/actions/workflows/build.yml)
[![License](https://img.shields.io/badge/LICENSE-MIT-blue.svg)](LICENSE)

The Fire-Flyer File System (3FS) is a high-performance distributed file system designed to address the challenges of AI training and inference workloads. It leverages modern SSDs and RDMA networks to provide a shared storage layer that simplifies development of distributed applications. Key features and benefits of 3FS include:

- Performance and Usability
  - **Disaggregated Architecture** Combines the throughput of thousands of SSDs and the network bandwidth of hundreds of storage nodes, enabling applications to access storage resource in a locality-oblivious manner.
  - **Strong Consistency** Implements Chain Replication with Apportioned Queries (CRAQ) for strong consistency, making application code simple and easy to reason about.
  - **File Interfaces** Develops stateless metadata services backed by a transactional key-value store (e.g., FoundationDB). The file interface is well known and used everywhere. There is no need to learn a new storage API.

- Diverse Workloads
  - **Data Preparation** Organizes outputs of data analytics pipelines into hierarchical directory structures and manages a large volume of intermediate outputs efficiently.
  - **Dataloaders** Eliminates the need for prefetching or shuffling datasets by enabling random access to training samples across compute nodes.
  - **Checkpointing** Supports high-throughput parallel checkpointing for large-scale training.
  - **KVCache for Inference** Provides a cost-effective alternative to DRAM-based caching, offering high throughput and significantly larger capacity.

## Documentation

* [Design Notes](docs/design_notes.md)
* [Setup Guide](deploy/README.md)
* [USRBIO API Reference](src/lib/api/UsrbIo.md)
* [P Specifications](./specs/README.md)

## Performance

### 1. Peak throughput

The following figure demonstrates the throughput of read stress test on a large 3FS cluster. This cluster consists of 180 storage nodes, each equipped with 2×200Gbps InfiniBand NICs and sixteen 14TiB NVMe SSDs. Approximately 500+ client nodes were used for the read stress test, with each client node configured with 1x200Gbps InfiniBand NIC. The final aggregate read throughput reached approximately 6.6 TiB/s with background traffic from training jobs.

![Large block read throughput under stress test on a 180-node cluster](docs/images/peak_throughput.jpg)

To benchmark 3FS, please use our [fio engine for USRBIO](benchmarks/fio_usrbio/README.md).

### 2. GraySort

We evaluated [smallpond](https://github.com/deepseek-ai/smallpond) using the GraySort benchmark, which measures sort performance on large-scale datasets. Our implementation adopts a two-phase approach: (1) partitioning data via shuffle using the prefix bits of keys, and (2) in-partition sorting. Both phases read/write data from/to 3FS.

The test cluster comprised 25 storage nodes (2 NUMA domains/node, 1 storage service/NUMA, 2×400Gbps NICs/node) and 50 compute nodes (2 NUMA domains, 192 physical cores, 2.2 TiB RAM, and 1×200 Gbps NIC/node). Sorting 110.5 TiB of data across 8,192 partitions completed in 30 minutes and 14 seconds, achieving an average throughput of *3.66 TiB/min*.

![](docs/images/gray_sort_server.png)
![](docs/images/gray_sort_client.png)

### 3. KVCache

KVCache is a technique used to optimize the LLM inference process. It avoids redundant computations by caching the key and value vectors of previous tokens in the decoder layers.
The top figure demonstrates the read throughput of all KVCache clients (1×400Gbps NIC/node), highlighting both peak and average values, with peak throughput reaching up to 40 GiB/s. The bottom figure presents the IOPS of removing ops from garbage collection (GC) during the same time period.

![KVCache Read Throughput](./docs/images/kvcache_read_throughput.png)
![KVCache GC IOPS](./docs/images/kvcache_gc_iops.png)

## Check out source code

Clone 3FS repository from GitHub:

	git clone https://github.com/deepseek-ai/3fs

When `deepseek-ai/3fs` has been cloned to a local file system, run the
following commands to check out the submodules:

```bash
$ cd 3fs
$ git submodule update --init --recursive
$ ./patches/apply.sh
```

## Install dependencies

Install dependencies:

```bash
# for Ubuntu 20.04.
apt install cmake libuv1-dev liblz4-dev liblzma-dev libdouble-conversion-dev libdwarf-dev libunwind-dev \
  libaio-dev libgflags-dev libgoogle-glog-dev libgtest-dev libgmock-dev clang-format-14 clang-14 clang-tidy-14 lld-14 \
  libgoogle-perftools-dev google-perftools libssl-dev libclang-rt-14-dev gcc-10 g++-10 libboost1.71-all-dev

# for Ubuntu 22.04.
apt install cmake libuv1-dev liblz4-dev liblzma-dev libdouble-conversion-dev libdwarf-dev libunwind-dev \
  libaio-dev libgflags-dev libgoogle-glog-dev libgtest-dev libgmock-dev clang-format-14 clang-14 clang-tidy-14 lld-14 \
  libgoogle-perftools-dev google-perftools libssl-dev gcc-12 g++-12 libboost-all-dev

# for openEuler 2403sp1
yum install cmake libuv-devel lz4-devel xz-devel double-conversion-devel libdwarf-devel libunwind-devel \
    libaio-devel gflags-devel glog-devel gtest-devel gmock-devel clang-tools-extra clang lld \
    gperftools-devel gperftools openssl-devel gcc gcc-c++ boost-devel
```

Install other build prerequisites:

- [`libfuse`](https://github.com/libfuse/libfuse/releases/tag/fuse-3.16.1) 3.16.1 or newer version
- [FoundationDB](https://apple.github.io/foundationdb/getting-started-linux.html) 7.1 or newer version
- [Rust](https://www.rust-lang.org/tools/install) toolchain: minimal 1.75.0, recommended 1.85.0 or newer version (latest stable version) 

## Install prerequisites:
### 1. Rust 
#### sslVerify 
```sh
$ sudo apt install curl wget 
$ git config --global http.sslVerify false
$ git config --global --get http.sslVerify

$ cat ~/.wgetrc
check-certificate=off

$ cat ~/.curlrc
insecure
```
#### 인증서 update
```sh
$ sudo apt update && sudo apt install --reinstall ca-certificates
```

#### Rust
```sh
$ curl --insecure --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

* 수동설치 
$ wget --no-check-certificate https://sh.rustup.rs -O rustup.init.w.sh
$ sh rustup-init.sh 

$ curl --insecure https://sh.rustup.rs  -sSf > rustup.init.c.sh
```
```sh
$ . "$HOME/.cargo/env"   
$ rustup update
$ rustup --version
$ cargo --version
$ rustc --version
```

#### Rust 수동 설치 
```sh
$ curl -L https://github.com/rust-lang/rustup/archive/refs/tags/1.26.0.tar.gz -o rustup.tar.gz
```

#### rust 에러 조치 방법
* 에러 현상
```노
Rustup이 http://static.rust-lang.org/dist/channel-rust-stable.toml.sha256 파일을 다운로드하는 과정에서 TLS 인증서 오류(Invalid Peer Certificate: BadSignature)가 발생한 것입니다.
```

* ssh 접근 테스트 해보기 
```sh
$ curl -v https://static.rust-lang.org/dist/channel-rust-stable.toml.sha256
=> 출력 결과에서 "certificate verification failed" 같은 오류가 나타나면 네트워크 문제입니다. 
=> 이러면 수작업으로 파일 받아서 설치 해야 한다. 
```

* 임시로 https 인증서 검증을 강제 비활성화
```sh
export RUSTUP_USE_HYPER=1
export CARGO_HTTP_CAINFO=
export CARGO_HTTP_CHECK_REVOKE=false
rustup update
```
* 사설 인증서 설치
```sh
sudo apt update && sudo apt install --reinstall ca-certificates
```
* 직접설치
```sh
$ curl -O https://sh.rustup.rs
$ sh rustup-init.sh --no-modify-path
$ source $HOME/.cargo/env

$ export RUSTUP_DIST_SERVER=https://static.rust-lang.org
$ export RUSTUP_UPDATE_ROOT=https://static.rust-lang.org/rustup

$ rustup default stable
$ rustup update
$ rustup --version
$ cargo --version
$ rustc --version
```


### 2. FoundationDb
* download & install  
```sh
$ wget --no-check-certificate  https://github.com/apple/foundationdb/releases/download/6.3.23/foundationdb-clients_6.3.23-1_amd64.deb
$ wget --no-check-certificate  https://github.com/apple/foundationdb/releases/download/6.3.23/foundationdb-server_6.3.23-1_amd64.deb
$ sudo dpkg -i  foundationdb-clients_6.3.23-1_amd64.deb foundationdb-server_6.3.23-1_amd64.deb

```
### 3. Fuse 3.16
```sh
$ wget --no-check-certificate https://github.com/libfuse/libfuse/releases/download/fuse-3.16.1/fuse-3.16.1.tar.gz
$ gunzip fuse-3.16.1.tar.gz
$ tar xvf gunzip fuse-3.16.1.tar.gz
$ sudo apt install -y meson ninja-build pkg-config libfuse3-dev
$ meson setup build
$ meson setup --wipe build   #<-- remove
$ meson setup --reconfigure  build  #<-- rebuild
$ cd build
$ ninja 
$ sudo ninja install 
$ fusermount3 --version 
$ lsmod  | grep fuse
$ sudo usermod -aG fuse $(whoami)
$ newgrp fuse
```
* fuse github
```sh
$ sudo apt update
$ sudo apt install -y meson ninja-build pkg-config libfuse3-dev
$ git clone https://github.com/libfuse/libfuse.git
$ cd libfuse
$ meson setup build
$ cd build
$ ninja
$ sudo ninja install
$ fusermount3 --version
$ lsmod | grep fuse
$ sudo usermod -aG fuse $(whoami)
$ newgrp fuse
```
#### 1. meson
Meson : Meson은 고속 빌드 시스템으로, 소프트웨어 프로젝트를 구성하고 빌드하는 도구입니다.
* 빠르고 효율적: 기존의 autotools나 CMake보다 빌드 속도가 빠름
* 간단한 문법: meson.build라는 설정 파일을 사용하여 프로젝트를 쉽게 정의 가능
* 다양한 언어 지원: C, C++, Rust, Python 등 여러 언어 지원
* Ninja 기반 빌드: 기본적으로 Ninja 빌드 시스템을 사용하여 컴파일
#### 2. Ninja
Ninja는 경량 빌드 시스템으로, Meson과 같은 빌드 도구가 생성한 빌드 파일을 실행하는 역할을 합니다.
* 속도가 빠름: Make보다 훨씬 빠르게 실행됨
* 간단한 빌드 파일: Meson, CMake 등의 빌드 시스템이 자동으로 Ninja 빌드 파일을 생성
* 병렬 빌드 지원: 멀티코어 CPU를 활용하여 빠른 컴파일 가능


Checking whether type "struct stat" has member "st_atimespec" : NO 
Library iconv found: NO
Found Pkg-config: NO
Run-time dependency udev found: NO (tried pkgconfig and cmake)



## Build 3FS

* Build 3FS in `build` folder:
```sh
$ cmake -S . -B build -DCMAKE_CXX_COMPILER=clang++-14 -DCMAKE_C_COMPILER=clang-14 -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCMAKE_EXPORT_COMPILE_COMMANDS=ON
$ cmake --build build -j 32
```

## Run a test cluster

Follow instructions in [setup guide](deploy/README.md) to run a test cluster.

## Report Issues

Please visit https://github.com/deepseek-ai/3fs/issues to report issues.

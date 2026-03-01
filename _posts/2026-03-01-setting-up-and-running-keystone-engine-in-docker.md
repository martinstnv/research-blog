---
layout: single
title: Setting Up and Running Keystone Engine in Docker
date: 2026-03-01
classes: wide
---

> Setting up the Keystone Engine on macOS can be tricky due to dynamic library issues and other architecture issues. Docker solves this by running everything in a clean, reproducible Linux container.

Create the following Dockerfile to build an environment with Python and Keystone Engine installed:

```dockerfile
FROM python:3.11-slim

RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/keystone-engine/keystone.git /keystone
WORKDIR /keystone/build
RUN cmake .. -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=ON \
    && make \
    && make install

RUN pip install keystone-engine

WORKDIR /workspace
```

Build the Docker image with the following command:

```sh
docker build -t keystone-python .
```

Create an example Python script to assemble x86 instructions:

```py
from keystone import Ks, KS_ARCH_X86, KS_MODE_32

code = """
start:
    nop
    nop
    nop
    nop
"""

ks = Ks(KS_ARCH_X86, KS_MODE_32)

instructions, count = ks.asm(code)
hex_instructions    = ""

for instruction in instructions:
    hex_instructions += "\\x{0:02x}".format(int(instruction)).rstrip("\n")

print(hex_instructions)
```

Run your script inside the Docker container:

```sh
docker run --rm -v $(pwd):/workspace keystone-python python script.py
```

Alternatively, you can use the prebuilt Docker image from Docker Hub:

```sh
docker pull martinstoynov/keystone-python:latest
docker run --rm -v $(pwd):/workspace martinstoynov/keystone-python python script.py
```
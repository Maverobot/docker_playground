FROM ubuntu:20.04

# Disable Prompt During Packages Installation
ARG DEBIAN_FRONTEND=noninteractive

RUN groupadd -g 1001 distcc && useradd -r -u 1001 -g distcc distcc

RUN apt-get update && apt-get install -y software-properties-common

RUN add-apt-repository ppa:ubuntu-toolchain-r/test

RUN apt-get update && apt-get install -y \
    build-essential ccache distcc gcc-10 g++-10\
    && rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 70 \
    --slave /usr/bin/g++ g++ /usr/bin/g++-10 --slave /usr/bin/gcov gcov /usr/bin/gcov-10

RUN ln -s /usr/bin/c++ /usr/lib/distcc/
RUN ln -s /usr/bin/cc /usr/lib/distcc/

USER distcc

ENTRYPOINT [\
    "distccd", \
    "--daemon", \
    "--no-detach", \
    "--port", "3632", \
    "--stats", \
    "--stats-port", "3633", \
    "--log-stderr", \
    "--listen", "0.0.0.0"\
]

# By default the distcc server will accept clients from everywhere.
# Feel free to run the docker image with different values for the
# following params.
CMD [\
    "--allow", "0.0.0.0/0", \
    "--nice", "5", \
    "--jobs", "8" \
]

# TODO: allow configuring other compilers other than gcc/g++-10

# Usage:
# Install:
#   sudo add-apt-repository ppa:ubuntu-toolchain-r/test
#   sudo apt install ccache distcc build-essential ccache distcc gcc-10 g++-10
#   sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 70 --slave /usr/bin/g++ g++ /usr/bin/g++-10 --slave /usr/bin/gcov gcov /usr/bin/gcov-10
# Add the line into the .bashrc/config.fish/.zshrc:
#   export DISTCC_HOSTS="<ip-of-the-docker-machine>/7 localhost/4"
# Build cmake project with:
#   cmake .. -DCMAKE_C_COMPILER_LAUNCHER="ccache;distcc" -DCMAKE_CXX_COMPILER_LAUNCHER="ccache;distcc" && cmake --build . -- -j $(distcc -j)

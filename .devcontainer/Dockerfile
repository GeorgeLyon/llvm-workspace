FROM swiftlang/swift:nightly-main-jammy

ENV DEBIAN_FRONTEND="noninteractive"
ENV LC_ALL="C"

# Install Dependencies
RUN apt-get update
RUN apt-get install -y \
      clang \
      llvm \
      make \ 
      ninja-build

# Build CMake (the version from apt-get installs GCC, the presence of which sometimes confuses LLVM's build)
ADD https://github.com/Kitware/CMake/releases/download/v3.23.2/cmake-3.23.2.tar.gz /tools/cmake.tar.gz
RUN cd /tools && \
    tar -xf cmake.tar.gz && \
    cd cmake-3.23.2 && \
    ./bootstrap \
      --parallel=12 \
      -- \
      -DCMAKE_USE_OPENSSL:BOOL=OFF \
      -DCMAKE_BUILD_TYPE:STRING=Release \
      -DCMAKE_BUILD_LTO:BOOL=ON && \
    make && \
    make install

# Use LLVM with CMake
COPY cmake-tools-kits.json /root/.local/share/CMakeTools/cmake-tools-kits.json

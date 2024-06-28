FROM rockylinux:9

RUN dnf upgrade -y; \
    dnf install --enablerepo=crb -y \
        git \
        cmake \
        gcc \
        perl \
        python3 \
        mesa-libGL-devel \
        mesa-libEGL-devel \
        libXrender-devel \
        *xcb* \
        xcb* \
        *xkb* \
        fontconfig-devel \
        freetype-devel \
        libXext-devel \
        libX11-devel \
        libSM-devel \
        libICE-devel \
        glib2-devel \
        libinput-devel \
        libinput-utils \
        xorg-x11-drv-libinput-devel \
        libXfixes-devel \
        libXi-devel \
        libcurl-devel \
        wget \
        xz; \
    curl -LO https://github.com/ninja-build/ninja/releases/download/v1.12.1/ninja-linux.zip; \
    unzip ninja-linux.zip; \
    install ninja /usr/local/bin; \
    rm ninja-linux.zip

RUN mkdir /qt6-src; \
    cd /qt6-src; \
    wget https://download.qt.io/archive/qt/6.2/6.2.8/submodules/qtbase-everywhere-opensource-src-6.2.8.tar.xz; \
    tar -xf qtbase-everywhere-opensource-src-6.2.8.tar.xz; \
    mkdir qtbase-build; \
    cd qtbase-build; \
    ../qtbase-everywhere-src-6.2.8/configure -xcb -prefix /qt6; \
    cmake --build . --parallel; \
    cmake --install .; \
    cd ../; \
    rm qtbase-everywhere-opensource-src-6.2.8.tar.xz; \
    rm -rf qtbase-everywhere-src-6.2.8.tar.xz

RUN cd /qt6-src; \
    wget https://download.qt.io/archive/qt/6.2/6.2.8/submodules/qtdeclarative-everywhere-opensource-src-6.2.8.tar.xz; \
    tar -xf qtdeclarative-everywhere-opensource-src-6.2.8.tar.xz; \
    cd qtdeclarative-everywhere-src-6.2.8; \
    cmake -G Ninja -DCMAKE_PREFIX_PATH=/qt6/lib/cmake; \
    cmake --build . --parallel; \
    cmake --install .; \
    cd ../; \
    rm qtdeclarative-everywhere-opensource-src-6.2.8.tar.xz; \
    rm -rf qtdeclarative-everywhere-src-6.2.8

RUN git clone -b v5.2.1 https://github.com/InsightSoftwareConsortium/ITK itk-src; \
    mkdir itk-build; \
    cd itk-build; \
    cmake -GNinja \
        -DCMAKE_RELEASE_TYPE=Release \
        -DModule_MorphologicalContourInterpolation:BOOL=ON \
        -DBUILD_EXAMPLES:BOOL=OFF \
        -DBUILD_TESTING:BOOL=OFF \
        -DCMAKE_INSTALL_PREFIX=/itk \
        ../itk-src; \
    cmake --build . --parallel

RUN git clone -b v9.1.0 --recursive https://gitlab.kitware.com/vtk/vtk.git /vtk-src; \
    mkdir vtk-build; \
    cd vtk-build; \
    cmake -GNinja \
        -DCMAKE_BUILD_TYPE=Release \
        -DBUILD_SHARED_LIBS=OFF \
        -DVTK_USE_SYSTEM_LIBRARIES=ON \
        -DVTK_RENDERING_BACKEND=OpenGL2 \
        -DBUILD_TESTING=OFF \
        -DBUILD_EXAMPLES=OFF \
        -DCMAKE_PREFIX_PATH=/qt6/lib/cmake \
        -DVTK_GROUP_ENABLE_Qt:BOOL=YES \
        -DVTK_MODULE_ENABLE_VTK_GUISupportQtQuick:BOOL=NO \
        -DVTK_MODULE_ENABLE_VTK_GUISupportQtSQL:BOOL=NO \
        -DVTK_REQUIRED_OBJCXX_FLAGS="" \
        -DCMAKE_INSTALL_PREFIX=/vtk \
        ../vtk-src; \
    cmake --build . --parallel

RUN git clone --recursive https://github.com/pyushkevich/itksnap.git itksnap-src; \
    cd itksnap-src; \
    git checkout 0d79853; \
    cd /; \
    mkdir itksnap-build; \
    cd itksnap-build; \
    cmake -DCMAKE_PREFIX_PATH=/qt6/lib/cmake -DITK_DIR=/itk-build -DVTK_DIR=/vtk-build ../itksnap-src/; \ 
    cmake --build . --parallel 7; \
    make package

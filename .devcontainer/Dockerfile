FROM ros:jazzy-ros-base

ENV DEBIAN_FRONTEND=noninteractive

# 1. 필수 시스템 빌드 도구 및 MuJoCo 렌더링/GUI 의존성(OpenGL, GLFW, Mesa 등) 설치
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    python3-pip \
    python3-venv \
    python3-colcon-common-extensions \
    wget \
    nano \
    curl \
    sudo \
    libgl1-mesa-dev \
    libglew-dev \
    libosmesa6-dev \
    libglfw3 \
    libglfw3-dev \
    patchelf \
    ffmpeg \
    && rm -rf /var/lib/apt/lists/*

# 2. 파이썬 기반 MuJoCo 및 로보틱스 관련 라이브러리 설치
RUN python3 -m venv --system-site-packages /opt/venv \
    && /opt/venv/bin/pip install --no-cache-dir \
    mujoco \
    numpy \
    scipy \
    matplotlib \
    mediapy \
    gymnasium

ENV PATH="/opt/venv/bin:${PATH}"

# 3. UID 1000 충돌 방지 및 일반 사용자(ros) 생성
ARG USERNAME=ros
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN if id -u $USER_UID > /dev/null 2>&1; then \
        EXISTING_USER=$(getent passwd $USER_UID | cut -d: -f1); \
        usermod -l $USERNAME -d /home/$USERNAME -m $EXISTING_USER; \
        groupmod -n $USERNAME $EXISTING_USER; \
    else \
        groupadd --gid $USER_GID $USERNAME && \
        useradd --uid $USER_UID --gid $USER_GID -m $USERNAME; \
    fi \
    && echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME
WORKDIR /home/$USERNAME

# 4. ROS 2 환경변수 자동 로드 설정
RUN echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
RUN mkdir -p /home/$USERNAME/Workspaces

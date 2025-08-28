FROM osrf/ros:noetic-desktop-full

# ================== Environment Variables ==================
ENV DEBIAN_FRONTEND=noninteractive \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    SHELL=/bin/bash \
    USER=arg \
    UID=1000 \
    HOME=/home/arg

# ================== Create User ==================
RUN adduser --disabled-password --gecos "Default user" --uid ${UID} ${USER} \
 && echo "root:root" | chpasswd \
 && echo "${USER}:111111" | chpasswd \
 && echo "${USER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# ================== Install Extra Tools ==================
RUN apt-get update && apt-get install -y --no-install-recommends \
    git vim gedit wget curl sudo \
    build-essential cmake unzip \
    python3-pip python3-opencv python3-numpy python3-empy python3-dev python3-tk \
    graphviz net-tools \
 && rm -rf /var/lib/apt/lists/*

# -------- Fix Python packaging for catkin on Noetic --------
ENV PIP_DISABLE_PIP_VERSION_CHECK=1
# 改一下值來打破 cache（或 build 時加 --no-cache）
ARG CACHE_BUST=20250828a

# 🔴 這裡一定要包括 python3-yaml，並且清掉 apt 版 catkin 的入口點
RUN apt-get purge -y \
      python3-importlib-metadata \
      python3-catkin-tools \
      python3-osrf-pycommon \
      python3-yaml || true \
 && rm -rf /usr/lib/python3/dist-packages/importlib_metadata* || true \
 && rm -f /usr/bin/catkin || true \
 && apt-get autoremove -y && apt-get clean

# 升級 pip 到 /usr/local
RUN curl -sS https://bootstrap.pypa.io/pip/3.8/get-pip.py -o /tmp/get-pip.py \ 
    && python3 /tmp/get-pip.py \ 
    && rm /tmp/get-pip.py

# Noetic 穩定相容組合（關鍵：importlib-metadata==4.13.0 + setuptools==58.2.0）
RUN python3 -m pip install --no-cache-dir --upgrade \
      setuptools==58.2.0 \
      wheel==0.38.4 \
      packaging==23.2 \
      importlib-metadata==4.13.0 \
      osrf-pycommon==2.0.2 \
      catkin-tools==0.9.4

# 確保 /usr/local 優先（避免叫到 /usr/bin 的舊東西）
ENV PATH=/usr/local/bin:$PATH
ENV PYTHONPATH=/usr/local/lib/python3.8/dist-packages:$PYTHONPATH

# 建置期自檢
RUN python3 - <<'PY'
import inspect, importlib_metadata as im, setuptools, yaml, sys, shutil, os
assert setuptools.__version__ == "58.2.0", setuptools.__version__
assert im.version("importlib-metadata") == "4.13.0", im.version("importlib-metadata")
print("[OK] setuptools:", setuptools.__version__, "=>", inspect.getfile(setuptools))
print("[OK] importlib-metadata:", im.version("importlib-metadata"), "=>", im.__file__)
print("[OK] PyYAML:", yaml.__version__, "=>", yaml.__file__)
print("[OK] which catkin:", shutil.which("catkin"))
print("[OK] sys.path[0..4]:", sys.path[:5])
PY


##================== Install PIP ==================

# 升級/降版 pip 到對 Py3.8 穩定的版本
RUN python3 -m pip install --no-cache-dir --upgrade "pip<24"

# 安裝 openai（也可選定一版）
RUN python3 -m pip install --no-cache-dir openai
# 或：RUN python3 -m pip install --no-cache-dir "openai==1.51.2"

# ================== Final Settings ==================
USER ${USER}
WORKDIR ${HOME}
CMD ["/bin/bash"]

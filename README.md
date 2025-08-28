# BT-integration

## Clone Repository

```bash
git clone --recursive git@github.com:ARG-NCTU/bt_integration.git
```

## Update Repository and Submodules

```bash
git pull
git submodule sync --recursive
git submodule update --init --recursive
```

## Setting HuggingFace token

```bash
vim ~/.bashrc
```

Go to OpenAI API page: this [link](https://platform.openai.com/settings/organization/api-keys) to add your own key

Then add this line (Replace with your key):
```bash
export OPENAI_API_KEY=sk-...xxxx
```

## Enter the Repository

```bash
cd bt_integration
```

## Set Up Environment

### 1. Enter Docker Environment

#### 1.1 Docker Run

Run this script to pull and run the Docker image on your workstation.

```bash
source Docker/ros1-cpu/run.sh
```

#### 1.2 Docker Join

Use the same command to re-enter the running Docker container.

```bash
source Docker/ros1-cpu/run.sh
```

### 2. Build Catkin Workspace

Fix python packages error
```bash
python3 -m pip install --no-cache-dir --upgrade \
      setuptools==58.2.0 \
      wheel==0.38.4 \
      packaging==23.2 \
      importlib-metadata==4.13.0 \
      osrf-pycommon==2.0.2 \
      catkin-tools==0.9.4
```

Build ROS1 Workspace
```bash
source build_ros1_all.sh
```

## Usage

Behavior Tree Generation (from gpt3.5)
```bash
source Docker/ros1-cpu/run.sh
source environment_ros1.sh
roslaunch bt_prompt behavior_tree_generation.launch
```

For first time generation, use default prompt (ros1_ws/src/bt_prompt/config/exp/L/test/prompt.txt): Hi, GPT!
Modify the file content, refer to ros1_ws/src/bt_prompt/config/prompt/prompt.txt.

Rerun the code
```bash
roslaunch bt_prompt behavior_tree_generation.launch
```

# BT-integration

This repo is for integration of all behavior tree submodules.

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

## Setting OPENAI API key

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

Build ROS1 Workspace
```bash
source build_ros1_all.sh
```

## Usage for behavior tree generation

Behavior Tree Generation (from gpt-3.5-turbo-instruct)
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

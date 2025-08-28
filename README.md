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

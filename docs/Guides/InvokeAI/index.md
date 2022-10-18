# InvokeAI - pip only setup guide

## Requirements

- [Python 3.9+](https://www.python.org/downloads/)
- [git](https://git-scm.com/downloads)
- Stronlgy recommended to use a GPU (This tutorial is for Nvidia GPUs, but AMD GPUs should work as well) 4gb+ VRAM
- 12 GB of disk
- a bit of time (~15min)

## Setup

Im going to use the build in venv module to create a virtual environment for this project. The original way of INvokeAI is using Annaconda. Look into their repo for more information.

This guide is tested and working on Windows. For Linux it should work too, sorry Mac guys.

!!! tip
    A "one-click-installer" is in the works, but not ready yet.

### 1. Install Python

If you have problems on this step, google is your friend :D
Tip: dont forget to add python to your PATH

### 2. Create a virtual environment

In Order to prevent Windows file length issues, we will create the virtual environment in the root of a drive.

Open a command prompt in a root folder and type:

??? info "Navigation inside the command prompt"
    To find the root of a drive, type `cd /` in the command prompt.
    To change drives, type `cd <drive letter>:`

``` cmd
mkdir InvokeAI
```

to create a folder for the project.
Enter the folder and create a virtual environment:

``` cmd
cd InvokeAI
python -m venv .venv
```

### 3. Activate the virtual environment

``` cmd
.venv\Scripts\activate
```

### 4. Install InvokeAI

Get the release from the [releases page](https://github.com/invoke-ai/InvokeAI/releases) and extract it into the invokeai folder.

### 5. Install the requirements

While the virtual environment is activated (in not see step [3](#3-activate-the-virtual-environment)), type:

``` cmd
pip install -r requirements.txt
```

### 6. Download the model

Download the model from [here](https://huggingface.co/CompVis/stable-diffusion-v-1-4-original)

Copy the `sd-v1-4.ckpt` file into the `models\ldm\stable-diffusion-v1` folder and rename it to `model.ckpt`.

### 7. Preload the modles

``` cmd
python scripts/preload_models.py
```

## Start using it

``` cmd
python scripts\invoke.py --gui
```

For cli remove the `--gui` argument, for more see the official [wiki](https://invoke-ai.github.io/InvokeAI/features/CLI/)

Output directory will be `output\img-samples`

## Updating

To update InvokeAI, just download the new release and overwrite the files and repeat the steps [5](#5-install-the-requirements) and [7](#7-preload-the-modles).
Or you can just use the `git pull` command.


## Notes

This setup is now fully portable, you can move the InvokeAI folder to another drive or even another computer and it will work.

To make the start easier, we add a simple .bat file to the root of the InvokeAI folder (WIN only):

!!! warning
    "portable" in a way, that it works on similar systems. If its too different, it should still work in cpu only mode.

``` bat
".venv/Scripts/python.exe" scripts\invoke.py --gui
```

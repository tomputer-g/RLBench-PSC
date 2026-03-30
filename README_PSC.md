# Steps

Create a conda environment:

```bash
conda create -n RLBench python=3.10
conda activate RLBench
```
## Xvfb

In order for X to be setup on PSC, we can't do any of the steps in the main README (folder under /etc/X11 is read-only, and we don't have `sudo`.) We could also forward a X connection from your laptop to the PSC node, but there are some mismatches in protocol that are hard to resolve. Instead, use a X virtual framebuffer (`Xvfb`).

This does mean you can't see the outputs live. But it allows stuff to run:

```bash
conda install -c conda-forge xorg-xserver-xvfb
```

Whenever you start a GPU node, do this (ref https://github.com/stepjam/RLBench/issues/257#issuecomment-2538534741):

```bash
Xvfb :77 -screen 0 1024x768x16 &
export DISPLAY=:77
```

To test this:
```bash
glxgears
```
Should run without warnings, and output some respectable FPSes. The below results are from a V100-16 node:

```
14606 frames in 5.0 seconds = 2921.182 FPS
15223 frames in 5.0 seconds = 3044.468 FPS
15245 frames in 5.0 seconds = 3048.829 FPS
15330 frames in 5.0 seconds = 3065.929 FPS
```

Now, install RLBench:

```bash
# set env variables (recommended to set in bashrc)
export COPPELIASIM_ROOT=${HOME}/CoppeliaSim
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$COPPELIASIM_ROOT
export QT_QPA_PLATFORM_PLUGIN_PATH=$COPPELIASIM_ROOT

wget https://downloads.coppeliarobotics.com/V4_1_0/CoppeliaSim_Edu_V4_1_0_Ubuntu20_04.tar.xz
mkdir -p $COPPELIASIM_ROOT && tar -xf CoppeliaSim_Edu_V4_1_0_Ubuntu20_04.tar.xz -C $COPPELIASIM_ROOT --strip-components 1
rm -rf CoppeliaSim_Edu_V4_1_0_Ubuntu20_04.tar.xz
```

To install the RLBench python package:

```bash
cd <path-to-this-repo>
pip install .
```



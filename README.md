# Locomotion ROS Noetic Workspace

ROS 1 workspace for the KIT lecture "Humanoid Robots: Locomotion and Whole Body Control". It
provides a [devcontainer](https://code.visualstudio.com/docs/devcontainers/containers) config, as well as a repos file for importing all the required packages.

## Setup

Move your code into the `src` directory.

### Start the container using VS Codes devcontainer plugin

Open the workspace in VS Code, make sure you have the devcontainer plugin installed. Open the
command pallet (CTRL+SHIFT+P) and search for `Open in Container`.

### Start the container using the cli

Install the [devcontainer-cli](git@github.com:devcontainers/cli.git). Start the container with
```bash
devcontainer up --workspace-folder ./
```

Now enter the container by calling 
```bash
devcontainer exec --workspace-folder ./ zsh
```

## Pulling the dependencies

Inside the container run

```bash
vcs import < reemc.repos
```

Now source ROS by running `source /opt/ros/noetic/setup.zsh` (the container uses zsh as a shell because
it's 2024). The build the workspace by running `catkin build`. Now you can source the freshly built
workspace by running `source devel/setup.zsh`.

## Verify everything works


Open three terminals (use vscode or tmux or install terminator using `sudo apt install terminator`).
Make sure your workspace is sourced. In on terminal, start the simulation using 
```bash
roslaunch reemc_gazebo reemc_empty_world.launch
```

and in the other the controllers with
```bash
roslaunch reemc_controller_configuration joint_trajectory_controllers.launch
```

In the third terminal, run the example code with
```
rosrun facepalm facepalm.py
```

## Troubleshooting

- no gui
    - did you run `xhost +` on your host system?
- still no gui
    - first, install `x11-apps` and `mesa-utils` in the container
    ```bash
    sudo apt update
    sudo apt install x11-apps mesa-utils
    ```
    - run `xeyes` to verify that x-socket forwarding is working. You should see a window open with
      two eyes following your cursor
        - if it says something like `cant open display :1`, check the `/tmp/.X11-unix` folder using
          `ls /tmp/.X11-unix` and try out every display socket listed there. For example `ls` would
          show something like `X0 X1 X2 X3`. Test each socket by running `DISPLAY=:N xeyes`, where
          `N` is the number of a socket, so for X1 you would write something like `DISPLAY=:1
          xeyes`.
          If you found a working socket, from now one write `export DISPLAY=:N` in every new shell
          session in the container.
    - run `glxgears` to verify that hardware acceleration is working. You should see a window open
      with a bunch of gears turning. If you don't, or only see a black window, your hardware
      acceleration is broken and stuff like rviz or gazebo won't work
        - if you have a Nvidia GPU and some sort of non-default driver setup (e.g. everything that
          has to do with cuda), make sure to install and setup the [`nvidia-container-toolkit`](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
- build crashes
    - if you are using docker desktop, try increasing the RAM assigned to your docker
    - build with fewer cores using the `-j` flag for catkin, e.g. `catkin build -j 4`

- something with ros didn't work
    - did you source the workspace?

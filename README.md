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
- something with ros didn't work
    - did you source the workspace?

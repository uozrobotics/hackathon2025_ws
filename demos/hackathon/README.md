## Running

To start the demo, just execute the following command (from this directory):
```
docker compose up
```

This command will start all the docker services defined in the `compose.yaml` file.
The `bash` service is however not started automatically because it is only used to open a shell in
the docker.


### Keeping the simulator open

You can start the services individually by specifying their name after the `compose up` arguments.
It can be useful to keep the simulator open while restarting the robot programs.
You can start only the simulator by executing:
```
docker compose up -d simulator
```
The `-d` option starts it in the background.
After that, you can start everything else using the command
```
docker compose up
```
This command can be interrupted by typing _Ctrl+C_ and can be executed again while keeping the
simulator open.
When you no longer need the simulator, you can stop it with
```
docker compose down simulator
```

### Opening a shell in the docker

If you want to open execute some ROS command, you can open a shell in the ROS2 environment using
```
docker compose run --rm bash
```
The `--rm` options deletes the container when the shell is closed.


## FAQ

### How to use NVIDIA GPU?

You need to install the [NVIDIA container toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html).

After that, edit the file `compose.yaml` to replace the service `x11_base` by `x11_gpu`.
The beginning of the file must look like this:
```yaml
x-yaml-anchors:  # create anchors "&something" that can be referenced using "*something"
  base: &base
    extends:
      file: ../../docker/common.yaml
      service: x11_gpu
    ...
```
These services are defined in the file `docker/common.yaml` of the workspace and provide all the
required docker options.
The `x11_gpu` service add special options to use the NVIDIA GPUs.


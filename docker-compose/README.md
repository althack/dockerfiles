# Docker Compose

This directory contains Docker Compose examples for running RViz and modern
Gazebo in containers with GUI support.

These examples assume a Linux host running X11. They bind-mount the host X11
socket, pass through `DISPLAY` and `XAUTHORITY`, and use host networking. They
will need additional display configuration on Wayland and are not directly
portable to Docker Desktop on macOS or Windows.

## Installation

Install Docker Engine and the Docker Compose plugin on your Linux system.

- Install Docker: Follow the official installation guide for your platform:
<https://docs.docker.com/get-docker/>

- Install Docker Compose (if not included with Docker):
<https://docs.docker.com/compose/install/>

To verify your installation, run:

```bash
docker --version
docker compose version
```

## Usage

Before starting a container, allow local Docker containers to access the X
server:

```bash
xhost +local:docker
```

Then run the example for the application and version you want:

```bash
docker compose -f <rviz-or-gz>/<version>-docker-compose.yml up
```

For example, launch Gazebo Harmonic:

```bash
docker compose -f gz/harmonic-docker-compose.yml up
```

Or launch RViz with ROS 2 Jazzy:

```bash
docker compose -f rviz/jazzy-docker-compose.yml up
```

## Notes

- The Compose files use host networking (`network_mode: host`) to simplify GUI display setup.

- GUI support is enabled by sharing the X11 socket and setting appropriate environment variables (`DISPLAY`, `XAUTHORITY`).

- Software rendering is enforced (`LIBGL_ALWAYS_SOFTWARE=1`) for compatibility.

- Examples under `gz/` launch `gz sim`; examples under `rviz/` launch RViz 2.

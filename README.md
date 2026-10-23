# dockerfiles

[![Documentation](https://github.com/althack/dockerfiles/actions/workflows/site.yml/badge.svg)](https://github.com/althack/dockerfiles/actions/workflows/site.yml)
[![Dockerfiles](https://github.com/althack/dockerfiles/actions/workflows/docker.yml/badge.svg)](https://github.com/althack/dockerfiles/actions/workflows/docker.yml)

These are Docker images and runtime examples I use for ROS and Gazebo development.

The images are built and published automatically, including multi-platform builds where supported. Images will continue to be supported as long as the underlying ROS or Gazebo release has not reached EOL.

For VS Code development, I now recommend using my [ROS 2 Dev Container Feature](https://github.com/althack/devcontainers) with the [VSCode ROS2 Workspace Template](https://github.com/althack/vscode_ros2_workspace).

## Images

The currently maintained image families are available on Docker Hub:

- [althack/ros2](https://hub.docker.com/r/althack/ros2)
- [althack/gz](https://hub.docker.com/r/althack/gz)

Older ROS, Gazebo Classic, and Ignition images are still available for historical releases.

## Running GUI applications

Just running a Docker image is usually not enough to launch GUI applications like RViz or Gazebo. They also need access to things like the display server, networking, and sometimes GPU resources.

This repo includes Docker Compose configurations for running RViz and Gazebo with the required runtime configuration.

See the [Docker Compose documentation](docker-compose/README.md) for setup and usage.

## Build from source

You can build all currently supported images directly from source:

```bash
./build.py all
```

Or just build one:

```bash
./build.py ros2-jazzy-base
```

Or build one distro group:

```bash
./build.py ros2-jazzy
```

To see help information and build options:

```bash
./build.py --help
```

### Shell completion

Enable tab completion for the current shell session:

```bash
eval "$(_BUILD_PY_COMPLETE=bash_source ./build.py)"
```

Then try:

```bash
./build.py <TAB><TAB>
```

To make completion persistent for bash, add this to your `~/.bashrc`:

```bash
eval "$(_BUILD_PY_COMPLETE=bash_source /path/to/dockerfiles/build.py)"
```

## Developing this repository

You can develop either in the provided development container or in a local
Python virtual environment.

### Development container

Open the repository in VS Code and run **Dev Containers: Reopen in Container**.
The container provides Python 3.12 and installs the editable package with its
`scripts` and `dev` dependencies automatically. A separate virtual environment
is not needed because the container provides the isolated development
environment.

After changing the container configuration, run **Dev Containers: Rebuild Container**.

### Local development

Create a repository-local virtual environment and install the development
dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -e ".[scripts,dev]"
```

In VS Code, run **Python: Select Interpreter** and choose
`${workspaceFolder}/.venv/bin/python`. New terminals will activate the selected
environment automatically.

### Tasks

Use **Tasks: Run Task** to run `test`, `generate`, `build`, or `build target`.
Run `install` after changing dependencies in `pyproject.toml`; `upgrade` updates
dependencies to the latest versions allowed by that file.

## Template accessor package

You can install the template accessor package in another repo without publishing it to PyPI.

Install from GitHub:

```bash
pip install "git+https://github.com/althack/dockerfiles.git"
```

Pin to a branch, tag, or commit:

```bash
pip install "git+https://github.com/althack/dockerfiles.git@main"
```

For local development:

```bash
pip install -e ".[scripts,dev]"
```

Example usage:

```python
from dockerfiles_templates import Templates

templates = Templates(templates_path="templates.yml")
for entry in templates.entries(eol=False):
    print(entry["family"], entry["name"])
```

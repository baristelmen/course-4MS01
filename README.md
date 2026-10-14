<!-- PROJECT LOGO -->
<br />
<div align="center">

  <h3 align="center">4MS01 - Course #5</h3>

  <p align="center">
    Modeling behavior of materials using MFront and Python bindings in Jupyter Lab. <br />
    <br />
    <a href="https://github.com/baristelmen/MS201/issues">Report Bug</a>
    ·
    <a href="https://github.com/baristelmen/MS201/pulls">Request Feature</a>
  </p>
</div>

## Table of Contents

- [About this repository](#about-this-repository)
- [Getting started](#getting-started)
  - [Requirements](#requirements)
  - [About the data in this repository](#about-the-data-in-this-repository)
  - [Get the repository](#get-the-repository)
  - [Option A: Docker Compose (recommended)](#option-a-docker-compose-recommended)
  - [Option B: Standalone container](#option-b-standalone-container)
- [Getting help](#getting-help)
- [Contributing](#contributing)
- [License](#license)

## About this repository

This repository contains the course material and a ready-to-use environment for the **MEC_4MS01_TA** course. It provides:

- **`course_content/`** – notebooks and material used during the course.
- **`docker/`** – a Docker Compose setup that pulls the `jupyter-mfront-ms201` image, a JupyterLab environment with TFEL/MFront and its Python bindings pre-installed.

No local installation of MFront or Python packages is needed: everything runs inside the container, and your work is saved to a shared folder on your computer.

## Getting started

### Requirements

- [Docker](https://docs.docker.com/get-docker/) (Docker Desktop on Windows/macOS, Docker Engine on Linux) with the Compose plugin, which ships with standard installations.
- Hardware virtualization enabled in BIOS/UEFI (Windows/macOS).
- At least 4 GB of RAM and a network connection for the first download.

The image is published for both `amd64` and `arm64`, so Docker pulls the right version for your CPU (including Apple Silicon).

### About the data in this repository

> [!IMPORTANT]
> **The data in this repository is sample data only.** It shows the expected folder structure and file formats, but it is **not** the data you will work with during the course.
>
> Before you start, replace it with your own course data:
>
> 1. **Download your data** from the JupyterHub server provided during the course.
> 2. **Replace the sample data** in the `data` folder with the files you downloaded, using either method:
>    - **On your computer:** copy the files into the `data` folder with your file explorer (Finder, Windows Explorer, etc.).
>    - **In JupyterLab:** once the environment is running (see below), open the `work` folder and drag the files into the file browser, or use the **Upload** button.
>
> Both methods write to the same folder, so the files appear on your computer and in JupyterLab.

### Get the repository

```shell
git clone https://github.com/baristelmen/4MS01.git
cd 4MS01/docker
```

Or [download the ZIP](https://github.com/baristelmen/4MS01/archive/refs/heads/main.zip), unzip it, and open a terminal in the `docker` folder.

Both options use the image `baristelmen/jupyter-mfront:lab-4.6.3`, which is built on the official Jupyter Docker Stacks `minimal-notebook`. The login password is set with the `JUPYTER_TOKEN` environment variable. The default password is **`MS201`**.

> **Linux users:** create the shared folder yourself (`mkdir data`) before the first start. If Docker creates it, it belongs to `root`, and JupyterLab cannot save files there.

### Option A: Docker Compose (recommended)

**Set the password.** Edit `docker-compose.yaml` in the `docker` folder and change the value of `JUPYTER_TOKEN`:

```yaml
    environment:
      - JUPYTER_TOKEN=MS201      # change to your own password
```

To use a different port on your computer, change the left side of `"8888:8888"` (for example `"9999:8888"`).

**Start the environment:**

```shell
docker compose up -d
```

The first run downloads the image, which can take a while. Then:

1. Open **http://localhost:8888/lab** in your browser.
2. Enter the password set in `docker-compose.yaml`.
3. Save your work in the `work` folder in JupyterLab. It maps to the `data` folder on your computer, so notebooks and results persist after the container stops.

To change the password later, edit `docker-compose.yaml` and run `docker compose up -d` again. Compose recreates the container with the new value, and your files in `data` are kept.

To stop the environment:

```shell
docker compose down
```

### Option B: Standalone container

**Set the password** with `-e JUPYTER_TOKEN=...` when starting the container:

```shell
docker run -d --name ms201 \
  -p 8888:8888 \
  -e JUPYTER_TOKEN=MS201 \
  -v "$(pwd)/data:/home/jovyan/work" \
  baristelmen/jupyter-mfront:lab-4.6.3
```

On Windows PowerShell, replace `$(pwd)` with `${PWD}` and the `\` line breaks with a backtick (`` ` ``). Then open **http://localhost:8888/lab** and enter the password you chose.

To change the password, remove the container and start it again with a new value. Your files in `data` are kept:

```shell
docker stop ms201 && docker rm ms201
```

> [!CAUTION]
> **Forgot to set `JUPYTER_TOKEN`?** Jupyter generates a random token, and you cannot log in with `MS201`.
> Find the generated token in the startup logs:
> ```shell
> docker logs ms201
> ```

## Getting help

- **Problems with this repository or the environment:** open an issue on the [Issues page](https://github.com/baristelmen/4MS01/issues). Include the steps to reproduce, the error message, and your OS and Docker version (`docker --version`).
- **TFEL/MFront:** see the [TFEL documentation](https://thelfer.github.io/tfel/web/index.html) and the [MFront user guide](https://thelfer.github.io/tfel/web/mfront-userguide.html).
- **Docker:** see the [Docker](https://docs.docker.com) and [Docker Compose](https://docs.docker.com/compose) documentation.

## Contributing

Contributions are welcome! If you find an issue or have an improvement to suggest, feel free to open an issue or submit a pull request.

## License

The content of this repository (course material, configuration files, and documentation) is open source under the [MIT License](LICENSE). Anyone is free to use, modify, and share it.

The container image bundles third-party software that remains under its own license:

| Software | License |
| --- | --- |
| [TFEL/MFront](https://thelfer.github.io/tfel/web/index.html) | GPL-3.0 or CeCILL-A (dual license) |
| [JupyterLab](https://github.com/jupyterlab/jupyterlab) | BSD-3-Clause |
| [Jupyter Docker Stacks](https://github.com/jupyter/docker-stacks) (base image) | BSD-3-Clause |
| [Python](https://www.python.org/) | PSF License |
| [Docker](https://www.docker.com/) | Apache-2.0 (Docker Engine); Docker Desktop has its own [subscription terms](https://www.docker.com/legal/docker-subscription-service-agreement/) |

Using this environment means accepting the licenses of these components.
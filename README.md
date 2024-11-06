# Ollama and OpenWeb UI Installation Guide (NVIDIA Jetson)

© 2024 [Ai{DEAL} Studio](https://anpengcheng.cn/). All rights reserved.

This guide will walk you through installing Ollama and OpenWeb UI on an NVIDIA Jetson device. After installation, Ollama will run in GPU mode, and you can access it through the browser on other devices. Follow the steps below to ensure a smooth installation.

## Contents

1. [Verify L4T Version](#verify-l4t-version)
2. [OPTIONAL: Reflashing Your Device](#optional-reflashing-your-device)
3. [Installation](#installation)
4. [Usage](#usage)

## Verify L4T Version

To check the L4T (Linux for Tegra) version on your NVIDIA Jetson device (e.g., Jetson Nano, Jetson Xavier), follow these steps:

1. Open the GNOME Terminal on your Jetson device.

2. Run `head -n 1 /etc/nv_tegra_release` to retrieve your current L4T version.

### Supported L4T Versions:

- 35.3.1
- 35.4.1
- 35.5.0
- 36.3.0

## OPTIONAL: Reflashing Your Device

If your L4T version does not match any of the supported versions listed above, you may need to re-flash the system on your NVIDIA Jetson device.

Use SDK Manager on another computer to re-flash the device.You can download the SDK Manager and follow the tutorial from the official [NVIDIA Developer](https://developer.nvidia.com/sdk-manager) site.

It is recommended to install JetPack 6.0 (rev. 2) SDK version during the re-flashing process.

![reflashing_jetpack6.0](/screenshots/sdkm-chose6.png)

## Installation

1. Keep `apt` up to date:

   ```bash
   sudo apt update && sudo apt upgrade
   ```

2. Install `jetpack`:

   ```bash
   sudo apt install jetpack
   ```

3. Add your user to the docker group and restart the Docker service to apply the change:

   ```bash
   sudo usermod -aG docker $USER && \
   newgrp docker && \
   sudo systemctl daemon-reload && \
   sudo systemctl restart docker
   ```

4. Install [jetson-examples](https://github.com/Seeed-Projects/jetson-examples):

   ```bash
   pip3 install jetson-examples
   ```

5. Reboot system

   ```bash
   sudo reboot
   ```

6. Install Ollama

   ```bash
   reComputer run ollama
   ```

   Optional: If you run the above command via `ssh` and encounter the error `command not found: reComputer`, you can resolve this by executing the following command:

   ```bash
   source ~/.profile
   ```

7. Install models (e.g. llama3.2) from [Ollama Library](https://ollama.com/library)

   ```bash
   ollama pull llama3.2
   ```

8. Install and run Open WebUI through docker

   ```bash
   docker run -d -p 3000:8080 --gpus all --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:cuda
   ```

## Usage

Once the installation is finished, you can access the GUI by visiting `YOUR_SERVER_IP:3000` in your browser.
![open_webui_services](/screenshots/gui.png)

Access the API endpoints by navigating to `YOUR_SERVER_IP/ollama/docs#/`. For comprehensive documentation, please refer to the official resources: the [Ollama API Documentation](https://github.com/ollama/ollama/blob/main/docs/api.md) (**recommended**) and [Open WebUI API Endpoints](https://docs.openwebui.com/getting-started/advanced-topics/api-endpoints/).

To install additional models via Ollama, please follow the steps below:

1. Access the server via `ssh`

   ```bash
   ssh username@YOUR_SERVER_IP
   ```

   Optional: Ensure the server’s firewall allows SSH connections. If SSH is not permitted, you can enable it with the following command:

   ```bash
   sudo ufw allow ssh
   ```

2. Run Ollama

   ```bash
   reComputer run ollama
   ```

   Optional: If you run the above command via `ssh` and encounter the error `command not found: reComputer`, you can resolve this by executing the following command:

   ```bash
   source ~/.profile
   ```

3. Select and install a model

   Browse the [Ollama library](https://ollama.com/library) and follow the provided instructions to install your desired model.

4. Refresh the OpenWeb UI

   Once the model is installed, refresh the Open WebUI at `YOUR_SERVER_IP:3000`. The newly downloaded model should now appear in the interface.

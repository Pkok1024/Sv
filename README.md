{

  "cells": [

    {

      "cell_type": "markdown",

      "metadata": {

        "id": "view-in-github",

        "colab_type": "text"

      },

      "source": [

        "<a href=\"https://colab.research.google.com/github/Pkok1024/Sv/blob/main/counterfeit.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"

      ]

    },

    {

      "cell_type": "code",

      "execution_count": null,

      "metadata": {

        "id": "sBbcB4vwj_jm"

      },

      "outputs": [],

      "source": [

        "!curl -Lo memfix.zip https://github.com/nolanaatama/sd-webui/raw/main/memfix.zip\n",

        "!unzip /content/memfix.zip\n",

        "!apt install -qq libunwind8-dev\n",

        "!apt install -qq libcairo2-dev pkg-config python3-dev\n",

        "!dpkg -i *.deb\n",

        "%env LD_PRELOAD=libtcmalloc.so\n",

        "!rm *\n",

        "!pip install --upgrade fastapi==0.90.1\n",

        "!pip install torch==1.13.1+cu116 torchvision==0.14.1+cu116 torchaudio==0.13.1 torchtext==0.14.1 torchdata==0.5.1 --extra-index-url https://download.pytorch.org/whl/cu116 -U\n",

        "!curl -Lo sd-webui.zip https://huggingface.co/nolanaatama/webui/resolve/main/sd-webui.zip\n",

        "!unzip /content/sd-webui.zip\n",

        "!git clone https://github.com/nolanaatama/sd-webui-tunnels /content/sd-webui/extensions/sd-webui-tunnels\n",

        "!git clone https://github.com/Mikubill/sd-webui-controlnet /content/sd-webui/extensions/sd-webui-controlnet\n",

        "!git clone https://github.com/fkunn1326/openpose-editor /content/sd-webui/extensions/openpose-editor\n",

        "!git clone https://github.com/hnmr293/posex /content/sd-webui/extensions/posex\n",

        "!git clone https://github.com/camenduru/sd-civitai-browser /content/sd-webui/extensions/sd-civitai-browser\n",

        "!git clone https://github.com/DominikDoom/a1111-sd-webui-tagcomplete /content/sd-webui/extensions/a1111-sd-webui-tagcomplete\n",

        "!git clone https://github.com/hako-mikan/sd-webui-supermerger /content/sd-webui/extensions/sd-webui-supermerger\n",

        "!curl -Lo /content/sd-webui/extensions/sd-webui-images-browser.zip https://huggingface.co/nolanaatama/webui/resolve/main/sd-webui-images-browser.zip\n",

        "%cd /content/sd-webui/extensions\n",

        "!unzip /content/sd-webui/extensions/sd-webui-images-browser.zip\n",

        "%cd /content\n",

        "# Model Code\n",

        "!curl -Lo /content/sd-webui/models/Stable-diffusion/counterfeitv3.safetensors https://huggingface.co/gsdf/Counterfeit-V3.0/resolve/main/Counterfeit-V3.0_fp32.safetensors\n",

        "!curl -Lo /content/sd-webui/models/Stable-diffusion/counterfeitv3.vae.pt https://huggingface.co/gsdf/Counterfeit-V2.5/resolve/main/Counterfeit-V2.5.vae.pt\n",

        "# ControlNet\n",

        "# Remove '#' from the beginning of the line(s) below to download the selected ControlNet model(s)\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11e_sd15_ip2p.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11e_sd15_ip2p_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11e_sd15_shuffle.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11e_sd15_shuffle_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11p_sd15_canny.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15_canny_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11f1p_sd15_depth.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15_depth_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11p_sd15_inpaint.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15_inpaint_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11p_sd15_lineart.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15_lineart_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11p_sd15_mlsd.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15_mlsd_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11p_sd15_normalbae.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15_normalbae_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11p_sd15_openpose.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15_openpose_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11p_sd15_scribble.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15_scribble_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11p_sd15_seg.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15_seg_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11p_sd15_softedge.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15_softedge_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11p_sd15s2_lineart_anime.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11p_sd15s2_lineart_anime_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/control_v11u_sd15_tile.safetensors https://huggingface.co/nolanaatama/models/resolve/main/control_v11u_sd15_tile_fp16.safetensors\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/t2iadapter_canny_sd14v1.pth https://huggingface.co/nolanaatama/models/resolve/main/t2iadapter_canny_sd14v1.pth\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/t2iadapter_color_sd14v1.pth https://huggingface.co/nolanaatama/models/resolve/main/t2iadapter_color_sd14v1.pth\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/t2iadapter_depth_sd14v1.pth https://huggingface.co/nolanaatama/models/resolve/main/t2iadapter_depth_sd14v1.pth\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/t2iadapter_keypose_sd14v1.pth https://huggingface.co/nolanaatama/models/resolve/main/t2iadapter_keypose_sd14v1.pth\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/t2iadapter_openpose_sd14v1.pth https://huggingface.co/nolanaatama/models/resolve/main/t2iadapter_openpose_sd14v1.pth\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/t2iadapter_seg_sd14v1.pth https://huggingface.co/nolanaatama/models/resolve/main/t2iadapter_seg_sd14v1.pth\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/t2iadapter_sketch_sd14v1.pth https://huggingface.co/nolanaatama/models/resolve/main/t2iadapter_sketch_sd14v1.pth\n",

        "#!curl -Lo /content/sd-webui/extensions/sd-webui-controlnet/models/t2iadapter_style_sd14v1.pth https://huggingface.co/nolanaatama/models/resolve/main/t2iadapter_style_sd14v1.pth\n",

        "import shutil\n",

        "shutil.rmtree('/content/sd-webui/embeddings')\n",

        "!rm sd-webui.zip\n",

        "!rm sd-webui-images-browser.zip\n",

        "%cd /content/sd-webui\n",

        "!git clone https://huggingface.co/nolanaatama/embeddings\n",

        "%cd /content/sd-webui/models\n",

        "!git clone https://huggingface.co/nolanaatama/ESRGAN\n",

        "%cd /content/sd-webui/extensions/a1111-sd-webui-tagcomplete\n",

        "!git checkout f9f7732\n",

        "%cd /content/sd-webui\n",

        "# Web UI tunnel\n",

        "!COMMANDLINE_ARGS=\"--share --disable-safe-unpickle --no-half-vae --xformers --reinstall-xformers --enable-insecure-extension --gradio-queue --cloudflared\" REQS_FILE=\"requirements.txt\" python launch.py\n",

        "# If remotemoe failed to start, change '--remotemoe' to '--cloudflared' on the COMMANDLINE_ARGS line above to use cloudflare tunnel"

      ]

    },

    {

      "cell_type": "markdown",

      "source": [

        "## (OPTIONAL) LoRAs"

      ],

      "metadata": {

        "id": "0DwYF_aLUXKy"

      }

    },

    {

      "cell_type": "markdown",

      "source": [

        "### 1. After the gradio link show up, stop the first cell & clear the code output👆"

      ],

      "metadata": {

        "id": "JUtPlg328avv"

      }

    },

    {

      "cell_type": "markdown",

      "source": [

        "### 2. Load the LoRA & launch the web ui"

      ],

      "metadata": {

        "id": "xy_WyDzNUgd2"

      }

    },

    {

      "cell_type": "code",

      "source": [

        "# Copy the LoRA code from other LoRA setup (download the setup file after editing the LoRA code cell to avoid repeat input for next session)\n",

        "# How-to download the setup file: Click 'File' menu -> 'Download' -> 'Download .ipynb'\n",

        "# Load LoRA from Google Drive: https://youtu.be/G1QZfAPUMaM\n",

        "\n",

        "# LoRA 1\n",

        "#!curl ...\n",

        "\n",

        "# LoRA 2\n",

        "#!curl ...\n",

        "\n",

        "# LoRA 3\n",

        "#!curl ...\n",

        "\n",

        "# ...\n",

        "\n",

        "# Web UI tunnel\n",

        "!COMMANDLINE_ARGS=\"--share --disable-safe-unpickle --no-half-vae --xformers --reinstall-xformers --enable-insecure-extension- --gradio-queue --cloudflared\" REQS_FILE=\"requirements.txt\" python launch.py\n",

        "# If remotemoe failed to start, change '--remotemoe' to '--cloudflared' on the COMMANDLINE_ARGS line above to use cloudflare tunnel"

      ],

      "metadata": {

        "id": "3EOPSiOgUs4z"

      },

      "execution_count": null,

      "outputs": []

    },

    {

      "cell_type": "markdown",

      "metadata": {

        "id": "fhwIXzcgfkoR"

      },

      "source": [

        "# 📚 GitHub for more: [_@nolanaatama_](https://github.com/nolanaatama)\n",

        "# 📦 Repo: [Github](https://github.com/nolanaatama/sd-1click-colab)"

      ]

    }

  ],

  "metadata": {

    "accelerator": "GPU",

    "colab": {

      "provenance": [],

      "include_colab_link": true

    },

    "gpuClass": "standard",

    "kernelspec": {

      "display_name": "Python 3",

      "name": "python3"

    },

    "language_info": {

      "name": "python"

    }

  },

  "nbformat": 4,

  "nbformat_minor": 0

}

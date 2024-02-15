---
slug: setting-up-ollama-web-ui-ubuntu-docker
title: "Setting Up Ollama Web UI on Ubuntu Using Docker"
date: 2024-02-16T00:00:00.000Z
excerpt: "Learn how to set up Ollama Web UI on an Ubuntu server using Docker, complete with code examples and a brief overview of its key features."
coverImage: /images/posts/Ollama-Ubuntu-setup.jpg
tags:
  - Ollama
  - Ubuntu
  - Docker
  - Installation Guide
---

<script>
  import Callout from "$lib/components/molecules/Callout.svelte";
  import CodeBlock from "$lib/components/molecules/CodeBlock.svelte";
  import Image from "$lib/components/atoms/Image.svelte";
</script>

## Setting Up Ollama Web UI on Ubuntu Using Docker

Setting up Ollama Web UI on your Ubuntu server can significantly enhance your chat interface experience. This guide provides a detailed walkthrough, from installation prerequisites to Docker commands, ensuring you can follow along and set up your chat interface with ease.

## Prerequisites

Before we dive into the installation, ensure you have Docker installed on your Ubuntu server. If not, follow these initial steps:

<CodeBlock lang="bash">

```bash
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

</CodeBlock>


After the installation completes, you can verify that Docker is installed correctly by running:

<CodeBlock lang="bash">

```bash
docker --version
```

</CodeBlock>

Optionally: You might also want to ensure that Docker starts automatically with your system
<CodeBlock lang="bash">

```bash
sudo systemctl enable docker
```

</CodeBlock>

And to add your user to the Docker group so you can run Docker commands without sudo (replace your-user with your actual username):

<CodeBlock lang="bash">

```bash
sudo usermod -aG docker your-user
```

</CodeBlock>

Next make sure you have Ollama installed. If you're on linux run:

<CodeBlock lang="bash">

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

</CodeBlock>

To Update Ollama run that command again and it'll replace the old Ollama.

Side note to uninstall Ollama run:

<CodeBlock lang="bash"> 

```bash
sudo systemctl stop ollama
sudo systemctl disable ollama
sudo rm /etc/systemd/system/ollama.service
```

</CodeBlock>

To remove the downloaded models and Ollama service user and group:

<CodeBlock lang="bash">

```bash
sudo systemctl stop ollama
sudo systemctl disable ollama
sudo rm /etc/systemd/system/ollama.service
```

</CodeBlock>

To Stop Ollama run:


<CodeBlock lang="bash">

```bash
sudo systemctl stop ollama
```

</CodeBlock>

To Start Ollama run:

<CodeBlock lang="bash">

```bash
ollama serve
```

</CodeBlock>

## Step-by-Step Installation

1. Download the Latest Ollama Web UI Docker Image.

<CodeBlock lang="bash">

```bash
docker pull ghcr.io/ollama-webui/ollama-webui:main
```

</CodeBlock>

2. Run the Ollama Web UI Docker Image.

<CodeBlock lang="bash">

```bash
docker run -d --network=host -v ollama-webui:/app/backend/data -e OLLAMA_API_BASE_URL=http://127.0.0.1:11434/api --name ollama-webui --restart always ghcr.io/ollama-webui/ollama-webui:main
```

</CodeBlock>

Or

To build the container yourself, follow these steps:

<CodeBlock lang="bash">

```bash
docker build -t ollama-webui .
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v ollama-webui:/app/backend/data --name ollama-webui --restart always ollama-webui
```

</CodeBlock>

Then go to [http://127.0.0.1:11434](http://127.0.0.1:11434) or [http://localhost:3000](http://localhost:3000) in your browser to see the Ollama Web UI.

## To run on the internet so your friends can use your Ollama models experience your cool new WebUi follow these steps

Sign up for an account at ngrok. In short it will help us expose our local server to the internet. [ngrok](https://dashboard.ngrok.com/login)

<CodeBlock lang="bash">

```bash
docker pull ngrok/ngrok
```

</CodeBlock>

<CodeBlock lang="bash">

```bash
docker run --net=host -it -e NGROK_AUTHTOKEN=<YOUR_AUTH_TOKEN_GOES_HERE> ngrok/ngrok:latest http 11434
```

</CodeBlock>

## My Favorite Features

- Intuitive Interface & Responsive Design: A Ui that looks and feels like a familiar app.

- Local RAG Integration: Enhance chat interactions with document integration capabilities.

- Web Browsing Capability: Embed web content directly into your chats.

- Prompt Preset Support: Access and import predefined prompts easily.

- Download/Delete Models: Manage your models directly from the web UI.

- GGUF File Model Creation: Create Ollama models by uploading GGUF files.

- Multiple Model Support: Switch between various chat models for diverse interactions.

- Modelfile Builder: Customize chat elements and create modelfiles through the web UI.

- Many Models Conversations: Engage with multiple models simultaneously for enriched responses.

- Collaborative Chat: Orchestrate group conversations with multiple models.

- OpenAI API Integration: Integrate with OpenAI-compatible APIs for versatile chats.

- Regeneration History Access: Revisit your entire regeneration history.

- Chat History & Import/Export: Manage your conversation history.

- Role-Based Access Control (RBAC): Ensure secure access with restricted permissions.

## Conclusion

By following these steps, you can successfully set up Ollama Web UI on your Ubuntu server
using Docker. This guide aims to make the installation process as straightforward as possible, allowing you to enjoy the full range of features Ollama Web UI offers.

> [!info]
> This tutorial will show how to install and run `ollama` and a nice **web ui** frontend for it

# Ollama

## Install via **brew**

```bash
brew install ollama
```

## Running Ollama as a daemon

> [!note]
> To start ollama now and restart at login


```bash
brew services start ollama
```

## Discard Ollama as a background service

> [!tip] 
> If you don't want/need a background service you can just run:
  
  ```bash
  /opt/homebrew/opt/ollama/bin/ollama serve
  ```

# Open Web UI

## Install and run via podman

```bash
podman run -d \
  --name open-webui \
  --net=host \
  --env 'OLLAMA_BASE_URL=http://localhost:11434' \
  --env 'ANONYMIZED_TELEMETRY=False' \
  -v open-webui:/app/backend/data \
  --label io.containers.autoupdate=registry \
  ghcr.io/open-webui/open-webui:main
```
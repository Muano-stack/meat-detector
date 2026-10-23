# Meat type detector

CES117V capstone project. A YOLOv8n model, fine-tuned on a labelled dataset of raw chicken, beef,
pork and lamb, detects those types of meat in uploaded videos and photos. The model is served by a
Flask app, packaged with Docker, built by GitHub Actions and run on Azure Container Apps.

## Files

| File | What it does |
|---|---|
| `app.py` | Flask backend. Loads `best.pt` and handles `/`, `/health`, `/predict` and `/outputs/<name>`. |
| `best.pt` | The fine-tuned YOLOv8n weights. |
| `templates/index.html` | The web page for uploading a video or photo and seeing the result. |
| `requirements.txt` | Python packages (PyTorch CPU is installed in the Dockerfile). |
| `Dockerfile` | Build and run instructions for the container. |
| `.github/workflows/docker.yml` | GitHub workflow that builds the Docker image and publishes it to the GitHub Container Registry. |

## Run it locally

```
pip install flask ultralytics
python app.py
```

Then open http://127.0.0.1:8080 in a browser. (ffmpeg must be installed for the result
video to play in every browser.)

## Deploy

1. Push this folder to a GitHub repository. The workflow builds the image and publishes it as
   `ghcr.io/<owner>/meat-detector:latest`. Make the package public.
2. In the Azure portal, create a Container App that uses that image, with ingress enabled on
   target port 8080, 1 CPU and 2 GiB of memory.

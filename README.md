# ComfyUI Serverless Deployment

This repository contains a Docker configuration for deploying a ComfyUI workflow with specific models and custom nodes in a serverless environment.

## Workflow Details

This deployment is configured for the `flux-schnell-RealESRGAN_x2` workflow, which includes:

- **Base Image**: RunPod ComfyUI worker (5.1.0-base)
- **Custom Nodes**: comfyui-kjnodes
- **Models**:
  - flux1-schnell-fp8.safetensors (FLUX1 checkpoint)
  - RealESRGAN_x2.pth (upscaler)

## Quick Start

### Local Development

To build and run the Docker container locally:

```bash
# Build the Docker image
docker build -t comfyui-serverless .

# Run the container
docker run -p 8188:8188 comfyui-serverless
```

After starting the container, access ComfyUI at `http://localhost:8188`.

### Deploying on RunPod

1. Log in to your RunPod account
2. Create a new pod with a GPU template
3. In the "Container" section, select "Custom" and enter the Docker Hub image URL or use a private registry
4. For a private repository, provide your Docker credentials
5. Start the pod and access ComfyUI through the exposed port

## Dockerfile Explanation

The Dockerfile:
1. Uses the RunPod ComfyUI worker as a base image
2. Installs the KJNodes custom node package
3. Downloads the flux1-schnell model and places it in the checkpoints folder
4. Downloads the RealESRGAN_x2 upscaler and places it in the upscale_models folder

## Customization

To customize this deployment:

1. Modify the Dockerfile to add additional custom nodes:
   ```dockerfile
   RUN comfy-node-install [node-package-name]
   ```

2. Add more models by adding additional download commands:
   ```dockerfile
   RUN comfy model download --url [model-url] --relative-path [destination-path] --filename [filename]
   ```

3. To use a different base image, update the FROM line in the Dockerfile

## License

This project is provided as-is. The models used may have their own licenses - please refer to their respective sources for license information.

## Links

- [ComfyUI GitHub Repository](https://github.com/comfyanonymous/ComfyUI)
- [RunPod Documentation](https://docs.runpod.io/)
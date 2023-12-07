#Notes

```shell
docker build -t automatic1111:0.0 stable-diffusion-webui
docker run -d -p 7860:7860 --name stableDiff --gpus=all automatic1111:0.0
```

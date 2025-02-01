```bash
docker pull ultralytics/yolov5
```

```bash
docker run --gpus all -it --rm -v $(pwd):/usr/src/app ultralytics/yolov5:latest \
    python detect.py --weights yolov5s.pt --img 640 --conf 0.25 --source data/images/

```


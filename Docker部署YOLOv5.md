```bash
docker pull ultralytics/yolov5
```

```bash
docker run （如果有GPU这里可以加上 <--gpus all> ）-it --rm ultralytics/yolov5:latest python detect.py --weights yolov5s.pt --img 640 --conf 0.25 --source data/images/

```


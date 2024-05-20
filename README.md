# Image Project: Working Safety Monitoring Using Yolov9 (Object Detection)

## Introduction

This project aims to monitor worker safety in various environments using YOLOv9, an advanced object detection model. The primary goal is to identify potential safety hazards and ensure that safety protocols are being followed.

## How to apply model YOLOv9

1. Setting up environment
   - Clone model from https://github.com/WongKinYiu/yolov9
     ```bash
     git clone https://github.com/WongKinYiu/yolov9.git
     cd yolov9
     ```
   - Download requirements
     ```bash
     pip install -r requirements.txt
     ```
2. Training
- You can decrease epochs to reduce time in training or you can increase batch whether having GPU is enough powerful.
```bash
!python train_dual.py \
  --workers 8 \
  --batch 8 \
  --img 640 \
  --epochs 5 \
  --data /mydrive/working_safety_monitoring/yolov9/data.yaml \
  --weights /mydrive/working_safety_monitoring/yolov9-c.pt \
  --device 0 \
  --cfg /mydrive/working_safety_monitoring/yolov9/models/detect/yolov9_custom.yaml \
  --hyp /mydrive/working_safety_monitoring/yolov9/data/hyps/hyp.scratch-high.yaml
```
3. Predicting
- `IMPORTANCE` : at the present, source code is having a little issues when we run evaluation and inference with model YOLOv9. To fix those issues, you should access into file `utils/general.py` and fix code at line 903 from `prediction = prediction[0]` to `prediction = prediction[0][1]` [here](http://~github.com/WongKinYiu/yolov9/issues/11#issuecomment-1960627261)
```bash
!python val.py \
  --img 640 \
  --batch 8 \
  --conf 0.001 \
  --iou 0.7 \
  --device 0 \
  --data /mydrive/working_safety_monitoring/yolov9/data.yaml \
  --weights /mydrive/working_safety_monitoring/yolov9/runs/train/exp2/weights/best.pt
``` 
4. Inference
```bash
!python3 detect.py \
  --img 640 \
  --conf 0.1 \
  --weights /mydrive/working_safety_monitoring/yolov9/runs/train/exp2/weights/best.pt \
  --data /mydrive/working_safety_monitoring/yolov9/data.yaml \
  --source /mydrive/working_safety_monitoring/construction10.jpg
```
## Results

![image](https://github.com/mtue04/working-safety-monitoring/assets/120604992/b0c436df-88a6-416b-bd86-b1420e1d2731) \
![construction1](https://github.com/mtue04/working-safety-monitoring/assets/120604992/eecc77ac-c83c-49b3-994c-03fb575cd9ec) \
![construction3](https://github.com/mtue04/working-safety-monitoring/assets/120604992/75b3550e-413f-4587-95e6-81bef2351a79)

## References

1. Image Project: Working Safety Monitoring Using Yolov9 (Object Detection)
2. YOLOv9:
  - Paper: https://arxiv.org/pdf/2402.13616
  - GitHub: https://github.com/WongKinYiu/yolov9
3. Dataset: https://universe.roboflow.com/computer-vision/worker-safety/

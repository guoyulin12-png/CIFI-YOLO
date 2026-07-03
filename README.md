````markdown
# CIFI-YOLO

Official implementation of **CIFI-YOLO: A SWaP-Aware Object Detector for UAV Optical Sensors via Cross-Iterative Fusion and Gated Attention**.

This repository provides the anonymized source code, model definitions, training scripts, testing scripts, configuration files, and reproduction instructions for the experimental results reported in the manuscript.

## 1. Environment

The code was tested with the following environment:

- Python 3.8+
- PyTorch 2.0.1
- CUDA 11.x
- NVIDIA RTX 3080 Ti GPU

Install dependencies:

```bash
pip install -r requirements.txt

## 2. Dataset Preparation

The experiments use three UAV-oriented object detection datasets:
VisDrone2019
UAV-DT
CODrone
Please organize each dataset in YOLO format:

datasets/
├── VisDrone2019/
│   ├── images/
│   │   ├── train/
│   │   ├── val/
│   │   └── test/
│   └── labels/
│       ├── train/
│       ├── val/
│       └── test/
├── UAV-DT/
└── CODrone/

configs/datasets/visdrone.yaml
configs/datasets/uavdt.yaml
configs/datasets/codrone.yaml

## 3. raining

Train CIFI-YOLO on VisDrone2019:

python train.py --model configs/models/cifi_yolo.yaml --data configs/datasets/visdrone.yaml --img 640 --epochs 200 --batch 4 --optimizer AdamW --lr 1e-4 --weight-decay 1e-4 --device 0

Train on UAV-DT:

python train.py --model configs/models/cifi_yolo.yaml --data configs/datasets/uavdt.yaml --img 640 --epochs 200 --batch 4 --optimizer AdamW --lr 1e-4 --weight-decay 1e-4 --device 0

Train on CODrone:

python train.py --model configs/models/cifi_yolo.yaml --data configs/datasets/codrone.yaml --img 640 --epochs 200 --batch 4 --optimizer AdamW --lr 1e-4 --weight-decay 1e-4 --device 0

## 4. Evaluation
Evaluate a trained checkpoint:

python test.py --weights weights/cifi_yolo.pt --data configs/datasets/codrone.yaml --img 640 --batch 4 --device 0

Measure inference latency:

python test.py --weights weights/cifi_yolo.pt --data configs/datasets/codrone.yaml --img 640 --batch 1 --device 0 --profile --amp False

## 5. Inference

Run inference on custom images:

python detect.py --weights weights/cifi_yolo.pt --source examples/images --img 640 --device 0

## 6. Main Results

All baseline models were reproduced under the same experimental settings, including input resolution, training epochs, optimizer, batch size, and evaluation protocol.

| Dataset | Params | GFLOPs | Latency | AP50 | APS | F1-score | Recall |
|---|---:|---:|---:|---:|---:|---:|---:|
| VisDrone2019 | 3.89M | 30.4 | 9.08 ms | 44.5 | 25.0 | 0.55 | 53.2 |
| UAV-DT | 3.89M | 30.4 | 9.08 ms | 38.2 | 19.9 | 0.50 | 47.9 |
| CODrone | 3.89M | 30.4 | 9.08 ms | 33.8 | 17.0 | 0.47 | 45.2 |




























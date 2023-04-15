# fine-tune

## 1. 环境，跑通

pytorch opencv matplotlib pandas tqdm pyyaml scipy tensorboard wandb

- `data/power_system_detection.yaml`中填绝对路径了，否则报错，因为是相对于子模块的文件位置来寻址
- 出现错误
  
  ```bash
    from_which_layer = from_which_layer[fg_mask_inboxes]
    RuntimeError: indices should be either on cpu or on the same device as the indexed tensor (cpu)
  ```

  把indicies如这里的`fg_mask_inboxes`，改成`fg_mask_inboxes.to('cpu')`

## 2. 微调
- 改cfg，参数里的`number of classes`
  - `yolov7\cfg\power_system\yolov7-w6-guangzhou.yaml`
- 改数据集配置文件，数据集需要构建成yolo格式的：
  - `yolov7\data\2303_all.yaml`
- 超参数，可选修改，一般是调低lr
  - `yolov7\data\hyp.scratch.custom.yaml`

```bash
# finetune w6 models
python train_aux.py --workers 8 --device 0 --batch-size 4 --data data/2303-all.yaml --img 1280 1280 --cfg cfg/power_system/yolov7-w6-guangzhou.yaml --weights weights/yolov7-w6.pt --name yolov7-w6-2303-all --hyp data/hyp.scratch.custom.yaml
```

```bash
python detect.py --weights runs\train\yolov7-w6-2303-all\weights\best.pt --conf 0.15 --img-size 1280 --source inference\guangzhou-2303\video.mp4
```

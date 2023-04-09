# fine-tune

pytorch opencv matplotlib pandas tqdm pyyaml scipy tensorboard wandb

- `data/power_system_detection.yaml`中填绝对路径了，否则报错，因为是相对于子模块的文件位置来寻址
- 出现错误
  
  ```bash
    from_which_layer = from_which_layer[fg_mask_inboxes]
    RuntimeError: indices should be either on cpu or on the same device as the indexed tensor (cpu)
  ```

  把indicies如这里的`fg_mask_inboxes`，改成`fg_mask_inboxes.to('cpu')`

```bash
# finetune w6 models
python train_aux.py --workers 8 --device 0 --batch-size 4 --data data/power_system_detection.yaml --img 1280 1280 --cfg cfg/power_system/yolov7-w6.yaml --weights weights/yolov7-w6.pt --name yolov7-w6_power-det --hyp data/hyp.scratch.custom.yaml
```

```bash
python detect.py --weights runs/train/yolov7-w6_power-det3/weights/best.pt --conf 0.15 --img-size 1280 --source inference/cranes
```

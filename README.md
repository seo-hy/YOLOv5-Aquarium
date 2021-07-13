# YOLOv5 Custom Test : Aquarium

### Setting 
```angular2html
docker pull ultralytics/yolov5:v5.0
sudo docker run -it --gpus '"device=0,1,2"' -p {port}:22 --ipc=host --name {set container name} {image id} /bin/bash
```

Use dataset from roboflow and edit data.yaml

If you do the labeling yourself, install labelImg and select yolo version(txt file)
  ```
  pip install labelImg
  labelImg
  ```

* data.yaml
```angular2html
train: ./dataset/train/images
val: ./dataset/valid/images

nc: 7
names: ['fish', 'jellyfish', 'penguin', 'puffin', 'shark', 'starfish', 'stingray']
```
```angular2html
├── dataset
│   ├── test
│   │   ├── images
│   │   └── labels
│   ├── train
│   │   ├── images
│   │   └── labels
│   └── valid
│       ├── images
│       └── labels
```

### Train
```angular2html
python3 train.py --img 416 --batch 66 --epochs 400 --data ./dataset/data.yaml --cfg ./models/yolov5s.yaml --weights yolov5s.pt --name aquarium_test
```

### Detect
```angular2html
python3 detect.py --weights ./runs/train/aquarium_test/weights/best.pt  --img 416 --conf 0.5 --source /home/seohyun/YOLOv5-Aquarium/dataset/valid/images/IMG_2278_jpeg_jpg.rf.3c7006d683b0fc62b9b5d84a2868c31c.jpg
```
![result](https://user-images.githubusercontent.com/68395698/121138582-cf057380-c872-11eb-8b11-0a54e2aef3aa.png)


### Docker Container 
* ssh setting
```angular2html
apt-get update
apt-get install ssh
```

```angular2html
passwd
service ssh restart
vi /etc/ssh/sshd_config
```
* Do the following changes : Uncomment
```
 PermitRootLogin yes
 Port 22
 AddressFamily any
 ListenAddress 0.0.0.0
 ListenAddress ::
 PasswordAuthentication yes
```

* exec
```angular2html
docker start {container name}
sudo docker exec -it {container name} /bin/bash
#service ssh restart
docker stop {container name}
```

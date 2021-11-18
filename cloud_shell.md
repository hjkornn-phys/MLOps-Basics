# Google Clould Shell

[Direct Link](https://shell.cloud.google.com)

## 1. Create venv and activate

```bash
$ python -m venv venv  # venv라는 가상환경을 만든다
$ source venv/bin/activate  # venv 활성화. (venv)로 표시된다. 이 코드는 매번 다시 실행해야 한다
$ git clone https://github.com/hjkornn-phys/MLOps-Basics  # Repo에서 내용 가져오기

$ pip3 install pip --upgrade  # pip3를 업그레이드
$ pip3 install -r requirements.txt  # 필요한 lib을 설치한다
$ pip3 install dvc
$ pip3 install 'dvc[gdrive]'
```

## 2.  Inititate dvc, wandb



```bash
$ cd ~  # 최상단 폴더
$ dvc init -f  # dvc initiate
$ dvc remote add -d storage gdrive://198ADfCqBvA3St0Et7O-fyVwGYwpbKt8U  # 기본 저장소를 정비메모처리/MLOps 로 한다

$ wandb login
$ wandb init
```

## 3. Train model,  push to dvc



```bash
$ cd ~/MLOps-Basics/week_4_onnx  # 이 dir에 onnx로 바꾸는 파일과 inference 모두 존재한다
$ python3
>>> import wandb  # train 마다 꼭 해줘야 한다. 고치는 중
>>> wandb.init()  # train 마다 꼭 해줘야 한다.
>>> import os  
>>> os.system("python3 train.py")  # bash에 보내는 명령어
>>> exit()
$ mkdir ./dvcfiles
$ cd ./dvcfiles
$ dvc add ../models/best-checkpoint.ckpt --file trained_model.dvc  # stage model as trained_model.dvc
$ dvc push trained_model.dvc  # push model to gdrive
```

## 4. Versioning Models with git

```bash
$ git tag -a "v1.0" -m "Version 1.0"  # 버전명과 메시지로 commit
$ git commit -m "updated model version"
$ git push
# push the tag also
$ git push origin v1.0
```

## 5. Pull model from dvc, Convert model to ONNX, Inference with ONNX runtime

```bash
$ rm ../models/best-checkpoint.ckpt
$ ls ..models

$ dvc pull trained_model.dvc
$ ls ..models
$ cd ~/MLOps-Basics/week_4_onnx
$ python3 convert_model_to_onnx.py
$ pip3 install onnxruntime
$ python3 inference_onnx.py
```


---
tags:
- Diff-SVC
- DiffSVC
- Pre-trained Model
---
# Pre-trained Models For Diff-SVC_nofs2
[简体中文](README.md)  | English
## Explanation


### Repos

|                      Repo                       |                             Link                             |
| :---------------------------------------------: | :----------------------------------------------------------: |
|                    Diff-SVC                     |     [Click Me!](https://github.com/prophesier/diff-svc)      |
|               NSF-HiFiGAN Vocoder               |       [Click Me!](https://openvpi.github.io/vocoders)        |
|                M4Singer Dataset                 |      [Click Me!](https://github.com/M4Singer/M4Singer)       |
| DiffSVCBaseModel(My friend's pre-trained model) | [Click Me!](https://huggingface.co/HuanLin/DiffSVCBaseModel) |
|            Genshin Impact SVC Models            | [Click Me!](https://huggingface.co/Erythrocyte/Diff-SVC_Genshin_Models) |

### Introduction

The models are pre-trained models for Diff-SVC, if your datasets for training are less than 3 hours, you can use the pre-trained model to train your Diff-SVC model. The pre-trained models are trained by M4Singer. M4Singer is a large singing dataset, the dataset contains 30+ hours of audio. The pre-trained model only supports 44.1KHz NSF-HiFiGAN vocoder. Because of mixed male and female singing data when training the pre-trained model, the pre-trained model can use to train all types of Diff-SVC models.

**The pre-trained models provide many versions, please refer the following sheet：**

|                   Parameter                    |   lr   |                    Notice                     |                           Version                            |
| :--------------------------------------------: | :----: | :-------------------------------------------: | :----------------------------------------------------------: |
| rc: 384 /  rl: 20  / Steps: 110k / sr: 44.1KHz | 0.0024 |   total audio duration ＜ 1h, try it first    | [base_44.1KHz_384_20_110k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_110k.zip) |
| rc: 384 /  rl: 20  / Steps: 94k / sr: 44.1KHz  | 0.0012 | 1h ≤ total audio duration ＜ 2h, try it first | [base_44.1KHz_384_20_94k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_94k.zip) |
| rc: 384 /  rl: 20  / Steps: 50k / sr: 44.1KHz  | 0.0012 | 2h ≤ total audio duration ＜ 3h, try it first | [base_44.1KHz_384_20_50k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_50k.zip) |
| rc: 512 / rl: 20  / Steps: 110k / sr: 44.1KHz  | 0.0032 |   total audio duration ＜ 1h, try it first    | [base_44.1KHz_512_20_110k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_110k.zip) |
| rc: 512 /  rl: 20  / Steps: 94k / sr: 44.1KHz  | 0.0016 | 1h ≤ total audio duration ＜ 2h, try it first | [base_44.1KHz_512_20_94k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_94k.zip) |
| rc: 512 /  rl: 20  / Steps: 50k / sr: 44.1KHz  | 0.0016 | 2h ≤ total audio duration ＜ 3h, try it first | [base_44.1KHz_512_20_50k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_50k.zip) |

> rc: 'residual_channels' parameter in config file (```config_nsf.yaml```) ; rl: 'residual_layers' parameter in config file (```config_nsf.yaml```) ; sr: sampling rate<br>
> The config file is in the `diff-svc/training` directory;<br>
> You need to change the learning rate (lr) according to the actual condition.  The learning rate may be different with different batch sizes. In the sheet, I used 48 batch size<br>If you have more than 3 hours of high-quality datasets, recommend training models without any pre-trained model.

## How to use(You need in the diff-svc directory)

### 第一步

先按照正常训练流程进行训练，待出现 ```epoch 1``` 后按下 ```Ctrl+C``` 停止训练。

#### 训练流程

1. 准备数据集，数据集目录格式参考如下（xxx 代表要训练的模型名字）

   ```
   data
     ┗━raw
        ┗━xxx
           ┗━ *.wav 音频文件
   ```

2. 修改配置文件 trainings/config_nsf.yaml

   修改方法参考：[修改超参数配置](https://github.com/prophesier/diff-svc/blob/main/doc/train_and_inference.markdown#22-%E4%BF%AE%E6%94%B9%E8%B6%85%E5%8F%82%E6%95%B0%E9%85%8D%E7%BD%AE)

3. 对数据进行预处理

   **Windows平台参考如下指令**

   ```bash
   set PYTHONPATH=.
   set CUDA_VISIBLE_DEVICES=0 
   python preprocessing/binarize.py --config training/config_nsf.yaml
   ```

   **Linux平台参考如下指令**

   ```bash
   export PYTHONPATH=.
   CUDA_VISIBLE_DEVICES=0 python preprocessing/binarize.py --config training/config_nsf.yaml
   ```

4. 开始训练模型（xxx 代表模型名字）

   **Windows平台参考如下指令**

   ```bash
   set CUDA_VISIBLE_DEVICES=0 
   python run.py --config training/config_nsf.yaml --exp_name xxx --reset  
   ```

   **Linux平台参考如下指令**

   ```bash
   CUDA_VISIBLE_DEVICES=0 python run.py --config training/config_nsf.yaml --exp_name xxx --reset
   ```

**更加详细的训练流程参考**：[训练流程参考](https://www.yuque.com/jiuwei-nui3d/qng6eg/abaxpwozc2h5yltt)

### 第二步

**Windows平台：**

1. 打开目录： ```checkpoints/训练的模型目录```

2. 删除目录下的 ```所有文件``` 和 ```文件夹```

3. 根据自己练的模型的 ```网格规模``` 下载 合适的预训练模型，并解压到你训练的模型的目录

4. 打开 配置文件目录```(training)``` ，然后修改 ```config_nsf.yaml``` 中的 ```学习率 (lr)``` 为合适的值，值参考上表

5. 依次在命令行中输入如下指令并开始训练 (xxx代表你的模型名字) ：

    ```sh
    set CUDA_VISIBLE_DEVICES=0 
    python run.py --config training/config_nsf.yaml --exp_name xxx --reset
    ```

6. 如果开始训练后 **不是从 epoch 1 开始**，就说明预训练模型加载成功了。


**Linux平台：**

1. 输入如下指令删除训练模型目录下的所有文件以及文件夹

    ```sh
    rm -rf checkpoints/训练的模型目录/*
    ```

2. 输入如下指令切换到训练的模型目录

    ```bash
    cd checkpoints/训练的模型目录
    ```

3. 输入如下命令下载预训练模型，一定要和训练的模型的网格规模对应

    ```bash
    # 训练38420网格规模的模型下载这个
    # 数据集规模 ＜ 1h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_50k.zip
    # 1h ≤ 数据集规模 ＜ 2h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_94k.zip
    # 2h ≤ 数据集规模 ＜ 3h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_110k.zip
    ```
    ```bash
    # 训练51220网格规模的模型下载这个
    # 数据集总时长 ＜ 1h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_50k.zip
    # 1h ≤ 数据集规模 ＜ 2h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_94k.zip
    # 2h ≤ 数据集总时长 ＜ 3h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_110k.zip
    ```

4. 输入如下指令解压预训练模型，并等待解压完成

    ```bash
    # 38420网格规模版本
    # 50ks 版本
    unzip base_44.1KHz_384_20_50k.zip
    # 94ks 版本
    unzip base_44.1KHz_384_20_94k.zip
    # 110ks 版本
    unzip base_44.1KHz_384_20_110k.zip
    ```
    ```bash
    # 51220网格规模版本
    # 50ks 版本
    unzip base_44.1KHz_512_20_50k.zip
    # 94ks 版本
    unzip base_44.1KHz_512_20_94k.zip
    # 110ks 版本
    unzip base_44.1KHz_512_20_110k.zip
    ```

5. 修改训练配置文件 ```(config_nsf.yaml)```

    如果没有安装 ```jupyter-lab``` 之类的可视化训练环境，参考如下步骤修改配置文件：

    [vim具体使用方法参考](https://www.runoob.com/linux/linux-vim.html)

    ```bash
    # 返回至 Diff-SVC 目录
    cd ../../
    # 修改配置文件
    vim training/config_nsf.yaml
    ```
\* **如果已经安装 ```jupyter-lab``` 之类的可视化训练环境，参考如下步骤修改配置文件，这里以 jupyter-lab 为例**

1. 双击打开 ```training``` 目录；

2. 找到 ```config_nsf.yaml``` ，并右击该文件，在菜单中选择 ```Open With→Editor```，并点击；

3. 点击右侧编辑窗口，并按下 ```Ctrl+F``` 搜索 ```lr``` ；

4. 将 ```lr:``` 后面的值改成对应的参考值，参考值见上表；

5. 按下 ```Ctrl+S``` 保存文件。

6. 返回到Diff-SVC根目录，在终端输入如下指令开始训练 (xxx代表你的模型名字) ：


    ```bash
    CUDA_VISIBLE_DEVICES=0 python run.py --config training/config_nsf.yaml --exp_name xxx --reset 
    ```

7. 如果开始训练后 **不是从 epoch 1 开始**，就说明预训练模型加载成功了。

## 精简模型(以下操作均需要在 Diff-SVC 目录下进行)

如果要公开已训练的模型，建议对模型进行精简，精简后的模型 ```只能用于推理``` ，无法继续训练。

可在Diff-SVC的目录下输入如下指令进行精简模型，精简后会在Diff-SVC目录生成 ```clear``` 开头的模型，即为精简过的模型。

```bash
python  simplify.py --proj 你训练的模型名字 --steps 最终步数
```

精简模型后如果要发布模型，可以把目录 ```checkpoints/你训练的模型``` 中的 ```config.yaml``` 复制出来，然后打包在一起发布。

### Windows平台打包模型

1. 在 diff-svc 目录建立一个文件夹，名字为 ```你训练的模型的名字```；

2. 将 clear 开头的模型复制到刚才新建的文件夹里面；

3. 将目录 ```checkpoints/你训练的模型``` 中的 ```config.yaml``` 文件复制到刚才新建的文件夹里面；

4. 复制完成后，将刚才创建的的文件夹通过 ```任意压缩工具``` 压缩；

5. 将压缩包上传到网盘，并将分享链接发给他人。

### Pack model on linux

1. Run this command to make a new folder

    ```bash
    mkdir {your speaker name}
    ```

2. Then,run this command to copy the pre-trained model's config into your model folder 

    ```bash
    cp checkpoints/{your speaker name}/config.yaml {your speaker name}/
    ```

3. Run this command to copy your simplify model into the folder which your create just now

    ```bash
    cp {your simplify model name with extension} {your created folder}/
    ```

4. To pack your simplify model into .zip file, you can run this command

    ```bash
    zip -r {model name}.zip {your created folder}
    ```

    If you machine can't use ```zip``` command, you can run this command to pack your simplify model into .tar.gz file

    ```bash
    tar -zcvf {model name}.tar.gz {your created folder}
    ```

5. To download it, you can use ```ftp``` to download to your computer or upload to your webdriver. If you are using ```jupyter-lab``` , you can right-click your packed file and click ```Download``` to download it to your computer, then upload it to webdriver
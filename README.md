---
tags:
- Diff-SVC
- DiffSVC
- Pre-trained Model
---
# Pre-trained Models For Diff-SVC_nofs2
简体中文  | [English](README_EN.md)
## 说明


### 仓库地址

| 仓库      | 传送门 |
| :---------: | :---------: |
| Diff-SVC      | [点此传送](https://github.com/prophesier/diff-svc)  |
| 44.1KHz声码器地址   | [点此传送](https://openvpi.github.io/vocoders)|
| M4Singer数据集   | [点此传送](https://github.com/M4Singer/M4Singer) |
| DiffSVCBaseModel(有更多Steps版本的) | [点此传送](https://huggingface.co/HuanLin/DiffSVCBaseModel) |
| 原神SVC模型合集 | [点此传送](https://huggingface.co/Erythrocyte/Diff-SVC_Genshin_Models) |

### 模型介绍

该模型为 **Diff-SVC** 的 **预训练模型**，如果训练用数据集过少(**总时长低于3小时**)，可以配合该预训练模型进行训练。该预训练模型由 **总时长为30小时左右** 的 **M4singer** 大型 **歌声数据集** 训练而成，目前已提供不同步数版本，按需选用。该预训练模型 **仅支持** 训练使用 **44.1KHz声码器** 的模型。由于训练时候混合了男声和女声，理论上可以接着底模训练任意模型。

**目前该预训练模型提供如下网格规模以及步数的版本(见下表)：**

|     底模参数      | lr 修改参考 |                        备注                        |                        对应版本压缩包                        |
| :-------------------: | :----------------------------------------------------------: | :------------------: | :------------------: |
| 宽度：384 /  深度：20  / Steps：110k / 采样率：44.1KHz | 0.0024 | 数据集规模 ＜ 1h，可以先尝试这个 | [base_44.1KHz_384_20_110k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_110k.zip) |
| 宽度：384 /  深度：20  / Steps：94k / 采样率：44.1KHz | 0.0012 | 1h ≤ 数据集规模 ＜ 2h，可以先尝试这个 | [base_44.1KHz_384_20_94k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_94k.zip) |
| 宽度：384 /  深度：20  / Steps：50k / 采样率：44.1KHz |        0.0012        | 2h ≤ 数据集规模 ＜ 3h，可以先尝试这个 | [base_44.1KHz_384_20_50k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_50k.zip) |
| 宽度：512 / 深度 ：20  / Steps：110k / 采样率：44.1KHz |        0.0032        | 数据集规模 ＜ 1h，可以先尝试这个 | [base_44.1KHz_512_20_110k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_110k.zip) |
| 宽度：512 /  深度：20  / Steps：94k / 采样率：44.1KHz | 0.0016 | 1h ≤ 数据集规模 ＜ 2h，可以先尝试这个 | [base_44.1KHz_512_20_94k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_94k.zip) |
| 宽度：512 /  深度：20  / Steps：50k / 采样率：44.1KHz | 0.0016 | 2h ≤ 数据集规模 ＜ 3h，可以先尝试这个 | [base_44.1KHz_512_20_50k.zip](https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_50k.zip) |

> 宽度 指 配置文件(```config_nsf.yaml```) 中的 residual_channels 项；深度 指 配置文件(```config_nsf.yaml```) 中的 residual_layers 项；<br>
> 配置文件(```config_nsf.yaml```) 在 diff-svc 的 ```training``` 目录里面；<br>
> 学习率 lr 需要根据实际情况修改，不同 bs 的起步 lr 可能不同，表格中的 lr 是以 48 bs 为参考的；<br>总时长 ```≥3h``` 的高质量数据集，建议 ```不用底模```，从零开始训练。

## 使用方法(以下操作均需要在 Diff-SVC 目录下进行)

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

   如果用的是自己提供的训练集，一般只需修改这些：`residual_channels`、`residual_layers`、`lr`、`max_sentences`、`binary_data_dir`、`raw_data_dir`、`speaker_id`、`work_dir`

   如果用了我提供的 Diff-SVC 训练用数据集（一般在模型发布页会有对应训练集），只需修改这些：`residual_channels`、`residual_layers`、`lr`、`max_sentences`

   修改方法参考：[修改超参数配置](https://github.com/prophesier/diff-svc/blob/main/doc/train_and_inference.markdown#22-%E4%BF%AE%E6%94%B9%E8%B6%85%E5%8F%82%E6%95%B0%E9%85%8D%E7%BD%AE)

3. 对数据进行预处理（如果已经完成预处理，请跳过此步）

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
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_110k.zip
    # 1h ≤ 数据集规模 ＜ 2h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_94k.zip
    # 2h ≤ 数据集规模 ＜ 3h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_384_20_50k.zip
    ```
    ```bash
    # 训练51220网格规模的模型下载这个
    # 数据集总时长 ＜ 1h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_110k.zip
    # 1h ≤ 数据集规模 ＜ 2h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_94k.zip
    # 2h ≤ 数据集总时长 ＜ 3h
    wget https://huggingface.co/Erythrocyte/Diff-SVC_Pre-trained_Models/resolve/main/base_44.1KHz_512_20_50k.zip
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

### Linux平台打包模型

1. 输入以下命令创建文件夹

```bash
mkdir 你训练的模型名字
```

2. 输入以下指令将配置文件复制到刚才新建的文件夹

```bash
cp checkpoints/你训练的模型/config.yaml 你刚才创建的文件夹/
```

3. 输入以下指令将精简后的模型复制到刚才新建的文件夹（xxx 代表精简后的 ckpt 的名字）

```bash
cp xxx 你刚才创建的文件夹/
```

4. 输入以下指令打包模型（zip方式）

```bash
zip -r 模型名字.zip 你刚才创建的文件夹
```

如果用不了 zip 指令，可以自己安装一个 zip ，或者用如下指令打包

```
tar -zcvf 模型名字.tar.gz 你刚才创建的文件夹
```

5. 通过 ```sftp``` 等等方式将模型下载下来上传网盘。如果用的是 ```jupyter-lab``` 之类的可视化训练环境，可以右击压缩包点击 ```download``` 下载，然后上传网盘。
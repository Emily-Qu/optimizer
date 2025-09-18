# MiniNet-OptBench

本仓库可以快速上手，用于训练/微调中等规模GPT等模型并进行优化器基准测试。包含两个模块：nanoGPT用于文本生成数据集的训练和测试；cifar10-airbench用于cifar-10数据集的图像分类问题。
---

## 安装
```sh
git clone https://github.com/Emily-Qu/optimizer.git
```

## nanoGPT

### 配置环境

```sh
pip install torch numpy transformers datasets tiktoken wandb tqdm

```
依赖项:

- [pytorch](https://pytorch.org) <3
- [numpy](https://numpy.org/install/) <3
-  `transformers` for huggingface transformers <3 (to load GPT-2 checkpoints)
-  `datasets` for huggingface datasets <3 (if you want to download + preprocess OpenWebText)
-  `tiktoken` for OpenAI's fast BPE code <3
-  `wandb` for optional logging <3
-  `tqdm` for progress bars <3
(requires `torch` and `torchvision` to be installed)

---

### 快速开始

准备数据：

nanoGPT可以为shakespeare,shakespeare_char,openwebtext,openwebtext-small(对openwebtext进行3%采样得到的轻量数据集)四个数据集提供基准测试。
在./optimizer/nanoGPT/data目录下，分别有对这四个数据集的子目录，以openwebtext-small为例，执行以下命令：
```sh
cd ./optimizer/nanoGPT
python data/openwebtext-small/prepare.py

```
这样会创建`train.bin`和`val.bin`两个数据文件，现在就可以开始进行实验：
单卡运行时：

```sh
python train.py config/train_openwebtext-small.py

```
多卡运行时：

```sh
torchrun --standalone --nproc_per_node=8 train.py config/train_openwebtext-small.py
```


文件说明：train.py文件开头的配置是实验默认配置；config/train_openwebtext-small.py文件是针对实验的配置，会覆盖重复的参数设置；



### 关于魔改
用户可以根据需要修改config目录下文件以及train.py文件和model.py文件中的配置


## cifar-10

### 配置环境

```sh
cd cifar10-airbench
pip install -r requirements.txt
```

### 关于魔改

代码已经内置数据集的准备，因此研究人员可以使用该项目快速验证新的算法或模型。直接修改代码的配置，优化器等部分即可快速开始实验
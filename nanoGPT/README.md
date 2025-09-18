# MiniNet-OptBench

本仓库快速上手且自动化程度较高，用于训练/微调中等规模GPT等模型并进行优化器基准测试。
---

## 安装及配置环境

使用 Conda 创建并激活环境：

```sh
conda env create -f environment.yml
conda activate MiniNet-OptBench
````

---

## 安装及配置环境

运行提供的shell脚本来对优化器进行基准测试：

```sh
bash generate_loss_table.sh
```

该脚本将自动完成：

数据预处理与缓存管理（生成 data/{dataset}/train.bin 和 data/{dataset}/val.bin）
各优化器的超参数设置
结果自动汇总与记录（保存至 results/bench_runs.csv）
---

## 核心功能配置

### 数据集选择

```bash
DATASET=`shakespeare`
```

支持的数据集：

* `shakespeare`
* `shakespeare_char`
* `openwebtext`
* `openwebtext-small`(从openwebtext数据集采样3%后的小型数据集)
---

### 优化器配置

* **Adam / AdamW 系列**

```bash
ADAM_LRS="0.0001 0.0003 0.001"   
ADAMW_LRS="0.0001 0.0003 0.001"
```

---




## 🔧 How to Run Different Optimizers

The script defines grids for each optimizer family:

* **Adam / AdamW**

  ```bash
  ADAM_LRS="0.0003 0.001" 
  ADAMW_LRS="0.0003 0.001"
  ```

* **SGD**

  ```bash
  SGD_LRS="0.01 0.05" 
  SGD_MOMS="0.0 0.9"
  ```

* **Muon**

  ```bash
  MUON_LRS="0.01 0.02" 
  MUON_MOMS="0.95 0.99"
  ```

* **Muon + Aux Adam (hybrid)**

  ```bash
  OPT_LIST='("MUON_WITH_AUX_ADAM")'
  ```

Each run is tagged with a descriptive `RUN_NAME` like `MUON_lr0.01_m0.95` so you can track curves.

---

## 📊 Visualizing Results

Once training runs are done, you can generate loss-vs-iteration plots:

```bash
python viz_curves.py --csv results/<DATASET>_runs.csv --dataset <DATASET> --split val_eval --outdir results/figs
```

This will produce:

* Best run per optimizer
* All runs overlaid
* Separate plots per optimizer

---

## 🙏 Acknowledgements

Thanks to **Cardinal Operations** for funding, which made this repository possible.
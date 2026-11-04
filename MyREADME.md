一.代码克隆和数据集下载

1.通过 **git** 克隆代码

```
git clone <本仓库地址> 
```

2.下载数据集

本次复现数据集未上传仓库，根据原仓库作者，所有基准数据集均可在 **Hugging Face** 数据集中心获取，以下为链接：

https://huggingface.co/datasets/Philley/DHG-Bench-dataset

从 **Hugging Face** 下载数据集，得到文件夹：**DHG-Bench-dataset**，取出里面的 **data.zip** 压缩包，解压得到 **data** 文件夹，放入 **HGNNStudy** 文价夹中。

最后结构如下所示:

```bash
HGNNStudy
  ├── data
  │   ├── fair_data
  │   ├── hete_data
  |   ├── hgcls_data
  │   ├── trad_data
  |   └── ...
  └── dhgbench
  │   ├── main.py
  │   ├── parameter_parser.py
  │   ├── lib_dataset
  │   ├── lib_models
  │   ├── requirements.txt
  └── └── ...
```



二.运行环境安装

1.安装 uv

原仓库作者未说明使用什么包管理工具，所以本次复现使用 **uv** 进行包管理，**uv** 的下载不做说明，可以参考下面给出的链接，已安装 **uv** 请忽略：

https://github.com/astral-sh/uv

2.虚拟环境创建与激活

进入 **HGNNStudy\dhgbench** 目录，打开 **cmd** 终端，输入下列语句创建并激活虚拟环境，注意这里一行是一条语句，一共两条：

```
uv venv --python 3.9.21 .venv
.venv\Scripts\activate
```

 3.依赖安装

根据原仓库作者的依赖版本要求，本次复现用的是最低的符合原作者要求的版本，如下所示：

* Python==3.9.21
* Pytorch==2.2.2
* torch_geometric==2.6.1
* torch-cluster==1.6.3
* torch-scatter==2.1.2
* torch-sparse==0.6.18
* torch-spline-conv==1.2.2
* deeprobust==0.2.11
* ipdb==0.13.13
* numpy==1.24.3
* dask==2024.8.0
* pandas==2.0.3

所有依赖及对应版本均在 **requirements.txt** 中记录，激活 **uv** 虚拟环境后通过下列命令安装依赖(这是一整条命令)：

```
uv pip install -r requirements.txt -f https://data.pyg.org/whl/torch-2.2.0+cu121.html --extra-index-url https://download.pytorch.org/whl/cu121
```



三.实验复现

1.table 1 复现

1.1激活环境

这里采用 **cmd** 终端运行，首先进入**HGNNStudy\dhgbench** 目录的 cmd 终端，输入以下命令激活 uv 虚拟环境：

```
.venv\Scripts\activate
```



1.2 输入命令进行训练

复现采用5次取平均值的方式得到最终结果，下面给出一个命令示例和对应的解释：

```
python main.py --dname=cora --task_type=node_cls --method=HGNN --is_default=True --num_seeds=5
```

- **`--dname=cora`**
  - **作用**：指定**数据集**（Dataset Name）。这里是经典的引文网络数据集 `cora`。
- **`--task_type=node_cls`**
  - **作用**：指定**任务类型**（Task Type）。`node_cls` 代表 Node Classification（节点分类任务）；`edge_pred`代表Edge Prediction（超边预测）；`hg_cls` 代表 Hypergraph Classification （超图分类）。
- **`--method=HGNN`**
  - **作用**：指定**使用的模型**。这里是超图神经网络 `HGNN`，你只需参考论文命名替换成你想要的模型即可。
- **`--is_default=True`**
  - **作用**：**加载默认超参数配置**。开启后，程序会自动读取 `lib_yamls`目录下，对应任务类型（node_yamls目录）——模型配置文件（config_hgnn.yaml）——数据集（cora）的默认配置，配置中针对 `cora` 数据集预设好的参数（例如 `epochs: 100`, `lr: 1e-3` 等）。
- **`--num_seeds=5`**
  - **作用**：指定**实验运行次数**（随机种子数量）。设置为 `5` 意味着程序会跑 5 次（Seed 0 到 Seed 4），并在最后输出这 5 次结果的**平均值（Mean）**和**标准差（Std）**。

通过更改命令行的参数设置，可以让不同模型实现不同数据集下的不同类型任务。








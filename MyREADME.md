一.代码克隆和数据集下载

1.通过 **git** 克隆代码

```
git clone <本仓库地址> 
```

2.下载数据集

根据原仓库作者，所有基准数据集均可在 Hugging Face 数据集中心获取，以下为链接：

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

根据原仓库作者的依赖版本要求，本次复现用的是最低的符合要求的版本，如下所示：

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
















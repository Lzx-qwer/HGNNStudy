# DHG-Bench: A Comprehensive Benchmark for Deep Hypergraph Learning 论文部分结果复现

## <span id="citation">📖  原论文引用</span>

```bibtex
@inproceedings{li2026dhg,
  title={DHG-bench: A comprehensive benchmark for deep hypergraph learning},
  author={Li, Fan and Wang, Xiaoyang and Zhang, Wenjie and Zhang, Ying and Lin, Xuemin},
  booktitle={International Conference on Learning Representations},
  volume={2026},
  pages={47693--47727},
  year={2026}
}
```

## **<span id="citation">📖  原仓库地址</span>**

```
https://github.com/Coco-Hut/DHG-Bench
```



## 一.代码克隆和数据集下载

#### 1.通过 **git** 克隆代码

```
git clone https://github.com/Lzx-qwer/HGNNStudy.git 
```

#### 2.下载数据集

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



## 二.运行环境安装

#### 1.安装 uv

原仓库作者未说明使用什么包管理工具，所以本次复现使用 **uv** 进行包管理，**uv** 的下载不做说明，可以参考下面给出的链接，已安装 **uv** 请忽略：

https://github.com/astral-sh/uv

#### 2.虚拟环境创建与激活

进入 **HGNNStudy\dhgbench** 目录，打开 **cmd** 终端，输入下列语句创建并激活虚拟环境，注意这里一行是一条语句，一共两条：

```
uv venv --python 3.9.21 .venv
.venv\Scripts\activate
```

####  3.依赖安装

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



## 三.实验复现

#### 1.table 1 复现

##### 1.1激活环境

这里采用 **cmd** 终端运行，首先进入**HGNNStudy\dhgbench** 目录的 cmd 终端，输入以下命令激活 uv 虚拟环境：

```
.venv\Scripts\activate
```



##### 1.2 输入命令进行训练

复现采用5次取平均值的方式得到最终结果，下面给出一个命令示例和对应的解释：

```
python main.py --dname=cora --task_type=node_cls --method=HGNN --num_seeds=5
```

- **`--dname=cora`**
  - **作用**：指定**数据集**（Dataset Name）。这里是经典的引文网络数据集 `cora`，具体一共有哪些数据集可查看`HGNNStudy\dhgbench\lib_dataset\_init_.py`文件。
- **`--task_type=node_cls`**
  - **作用**：指定**任务类型**（Task Type）。`node_cls` 代表 Node Classification（节点分类任务）；`edge_pred`代表Edge Prediction（超边预测）；`hg_cls` 代表 Hypergraph Classification （超图分类）。
- **`--method=HGNN`**
  - **作用**：指定**使用的模型**。这里是超图神经网络 `HGNN`，具体一共有哪些模型可查看`HGNNStudy\dhgbench\lib_models\_init_.py`文件。
- **`--num_seeds=5`**
  - **作用**：指定**实验运行次数**（随机种子数量）。设置为 `5` 意味着程序会跑 5 次（Seed 0 到 Seed 4），并在最后输出这 5 次结果的**平均值（Mean）**和**标准差（Std）**。



##### **1.3 复现命令说明**

通过更改命令行的参数设置，可以让不同模型实现不同数据集下的不同类型任务。只需要更改**`--dname`**的参数来更换数据集，以及更改**`--method`**参数来更改模型即可。











***注意：***原仓库 **Readme** 文件中，作者给出的示例运行命令包含`--is_default=True`参数，其本意为通过 **Ture** 或 **False** 来控制是否使用作者提供的默认配置，但是源代码在解析时存在**Bug**未指定**is_default**的参数类型），导致后面的 **Ture** 或 **False** 会被默认识别为**str**字符串，在解析时又使用`if args.is_default`语句判断 **Ture** 或 **False** ，最终导致无论指定 **Ture** 或 **False**，字符串始终有值，始终会被判断为**True**，始终走作者默认配置，这样最终导致不少复现准确率与论文数据偏差过大，只有命令中完全删除`--is_default`参数，这样该参数默认为**False**，才可以不走默认配置。




## <span id="references"> 四.算法引用</span>

| **ID** | **Method** | **Paper** |     **Venue**     |  
| :----: | :----: | :----: |:----: |  
|1 | HGNN | [Hypergraph neural networks](https://cdn.aaai.org/ojs/4235/4235-13-7289-1-10-20190705.pdf) | AAAI 2019 | 
|2|HyperGCN|[HyperGCN: A New Method of Training Graph Convolutional Networks on Hypergraphs](https://proceedings.neurips.cc/paper/2019/file/1efa39bcaec6f3900149160693694536-Paper.pdf)|NeurIPS 2019| 
|3| HNHN | [HNHN: Hypergraph Networks with Hyperedge Neurons](https://grlplus.github.io/papers/40.pdf) | ICML WS 2020 | 
|4| HCHA | [Hypergraph convolution and hypergraph attention](https://www.sciencedirect.com/science/article/abs/pii/S0031320320304404?via=ihub) | PR 2020 | 
|5| UniGNN | [UniGNN: a Unified Framework for Graph and Hypergraph Neural Networks](https://www.ijcai.org/proceedings/2021/0353.pdf) | IJCAI 2021 | 
|6| AllSetformer | [You are AllSet: A Multiset Function Framework for Hypergraph Neural Networks](https://openreview.net/forum?id=hpBTIv2uy_E) | ICLR 2022 | 
|7|HyperND|[Nonlinear Feature Diffusion on Hypergraphs](https://proceedings.mlr.press/v162/prokopchik22a/prokopchik22a.pdf)|ICML 2022|
|8| EHNN|[Equivariant hypergraph neural networks](https://arxiv.org/pdf/2208.10428)|ECCV 2022|Y|Y|
|9| LEGCN|[Semi-supervised Hypergraph Node Classification on Hypergraph Line Expansion](https://arxiv.org/pdf/2005.04843)|CIKM 2022|
|10| ED-HNN | [Equivariant Hypergraph Diffusion Neural Operators](https://openreview.net/forum?id=RiTjKoscnNd) | ICLR 2023 | 
|11|PhenomNN|[From Hypergraph Energy Functions to Hypergraph Neural Networks](https://proceedings.mlr.press/v202/wang23d/wang23d.pdf)|ICML 2023|
|12|SheafHyperGNN|[Sheaf Hypergraph Networks](https://proceedings.neurips.cc/paper_files/paper/2023/file/27f243af2887d7f248f518d9b967a882-Paper-Conference.pdf)|NeurIPS 2023|
|13|HJRL|[Hypergraph Joint Representation Learning for Hypervertices and Hyperedges via Cross Expansion](https://openreview.net/forum?id=fxLaL5s6UH)|AAAI 2024|
|14| DPHGNN |[DPHGNN: A Dual Perspective Hypergraph Neural Networks](https://arxiv.org/pdf/2405.16616)|KDD 2024| 
|15|T-HyperGNNs |[T-HyperGNNs: Hypergraph Neural Networks via Tensor Representations](https://ieeexplore.ieee.org/document/10462516)| TNNLS 2024|
|16|HyperGT|[Hypergraph Transformer for Semi-Supervised Classification](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10446248)|ICASSP 2024|
|17|TF-HNN |[Training-Free Message Passing for Learning on Hypergraphs](https://openreview.net/pdf?id=4AuyYxt7A2)| ICLR 2025|






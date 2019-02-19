#介绍
facets项目包含2个可视化用来理解和分析机器学习的数据：facets overview和facets dive.
可视化，
在facets项目的描述网页可以找到可视化的现场演示。（https://pair-code.github.io/facets/）

#Facets Overview


Overview提供了一个关于一个或多个数据集的高级视角。提供了特征对特征的统计分析视角，同样可以用于对比统计不同的数据集。工具可以处理数字和字符特征，包括每一个特征。

Overview 能帮助数据集的问题，包括，
*意外的特征值
*缺少大量示例的特征值
*训练/服务偏差
*训练/测试/验证设置偏差


可视化的关键方面是跨多个数据集的异常值检测和分布比较。
红色突出显示有趣的值（例如，高比例的缺失数据或跨多个数据集的特征分布）。
特征可以按照感兴趣的值排序，例如缺少值的数量或不同数据集之间的偏差。

有关Overview用法的详细信息，请参见其[README]（./facets_overview/README.md）。

##Facets Dive

UCI人口普查数据的Dive可视化（/img/dive-census.png“UCI人口普查数据的dive可视化-Lichman，M。（2013）。UCI机器学习存储库[http://archive.ics.uci.edu/ml/datasets/Census + Income]。Irvine，CA：加利福尼亚大学信息与计算机科学学院“）

dive是一种交互式探索多达数万个数据点的工具，允许用户在高级概述和低级细节之间进行无缝切换。
每个示例在可视化中被表示为单个项目，并且可以通过其特征值在多个维度上通过刻面/屈服来定位点。通过结合平滑动画和缩放与细分和过滤，Dive可以轻松地在复杂数据集中发现模式和异常值。

有关dive使用的详细信息，请参见其[README]（./facets_dive/README.md）。

＃建立

##安装
```
git clone https://github.com/PAIR-code/facets
cd facets
```

##启用Jupyter notebook 的使用

jupyter扩展可视化代码的预构建版本可以在facets-dist目录中找到。

为了在Jupyter notebook中使用这些可视化功能：
1.安装jupyter notebook软件：http://jupyter.org/install.html
2.将可视化文件作为nbextension安装到Jupyter中：```jupyter nbextension install facets-dist / -user``（从facets顶级目录运行）。您不需要为此扩展运行任何跟进```jupyter nbextension enable```命令。
3.要启用概览可视化，您还必须安装协议缓冲区python运行时库：https：//github.com/google/protobuf/tree/master/python。如果您使用pip或anaconda来安装Jupyter，则可以使用相同的工具来安装运行时库。

注意：如可以在[Dive demo Jupyter notebook]（./facets_dive/Dive_demo.ipynb）中完成的大量数据可视化时，您需要以增加的IOPub数据速率启动 notebook服务器。
这可以用命令```jupyter notebook --NotebookApp.iopub_data_rate_limit = 10000000```完成。

建立可视化

如果您对可视化进行代码更改，并希望重建它们以供Jupyter笔记本使用，请遵循以下说明：
1.安装bazel：https://bazel.build/
2.构建可视化：```bazel build facets：facets_jupyter```（从facets顶级目录运行）
3.将生成的硫化html文件移动到facets-dist目录中。
4.重新安装facets-dist jupyter扩展名，如上一节所述。

**免责声明：这不是官方的Google产品**
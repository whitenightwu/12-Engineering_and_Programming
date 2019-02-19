以下说明中的示例代码也可以在[本例的Jupyter notebook]（./Overview_demo.ipynb）中找到。

＃特征统计协议缓冲区

Overview可视化由特征统计协议缓冲区提供支持。
特征统计协议缓冲区消息存储机器学习系统的一组输入数据的各个特征列的汇总统计信息（尽管它将被广泛地用于任何数据集合的统计信息）。

顶级proto是DatasetFeatureStatisticsList，它是DatasetFeatureStatistics的列表。
每个DatasetFeatureStatistics表示单个数据集的特征统计信息。
每个DatasetFeatureStatistics包含FeatureNameStatistics的列表，其中包含单个数据集中单个特征的统计信息。

特征统计信息根据特征数据类型（数字，字符串或原始字节）而有所不同。
对于数字特征，统计信息包括最小值，平均值，中值，最大值和标准偏差等指标。
对于字符串特征，统计信息包括平均长度，唯一值数和模式等指标。

特征统计信息包括用于加权统计信息的可选字段。
如果数据集具有示例权重特征，则除了标准统计信息之外，还可以使用它来计算每个要素的加权统计量。
如果proto包含加权字段，则可视化将显示加权统计信息，用户将能够在每个功能的图表的未加权版本和加权版本之间切换。

特征统计信息包括用于自定义统计信息的可选字段。
如果数据集中的特征有其他统计信息，团队希望跟踪和可视化，则可以将其添加到自定义统计信息字段中，这是自定义统计名称与自定义统计值（数字或字符串）的映射。
这些自定义统计信息将与标准统计信息一起显示。

＃特征统计生成

可以通过facets_overview/python目录中提供的python代码为数据集创建特征统计协议缓冲区。
数据集可以从张量流示例协议缓冲区的TfRecord文件或pandas数据帧中分析。

要从pandas DataFrame创建proto，请在文件facets_overview/python/ generic_feature_statistics_generator.py中使用ProtoFromDataFrames方法。
要从TfRecord文件创建proto，请在文件facets_overview/python/
feature_statistics_generator.py中使用ProtoFromTfRecordFiles方法（这需要为python依赖项安装tensorflow）。
请参阅这些文件以获取更多文档。

示例代码：
```python
import generic_feature_statistics_generator as gfsg
import pandas as pd
df = pd.DataFrame（{'num'：[1，2，3，4]，'str'：['a'，'a'，'b'，None]}
proto = gfsg.ProtoFromDataFrames（[{'name'：'test'，'table'：df}]）
```

＃可视化

proto可以使用安装的nbextension容易地在Jupyter notebook中可视化。
proto被加密，然后通过元素上的“protoInput”属性作为输入提供给facet-overview Polymer Web组件。
然后，Web组件显示在notebook的输出单元中。

示例代码（从上面的示例继续）：
```python
从IPython.core.display导入显示，HTML
protostr = base64.b64encode(proto.SerializeToString()).decode("utf-8")
HTML_TEMPLATE ="""<link rel ="import"href ="/nbextensions/facets-dist/facets-jupyter.html">
????????<facets-overview id ="elem"> </facets-overview>
????????<SCRIPT>
??????????document.querySelector("＃elem").protoInput ="{protostr}";
????????</SCRIPT> """
html = HTML_TEMPLATE.format(protostr = protostr)
display(HTML(HTML))
```

`protoInput`属性接受DatasetFeatureStatisticsList协议缓冲区的以下三种形式之一：
* DatasetFeatureStatisticsList JavaScript类的一个实例，它是由协议缓冲区编译器缓冲区创建的类。
*包含协议缓冲区的序列化二进制文件的UInt8Array。
*一个包含base-64编码序列化协议缓冲区的字符串，如上面的代码示例所示。

###了解可视化

可视化包含两个表：一个用于数字特征，一个用于分类（字符串）特征。
每个表包含该类型的每个特征的一行。
这些行包含计算的统计信息和图表，显示数据集中该特征的值的分布。

对于数据集中的大量示例，可能存在问题的统计信息（如功能缺失（无任何值））以红色和粗体显示。

###全局控制

可视化的顶部是影响各个表的控件。

排序次数下拉菜单更改每个表格中功能的排序顺序。选项是：
*特征顺序：在特征统计proto中按其自然顺序排序
*不均匀性：按照值的分布不均匀（使用熵）
*按字母顺序排列
*金额缺失/零：由特征值缺少或包含数字0而排序，最大数量为第一个。
*分配距离（仅在比较多个数据集时可用）：按照每个特征的分布形状之间的最大差异（使用卡方形测试）进行排序。

名称过滤器输入框允许通过与提供的文本匹配的功能名称过滤表。

功能复选框允许根据每个功能的值类型进行过滤，例如float，int或string。

###图表

显示表中功能的图表由图表上方的下拉菜单控制。
数字功能的选项有：
*所有值的直方图，具有10个等宽度的桶
*可视化所有值的十进制数
*可视化每个示例数值的十进制数
*对于作为pandas DataFrames的数据集，每个示例每个示例都有一个值，因此可视化是微不足道的，显示具有值1的所有十进制数。
??但是对于tf.Examples，对于给定的示例，功能可以具有任意数量的值。
??一个例子是一个地址功能，列出一个人的所有已知地址（每个示例代表一个人）。

字符串功能的选项有：
*数据集中每个值的计数的列表（如果某个要素具有20个或更少的唯一值，则使用该列）。
??存在切换显示计数表。
*每个特征值表示的整个数据集中所有值的百分比的累积分布函数图（如果特征具有超过20个唯一值，则使用该累积分布函数图）。
??存在切换显示计数表。
*可视化每个示例数值的十进制数。

此外，功能统计信息proto允许为任何功能存储自定义图表。
如果输入原型到可视化包含任何自定义图表，它们也将列在下拉列表中。

下拉列表旁边的复选框控制图表的其他一些功能：
*日志复选框将y轴更改为从线性记录。
*展开复选框显示更大版本的图表。
*加权复选框（仅当数据集包含加权统计信息以及正常统计信息时才显示）显示图表的加权版本以及统计信息的加权版本。
*百分比复选框（仅在可视化比较多个数据集时才显示）将y轴更改为每个完整数据集的百分比，而不是原始计数。
??这样可以轻松比较数据集之间的分布，这些数据集具有显着不同的示例总数（例如测试和列车数据集）。

＃运行演示（功能测试）

概述有多个演示，可用作功能测试，以确保新构建正常工作。
这些演示都在facets_overview/functional_tests下找到。
要运行一个例如“simple”测试，运行```bazel run facets_overview/functional_tests/simple:devserver```，然后浏览器导航到“localhost:6006/facets-overview/functional-tests/simple/index .html“来查看生成的可视化。
?
＃运行单元测试

运行```bazel run facets_overview/common/test:devserver```，然后浏览器导航到“localhost:6006/facets-overview/facets-overview/common/test/runner.html”。
测试的输出可以在开发者控制台中看到。
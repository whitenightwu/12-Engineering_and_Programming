#����
facets��Ŀ����2�����ӻ��������ͷ�������ѧϰ�����ݣ�facets overview��facets dive.
���ӻ���
��facets��Ŀ��������ҳ�����ҵ����ӻ����ֳ���ʾ����https://pair-code.github.io/facets/��

#Facets Overview


Overview�ṩ��һ������һ���������ݼ��ĸ߼��ӽǡ��ṩ��������������ͳ�Ʒ����ӽǣ�ͬ���������ڶԱ�ͳ�Ʋ�ͬ�����ݼ������߿��Դ������ֺ��ַ�����������ÿһ��������

Overview �ܰ������ݼ������⣬������
*���������ֵ
*ȱ�ٴ���ʾ��������ֵ
*ѵ��/����ƫ��
*ѵ��/����/��֤����ƫ��


���ӻ��Ĺؼ������ǿ������ݼ����쳣ֵ���ͷֲ��Ƚϡ�
��ɫͻ����ʾ��Ȥ��ֵ�����磬�߱�����ȱʧ���ݻ�������ݼ��������ֲ�����
�������԰��ո���Ȥ��ֵ��������ȱ��ֵ��������ͬ���ݼ�֮���ƫ�

�й�Overview�÷�����ϸ��Ϣ����μ���[README]��./facets_overview/README.md����

##Facets Dive

UCI�˿��ղ����ݵ�Dive���ӻ���/img/dive-census.png��UCI�˿��ղ����ݵ�dive���ӻ�-Lichman��M����2013����UCI����ѧϰ�洢��[http://archive.ics.uci.edu/ml/datasets/Census + Income]��Irvine��CA�����������Ǵ�ѧ��Ϣ��������ѧѧԺ����

dive��һ�ֽ���ʽ̽�������������ݵ�Ĺ��ߣ������û��ڸ߼������͵ͼ�ϸ��֮������޷��л���
ÿ��ʾ���ڿ��ӻ��б���ʾΪ������Ŀ�����ҿ���ͨ��������ֵ�ڶ��ά����ͨ������/��������λ�㡣ͨ�����ƽ��������������ϸ�ֺ͹��ˣ�Dive�������ɵ��ڸ������ݼ��з���ģʽ���쳣ֵ��

�й�diveʹ�õ���ϸ��Ϣ����μ���[README]��./facets_dive/README.md����

������

##��װ
```
git clone https://github.com/PAIR-code/facets
cd facets
```

##����Jupyter notebook ��ʹ��

jupyter��չ���ӻ������Ԥ�����汾������facets-distĿ¼���ҵ���

Ϊ����Jupyter notebook��ʹ����Щ���ӻ����ܣ�
1.��װjupyter notebook�����http://jupyter.org/install.html
2.�����ӻ��ļ���Ϊnbextension��װ��Jupyter�У�```jupyter nbextension install facets-dist / -user``����facets����Ŀ¼���У���������ҪΪ����չ�����κθ���```jupyter nbextension enable```���
3.Ҫ���ø������ӻ����������밲װЭ�黺����python����ʱ�⣺https��//github.com/google/protobuf/tree/master/python�������ʹ��pip��anaconda����װJupyter�������ʹ����ͬ�Ĺ�������װ����ʱ�⡣

ע�⣺�������[Dive demo Jupyter notebook]��./facets_dive/Dive_demo.ipynb������ɵĴ������ݿ��ӻ�ʱ������Ҫ�����ӵ�IOPub������������ notebook��������
�����������```jupyter notebook --NotebookApp.iopub_data_rate_limit = 10000000```��ɡ�

�������ӻ�

������Կ��ӻ����д�����ģ���ϣ���ؽ������Թ�Jupyter�ʼǱ�ʹ�ã�����ѭ����˵����
1.��װbazel��https://bazel.build/
2.�������ӻ���```bazel build facets��facets_jupyter```����facets����Ŀ¼���У�
3.�����ɵ���html�ļ��ƶ���facets-distĿ¼�С�
4.���°�װfacets-dist jupyter��չ��������һ��������

**�����������ⲻ�ǹٷ���Google��Ʒ**
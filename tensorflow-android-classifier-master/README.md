#介绍
Fruit Detection是一款利用Tensorflow深度学习的框架来实现移动端识别水果品种的APP。Fruit Detection利用现有的Google Inception-V3模型对图像进行训练，来完成一个可以识别水果品种，并对新训练的模型迁移到移动android端平台。
#实现步骤
1、准备训练数据样本：收集训练模型的图片数据，为模型迁移训练专门新建一个文件夹用于存放图片，打开训练样本文件夹fruits，共有三种类别，每个类别大约100张样本图片。

2、开始训练样本：
在存放训练数据的文件夹目录下打开cmd，输入如图所示命令，retained.py是google提供的迁移训练脚本。在运行 retrain.py 脚本时，需要配置一些运行命令参数，比如指定模型输入输出相关名称和其他训练要求的配置。


图4

训练过程中，当前目录生成bottlenecks文件夹，存放训练中产生的中间瓶颈文件。等待训练完成，我们将得到新生成的retrained_labels.txt标签文件以及retrained_graph.pd文件。

3、优化模型文件
通过调用 optimize_for_inference 脚本，会自动删除模型中输入层和输出层之间所有不需要的节点。同时该脚本还做了一些其他优化以提高运行速度。例如它把显式批处理标准化运算跟卷积权重进行了合并，从而降低了计算量。

图5

在当前文件夹下生成optimized_graph.pb文件。
调用 quantize_graph 脚本对模型进行再优化。

图6
在当前文件夹下生成rounded_graph.pb文件。

4、把新训练的模型导入到Android中
把新训练的rounded_graph.pb文件和retrained_labels.txt文件复制到android项目下的assets文件夹下，修改TensorFlowImageClassifier.create 方法传入的参数，如图：

图7

运行Android Studio，可以看到手机模拟器的运行结果
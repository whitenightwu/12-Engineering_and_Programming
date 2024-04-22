
protocol buffer

II)语法
1.proto语言与C语言差别不是很大，结构体struct字段换成message

2.Required: 表示是一个必须字段，必须相对于发送方，在发送消息之前必须设置该字段的值，对于接收方，必须能够识别该字段的意思。发送之前没有设置required字段或者无法识别required字段都会引发编解码异常，导致消息被丢弃。Google的一些工程师得出了一个结论：使用required弊多于利；他们更 愿意使用optional和repeated而不是required。

3.Optional：表示是一个可选字段，可选对于发送方，在发送消息时，可以有选择性的设置或者不设置该字段的值。对于接收方，如果能够识别可选字段就进行相应的处理，如果无法识别，则忽略该字段，消息中的其它字段正常处理。---因为optional字段的特性，很多接口在升级版本中都把后来添加的字段都统一的设置为optional字段，这样老的版本无需升级程序也可以正常的与新的软件进行通信，只不过新的字段无法识别而已，因为并不是每个节点都需要新的功能，因此可以做到按需升级和平滑过渡。
[default]可以提供默认值，对于基本数据类型，不设默认值将会同C语言一样产生类似默认值。
默认值：对string来说，默认值是空字符串。对bool来说，默认值是false。对数值类型来说，默认值是0。对枚举来说，默认值是枚举类型定义中的第一个值。

4.Repeated：表示该字段可以包含0~N个元素。其特性和optional一样，但是每一次可以包含多个值。可以看作是在传递一个数组的值。只有类型为原始数字类型的repeated field才能被声明为“packed”。例：repeated int32 d = 4[packed=true];
由于一些历史原因，基本数值类型的repeated的字段并没有被尽可能地高效编码。在新的代码中，用户应该使用特殊选项[packed=true]来保证更高效的编码

5.对于基本类型，protobuf也采用了第一种编码方式，后来发现这种编码方式效率太低，因而可以添加[packed = true]的描述将其转换成第三种编码方式（第二种方式的变种，对基本数据类型，比第二种方式更加有效）：

如果使用protobuf：先写入tag，后写入字段的总字节数，再写入每个项数据。即：
Tag	dataByteSize	Data	Data	…
目前protobuf只支持基本类型的packed修饰，因而如果将packed添加到非repeated字段或非基本类型的repeated字段，编译器在编译.proto文件时会报错。

可重复项带有[packed=true]后，所有元素打成一个包，使用类似字符串的数据打包形式

有一个varint的概念，详情见http://www.cnblogs.com/smark/archive/2012/05/03/2480034.html


6.关于enum：可以在一个消息定义的内部或外部定义枚举——这些枚举可以在.proto文件中的任何消息定义里重用。当然也可以在一个消息中声明一个枚举类型，而在另一个不同 的消息中使用它——采用MessageType.EnumType的语法格式。枚举的定义和C++相同，但是有一些限制。枚举值必须大于等于0的整数。

7.所有数据结构变量，都需要一个唯一的id，id从1开始。这与proto内部编码系统有关，1~20编码长度小，访问速度快。随着id值增加，后续变量访问速度会递减。


8.嵌套：嵌套的语法通常被错误地认为有子类化的关系支持嵌套消息，消息可以包含另一个消息作为其字段。也可以在消息内定义一个新的消息。
group“组”是指在消息定义中嵌套信息的另一种方法。
a.例
message SearchResponse {

  repeated group Result = 1 {

    required string url = 2;

    optional string title = 3;

    repeated string snippets = 4;

  }

}

b.一个“组”只是简单地将一个嵌套消息类型和一个字段捆绑到一个单独的声明中。


9.关于import
protobuf 接口文件可以像C语言的h文件一个，分离为多个，在需要的时候通过 import导入需要对文件。其行为和C语言的#include或者java的import的行为大致相同。

10.关于package
避免名称冲突，可以给每个文件指定一个package名称，对于java解析为java中的包。对于C++则解析为命名空间。

11.关于fixed32 和int32的区别。fixed32的打包效率比int32的效率高，但是使用的空间一般比int32多。因此一个属于时间效率高，一个属于空间效率高。根据项目的实际情况，一般选择fixed32，如果遇到对传输数据量要求比较苛刻的环境，可以选择int32.


12.在Protocol Buffers（protobuf）中，"none"不是一种特定的数据类型。
"none"通常用来表示某个字段没有值或者为空。在protobuf中，每个字段都有一个默认值，当一个字段没有被赋值时，它将使用默认值。对于不同的数据类型，protobuf的默认值是不同的：
对于布尔类型（bool），默认值是false。
对于整数类型（int32、int64、uint32、uint64），默认值是0。
对于浮点数类型（float、double），默认值是0.0。
对于字符串类型（string），默认值是空字符串。
对于枚举类型（enum），默认值是枚举定义中的第一个值。
对于嵌套消息类型（message），默认值是一个空的消息。
因此，当一个字段的值没有被设置时，它将被视为"none"，即使用默认值。可以通过检查字段的值是否等于默认值来确定该字段是否被赋值。


III）caffe.proto

1.在这个文件夹下还有一个.pb.cc和一个.pb.h文件,这两个文件都是由caffe.proto编译


2.message BlobProto中的是参数，data和diff之类
datum：数据的意思，data是它的复数；其中是数据，data和label之流；另外，encoded可以被忽略，我还没见过什么图像需要编码的。Caffe需要OpenCV，主要是由于考虑到图像需要解码，省略这一步，OpenCV可以无视掉


3.message SolverParameter
 If more than one test net field is specified (e.g., both net and test_net are specified), they will be evaluated in the field order given above: (1) test_net_param, (2) test_net, (3) net_param/net.  

4.phase/level/stage：
message NetStateRule {//描述的是一种规则，在层的定义里设置，用来决定Layer是否被加进网络
  // Set phase to require the NetState have a particular phase (TRAIN or TEST)
  // to meet this rule.
  optional Phase phase = 1;

  // Set the minimum and/or maximum levels in which the layer should be used.
  // Leave undefined to meet the rule regardless of level.
  optional int32 min_level = 2;
  optional int32 max_level = 3;

  // Customizable sets of stages to include or exclude.
  // The net must have ALL of the specified stages and NONE of the specified
  // "not_stage"s to meet the rule.
  // (Use multiple NetStateRules to specify conjunctions of stages.)
  repeated string stage = 4;
  repeated string not_stage = 5;
}
Phase是个枚举类型的变量，取值为{TRAIN, TEST}，这个好理解，表示的是网络的两个阶段（训练和测试）；Level是个整型变量，stage是个字符串变量。
使用NetStateRule的好处就是可以灵活的搭建网络，可以只写一个网络定义文件，用不同的NetState产生所需要的网络，比如常用的那个train和test的网络就可以写在一起。 加上level和stage，用法就更灵活。
例子：如下定义的网络经过初始化以后'innerprod'层就被踢出去了
state: { level: 2 } 
name: 'example' 
layer { 
  name: 'data' 
  type: 'Data' 
  top: 'data' 
  top: 'label' 
} 
layer { 
  name: 'innerprod' 
  type: 'InnerProduct' 
  bottom: 'data' 
  top: 'innerprod' 
  include: { min_level: 3 } 
} 
layer { 
  name: 'loss' 
  type: 'SoftmaxWithLoss' 
  bottom: 'innerprod' 
  bottom: 'label' 
}




IV)操作
protoc -I=src/caffe/proto/ --cpp_out=src/caffe/proto src/caffe/proto/caffe.proto

proto编译：
  protoc --proto_path=IMPORT_PATH --cpp_out=DST_DIR --java_out=DST_DIR --python_out=DST_DIR path/to/file.proto
      这里将给出上述命令的参数解释。
      1. protoc为Protocol Buffer提供的命令行编译工具。
      2. --proto_path等同于-I选项，主要用于指定待编译的.proto消息定义文件所在的目录，该选项可以被同时指定多个。
      3. --cpp_out选项表示生成C++代码，--java_out表示生成Java代码，--python_out则表示生成Python代码，其后的目录为生成后的代码所存放的目录。
      4. path/to/file.proto表示待编译的消息定义文件。
      注：对于C++而言，通过Protocol Buffer编译工具，可以将每个.proto文件生成出一对.h和.cc的C++代码文件。生成后的文件可以直接加载到应用程序所在的工程项目中。如：MyMessage.proto生成的文件为MyMessage.pb.h和MyMessage.pb.cc。



template <typename Dtype>
bool Net<Dtype>::StateMeetsRule(const NetState& state,
    const NetStateRule& rule, const string& layer_name) {
  // Check whether the rule is broken due to phase.
  if (rule.has_phase()) {
      if (rule.phase() != state.phase()) {
        LOG(INFO) << "The NetState phase (" << state.phase()
          << ") differed from the phase (" << rule.phase()
          << ") specified by a rule in layer " << layer_name;
        return false;
      }
  }
  // Check whether the rule is broken due to min level.
  if (rule.has_min_level()) {
    if (state.level() < rule.min_level()) {
      LOG(INFO) << "The NetState level (" << state.level()
          << ") is above the min_level (" << rule.min_level()
          << ") specified by a rule in layer " << layer_name;
      return false;
    }
  }
  // Check whether the rule is broken due to max level.  
  if (rule.has_max_level()) {  
    if (state.level() > rule.max_level()) {  
      LOG(INFO) << "The NetState level (" << state.level()  
          << ") is above the max_level (" << rule.max_level()  
          << ") specified by a rule in layer " << layer_name;  
      return false;  
    }  
  }  
  // Check whether the rule is broken due to stage. The NetState must  
  // contain ALL of the rule's stages to meet it.  
  for (int i = 0; i < rule.stage_size(); ++i) {  
    // Check that the NetState contains the rule's ith stage.  
    bool has_stage = false;  
    for (int j = 0; !has_stage && j < state.stage_size(); ++j) {  
      if (rule.stage(i) == state.stage(j)) { has_stage = true; }  
    }  
    if (!has_stage) {  
      LOG(INFO) << "The NetState did not contain stage '" << rule.stage(i)  
                << "' specified by a rule in layer " << layer_name;  
      return false;  
    }  
  }  
  // Check whether the rule is broken due to not_stage. The NetState must  
  // contain NONE of the rule's not_stages to meet it.  
  for (int i = 0; i < rule.not_stage_size(); ++i) {  
    // Check that the NetState contains the rule's ith not_stage.  
    bool has_stage = false;  
    for (int j = 0; !has_stage && j < state.stage_size(); ++j) {  
      if (rule.not_stage(i) == state.stage(j)) { has_stage = true; }  
    }  
    if (has_stage) {  
      LOG(INFO) << "The NetState contained a not_stage '" << rule.not_stage(i)  
                << "' specified by a rule in layer " << layer_name;  
      return false;  
    }  
  }  
  return true;  
}  






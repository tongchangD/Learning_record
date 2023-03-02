# Jeston_ros相关

[TOC]

# Jeston 相关

## jeston系统搭建

- [镜像下载地址](http://download.miivii.com:3000/s/NHFn4DjRx3DGSPz?path=%2F)

- 可用米文动力自带刷机包：miivii_ftool 

  :warning:  此处需要注意 安装版本 涉及到之后的深度学习包的安装版本

  :warning:  刷机完之后，可以使用`jetson_release`查看相应版本

## Jeston_ros环境搭建

### Jeston_ros1环境搭建：开发环境

ros1 环境搭建比价简洁，ROS1的最新版本noetic，编译环境为catkin，其安装步骤如下，详情可以参考[ros1官网](http://wiki.ros.org/noetic/Installation/Ubuntu):

1. 设置安装源；

   官方默认安装源:

   ```shell
   sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
   ```

   或来自国内清华的安装源

   ```shell
   sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
   ```

   或来自国内中科大的安装源

   ```shell
   sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
   ```

2. 设置key；

   ```shell
   sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
   ```

3. 安装；

   3.1 首先更新apt

   ```shell
   sudo apt update
   ```

   3.2 再安装所需类型的 ROS:ROS多个类型:**Desktop-Full**、**Desktop**、**ROS-Base**.这里介绍较为常用的Desktop-Full(官方推荐)安装: ROS， rqt， rviz， robot-generic libraries， 2D/3D simulators， navigation and 2D/3D perception

   ```shell
   sudo apt install ros-noetic-desktop-full
   ```

4. 配置环境变量。

   配置环境变量，方便在任意终端中使用ROS.

   ```shell
   echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   ```

5. ROS的卸载

   ```shell
   sudo apt remove ros-noetic-*
   ```

   注意: 在 ROS 版本 noetic 中无需构建软件包的依赖关系，没有`rosdep`的相关安装与配置。

6. 安装rosdep

   初始化rosdep.rosdep可以更方便的编译的源代码安装系统依赖项，并需要在ROS中运行一些核心组件.

   ```shell
   sudo apt install python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
   sudo apt install python3-rosdep
   sudo rosdep init
   rosdep update
   ```

### Jeston_ros2环境搭建：开发环境

​	ros2 环境搭建也比价简洁，ROS2的最新版本foxy，编译环境为colcon，其安装步骤如下，详情可以参考[ros2官网]():

1. 设置UTF-8编译环境

   ```shell
   locale  # check for UTF-8
   sudo apt update && sudo apt install locales
   sudo locale-gen en_US en_US.UTF-8
   sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
   export LANG=en_US.UTF-8
   locale  # verify settings
   ```

2. 添加ros2的apt源，并将apt库添加到源列表

   ```shell
   sudo apt update && sudo apt install curl gnupg2 lsb-release
   sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg
   ```

   ```shell
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
   ```

3. 安装开发工具和ros工具

   ```shell
   sudo apt update && sudo apt install -y \
     build-essential \
     cmake \
     git \
     libbullet-dev \
     python3-colcon-common-extensions \
     python3-flake8 \
     python3-pip \
     python3-pytest-cov \
     python3-rosdep \
     python3-setuptools \
     python3-vcstool \
     wget
   # install some pip packages needed for testing
   python3 -m pip install -U \
     argcomplete \
     flake8-blind-except \
     flake8-builtins \
     flake8-class-newline \
     flake8-comprehensions \
     flake8-deprecated \
     flake8-docstrings \
     flake8-import-order \
     flake8-quotes \
     pytest-repeat \
     pytest-rerunfailures \
     pytest
   # install Fast-RTPS dependencies
   sudo apt install --no-install-recommends -y \
     libasio-dev \
     libtinyxml2-dev
   # install Cyclone DDS dependencies
   sudo apt install --no-install-recommends -y \
     libcunit1-dev
   ```

4. 获取ros2代码 

   ```shell
   mkdir -p ~/ros2_foxy/src
   cd ~/ros2_foxy
   wget https://raw.githubusercontent.com/ros2/ros2/foxy/ros2.repos
   vcs import src < ros2.repos
   ```

   代码安装 这一步很多时候都走不通 不能翻墙 连不上github网站 

   可以直接选择 `apt install ros-<版本名>-desktop` 进行安装

5. 使用rosdep安装依赖项

   ```shell
   sudo rosdep init
   rosdep update
   rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-5.3.1 urdfdom_headers"
   ```

6. 附带其他命令添加命令自动补全功能，即在命令行中用 TAB 键可以自动补全 ROS 2 的命令。

   ```shell
   sudo apt install python3-argcomplete
   ```

   如果涉及到 ROS 节点与 ROS 2 节点通讯，还要安装 ros1_bridge

   ```shell
   sudo apt install ros-dashing-ros1-bridge
   ```

7. 添加环境变量

```shell
source /opt/ros/<版本名>/setup.bash # ros1的版本名eg:melodic、noetic等 ros2的版本名eg： dashing、foxy、rolling等
```

### Jeston ros 环境测试

```shell
# 查看环境配置
Linux：
printenv | grep -i ROS
Windows：
set | findstr -i ROS
```

### Jeston ros 通信设置

配置域ID，可以实现同局域网内 ros终端 的 相互通信

````shell
Linux：
export ROS_DOMAIN_ID=<your_domain_id>
echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bashrc
Windows：
set ROS_DOMAIN_ID=<your_domain_id>
setx ROS_DOMAIN_ID <your_domain_id>
````

要在 shell 会话之间保持此设置，可以将该命令添加到 shell 启动脚本中：

```
echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bashrc
```

详情见[关于ROS_DOMAIN_ID](https://zhuanlan.zhihu.com/p/378752082)

# ROS2 turtlesim

ros2 小乌龟搭建，证明测试ros2 环境搭建OK，见[ROS2 实现小乌龟跟随](https://www.jianshu.com/p/146ce3754d50)，不在此处详述.



# ros demo 搭建

## ros1 demo搭建

略

因据官方文档发现ros2在通信、协作、协议上都有所增强，故此处不涉及ros1的搭建demo讲解，详情搭建可参照[ros1官网](https://www.ros.org/blog/getting-started/)，[autolabor ros1](http://www.autolabor.com.cn/book/ROSTutorials/chapter1/13-rosji-cheng-kai-fa-huan-jing-da-jian.html)

## ros2 demo 搭建

### ros2 C++ demo

1. 创建C++ 代码包，命令如下:

   ```shell
   ros2 pkg create --build-type ament_cmake --node-name cpp_demo cpp_demo
   ```

2. 代码包中具体内容:

   2.1 内容文档目录结构如下:

   ```shell
   cpp_demo
   ├── CMakeLists.txt
   ├── include
   │   └── cpp_demo
   ├── package.xml
   └── src
       ├── publisher_member_function.cpp
       └── subscriber_member_function.cpp
   ```

   2.2 CMakeLists.txt 代码内容编写

   ```shell
   cmake_minimum_required(VERSION 3.5)
   project(cpp_demo) # 指定包名
   
   # Default to C99
   if(NOT CMAKE_C_STANDARD)
     set(CMAKE_C_STANDARD 99)
   endif()
   
   # Default to C++14
   if(NOT CMAKE_CXX_STANDARD)
     set(CMAKE_CXX_STANDARD 14)
   endif()
   
   if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
     add_compile_options(-Wall -Wextra -Wpedantic)
   endif()
   
   # find dependencies
   find_package(ament_cmake REQUIRED) # 使用 ament
   # uncomment the following section in order to fill in
   # further dependencies manually.
   # find_package(<dependency> REQUIRED)
   
   find_package(rclcpp REQUIRED) # 查找 rclcpp 包
   find_package(std_msgs REQUIRED) # 查找 std_msgs 包 常见消息包
   
   add_executable(talker src/publisher_member_function.cpp)
   ament_target_dependencies(talker rclcpp std_msgs)
   add_executable(listener src/subscriber_member_function.cpp)
   ament_target_dependencies(listener rclcpp std_msgs)
   
   # 指定编译 保存
   install(TARGETS
     talker
     listener
     DESTINATION
     lib/${PROJECT_NAME})
    
   if(BUILD_TESTING)
     find_package(ament_lint_auto REQUIRED)
     # the following line skips the linter which checks for copyrights
     # uncomment the line when a copyright and license is not present in all source files
     #set(ament_cmake_copyright_FOUND TRUE)
     # the following line skips cpplint (only works in a git repo)
     # uncomment the line when this package is not in a git repo
     #set(ament_cmake_cpplint_FOUND TRUE)
     ament_lint_auto_find_test_dependencies()
   endif()
   
   ament_package()
   ```

   2.2 package.xml 文件内容

   ```xml
   <?xml version="1.0"?>
   <?xml-model href="http://download.ros.org/schema/package_format3.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
   <package format="3">
     <name>cpp_pubsub</name>
     <version>0.0.0</version>
     <description>tongchangD Package description</description>
     <maintainer email="tongchangD@tongchangD.tongchangD">tongchangD</maintainer>
     <license>tongchangD License declaration</license>
   
     <buildtool_depend>ament_cmake</buildtool_depend>
   
     <depend>rclcpp</depend>  <!-- # ros2 cpp 包 -->
     <depend>std_msgs</depend>  <!--# message 包 -->
   
     <test_depend>ament_lint_auto</test_depend>
     <test_depend>ament_lint_common</test_depend>
   
     <export>
       <build_type>ament_cmake</build_type>
     </export>
   </package>
   ```

   2.3 发布者  publisher_member_function.cpp 文件内容 

   ```c++
   #include <chrono>
   #include <functional>
   #include <memory>
   #include <string>
   
   #include "rclcpp/rclcpp.hpp"
   #include "std_msgs/msg/string.hpp"
   
   using namespace std::chrono_literals;
   /*
   * std::chrono_literals 详解
   * 时间相关的 具体 尚未细究，need TODO
   * 其中定义的部分函数
   * constexpr std::chrono::hours operator ""h(unsigned long long h)
   * {
   *     return std::chrono::hours(h);
   * }
   * constexpr std::chrono::duration<long double， ratio<3600，1>> operator ""h(long double h)
   * {
   *     return std::chrono::duration<long double， std::ratio<3600，1>>(h);
   * }
   *
   */
   class MinimalPublisher : public rclcpp::Node{
     public:
       MinimalPublisher(): Node("minimal_publisher")， count_(0)
       {
         this->declare_parameter("param");
         publisher_ = this->create_publisher<std_msgs::msg::String>("chatter"， 10);
         timer_ = this->create_wall_timer(
         500ms， std::bind(&MinimalPublisher::timer_callback， this)); // ros2 中 的定时器 另一个相似函数: create_timer
       }
   
   
     private:
       void timer_callback()
       {
         // 获得param 参数， 参数类型还包括 as_string()、as_double_array()等
         parameter_srting_ =this->get_parameter("param").as_int();
         auto message = std_msgs::msg::String(); // 创建message
         message.data = "Hello， world! " + std::to_string(count_++);
         RCLCPP_INFO(this->get_logger()， "Publishing: '%s'"， message.data.c_str());
   	  std::cout<<"parameter_srting_: "<<parameter_srting_<<endl;
         publisher_->publish(message); // 发布 
       }
       int parameter_srting_;
       rclcpp::TimerBase::SharedPtr timer_;
       rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
       size_t count_;
     };
   
   
     int main(int argc， char * argv[])
     {
       rclcpp::init(argc， argv); // 初始化
       rclcpp::spin(std::make_shared<MinimalPublisher>());
       rclcpp::shutdown();
       return 0;
     }
   ```

   2.4  监听者 subscriber_member_function.cpp 文件内容:

   ```c++
   #include <memory>
   
   #include "rclcpp/rclcpp.hpp"
   #include "std_msgs/msg/string.hpp"
   
   using std::placeholders::_1;
   
   class MinimalSubscriber : public rclcpp::Node{
     public:
       MinimalSubscriber() : Node("minimal_subscriber")
       {
         subscription_ = this->create_subscription<std_msgs::msg::String>(
         "topic"， 10， std::bind(&MinimalSubscriber::topic_callback， this， _1));
       }
   
     private:
       void topic_callback(const std_msgs::msg::String::SharedPtr msg) const
       {
         RCLCPP_INFO(this->get_logger()， "I heard: '%s'"， msg->data.c_str());
       }
       rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;};
   
   int main(int argc， char * argv[])
   {
     rclcpp::init(argc， argv);
     rclcpp::spin(std::make_shared<MinimalSubscriber>());
     rclcpp::shutdown();
     return 0;
   }
   ```

   

### ros2 python demo

1. 创建python 代码包，命令如下:

```shell
colcon pkg create --build-type ament_python --node-name python_demo test 
```

2. 代码包内具体内容:

   2.1 内部文档目录结构如下 

```shell
python_demo
├── package.xml
├── python_demo
│   ├── __init__.py  # 空包的证明是python 包
│   ├── listener.py  # 自己新建的，用来写具体代码
│   └── talker.py # 自己新建的，用来写具体代码
├── resource
│   └── python_demo
├── setup.cfg
└── setup.py
```

​	2.2 talker(发布者) 代码如下:

```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String
# 导包
'''
发送者
'''
class Talker(Node):
    def __init__(self):
        super().__init__('talker')   # 继承 Node class 的初始化函数，生成 node 名为 talker
        self.pub = self.create_publisher(String， 'chatter'， 10) # 创建了一个发布者，发布的话题名是chatter，话题消息是String，保存消息的队列长度是10，
        timer_period = 0.5  
        self.timer = self.create_timer(timer_period， self.timer_callback)  # 设定计时器，到时间就调用 timer_callback 函数
        self.i = 1

    def timer_callback(self): 
        '''回调函数'''
        msg = String()
        msg.data = 'Hello World: %d' % self.i
        self.pub.publish(msg)    # 向 chatter 上发布数据
        self.get_logger().info('Publishing: "%s"' % msg.data)   # 显示 log 信息
        self.i += 1
        
def main(args=None):   # 这里的 args 是从命令行中接收的参数
    rclpy.init(args=args)     # 在初始化时不再设定 node 的名字
    talker = Talker()
    rclpy.spin(talker)
    
if __name__ == '__main__':
    main()
```

​	2.3 listener(订阅者) 代码如下: 

```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String
# 导包
class Listener(Node):
    def __init__(self):
        super().__init__('listener')
        self.subscription = self.create_subscription(String， 'chatter'， self.listener_callback， 10) # 创建订阅者，订阅String消息，订阅的话题名叫做“chatter”，保存消息的队列长度是10，当订阅到数据时，会进入回调函数listener_callback
    def listener_callback(self， msg): # 回调函数
        self.get_logger().info('I heard: "%s"' % msg.data)

def main(args=None):
    rclpy.init(args=args)
    listener = Listener()
    rclpy.spin(listener)

if __name__ == '__main__':
    main()
```

​	2.4 package.xml 文件内容如下:

```xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format3.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
  <name>python_demo</name>
  <version>0.0.1</version>
  <description>tongchangD Package description</description>
  <maintainer email="tongchangD@tongchangD.tongchangD">root</maintainer>
  <license>tongchangD License declaration</license>
  <export>
    <build_type>ament_python</build_type>
  </export>
</package>
```

​	2.5 setup.py文件内容如下:

```python
from setuptools import setup
package_name = 'python_demo' # 设定包名，注册名，与package文件夹的名字可以不同，但一般只要不涉及到重名，最好用与package相同的名字
setup(
  name=package_name，
  version='0.0.1'，
  packages=[package_name]，# 内部的 subpackage 的名字
  data_files=[
      ('share/ament_index/resource_index/packages'，
          ['resource/' + package_name])，
      ('share/' + package_name， ['package.xml'])，
  ]，
  install_requires=['setuptools']，     # 依赖的package
  zip_safe=True，
  entry_points={ # 设置了在命令行中如何使用 greeting_module  中的 main 函数
      'console_scripts': [
          'my_talker = python_demo.talker:main'， # 指明 my_talker 的 运行路径
          'my_listener = python_demo.listener:main' # 指明 my_listener 的 运行路径
      ]
  }
)
```

​	2.6 setup.cfg文件内容如下，使得node 能够用标准的`ros2 run <pkg> <node> `方式运行:

```python
[develop]
script-dir=$base/lib/python_demo
[install]
install-scripts=$base/lib/python_demo
# 讲解:
#   ros2 默认调用的install/<pkg>/<lib>中的文件.此文件设置将编译输出的目标文件夹设置为 install/python_demo/lib/python_demo.
```

## ros demo colcon

将上述代码任意一个拷贝进工作路径，eg:~/colcon_ws/src，此路径自己设置.何处都可创建，进入该目录之后进行编译，命令如下:

```shell
colcon build # 编译路径下所有包
# or
colcon build --packages-select python_demo/cpp_demo # 编译指定包
```


## ros demo run

### ros C++ demo run

```shell
##############   测试自己新建的 cpp demo       #########################
# 监听者
ros2 run cpp_demo listener
# 发布者
ros2 run cpp_demo talker  

##############   测试自带的example demo       #########################
# 监听者
ros2 run example_rclcpp_minimal_subscriber subscriber_menbe_function
# 发布者
ros2 run example_rclcpp_minimal_publisher publisher_menbe_function
```

### ros python demo run

```shell
# 监听者
ros2 run python_demo my_listener # `my_talker` 在setup.py中可以进行修改
# 发布者
ros2 run python_demo my_talker # `my_talker` 在setup.py中可以进行修改
```

因C++和python代码 收发 topic 名一致，可以互通 ，因`话题是一个多对多的订阅/发布模型`

# ros node

## ros2 node

​	ROS2中的各项资源也是通过计算图联系到一起的. 计算图是一个由各种ROS2元素组成的网络，共同完成数据的传输，其中每一个完成具体功能的模块称之为“节点”（Node），例如控制车轮速度、获取雷达数据等，节点之间通过话题（Topic）、服务（Service）、动作（Actions）或者参数（Parameter）实现数据的收发.  一个完整的机器人系统应该有很多节点构成，




运行一个程序之后才能知道程序所包含的节点名，(知道源代码，直接搜`rclpy.spin`或`rclcpp::spin`也是可以知道的).

# ros service

## ros2 service

机器人系统中另外一些配置性质的数据，并不需要周期处理，即ros的另一种通信方式`服务`。

服务是基于客户端、服务器模型的通信机制，服务器端只在接收到客户端请求时才会提供反馈数据。

```shell
ros2 service list # 查看服务列表
```

服务中完成通信的数据也是有数据结构的，只是有请求和应答两部分的数据组成，

```shell
ros2 service type <service_name> # 查看某个服务的数据结构
eg:ros2 service type /clear
看到的服务是 std_srv/srv/Empty,Empty 类型表示服务请求部分的数据是没有的,发送请求的时候不需要任何数据 
ros2 service list -t # 查看所有服务的数据类型
ros2 service list --show-types # 查看所有服务的数据类型
ros2 service find <type_name> # 查看所有提供同样数据类型的服务
eg : ros2 service find std_srvs/srv/Empty # 查找所有 提供 std_srvs/srv/Empty数据类型的服务
ros2 interface show <type_name>.srv # 查看数据类型具体的数据结构是啥样的
ros2 service call <service_name> <service_type> <arguments> # 通过终端发送服务请求 arguments 参数部分 可省略
```



# ros topic 

## ros2 topic

ROS2将复杂的机器人系统拆解成许多模块节点，而节点之间则是通过”话题“这个通道完成数据交换的。

一个节点可以通过多个话题向外发布数据，也可以同时订阅多个其他节点发布的话题，相当于话题是一个多对多的订阅/发布模型。

话题也是节点之间实现数据传输的重要途径，作为机器人各个子系统之间交换数据的重要方式。

```shell
ros2 topic list # 查看系统中所有的话题
ros2 topic list -t # 查看系统中所有的话题 及其 每个话题传输的数据类型
ros2 topic echo <topic_name> # 查看topic 发送的数据内容
ros2 topic info <topic_name> # 查看话题的详细信息
ros2 topic pub <topic_name> <msg_type> 'args' # 命令行发布命令 通过tab键可以填充
ros2 topic hz <topic_name>  # 查看某个话题的发布频率
```



# ros Action 

## ros2 Action 

Action作为ros 推出的应用级的通信机制,主要解决需要运行一段时间的机器人任务.

action由底层的三个话题和服务组成:

- 一个任务目标(Goal 服务)
- 一个执行结果(result 服务)
- 周期数据反馈(Feeback,话题).

action是可抢占式的,由于需要执行一段时间,在执行过程中不想跑了,就可以随时发送取消指令,动作终止.

如果执行过程中发送一个新的action目标,则会直接中断上一个目标开始执行最新的任务目标

总体上来讲，action是一个客户端/服务器的通信模型，客户端发送一个任务目标，服务器端根据收到的目标执行并周期反馈状态，执行完成后反馈一个执行结果。 


```shell
ros2 node info <package_name> # 查看节点信息
# 从反馈信息列表中,可以看到订阅者、发布者、服务、动作服务器和客户端
ros2 action list # 查看action 列表
ros2 action list -t # 查看action 列表及其详细的action参数类型
ros2 action info  <> # 查看action 信息
ros2 interface show turtlesim/action/RotateAbsolute.action # 查看 action 数据类型
# --- 切分，第一段是客户端发送的请求目标，第二段描述的是action执行完成后的反馈结果，第三段描述的是action执行过程中的周期反馈
ros2 action send_goal <action_name> <action_type> <values> # 命令行发送action目标 <values> 是yaml格式描述的数据
```

# ros Parameter

## ros2 Parameter

参数主要作用是对节点功能的配置，在ROS2中，每个节点都有自己的参数，这些参数可以用整型数、浮点数、布尔型数、字符串和列表来描述。

```shell
ros2 param list # 查看系统中的参数列表 
ros2 param get <node_name> <parameter_name> # 查看某个参数的值
ros2 param set <node_name> <parameter_name> <value> # 设置某个参数的值 只能修改一次
ros2 param dump <node_name> # 保存节点所有的参数到文件中
ros2 run <package_name> <executable_name> --ros-args --params-file <file_name># 加载参数文件
```



# ros 重映射

ROS中的重映射机制可以帮助我们重定义节点的属性，比如节点名称、话题名称、服务名称等。

比如我们想修改看到的节点名 turtlesim，如下操作，便可以到node list的变化

```shell
ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle
```



# ros rqt_console

作为管理日志信息的可视化工具 rqt_console.

```shell
ros2 run rqt_console rqt_console # 启动rqt_console
```

<img src="https://minio.likelihood-tech.com/hackmd/uploads/d87a031277ec3d6008-14-09-26-02.png" style="zoom:50%;" />

在打开的窗口中存在三个子窗口。最上边一个窗口会显示所有ROS2系统中的日志，是内容最多的，如果我们想做筛选，可以在中间的窗口中选择日志级别，此时中间窗口则会剔除无关级别的日志，如果我们想在日志中搜索某一字符串，那就可以在最下边的窗口中输入，确任后该窗口只显示包含此字符串的日志。中间和下边的窗口还可以添加更多筛选规则。

## 日志级别

ROS2中的日志分成五个级别，级别自高到低分别是：  

- Fatal：致命级，描述系统为了自我保护即将终止的消息
- Error：错误级，描述非致命但是会阻碍程序运行的消息
- Warn：警告级，描述不损坏功能运行但是预期之外的行为的消息
- Info：信息级，描述系统正常运行时事件和状态消息
- Debug：调试级，描述系统一步一步运行的详细消息

 ROS2中默认开启的日志级别是Info，会自动显示info级别以上的所有日志，包括Info、Warn、Error、Fatal。  运行节点时，我们也可以修改允许发布的日志级别，比如我们只想显示Warn级别以上的日志，可以这样：  

```shell
ros2 run turtlesim turtlesim_node --ros-args --log-level WARN
```

 这时启动的仿真器就只会向外发布Warn级别以上的日志了，Info和Debug级别的日志都是看不到的。  rqt_console工具可以帮助我们有效的查看机器人系统中的日志信息，提高调试、测试、开发的效率，快速找到你想要看到的日志。  

# ros launch

ROS设计了launch启动文件，可以通过一个类似脚本的文件，一起启动多个节点并允许在文件中对节点进行配置。

ros1 中 launch 的格式只能是xml 形式的.

ros2 中 launch 的格式可以是xml、yaml、python脚本，其中python脚本可以设置启动顺序

## ros1 launch详解

详解可见有道云[地址](https://note.youdao.com/s/V74s5KCX)

## ros2 launch详解

### 创建一个launch文件

1. 首先创建一个launch文件夹，作为后续放置launch文件的目录：

   ```shell
   mkdir launch
   ```

2. 创建launch文件：

```shell
# Linux：
touch launch/turtlesim_mimic_launch.py
# macOs：
touch launch/turtlesim_mimic_launch.py
# Winows：
type nul > launch/turtlesim_mimic_launch.py
```

3. 创建launch文件内容：

   ```python
   from launch import LaunchDescription
   from launch_ros.actions import Node
   
   def generate_launch_description():
       return LaunchDescription([
     	Node(
           package='cpp_demo', # package表示节点所在的功能包 
           # namespace='cpp_demo', # 可省略 可以作为topic前缀，以防名称冲突
           executable='talker', # executable表示要运行的可执行文件名
           node_name='talker', # name表示启动后的节点名
           parameters=[{"param": "10"}], # 传递参数 
           output="screen", #  输出到屏幕上
   	),
   	Node(
           package='python_demo', # 命名空间
           # namespace='python_demo', # 可省略 可以作为topic前缀，以防名称冲突
           executable='my_talker', # executable表示要运行的可执行文件名
           node_name='my_talker' # name表示启动后的节点名
   	),
       Node(
           package='cpp_demo', # 命名空间
           # namespace='cpp_demo', # 可省略 可以作为topic前缀，以防名称冲突
           executable='listener', # executable表示要运行的可执行文件名
           node_name='listener' # name表示启动后的节点名
   	),
       ######### 以下方式未实验过 传参问题
   	Node(
           package='turtlesim',
           executable='mimic',
           name='mimic',
           remappings=[
             ('/input/pose', '/turtlesim1/turtle1/pose'),
             ('/output/cmd_vel', '/turtlesim2/turtle1/cmd_vel'),
           ])
   # 最后一个节点启动的是turtlesim功能包中的mimic节点，其中主要是在配置mimc的remapping重映射参数。mimic的输入话题从/input/pose重映射为/turtlesim1/turtle1/pose，输出话题从/output/cmd_vel重映射为/turtlesim2/turtle1/cmd_vel。   我们基本可以猜到，mimic节点通过订阅turtle1的位置，转换成对turtle2的速度指令发布出去，最后应该可以达到让turtle2模仿turtle1完成同样的运动。
   ])
   ```

### 运行launch文件

```shell
ros2 launch <launch_name.py>
# or
ros2 launch <package_name> <launch_file_name>
```

### 开机自动运行launch.py文件

- 第一步：创建一个.sh文件（示例.sh文件位置：/root/start/start_ros2.sh）， 文件里面写入以下内容

  ```shell
  source /root/colcon_ws/install/setup.bash
  ros2 launch /root/colcon_ws/launch/start_launch.py
  ```

- 第二步：配置开机运行.sh 设置

  - 1、 搜索 startup Applications
  - 2、 添加新的命令，并在Command 行添加如下代码

  ```shell
   gnome-terminal -x bash /root/start/start_ros2.sh
  ```

- 第三步： 重启看结果

# ros rqt_graph

 主要用来观测系统。查看消息的发布，从哪发，之后转到哪。



# ros bag 录制、回放数据

 ros2 bag是一个命令行工具，可以实现对ROS2系统中话题数据的录制和回放，选定的数据会被打包放到一个数据库文件中，未来使用该工具即可按照时间轴回放所有话题数据。

## 安装

```shell
sudo apt-get install ros-<distro>-ros2bag ros-<distro>-rosbag2*
```

## 配置

- 创建一个新的文件夹，未来保存数据库文件

  ```shell
  mkdir bag_files
  cd bag_files
  ```

- 选择一个话题

  ```shell
  ros2 topic echo <topic_name> # 可查看话题消息内容
  ```

- ros2 bag 录制话题数据

  ```shell
  ros2 bag record <topic_name> # 录制话题数据的命令
  ```

- ros2 bag 录制多个话题数据

  ```shell
  ros2 bag record -o subset <topic_name> <topic_name> # 多个话题用空格 隔开
  # -o 参数用来设置数据库文件名
  ```

- ros2 bag 查看数据库文件

  ```shell
  ros2 bag info <bag_file_name> # 
  ```

- ros2 bag 回放数据

  ```shell
  ros2 bag play subset
  ```



# ros2 自定义话题集服务消息数据类型

这块相当于就是在ros2 环境包下新建一个pkg，然后新建msg文件夹，文件夹里面创建msgname*.msg。msg内容示例如下

```python
string class_id
float64 score
```

修改CMakelists.txt文件

```shell
set(msg_files
msg/msgname*.msg
)
rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
   DEPENDENCIES std_msgs geometry_msgs
 )
```

修改 package.xml文件

```xml
<build_depend>rosidl_default_generators</build_depend>
<exec_depend>rosidl_default_runtime</exec_depend>
<member_of_group>rosidl_interface_packages</member_of_group>
```

最后编译 

```shell
colcon build --packages-select <pkg_name>
```



具体可以查看以下demo：[msg_demo vision_msgs](https://hub.xn--gzu630h.xn--kpry57d/ros-perception/vision_msgs)







# ros2 相关其他知识点

## ros2 常见命令

### 创建新功能包

```shell
# python
ros2 pkg create --build-type ament_python --node-name my_node my_package_py

# CMake：
ros2 pkg create --build-type ament_cmake --node-name my_node my_package_cpp
```

### 查看所有的节点

```shell
ros2 node list # 可以显示当前系统中运行的所有节点名称
```

### 查看某个节点信息

```shell
ros2 node info <node_name> # 可以查看该节点下所有信息，包括订阅者、发布者、服务、动作等等
```

### 查看topic 话题信息

```
ros2 topic echo <topic_name>
```





## colcon 常见命令

### colcon 构建包

```shell
colcon build --symlink-install
```

### colcon 测试

```shell
colcon test
```

### colcon 编译包

```shell
# 编译src下所有包
colcon build 
# 单独编译一个包
colcon build --packages-select my_package
```

### 从一个包运行一个测试

```shell
colcon test --packages-select YOUR_PKG_NAME --ctest-args -R YOUR_TEST_IN_PKG
```





# 附录：ros 相关链接，学习资料

## ros1相关

[Autolabor-ROS1机器人入门课程《ROS理论与实践》零基础教程](http://www.autolabor.com.cn/book/ROSTutorials/index.html)

[ros官网](http://wiki.ros.org/noetic/Installation/Ubuntu)

## ros2相关

[在NVIDIA Jetson 上使用 ROS 2 构建机器人应用程序 - NVIDIA 技术博客](https://developer.nvidia.com/zh-cn/blog/implementing-robotics-applications-with-ros-2-and-ai-on-jetson-platform-2/)

[NVIDIA AI IOT](https://github.com/orgs/NVIDIA-AI-IOT/repositories)

[RichardoMrMu/yolov5-fire-smoke-detect: This is a c++ implement of yolov5 and fire/smoke detect.](https://github.com/RichardoMrMu/yolov5-fire-smoke-detect)

[Jetson 系列——基于yolov5和deepsort的多目标头部识别，跟踪，使用tensorrt和c++加速_RichardoMu的博客-CSDN博客](https://blog.csdn.net/weixin_42264234/article/details/120152117)

[[NVIDIA\]-4 入手 Jetson Xavier NX 用户免密码自动root登录_darnell888的博客-CSDN博客](https://blog.csdn.net/darnell888/article/details/106027462)

[文件 - 米文动力下载服务](http://download.miivii.com:3000/s/NHFn4DjRx3DGSPz?path=%2F)

[ROS 2 github ](https://github.com/ros2)

[PyTorch for Jetson - version 1.10 now available - Jetson & Embedded Systems / Jetson Nano - NVIDIA Developer Forums](https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-10-now-available/72048)

[ros2官方文档](https://docs.ros.org/en/foxy/Installation.html)

[Docker+VS Code，在Jetson上优雅开发和部署ROS2 - 知乎](https://zhuanlan.zhihu.com/p/440079544)

[ROS 2安装+ 基本命令总结](https://www.jianshu.com/p/29cca79d343c)

[ROS 2 基本命令总结 - 简书](https://www.jianshu.com/p/29cca79d343c)

[wangzheqie (wangzheqie) / Repositories](https://github.com/wangzheqie?tab=repositories)



# ros2 过程中出现的部分错误及其解决方案

## 错误: rosdep 无法init 

解决方案1:见[地址](https://zhuanlan.zhihu.com/p/405478145)

解决方案2:见[地址](https://blog.csdn.net/weixin_46398948/article/details/120047411)

解决方案3:见[地址](https://blog.csdn.net/Hacker_MAI/article/details/105827254)

方案2 亲测有用

## 错误：未找到 launch_testing_ament_cmake 包

报错内容：

```shell
By not providing "Findlaunch_testing_ament_cmake.cmake" in
CMAKE_MODULE_PATH this project has asked CMake to find a package
configuration file provided by "launch_testing_ament_cmake"， but CMake did
not find one.
```

解决方案：

```shell
sudo apt install ros-dashing-launch*
```



## 错误：pip 安装时 常断

常事 pip 源装的慢 尝试北京外国语大学源

直接换源 或者终端运行时在最后加源就行eg:

```shell
pip install -i https://mirrors.bfsu.edu.cn/pypi/web/simple some-package
```



## 错误:  提示 cuda 版本不对 或者没有 一些cuda 或cudnn的包

​	兄弟，参照`jetson_release`查看相应版本，再安装指定的深度学习框架吧

```shell
# 若如 未找到该命令
sudo -H pip install jeston-stats
# 再运行
jeston_release  # 即可查看jetpack版本
```



## 注意点: python 或 pip 版本

jeston 自带版本和默认环境应该为python2.7

可以使用以下方式设置默认使用python3.*

```shell
### 注意 执行此行代码时一定得保持头脑异常清醒
ln -r /usr/bin/python
ln -s /usr/bin/python3.6 /usr/bin/python
### 保险一点可以 先执行以下命令
cd  /usr/bin/
ls -l | grep python # 查看原本python包软链接的谁 ，万一不行可再换回来
```

jeston 自带版本和默认环境应该为python2.7 则相应 的pip 肯定为pip2

执行代码`sudo gedit /usr/bin/pip`

修改如下:

```python
from pip import __main__
if __name__ == '__main__':
    sys.exit(__main__._main())
```



## 注意 点：~/.bashrc 和 /etc/profile的区别

在设置自启动的时候遇到的问题，

~/.bashrc:该文件包含专用于某个用户的bash shell的bash信息,当该用户登录时以及每次打开新的shell时,该文件被读取.

/etc/profile中设定的变量(全局)的可以作用于任何用户,而~/.bashrc等中设定的变量(局部)只能继承/etc/profile中的变量,他们是"父子"关系

~/.bashrc: 作用类似于/etc/bashrc, 只是针对用户自己而言，不对其他用户生效。

:warning:  **切注意：往/etc/profile 文件中加东西得慎重**


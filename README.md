# ROS-Roadmap
This is a roadmap of the ROS for beginners.

<br>

## 0. Basic Docs

* [ROS org](http://wiki.ros.org/)
  * [ROS Tutorials](http://wiki.ros.org/ROS/Tutorials)
* [ROS docs 1](https://robertchoi.gitbook.io/ros/)


<br><br>

## 1. ROS(Robot Operating System)

The Robot Operating System (ROS) is a set of software libraries and tools that help you build robot applications. <br>
From drivers to state-of-the-art algorithms, and with powerful developer tools, ROS has what you need for your next robotics project. And it's all open source.


<br><br>

## 2. ROS file system structure

### General structure
Similar to an operating system, ROS files are also organized in a particualr fashion. <br>
The following graph shows how ROS files and folder are organized on the disk:

![image](https://user-images.githubusercontent.com/89831708/184058451-482ddcd4-355c-4e96-bc1f-e12077fa2efd.png)

* ROS **packages** : The most basic unit of the ROS software. They contain the ROS runtime process(**nodes**), libraries, configuration files, and so on, which are organized together as a single unit. Packages are the atomic build item and release item in the ROS software. (the minimum build unit and the deploy unit)

  * **Package manifest** : It contains information about the package, author, license, dependencies, compilation flags, and so on. The `package.xml` file inside the ROS packge is the manifest file of that package.
  
  * **Messages** : They are a type of information that is sent from one ROS process to the other. They are regular text files with `.msg` extension that define the fields of the message.
  
  * **service** : It is a kind of request/reply interaction between processes. The reply and request data types can be defined inside the `src` folder inside the packages.

* **The workspace** <br>
In general terms, the workspace is a folder which contains packages, those packages contain our source files and the environment or workspace provides us with a wqy to compile those packages. It is useful when we want to compile various packages at the same time and it is a good way of centralizing all of our developments.



<br><br>

## 3. ROS Master, nodes and topics

One of the primary purpose of ROS is to facilitate communication between the ROS modules called nodes. Those nodes can be executed on a single machine or across several machines, obtaining a distributed system. The advantage of this structure is that each node can control one aspect of a system. For example you might have several nodes each be reposible of parsing row data from sensors and one node to process them.

<br>

* **ROS Master** <br>

Communication between nodes is established by the `ROS Master`. <br>
The `ROS Master` provides naming and registration services to the nodes in the ROS system. <br>
It is its job to track **publishers** and **subscribers** to the **topics**. <br><br>

> `ROS master` works much like a DNS server. Whenever any node starts in the ROS system, it will start looking for ROS master and register the name of the node with ROS master. Therefore, **ROS master has information about all the nodes that are currently running on the ROS system**. When information about any node changes, it will generate a call back and update with the latest information.

![image](https://user-images.githubusercontent.com/89831708/185063597-fd30e78c-e882-4751-b966-ff9b7403dcf5.png)

<br>

> ROS Master distributes the information about the topics to the nodes. Before a node can publish to a topic, it **sends the details of the topic(its name, data type..)**, to ROS master. `ROS master` will check whether **any otehr nodes are subscribed to the same topic**. If any nodes are subscribed to the same topic, ROS master will **share the nodes details of the publisher to the subscriber node**.

<br>
After receiving the node details, these **two nodes will interconnect** using the `TCPROS protocol`, which is based on `TCP/IP sockets`, and `ROS master` will relinquish its role in controlling them.

<br><br>

To start ROS master, open a terminal and run

```
roscore
```

Any ROS system must have **`only one master`**, even in a distributed system, and it should run on a computer that is reachable by all other computers to ensure that remote ROS nodes can access the master.**

<br><br>

* **ROS nodes**

Basically, `nodes` are **regular processes** but **with the capability to register** with the `ROS Master node` and **communicate with other nodes** in the system.
The ROS design idea is that each node is an **independent module** that interacts with other nodes using the `ROS communication capability`. <br>

The nodes can be created in various ways. From a terminal window a node can be created directly by typing a command after the command prompt, as shown in the examples to follow. Alternatively, **nodes can be created as part of a program written in Python or C++**. 
<br>
As example let's run the `tutlesim` node, in a new terminal run

```
$ rosrun turtlesim turtlesim_node
```

<br><br>


* **ROS topics** <br>

`Topics` are the means used by nodes **to transmit data**, it represents **the channel where message are sent and it has a message type attached to it** (you cannot send different types of message in a topic). <br>
In ROS, **data production** and **consumption** are **decoupled**, this means that a `node` can 

**publish message (`producer`)** or **subscribe to a topic (`consumer`)**.
<br>

Let's use `rqt_growth` which shows **the nodes and topics currently running**.

```
rosrun rqt_graph rqt_graph
```



<br><br>

## 4. Anatomy of a ROS node

The simplest C++ ROS node has a structure similar to the following

``` C++
#include "ros/ros.h"

int main(int argc, char **argv) {
 ros::init(argc, argv, "example_node");
 ros::NodeHandle n;
 
 ros::Rate loop_rate(50);
 
 while (ros::ok()) {
  // ... do some useful things ...
  ros::spinOnce();
  loop_rate.sleep();
 }
 return 0;
}
```

<br>

Let's analyze it line by line: the first line

```C++
#include "ros/ros.h"
```

adds the `header` containing all the basic ROS functionality. At the beginning of the main of the program.
<br>


```C++
ros::init(argc, argv, "example_node");
```

`ros::init` **initialize the node**, it is responsible for **collecting ROS specific informaion** from arguments passed at the command line and **set the node name** (remember: names must be unique across the ROS system). But it **does not contact the master**. <br>
To contact the master and register the node we need to call
<br>

```C++
ros::NodeHandle n;
```
<br>

When the first `ros::NodeHandle` is created it will call `ros::start()`, and when the last ros::NodeHandle is destroyed (e.g goes out of scope), it will call `ros::shutdown()`. This is the modst common way of handling the lifetime of a ROS node.<br>

Usually we want to run our node at a given frequency, to set the node frequency we use
```C++
ros::Rate loop_rate(50);
```
<br>
which is setting the desired rate at 50 Hz. Then we have the **main loop** of the node. Since we want to run this node until the ROS we need to the check the various states of shutdown.<br>
The most common way to do it is to call `ros::ok()`. Once `ros::ok()` returns false, the node has finished shutting down. That's why we have <br>
```C++
while (ros::ok()) {
 // ...
 }
```
<br>

Inside the loop we can make interseting things happen. In our example we simply run <br>
```C++
ros::spinOnce();
loop_rate.sleep();
```
<br>
The function `ros::spinOnce()` will call all the callbacks waiting to be called at that point in time while. If you remember we set the node frequency to 50Hz, the code we are running will probably take less than 20ms. The function `loop_rate.sleep()` will pause the node the remaining time.
<br>






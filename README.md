# ROS-Roadmap
This is a roadmap of the ROS for beginners.

<br>

## 0. Basic Docs

* [ROS org](http://wiki.ros.org/)
  * [ROS Tutorials](http://wiki.ros.org/ROS/Tutorials)
* [ROS docs 1](https://robertchoi.gitbook.io/ros/)


<br>

## 1. ROS(Robot Operating System)

The Robot Operating System (ROS) is a set of software libraries and tools that help you build robot applications. <br>
From drivers to state-of-the-art algorithms, and with powerful developer tools, ROS has what you need for your next robotics project. And it's all open source.


<br>

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



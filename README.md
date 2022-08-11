# ROS-Roadmap
This is a roadmap of the ROS for beginners.

<br>

## 1. Basic Docs

* [ROS org](http://wiki.ros.org/)
  * [ROS Tutorials](http://wiki.ros.org/ROS/Tutorials)
* [ROS docs 1](https://robertchoi.gitbook.io/ros/)
<br>

## 2. ROS file system structure

### General structure
Similar to an operating system, ROS files are also organized in a particualr fashion. <br>
The following graph shows how ROS files and folder are organized on the disk:

![image](https://user-images.githubusercontent.com/89831708/184058451-482ddcd4-355c-4e96-bc1f-e12077fa2efd.png)

* ROS **packages** : The most basic unit of the ROS software. They contain the ROS runtime process(**nodes**), libraries, configuration files, and so on, which are organized together as a single unit. Packages are the atomic build item and release item in the ROS software. (the minimum build unit and the deploy unit)

  * **Package manifest** : It contains information about the package, author, license, dependencies, compilation flags, and so on. The `package.xml` file inside the ROS packge is the manifest file of that package.
  
  * **Messages** : They are a type of information that is sent from one ROS process to the other. They are regular text files with `.msg` extension that define the fields of the message.
  
  * **service** : It is a kind of request/reply interaction between processes. The reply and request data types can be defined inside the `srv` folder inside the packages.

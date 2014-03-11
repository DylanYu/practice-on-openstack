Massive Deployment of OpenStack
===============================

Table of Contents
-----------------

1. RDO
2. Fuel
2. Puppet
2. SaltStack
3. Chef
4. Discussion

RDO
---

RDO是由RedHat公司推出的部署OpenStack集群的一个基于**Puppet**的部署工具，可以很快地通过RDO部署一套复杂的OpenStack环境。

对于RDO的使用，陈沙克有过两篇博客：
[CentOS 6.4 Openstack Havana 多节点安装（OVS+VLAN）](http://www.chenshake.com/centos-6-4-openstack-havana-multinode-installation/), 
[CentOS 6.4 Openstack Havana 多节点安装（OVS+GRE）](http://www.chenshake.com/how-node-installation-centos-6-4-openstack-havana-ovsgre/)

另有一篇CSDN上的： 
[RDO部署多节点OpenStack Havana(OVS+GRE)](http://blog.csdn.net/tiger435/article/details/16844155)

以及官方文档： 
[GettingStartedHavana w GRE](http://openstack.redhat.com/GettingStartedHavana_w_GRE)

从这些文章的介绍看，使用RDO部署OpenStack集群是快捷简便的事，我们只需提前设置好机器的网卡，修改几行配置文件，经过10-15分钟就可以自动配置完成一个OpenStack多节点集群，增加Compute节点也是[很简单的事](http://openstack.redhat.com/Adding_a_compute_node)，增加新机器的IP至原配置文件，重新运行之前的配置过程就可以进行快速部署。

尽管RDO仍然有一些bug，但都有解决办法。综合来看RDO是个不错的部署方案。

Fuel
----

[Mirantis](http://www.mirantis.com/)开发的OpenStack发行版提供了全图形化配置界面Fuel，采用了比RDO更方便的配置方式，即直接提供定制的ISO镜像供安装，安装后包括了宿主系统和OpenStack组建，有图形界面方便地配置所需服务，包括是否HA、网络类型(nova-network or neutron)、服务的宿主服务（glance是否基于swift）等。当有新的节点出现，master会主动感知并进行添加。

Fuel包括了裸机部署（采用HP的Cobbler）和配置管理（采用Puppet）。使用步骤如下：

1. 选定种子节点（Fuel Master Node)用于配置Cobbler Server和Puppet Master。
2. 通过Cobbler Server就可以安装裸机。
3. 通过Puppet Master安装OpenStack组件。Puppet组件是通过Cobbler送到各节点的。

相比较于RDO，Fuel不仅仅是在配置过程中封装了Puppet操作以简化步骤，Fuel给出了一整套解决方案，包括初始集群的各项设置和后续增加节点，都有方便直观的操作界面。


Puppet
------

Puppet是一个开源的软件自动化配置和部署工具，它使用简单且功能强大，很多大型IT公司均在使用puppet对集群中的软件进行管理和部署。

![Puppet架构图](http://dongxicheng.org/wp-content/uploads/2011/05/infrastructure.jpg)

puppet是基于c/s架构的。服务器端保存着所有对客户端服务器的配置代码，在puppet里面叫做manifest. 客户端下载manifest之后，可以根据manifest对服务器进行配置，例如软件包管理，用户管理和文件管理等等。

如上图所示，puppet的工作流程如下：（1）客户端puppetd调用facter，facter探测出主机的一些变量，例如主机名，内存大小，ip地址等。pupppetd 把这些信息通过ssl连接发送到服务器端； （2）服务器端的puppetmaster 检测客户端的主机名，然后找到manifest里面对应的node配置， 并对该部分内容进行解析，facter送过来的信息可以作为变量处理，node牵涉到的代码才解析，其他没牵涉的代码不解析。解析分为几个阶段，语法检查，如果语法错误就报错。如果语法没错，就继续解析，解析的结果生成一个中间的“伪代码”，然后把伪代码发给客户端；（3）客户端接收到“伪代码”，并且执行，客户端把执行结果发送给服务器；（4）服务器端把客户端的执行结果写入日志。

StackForge维护了专门的[puppet module for OpenStack](https://wiki.openstack.org/wiki/Puppet-openstack).

SaltStack
---------

[Using salt to install OpenStack on Ubuntu 12.04.2](https://github.com/EntropyWorks/salt-openstack)

Chef
----

[Chef for OpenStack Overview](https://www.openstack.org/summit/portland-2013/session-videos/presentation/chef-for-openstack-overview/)

Discussion
----------

大规模部署离不开高效的集群部署工具（e.g., Puppet, SaltStack, Chef），在集群规模不大时，使用这些工具需要不亚于手动配置的学习成本。当集群规模增加时，这些工具对效率的提升开始体现出来。

### Which one ?

描述了这么多，需要对各种配置方式进行对比以选出最佳方案。

RDO是RedHat对使用Puppet部署OpenStack的一个高度封装，用户不需要学习Puppet的复杂操作，可以直接进行傻瓜式操作。从实际效果而言，选择RDO是目前配置通用OpenStack集群的最佳选择，对于我们而言，需要改变的只是物理集群的操作系统需要改成RedHat。

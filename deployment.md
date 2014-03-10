Massive Deployment of OpenStack
===============================

Table of Contents
-----------------

1. RDO
2. SaltStack
3. Chef
4. Discussion

RDO
---

RDO是由RedHat公司推出的部署OpenStack集群的一个基于**Puppet**的部署工具，可以很快地通过RDO部署一套复杂的OpenStack环境。

对于RDO的使用，陈沙克有过几篇博客：
[CentOS 6.4 Openstack Havana 多节点安装（OVS+VLAN）](http://www.chenshake.com/centos-6-4-openstack-havana-multinode-installation/)
[CentOS 6.4 Openstack Havana 多节点安装（OVS+GRE）](http://www.chenshake.com/how-node-installation-centos-6-4-openstack-havana-ovsgre/)
另有一篇CSDN上的：
[RDO部署多节点OpenStack Havana(OVS+GRE)](http://blog.csdn.net/tiger435/article/details/16844155)
以及官方文档：
[GettingStartedHavana w GRE](http://openstack.redhat.com/GettingStartedHavana_w_GRE)

从这些文章的介绍看，使用RDO部署OpenStack集群是快捷简便的事，我们只需提前设置好机器的网卡，修改几行配置文件，经过10-15分钟就可以自动配置完成一个OpenStack多节点集群，增加Compute节点也是[很简单的事](http://openstack.redhat.com/Adding_a_compute_node)，增加新机器的IP至原配置文件，重新运行之前的配置过程就可以进行快速部署。

尽管RDO仍然有一些bug，但都有解决办法。综合来看RDO是个不错的部署方案。

SaltStack
---------

[Using salt to install OpenStack on Ubuntu 12.04.2](https://github.com/EntropyWorks/salt-openstack)

Chef
----

[Chef for OpenStack Overview](https://www.openstack.org/summit/portland-2013/session-videos/presentation/chef-for-openstack-overview/)

Discussion
----------

大规模部署离不开高效的集群部署工具（e.g., Puppet, SaltStack, Chef），在集群规模不大时，使用这些工具需要不亚于手动配置的学习成本。当集群规模增加时，这些工具对效率的提升开始体现出来。

RDO其实是RedHat对使用Puppet部署OpenStack的一个高度封装，用户不需要学习Puppet的复杂操作，可以直接进行傻瓜式操作。

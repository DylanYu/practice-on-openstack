===============================
Good Practice in OpenStack Code
===============================

Imports
========

* 多个模块分开导入::

   Yes: import os
        import sys
   No:  import sys, os

* Wildcard imports (from <module> import \*) should be avoided.

* Import in Human Alphabetical Order.

* 有时将导入后的模块进行重命名以增强可读性::

    import xmlrpc.client as xmlrpclib

* 或保持一致性::

   try:
       import UserString as _userString
   except ImportError:
       import collections as _userString

Maximum Line Length
====================

* Limit all lines to a maximum of 79 characters.

  使用pep8进行代码检测时会显示相关警告.

Use Spaces rather than Tab
===========================

首先这是因为在不同平台上tab所显示的视觉长度不一;

再者，Python3已经禁止将tab和space混用进行缩进.

使用getitem/setitem
===================

* Example::

   def __getitem__(self, key):
       return self.data[key]

   def __setitem__(self, key, val):
      self.data[key] = val

   obj = ClassA(...)
   v = obj['attr_1']
   obj['attr_2'] = 3

Docstring
==========

* 单行::

   """A one line docstring looks like this and ends in a period."""

* 多行，第一行作为summary::

   """A multi line docstring has a one-line summary, less than 80 characters.

   Then a new paragraph after a newline that explains in more detail any
   general information about the function, class or method. Example usages
   are also great to have here if it is a complex class for function.

   When writing the docstring for a class, an extra line should be placed
   after the closing quotations. For more in-depth explanations for these
   decisions see http://www.python.org/dev/peps/pep-0257/

   If you are going to describe parameters and return values, use Sphinx, the
   appropriate syntax is as follows.

   :param foo: the foo parameter
   :param bar: the bar parameter
   :returns: return_type -- description of the return value
   :returns: description of the return value
   :raises: AttributeError, KeyError
   """

Python 3.x compatible
=====================

* All Python 2.x-only constructs or dependencies should be avoided

* Instead of::

   except x,y:

  Use::

   except x as y:

* Deal with print::

   from __future__ import print_function


Naming
===================

* 包和模块

  Modules should have short, all-lowercase names. Underscores can be used in the module name if it improves readability. Python packages should also have short, all-lowercase names, although the use of underscores is discouraged.

* 类

  ClassName

* 异常

  Because exceptions should be classes, the class naming convention applies here. However, you should use the suffix "Error" on your exception names (if the exception actually is an error).

* 方法

  method_in_some_class

* 常量

  MAX_OVERFLOW

LOG
=====

* Example::

   _logger = logging.getLogger(__name__)

   _logger.addHandler(hdlr)
   _logger可以使用handler来帮它处理日志， handler主要有两种：
   StreamHandler: 输出到控制台
   FileHandler:   输出到文件

   try:
       ...
   except SomeError:
       _logger.warning("This is a warning.")

setuptools
==========

* 该工具可以自动解决模块的依赖问题，需要在requirements里描述所有的依赖项，如::

   pbr>=0.5.21,<1.0
   iso8601>=0.1.8
   requests>=1.1
   six>=1.4.1
   oslo.config>=1.2.0
   netaddr>=0.7.6
   Babel>=1.3

  在setup时会依次检查依赖项并安装

Unit Test
===========

* Example::

   Class SomeClassTestCase(testtools.TestCase):
       def test_method_a(self):
           self.assertEqual(0, self.a)

       def test_method_b(self):
           self.assertEqual(1, self.b)

* 运行单元测试

  To run the tests in the nova/tests/virt/libvirt/test_libvirt.py file::

   ./run_tests.sh test_libvirt

  To run the tests in the CacheConcurrencyTestCase class in nova/tests/virt/libvirt/test_libvirt.py::

   ./run_tests.sh test_libvirt.CacheConcurrencyTestCase

  To run the ValidateIntegerTestCase.test_invalid_inputs test method in nova/tests/test_utils.py::

   ./run_tests.sh test_utils.ValidateIntegerTestCase.test_invalid_inputs

* Flags::

   -V, --virtual-env           Always use virtualenv.  Install automatically if not present
   -N, --no-virtual-env        Don't use virtualenv.  Run tests in local environment
   -s, --no-site-packages      Isolate the virtualenv from the global Python environment
   -u, --update                Update the virtual environment with any newer package versions
   -p, --pep8                  Just run PEP8 and HACKING compliance check
   -P, --no-pep8               Don't run static code checks
   -c, --coverage              Generate coverage report


Commit Messages
================

First, provide a brief summary of 50 characters or less. Summaries of greater then 72 characters will be rejected by the gate.

Any patch to the master branch must specify in the commit message whether the patch should be backported. For example::

 backport: havana

The 'bug' line can reference a bug in a few ways. Gerrit creates a link to the bug when viewing the patch on review.openstack.org so that reviewers can quickly access the bug on Launchpad::

 Closes-Bug: #1234567 -- use 'Closes-Bug' if the commit is intended to fully fix and close the bug being referenced.
 Partial-Bug: #1234567 -- use 'Partial-Bug' if the commit is only a partial fix and more work is needed.
 Related-Bug: #1234567 -- use 'Related-Bug' if the commit is merely related to the referenced bug.

Once you use ‘git review’, two lines will be appended to the commit message: a blank line followed by a ‘Change-Id’. This is important to correlate this commit with a specific review in Gerrit, and it should not be modified.


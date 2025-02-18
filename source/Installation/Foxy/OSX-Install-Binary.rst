Installing ROS 2 on OS X
========================

.. contents:: Table of Contents
   :depth: 2
   :local:

This page explains how to install ROS 2 on OS X from a pre-built binary package.

System requirements
-------------------

We support OS X Sierra (10.12.x).

.. _Foxy_osx-install-binary-installling-prerequisites:

Installing prerequisites
------------------------

You need the following things installed before installing ROS 2.


*
  **brew** *(needed to install more stuff; you probably already have this)*:


  * Follow installation instructions at http://brew.sh/
  *
    *Optional*: Check that ``brew`` is happy with your system configuration by running:

    .. code-block:: bash

        brew doctor


      Fix any problems that it identifies.

*
  Use ``brew`` to install more stuff:

  .. code-block:: bash

       brew install python3

       # install asio and tinyxml2 for Fast-RTPS
       brew install asio tinyxml2

       # install dependencies for robot state publisher
       brew install tinyxml eigen pcre poco

       # OpenCV isn't a dependency of ROS 2, but it is used by some demos.
       brew install opencv

       # install OpenSSL for DDS-Security
       brew install openssl
       # if you are using ZSH, then replace '.bashrc' with '.zshrc'
       echo "export OPENSSL_ROOT_DIR=$(brew --prefix openssl)" >> ~/.bashrc

       # install Qt for RViz
       brew install qt freetype assimp

       # install console_bridge for rosbag2
       brew install console_bridge

       # install dependencies for rcl_logging_log4cxx
       brew install log4cxx

       # install CUnit for CycloneDDS
       brew install cunit

*
  Install rqt dependencies

  ``brew install sip pyqt5``

  Fix some path names when looking for sip stuff during install (see `ROS 1 wiki <http://wiki.ros.org/kinetic/Installation/OSX/Homebrew/Source#Qt_naming_issue>`__):

  ``ln -s /usr/local/share/sip/Qt5 /usr/local/share/sip/PyQt5``

  ``brew install graphviz``

  ``python3 -m pip install pygraphviz pydot``

  .. note::

      You may run into an issue installing ``pygraphviz``, "error: Error locating graphviz".
      Try the following install command instead:

      .. code-block:: bash

         python3 -m pip install --install-option="--include-path=/usr/local/include/" --install-option="--library-path=/usr/local/lib/" pygraphviz

*
  Install SROS2 dependencies

  ``python3 -m pip install lxml``

*
  Install additional runtime dependencies for command-line tools:

  .. code-block:: bash

       python3 -m pip install catkin_pkg empy ifcfg lark-parser lxml numpy pyparsing pyyaml setuptools argcomplete

Disable System Integrity Protection (SIP)
-----------------------------------------

OS X versions >=10.11 have System Integrity Protection enabled by default.
So that SIP doesn't prevent processes from inheriting dynamic linker environment variables, such as ``DYLD_LIBRARY_PATH``, you'll need to disable it `following these instructions <https://developer.apple.com/library/content/documentation/Security/Conceptual/System_Integrity_Protection_Guide/ConfiguringSystemIntegrityProtection/ConfiguringSystemIntegrityProtection.html>`__.

Downloading ROS 2
-----------------


* Go to the releases page: https://github.com/ros2/ros2/releases
* Download the latest package for OS X; let's assume that it ends up at ``~/Downloads/ros2-release-distro-date-macos-amd64.tar.bz2``.

  * Note: there may be more than one binary download option which might cause the file name to differ.

*
  Unpack it:

  .. code-block:: bash

       mkdir -p ~/ros2_foxy
       cd ~/ros2_foxy
       tar xf ~/Downloads/ros2-release-distro-date-macos-amd64.tar.bz2

Install additional DDS implementations (optional)
-------------------------------------------------

ROS 2 builds on top of DDS.
It is compatible with multiple DDS or RTPS (the DDS wire protocol) vendors.

The package you downloaded has been built with *optional* support for three vendors.
Run-time support for eProsima's Fast RTPS is included bundled by default.
If you would like to use one of the other vendors you will need to install their software separately.

Enable OpenSplice support
^^^^^^^^^^^^^^^^^^^^^^^^^

Download the latest release from https://github.com/ADLINK-IST/opensplice/releases and unpack it.
For ROS 2 releases up to and including Ardent, do not do anything else at this point.
For ROS 2 releases later than Ardent, set the ``OSPL_HOME`` environment variable to the unpacked directory that contains the ``release.com`` script.

Enable Connext support
^^^^^^^^^^^^^^^^^^^^^^

To use RTI Connext DDS there are options available for `university, purchase or evaluation <../Install-Connext-University-Eval>`

After installing, run RTI launcher and point it to your license file.

Set the ``NDDSHOME`` environment variable:

.. code-block:: bash

   export NDDSHOME=/Applications/rti_connext_dds-5.3.1

If you want to install the Connext DDS-Security plugins please refer to `this page <../Install-Connext-Security-Plugins>`.

Set up the ROS 2 environment
----------------------------

Source the ROS 2 setup file:

.. code-block:: bash

   . ~/ros2_foxy/ros2-osx/setup.bash


For ROS 2 releases up to and including Ardent, if you downloaded a release with OpenSplice support you must additionally source the OpenSplice setup file manually (this is done automatically for ROS 2 releases later than Ardent).
Only do this **after** you have sourced the ROS 2 one:

.. code-block:: bash

   . <path_to_opensplice>/x86_64.darwin10_clang/release.com



Try some examples
-----------------

In one terminal, set up the ROS 2 environment as described above and then run a ``talker``:

.. code-block:: bash

   ros2 run demo_nodes_cpp talker


In another terminal, set up the ROS 2 environment and then run a ``listener``:

.. code-block:: bash

   ros2 run demo_nodes_cpp listener


You should see the ``talker`` saying that it's ``Publishing`` messages and the ``listener`` saying ``I heard`` those messages.
Hooray!

If you have installed support for an optional vendor, see `this page </Tutorials/Working-with-multiple-RMW-implementations>` for details on how to use that vendor.

If you run into issues, see `the troubleshooting section <Foxy_osx-development-setup-troubleshooting>` on the source installation page.

Build your own packages
-----------------------

If you would like to build your own packages, refer to the tutorial `"Using Colcon to build packages" </Tutorials/Colcon-Tutorial>`.

Troubleshooting
---------------

Troubleshooting techniques can be found `here </Troubleshooting>`.

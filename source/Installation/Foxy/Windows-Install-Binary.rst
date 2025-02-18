Installing ROS 2 on Windows
===========================

.. contents:: Table of Contents
   :depth: 2
   :local:

This page explains how to install ROS 2 on Windows from a pre-built binary package.

System requirements
-------------------

As of beta-2 only Windows 10 is supported.

.. _Foxy_windows-install-binary-installing-prerequisites:

Installing prerequisites
------------------------

Install Chocolatey
^^^^^^^^^^^^^^^^^^

Chocolatey is a package manager for Windows, install it by following their installation instructions:

https://chocolatey.org/

You'll use Chocolatey to install some other developer tools.

Install Python
^^^^^^^^^^^^^^

Open a Command Prompt and type the following to install Python via Chocolatey:

.. code-block:: bash

   > choco install -y python --version 3.7.5

Install Visual C++ Redistributables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Open a Command Prompt and type the following to install them via Chocolatey:

.. code-block:: bash

   > choco install -y vcredist2013 vcredist140

Install OpenSSL
^^^^^^^^^^^^^^^

Download an OpenSSL installer from `this page <https://slproweb.com/products/Win32OpenSSL.html>`__.
Scroll to the bottom of the page and download *Win64 OpenSSL v1.0.2*.
Don't download the Win32 or Light versions.

Run the installer with default parameters.
The following commands assume you used the default installation directory:

* ``setx -m OPENSSL_CONF C:\OpenSSL-Win64\bin\openssl.cfg``

You will need to append the OpenSSL-Win64 bin folder to your PATH.
You can do this by clicking the Windows icon, typing "Environment Variables", then clicking on "Edit the system environment variables".
In the resulting dialog, click "Environment Variables", then click "Path" on the bottom pane, finally click "Edit" and add the path below.

* ``C:\OpenSSL-Win64\bin\``

Install Visual Studio
^^^^^^^^^^^^^^^^^^^^^

Install Visual Studio 2019.

If you already have a paid version of Visual Studio 2019 (Professional, Enterprise), skip this step.

Microsoft provides a free of charge version of Visual Studio 2019, named Community, which can be used to build applications that use ROS 2:

   https://visualstudio.microsoft.com/downloads/

Make sure that the Visual C++ features are installed.

An easy way to make sure they're installed is to select the ``Desktop development with C++`` workflow during the install.

   .. image:: https://i.imgur.com/2h0IxCk.png

Make sure that no C++ CMake tools are installed by unselecting them in the list of components to be installed.

Install additional DDS implementations (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ROS 2 builds on top of DDS.
It is compatible with multiple DDS or RTPS (the DDS wire protocol) vendors.

The package you downloaded has been built with optional support for multiple vendors: eProsima FastRTPS, ADLINK OpenSplice, and RTI Connext as the middleware options.
Run-time support for eProsima's Fast RTPS is included bundled by default.
If you would like to use one of the other vendors you will need to install their software separately.

ADLINK OpenSplice
~~~~~~~~~~~~~~~~~

If you want to use OpenSplice, you will need to download the `latest supported version <https://github.com/ADLINK-IST/opensplice/releases>`__.
For ROS 2 Foxy version 6.9.190403OSS-HDE-x86_64.win-vs2017 or later is required.

After unpacking, set the ``OSPL_HOME`` environment variable so that it points to the directory that contains the ``release.bat`` script.

RTI Connext
~~~~~~~~~~~

To use RTI Connext DDS there are options available for `university, purchase or evaluation <../Install-Connext-University-Eval>`

After installing, run RTI launcher and point it to your license file.

Set the ``NDDSHOME`` environment variable:

.. code-block:: bash

   set "NDDSHOME=C:\Program Files\rti_connext_dds-5.3.1"

If you want to install the Connext DDS-Security plugins please refer to `this page <../Install-Connext-Security-Plugins>`.

Install OpenCV
^^^^^^^^^^^^^^

Some of the examples require OpenCV to be installed.

You can download a precompiled version of OpenCV 3.4.6 from https://github.com/ros2/ros2/releases/download/opencv-archives/opencv-3.4.6-vc16.VS2019.zip .

Assuming you unpacked it to ``C:\opencv``\ , type the following on a Command Prompt (requires Admin privileges):

.. code-block:: bash

   setx -m OpenCV_DIR C:\opencv

Since you are using a precompiled ROS version, we have to tell it where to find the OpenCV libraries.
You have to extend the ``PATH`` variable to ``C:\opencv\x64\vc16\bin``.

Install dependencies
^^^^^^^^^^^^^^^^^^^^

There are a few dependencies not available in the Chocolatey package database.
In order to ease the manual installation process, we provide the necessary Chocolatey packages.

As some chocolatey packages rely on it, we start by installing CMake

.. code-block:: bash

   > choco install -y cmake

You will need to append the CMake bin folder ``C:\Program Files\CMake\bin`` to your PATH.

Please download these packages from `this <https://github.com/ros2/choco-packages/releases/latest>`__ GitHub repository.

* asio.1.12.1.nupkg
* cunit.2.1.3.nupkg
* eigen-3.3.4.nupkg
* tinyxml-usestl.2.6.2.nupkg
* tinyxml2.6.0.0.nupkg
* log4cxx.0.10.0.nupkg

Once these packages are downloaded, open an administrative shell and execute the following command:

.. code-block:: bash

   > choco install -y -s <PATH\TO\DOWNLOADS\> asio cunit eigen tinyxml-usestl tinyxml2 log4cxx

Please replace ``<PATH\TO\DOWNLOADS>`` with the folder you downloaded the packages to.

You must also install some python dependencies for command-line tools:

.. code-block:: bash

   python -m pip install -U catkin_pkg cryptography empy ifcfg lark-parser lxml netifaces numpy opencv-python pyparsing pyyaml setuptools

RQt dependencies
~~~~~~~~~~~~~~~~

.. code-block:: bash

   python -m pip install -U pydot PyQt5

Downloading ROS 2
-----------------

* Go the releases page: https://github.com/ros2/ros2/releases
* Download the latest package for Windows, e.g., ``ros2-foxy-*-windows-AMD64.zip``.

.. note::

    There may be more than one binary download option which might cause the file name to differ.

.. note::

    To download the ROS 2 debug libraries you'll need to download ``ros2-foxy-*-windows-debug-AMD64.zip``

* Unpack the zip file somewhere (we'll assume ``C:\dev\ros2_foxy``\ ).


Set up the ROS 2 environment
----------------------------

Start a command shell and source the ROS 2 setup file to set up the workspace:

.. code-block:: bash

   > call C:\dev\ros2_foxy\local_setup.bat

It is normal that the previous command, if nothing else went wrong, outputs "The system cannot find the path specified." exactly once.

Try some examples
-----------------

In a command shell, set up the ROS 2 environment as described above and then run a ``talker``\ :

.. code-block:: bash

   > ros2 run demo_nodes_cpp talker

Start another command shell and run a ``listener``\ :

.. code-block:: bash

   > ros2 run demo_nodes_py listener

You should see the ``talker`` saying that it's ``Publishing`` messages and the ``listener`` saying ``I heard`` those messages.
Hooray!

If you have installed support for an optional vendor, see `this page </Tutorials/Working-with-multiple-RMW-implementations>` for details on how to use that vendor.

Troubleshooting
^^^^^^^^^^^^^^^

* If at one point your example would not start because of missing dll's, please verify that all libraries from external dependencies such as OpenCV are located inside your ``PATH`` variable.
* If you forget to call the ``local_setup.bat`` file from your terminal, the demo programs will most likely crash immediately.
* If you see an error related with FastRTPS failing to be loaded, see `troubleshooting section in development install instructions <Windows-Development-Setup>`.

Troubleshooting techniques can also be found `here </Troubleshooting>`.

Build your own packages
-----------------------

If you would like to build your own packages, refer to the tutorial `"Using Colcon to build packages" </Tutorials/Colcon-Tutorial>`.

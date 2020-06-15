.. _calm_dsl:

-----------------------------------
Task 1: Preparing your environment
-----------------------------------

We will first prepare our environment to run calm-dsl. We will run calm-dsl on Docker for this lab.


Install requirements
++++++++++++++++++++

Docker
.......

Make sure you have Docker running on your laptop. To verify that Docker engine is running, run the following command:

.. code-block:: bash
  :name: verify-docker
  :caption: Check docker version

  $ docker --version

If you don't have Docker installed, please refer to the installation instructions here:

**MacOS:**

#. Intall  the latest version of Python, download from here: https://www.python.org/downloads/

#. Install Docker Desktop for Mac, details and download are found here: https://docs.docker.com/docker-for-mac/

#. Install Visual Sutdio Code, details can be found here: https://code.visualstudio.com/

.. note::
  Recommended extension to be added to Visual Studio Code, Click the **View** menu, select **Extensions**, then search for:

  - Remote container
  - Python
  - Docker

**Windows:**

#. Install Docker for Windows, details and download are found, here: https://hub.docker.com/editions/community/docker-ce-desktop-windows/

   .. note::
     When the installation finishes, Docker starts automatically. The whale |docker-icon| in the notification area indicates that Docker is running, and accessible from a terminal.
  
#. Install Linux kernel update, found here: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

   .. note::
     To install the Linux kernel update package:
  
     - Run the update package downloaded in the previous step.
     - You will be prompted for elevated permissions, select ‘yes’ to approve this installation.
     - Once the installation is complete, you are ready to begin using WSL2!

#. Open PowerShell with Administrator Privilages


Running calm-dsl
.................

calm-dsl image is already available on docker hub *ntnx/calm-dsl*, pull the image and run the container

.. code-block:: bash
  :name: pull-image
  :caption: Pull calm-dsl image

  $ docker pull ntnx/calm-dsl



Create a folder to hold you blueprint files for this lab.

.. code-block:: bash
  :name: run-container
  :caption: Run calm-dsl container

  $ mkdir basic_blueprint
  $ cd basic_blueprint
  $ docker run -it -v $PWD:/root/blueprint ntnx/calm-dsl

If everything is ok, you should be presented with container cli.

Initialize calm-dsl
+++++++++++++++++++

Run **calm init dsl** on the cli to initialize calm-dsl with the correct settings.

.. figure:: images/calm_init_dsl.png


To validate the settings you can check the status of the PC connecitivty:

.. code-block:: bash
  :name: check-dsl-status
  :caption: Check calm-dsl status

  $ calm get server status


Optional: integration with VSCode
++++++++++++++++++++++++++++++++++

Optionally you can integrate the running container with Visual Studio Code to allow lenting, follow the steps on this blog: https://www.nutanix.dev/2020/04/24/nutanix-calm-dsl-remote-container-development-part-1/

.. |proj-icon| image:: ../images/projects_icon.png
.. |docker-icon| image:: ../images/docker_icon.png
.. |mktmgr-icon| image:: ../images/marketplacemanager_icon.png
.. |mkt-icon| image:: ../images/marketplace_icon.png
.. |bp-icon| image:: ../images/blueprints_icon.png
.. |blueprints| image:: images/blueprints.png
.. |applications| image:: images/blueprints.png
.. |projects| image:: images/projects.png
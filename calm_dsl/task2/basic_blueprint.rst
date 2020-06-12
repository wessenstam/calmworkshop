.. comments
..

-----------------------------------
Task 2: Building a basic blueprint
-----------------------------------

The purpose of this lab is create **LAMP** (Linux Apache MySql PHP) stack using calm-dsl. The following services need to be deployed:
* **LB Tier:** HAProxy, count 1
* **Web Tier:** Apahce/PHP, count: ask the user for the number of instance to deploy
* **DB Tier:** MySQL or MariaDB, count 1, request the user to input database password


Step 1: Create blueprint file
..............................

Using your IDE (suggested Visual Studio Code) create an empty python file. Start importing the required libraries:

.. code-block:: python
  :name: initial_lamp_blueprint
  :caption: LAMP blueprint

  from calm.dsl.builtins import SimpleDeployment, SimpleBlueprint


  def main():
    print(None)

  if __name__ == '__main__':
    main()

.. note::

  Save the file in the same folder mapped (project folder) to calm-dsl container created earlier in task 1.


Step 2: Define default cedentials
..................................

Use a text file under **.local** under the project folder to store the defualt Password.

.. code-block:: bash
  :name: local-password
  :caption: Create local password file

  $ mkdir .local
  $ echo nutanix/4u > .local/centos

Import read_local_file and create credentials object.

.. code-block:: python
  :name: lamp_blueprint_password
  :caption: Initial LAMP blueprint
  :emphasize-lines: 2,4-5

  from calm.dsl.builtins import SimpleDeployment, SimpleBlueprint
  from calm.dsl.builtins import read_local_file, basic_cred

  CENTOS_PASSWD = read_local_file('centos')
  CENTOS_CRED = basic_cred('centos', CENTOS_PASSWD, name='CENTOS_CRED', default=True)

  def main():
    print(None)

  if __name__ == '__main__':
    main()

Step 3: Define VM Resource object
.................................

Assume all the services VM will have the same VM specs (OS, memory, disk and nic).

.. code-block:: python
  :name: lamp_blueprint_image_password
  :caption: LAMP blueprint
  :emphasize-lines: 3,8-26

  from calm.dsl.builtins import SimpleDeployment, SimpleBlueprint
  from calm.dsl.builtins import read_local_file, basic_cred
  from calm.dsl.builtins import vm_disk_package, AhvVmResources, AhvVmDisk, AhvVmNic, AhvVmGC, AhvVm

  CENTOS_PASSWD = read_local_file('centos')
  CENTOS_CRED = basic_cred('centos', CENTOS_PASSWD, name='CENTOS_CRED', default=True)

  CENTOS_IMAGE_SOURCE = 'https://cloud.centos.org/centos/8/x86_64/images/CentOS-8-ec2-8.1.1911-20200113.3.x86_64.qcow2'
  CentosPackage = vm_disk_package( name='centos_disk',
                                 config={'image': {'source': CENTOS_IMAGE_SOURCE}})

  class CentosVmResources(AhvVmResources):
    memory = 4
    vCPUs = 2
    cores_per_vCPU = 1
    disks = [AhvVmDisk.Disk.Scsi.cloneFromVMDiskPackage(CentosPackage, bootable=True)]
    nics = [AhvVmNic.DirectNic.ingress("RX-Automation")]
    guest_customization = AhvVmGC.CloudInit(
      config={
          'password': CENTOS_PASSWD,
          'ssh_pwauth': True,
          'chpasswd': { 'expire': False }
      })

  class CentosVm(AhvVm):
    resources = CentosVmResources


  def main():
    print(None)

  if __name__ == '__main__':
    main()


Step 4: Create deployment classes
.................................

For each service add a class along with the install scripts.

Scripts can be found on this link: https://github.com/halsayed/calm-dsl-workshop/tree/master/solution/task2/scripts

.. code-block:: python
  :name: lamp_blueprint_services
  :caption: LAMP blueprint

  from calm.dsl.builtins import action, CalmTask

  class ApachePHP(SimpleDeployment):
      provider_spec = CentosVm
      os_type = 'Linux'
      min_replicas = '@@{COUNT}@@'

      @action
      def __install__(self):
          CalmTask.Exec.ssh(name='install_apache', filename='scripts/Apache_install.sh')

  class HAProxy(SimpleDeployment):
      provider_spec = CentosVm
      os_type = 'Linux'

      @action
      def __install__(self):
          CalmTask.Exec.ssh(name='install_haproxy', filename='scripts/haproxy_install.sh')

  class MySQL(SimpleDeployment):
      provider_spec = CentosVm
      os_type = 'Linux'

      @action
      def __install__(self):
          CalmTask.Exec.ssh(name='install_mysql', filename='scripts/mysql_install.sh')

.. note::
  The number of apache instances is passed as Calm variable, we will define this in the next step.

Step 5: Define blueprint class
..............................

In this step, we will link all components to gether.

.. code-block:: python
  :name: lamp_blueprint_all
  :caption: LAMP blueprint

  from calm.dsl.builtins import CalmVariable

  class LAMPBlueprint(SimpleBlueprint):
      credentials = [CENTOS_CRED]
      deployments = [ApachePHP, HAProxy, MySQL]

      MYSQL_PASSWORD = CalmVariable.Simple.Secret('MYSQL_PASSWORD', label='MySQL root password', runtime=True)
      COUNT = CalmVariable.WithOptions.Predefined.string(['1', '2', '3'], default='1', name='COUNT',
                                                          label='Apache Count', runtime=True)

Step 6: Deploy blueprint
.........................

This is the final step, deploy the blueprint to Calm using calm-dsl cli

.. code-block:: bash
  :name: deploy-blueprint
  :caption: Deploy blueprint

  calm create bp --filename lamp_blueprint.py --name LAMP-CalmDSL

.. note::
  You can refer to the full python script on this repo:

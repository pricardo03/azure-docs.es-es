---
title: Configuración de un clúster de Linux RDMA para ejecutar aplicaciones MPI | Microsoft Docs
description: Cree un clúster de Linux con máquinas virtuales de tamaño H16r, H16mr, A8 o A9 para usar la red de RDMA de Azure para ejecutar aplicaciones de MPI.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/11/2018
ms.author: danlep
ms.openlocfilehash: 471fd4095fe45e76f94df8c61a07eeb9bbc1c120
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990734"
---
# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Configuración de un clúster de Linux RDMA para ejecutar aplicaciones MPI

Aprenda a configurar un clúster de Linux RDMA en Azure con [tamaños de máquina virtual de procesos de alto rendimiento](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para ejecutar aplicaciones de interfaz de paso de mensajes (MPI) paralelas. En este artículo se incluyen los pasos necesarios para preparar una imagen de HPC de Linux para ejecutar Intel MPI en un clúster. Después de la preparación, se implementa un clúster de máquinas virtuales con esta imagen y uno de los [tamaños de máquinas virtuales de Azure compatibles con RDMA](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#rdma-capable-instances). Use el clúster para ejecutar aplicaciones MPI que se comunican eficazmente a través de una red de latencia baja y alto rendimiento con tecnología de acceso directo a memoria remota (RDMA).

> [!IMPORTANT]
> Azure tiene dos modelos de implementación diferentes para crear recursos y trabajar con ellos: [Azure Resource Manager](../../../azure-resource-manager/resource-manager-deployment-model.md) y el clásico. Este artículo trata del modelo de implementación clásico. Microsoft recomienda que las implementaciones más recientes usen el modelo de Resource Manager.

## <a name="cluster-deployment-options"></a>Opciones de implementación de clúster
A continuación se facilitan distintos métodos que puede usar para crear un clúster de Linux RDMA con o sin un programador de trabajos.

* **Scripts de la CLI de Azure**: como se muestra más adelante en este artículo, use la [interfaz de la línea de comandos de Azure](../../../cli-install-nodejs.md) (CLI) para automatizar la implementación de un clúster de máquinas virtuales compatibles con RDMA. La CLI en modo de Administración de servicios crea los nodos del clúster en serie en el modelo de implementación clásica, por lo que la implementación de muchos nodos de proceso puede tardar varios minutos. Para habilitar la conexión de red de RDMA cuando se usa el modelo de implementación clásica, implemente las máquinas virtuales en el mismo servicio en la nube.
* **Plantillas de Azure Resource Manager**: también puede usar el modelo de implementación de Resource Manager para implementar un clúster de máquinas virtuales compatibles con RDMA que se conecte a la red de RDMA. También puede [crear su propia plantilla](../../../resource-group-authoring-templates.md) o consultar las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/documentation/templates/) para obtener plantillas proporcionadas por Microsoft o la comunidad para implementar la solución que desee. Las plantillas del Administrador de recursos proporcionan una manera rápida y confiable de implementar un clúster de Linux. Para habilitar la conexión de red de RDMA cuando se usa el modelo de implementación de Resource Manager, implemente las máquinas virtuales en el mismo conjunto de disponibilidad o conjunto de escalado de máquinas virtuales.
* **HPC Pack**: cree un clúster de Microsoft HPC Pack en Azure y agregue nodos de ejecución compatibles con RDMA que ejecuten una distribución de Linux compatible para acceder a la red de RDMA. Para más información, consulte [Introducción a los nodos de ejecución de Linux en un clúster de HPC Pack en Azure](hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-the-classic-model"></a>Pasos de implementación de ejemplo en el modelo clásico
Los pasos siguientes muestran cómo usar la CLI de Azure para implementar una máquina virtual de HPC de SUSE Linux Enterprise Server 12 (SLES) desde Azure Marketplace, personalizarla y crear una imagen de máquina virtual personalizada. Después puede usar la imagen para automatizar la implementación de un clúster de máquinas virtuales compatibles con RDMA.

> [!TIP]
> Siga los mismos pasos para implementar un clúster de máquinas virtuales compatibles con RDMA basadas en imágenes de HPC basadas en CentOS en Azure Marketplace. Es posible que, como se indica, algunos pasos sean ligeramente distintos.
>

### <a name="prerequisites"></a>Requisitos previos
* **Equipo cliente**: necesita un equipo cliente de Mac, Linux o Windows para comunicarse con Azure. Estos pasos se supone que está usado un cliente Linux.
* **Suscripción de Azure**: si no tiene ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/) en un par de minutos. En los clústeres más grandes, considere la posibilidad de una suscripción de pago por uso u otras opciones de compra.
* **Disponibilidad de tamaño de máquina virtual**: compruebe [Productos disponibles por región](https://azure.microsoft.com/regions/services/) para obtener información sobre la disponibilidad de la serie H u otros tamaños de máquina virtual compatible con RDMA en las regiones de Azure.
* **Cuota de núcleos**: es posible que tenga que aumentar la cuota de núcleos para implementar un clúster de máquinas virtuales de proceso intensivo. Por ejemplo, necesita al menos 128 núcleos si desea implementar máquinas virtuales A8 o A9 tal y como se muestra en este artículo. La suscripción también podría limitar el número de núcleos que se pueden implementar en ciertas familias de tamaño de máquina virtual, como la serie H. Para solicitar un aumento de cuota, [abra una solicitud de soporte técnico al cliente en línea](../../../azure-supportability/how-to-create-azure-support-request.md) sin cargo alguno.
* **CLI de Azure**: [instale](../../../cli-install-nodejs.md) la CLI de Azure y [conéctela a su suscripción de Azure](/cli/azure/authenticate-azure-cli) en el equipo cliente.

Revise también las consideraciones de implementación para los [Tamaños de máquina virtual de Azure compatibles con RDMA](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#rdma-capable-instances).

### <a name="provision-an-sles-12-hpc-vm"></a>Aprovisionamiento de una máquina virtual de HPC de SLES 12
Después de iniciar sesión en Azure con la CLI de Azure, ejecute `azure config list` para confirmar que la salida muestra el modo de Administración del servicio de Azure. Si no es así, ejecute este comando para establecer el modo:

    azure config mode asm


Escriba lo siguiente para enumerar todas las suscripciones que está autorizado a usar:

    azure account list

La suscripción activa actual se identifica con `Current` establecido en `true`. Si esta suscripción no es la que desea usar para crear el clúster, establezca el id. de suscripción adecuado como la suscripción activa:

    azure account set <subscription-Id>

Para ver las imágenes de HPC de SLES 12 disponibles públicamente en Azure, ejecute un comando similar al siguiente, asumiendo que el entorno de shell admite **grep**:

    azure vm image list | grep "suse.*hpc"

Aprovisione una máquina virtual compatible con RDMA con una imagen de HPC de SLES 12 SP3 ejecutando un comando similar al siguiente:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp3-hpc-v20170913

Donde:

* El tamaño (A9 en este ejemplo) es uno de los tamaños de máquina virtual compatibles con RDMA.
* El número de puerto externo de SSH (22 en este ejemplo, que es el valor predeterminado de SSH) es cualquier número de puerto válido. El número de puerto interno de SSH se establece en 22.
* Se crea un servicio en la nube en la región de Azure especificada por la ubicación. Especifique una ubicación en la que esté disponible el tamaño de máquina virtual que elija, como "Oeste de EE. UU.".
* Para que disponga de compatibilidad prioritaria para SUSE (que puede significar cargos adicionales), el nombre de la imagen de SLES 12 puede ser una de las opciones que tienen `priority` en el nombre, como: 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp3-hpc-priority-v20170913`

### <a name="customize-the-vm"></a>Personalización de la máquina virtual
Después de que la máquina virtual finalice el aprovisionamiento, SSH a la máquina virtual con la dirección IP externa de la máquina virtual (o el nombre DNS) y el número de puerto externo que configuró y, luego, personalícelo. Para detalles de la conexión, consulte [Inicio de sesión en una máquina virtual Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ejecute los comandos como el usuario que haya configurado en la máquina virtual, a menos que se requiera el acceso a la raíz para completar un paso.

> [!IMPORTANT]
> Microsoft Azure no proporciona acceso a la raíz a máquinas virtuales de Linux. Para obtener acceso administrativo al conectarse como un usuario a la máquina virtual, ejecute los comandos mediante `sudo`.
>
>

* **Actualizaciones**: instale las actualizaciones mediante zypper. También puede instalar utilidades NFS.

  > [!IMPORTANT]
  > En una máquina virtual de HPC de SLES 12, es recomendable no aplicar actualizaciones de kernel, que pueden causar problemas con los controladores de RDMA de Linux.
  >
  >
* **Intel MPI**: complete la instalación de Intel MPI en la máquina virtual ejecutando el siguiente comando:

    ```bash
    sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
    ```

* **Bloquear memoria**: para que los códigos de MPI bloqueen la memoria disponible para RDMA, agregue o cambie la configuración siguiente en el archivo /etc/security/limits.conf. Necesita acceso a la raíz para editar este archivo.

    ```bash
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > Para realizar pruebas, también puede establecer memlock en ilimitado. Por ejemplo: `* hard memlock unlimited`. Para más información, consulte los [métodos más conocidos para definir el tamaño de memoria bloqueada](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).
  >
  >
* **Claves SSH para máquinas virtuales de SLES**: genere claves SSH para establecer la confianza para la cuenta de usuario entre los nodos de proceso del clúster de SLES al ejecutar trabajos MPI. Si implementó una máquina virtual de HPC basada en CentOS, no siga este paso. Consulte las instrucciones más adelante en este artículo para establecer una relación de confianza de SSH sin contraseña entre los nodos del clúster después de capturar la imagen y de implementar el clúster.

  Para crear claves SSH, ejecute el comando `ssh-keygen`. Cuando se le pida una entrada, seleccione **Entrar** para generar las claves en la ubicación predeterminada sin tener que establecer una contraseña.

  Agregue la clave pública al archivo authorized_keys para las claves públicas conocidas.

  ```bash
  cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  ```

  En el directorio ~/.ssh, edite o cree el archivo `config`. Proporcione el intervalo de direcciones IP de la red privada que planea usar en Azure (10.32.0.0/16 en este ejemplo):

  ```bash
  host 10.32.0.*
  StrictHostKeyChecking no
  ```

  O bien, especifique la dirección IP de red privada de cada máquina virtual en el clúster como sigue:

  ```bash
  host 10.32.0.1
  StrictHostKeyChecking no
  host 10.32.0.2
  StrictHostKeyChecking no
  host 10.32.0.3
  StrictHostKeyChecking no
  ...
  ```

  > [!NOTE]
  > Configurar `StrictHostKeyChecking no` puede crear un posible riesgo de seguridad cuando no se especifica una dirección IP o un intervalo concretos.
  >
  >
* **Aplicaciones**: instale todas las aplicaciones que necesite o realice otras personalizaciones antes de capturar la imagen.

### <a name="capture-the-image"></a>Capturar la imagen
Para capturar la imagen, primero ejecute el comando siguiente en la máquina virtual Linux. Este comando desaprovisiona la máquina virtual, pero mantiene las cuentas de usuario y las claves SSH que configure.

```bash
sudo waagent -deprovision
```

Desde el equipo cliente, ejecute los siguientes comandos de CLI de Azure para capturar la imagen. Para más información, consulte [Captura de una máquina virtual clásica con Linux para usarla como imagen](capture-image-classic.md).  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Después de ejecutar estos comandos, se captura la imagen de máquina virtual para su uso y se elimina la máquina virtual. Ahora tiene la imagen personalizada lista para implementar un clúster.

### <a name="deploy-a-cluster-with-the-image"></a>Implementar un clúster con la imagen
Modifique el script Bash siguiente con los valores apropiados para su entorno y ejecútelo desde el equipo cliente. Debido a que Azure implementa las máquinas virtuales en serie en el modelo de implementación clásica, tardará unos minutos en implementar las ocho máquinas virtuales A9 sugeridas en este script.

```bash
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a classic VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>;
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Consideraciones para un clúster de HPC de CentOS
Si desea configurar un clúster basado en una de las imágenes de HPC de CentOS en Azure Marketplace en lugar de SLES 12 para HPC, siga los pasos generales de la sección anterior. Tenga en cuenta las siguientes diferencias al aprovisionar y configurar la máquina virtual:

- Intel MPI ya está instalado en una máquina virtual aprovisionada desde una imagen de HPC basada en CentOS.
- La configuración de memoria bloqueada ya está agregada en el archivo /etc/security/limits.conf de la máquina virtual.
- No se generan claves SSH en la máquina virtual aprovisionada para la captura. En su lugar, se recomienda establecer la autenticación basada en usuario después de implementar el clúster. Para más información, consulte la sección siguiente.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Configuración de confianza SSH sin contraseña en el clúster
En un clúster de HPC basado en CentOS, existen dos métodos para establecer la confianza entre los nodos de proceso: la autenticación basada en host y la autenticación basada en usuario. La autenticación basada en host está fuera del ámbito de este artículo y por lo general debe realizarse a través de un script de extensión durante la implementación. La autenticación basada en usuario es conveniente para establecer la confianza después de la implementación y requiere la generación y el uso compartido de claves SSH entre los nodos de proceso en el clúster. Este método se suele conocer como inicio de sesión SSH sin contraseña y es necesario al ejecutar trabajos de MPI.

A continuación encontrará un script de ejemplo `user_authentication.sh` para habilitar la autenticación basada en usuario en un clúster HPC basado en CentOS:

```bash
#!/bin/bash
# For CentOS user must first install epel-release, sshpass, and nmap (sshpass and nmap are available from epel-release for CentOS)

# usage ./user_authentication.sh [username] [password] [internalIP prefix]
# ./user_authentication.sh azureuser Azure@123 10.32.0
USER=$1
PASS=$2
IPPRE=$3
HEADNODE=`hostname`

mkdir -p .ssh
echo -e  'y\n' | ssh-keygen -f .ssh/id_rsa -t rsa -N ''

echo 'Host *' >> .ssh/config
echo 'StrictHostKeyChecking no' >> .ssh/config
chmod 400 .ssh/config
chown azureuser:azureuser /home/azureuser/.ssh/config

nmap -sn $IPPRE.* | grep $IPPRE. | awk '{print $5}' > nodeips.txt
for NAME in `cat nodeips.txt`; do sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'hostname' >> nodenames.txt;done

NAMES=`cat nodenames.txt` #names from names.txt file
for NAME in $NAMES; do
    sshpass -p $PASS scp -o "StrictHostKeyChecking no" -o ConnectTimeout=2 /home/$USER/nodenames.txt $USER@$NAME:/home/$USER/
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME "mkdir .ssh && chmod 700 .ssh"
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME "echo -e  'y\n' | ssh-keygen -f .ssh/id_rsa -t rsa -N ''"
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'touch /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'echo "Host *" >  /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'echo StrictHostKeyChecking no >> /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 400 /home/'$USER'/.ssh/config'
    cat .ssh/id_rsa.pub | sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'cat >> .ssh/authorized_keys'
    sshpass -p $PASS scp -o "StrictHostKeyChecking no" -o ConnectTimeout=2 $USER@$NAME:/home/$USER/.ssh/id_rsa.pub .ssh/sub_node.pub

    for SUBNODE in `cat nodeips.txt`; do
         sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$SUBNODE 'mkdir -p .ssh'
         cat .ssh/sub_node.pub | sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$SUBNODE 'cat >> .ssh/authorized_keys'
    done
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 700 .ssh/'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 640 .ssh/authorized_keys'
done
```

Use este script mediante los siguientes pasos. También puede modificar este script o usar cualquier otro método para establecer la autenticación de SSH sin contraseña entre los nodos del clúster de proceso.

Para ejecutar el script, debe conocer el prefijo para las direcciones IP de subred. Para obtener el prefijo, ejecute el siguiente comando en uno de los nodos del clúster. El resultado debe ser similar al de 10.1.3.5, y el prefijo es la parte 10.1.3.

```bash
ifconfig eth0 | grep -w inet | awk '{print $2}'
```

Ahora ejecute el script con tres parámetros: el nombre de usuario común en los nodos de proceso, la contraseña común de ese usuario en los nodos de proceso y el prefijo de subred que se devolvió en el comando anterior.

```bash
./user_authentication.sh <myusername> <mypassword> 10.1.3
```

El script hace lo siguiente:

* Crea un directorio en el nodo de host de host denominado .ssh, que es necesario para el inicio de sesión sin contraseña.
* Crea un archivo de configuración en el directorio .ssh que indica el inicio de sesión sin contraseña para permitir el inicio de sesión desde cualquier nodo del clúster.
* Crea archivos que contienen los nombres de nodo y las direcciones IP de nodo para todos los nodos del clúster. Estos archivos se mantienen después de ejecutar el script como referencia futura.
* Crea un par de claves pública y privada para cada nodo de clúster, incluido el nodo de host, y crea entradas en el archivo authorized_keys.

> [!WARNING]
> La ejecución de este script puede crear un posible riesgo de seguridad. Asegúrese de que no se distribuye la información de clave pública en ~/.ssh.
>

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Ejecución de MPI en un clúster de dos nodos básico
Si aún no lo hizo, configure primero el entorno para Intel MPI.

```bash
# For a SLES 12 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a>Ejecución de un comando de MPI
Ejecute un comando de MPI en uno de los nodos de proceso para mostrar que MPI se instaló correctamente y puede comunicarse entre al menos dos nodos de proceso. El siguiente comando **mpirun** ejecuta el comando **hostname** en dos nodos. Para el parámetro `-hosts`, pase las direcciones IP de dos nodos en la red virtual de Azure (por ejemplo, 10.32.0.4,10.32.0.5).

```bash
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
La salida debe enumerar los nombres de todos los nodos que se pasaron como entrada para `-hosts`. Por ejemplo, un comando **mpirun** con dos nodos devuelve una salida similar a la siguiente:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Ejecución de una prueba comparativa de MPI
El siguiente comando de Intel MPI ejecuta una prueba comparativa de pingpong para comprobar la configuración del clúster y la conexión a la red de RDMA.

```bash
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Debería ver un resultado similar al siguiente en un clúster de trabajo con dos nodos. En la red de RDMA de Azure, se espera una latencia igual o inferior a 3 microsegundos para los tamaños de mensaje de hasta 512 bytes.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 6 17:16:46 2018
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```
### <a name="sample-mpi-run-script"></a>Ejemplo de script de ejecución de MPI
A continuación se muestra un script Bash de ejemplo para configurar las variables necesarias para ejecutar una aplicación MPI. Cambie la ruta de acceso a `mpivars.sh` según sea necesario para la instalación de Intel MPI.

```bash
#!/bin/bash -x

# For a SLES 12 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the MPI job. Substitute with values appropriate for your application.

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

El formato del archivo host es el siguiente. Agregue una línea para cada nodo del clúster. Especifique las direcciones IP privadas de la red virtual definida anteriormente, no los nombres DNS. Por ejemplo, para dos hosts con direcciones IP 10.32.0.4 y 10.32.0.4, el archivo contiene lo siguiente:

```bash
10.32.0.4:16
10.32.0.5:16
```


## <a name="next-steps"></a>Pasos siguientes
* Implemente y ejecute las aplicaciones Linux MPI en el clúster de Linux.
* Consulte la [documentación de la biblioteca de Intel MPI](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) para obtener orientación sobre Intel MPI.
* Pruebe una [plantilla de inicio rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) para crear un clúster de Intel Lustre mediante una imagen HPC basada en CentOS. 

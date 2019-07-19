---
title: Alta disponibilidad de Azure Virtual Machines para SAP NetWeaver en Red Hat Enterprise Linux con Azure NetApp Files | Microsoft Docs
description: Alta disponibilidad de Azure Virtual Machines para SAP NetWeaver en Red Hat Enterprise Linux
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/14/2019
ms.author: radeltch
ms.openlocfilehash: 8679cfe54c8fb2c88b312f67ea9b2d7115cc479e
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503789"
---
# <a name="azure-virtual-machines-high-availability-for-sap-netweaver-on-red-hat-enterprise-linux-with-azure-netapp-files-for-sap-applications"></a>Alta disponibilidad de Azure Virtual Machines para SAP NetWeaver en Red Hat Enterprise Linux con Azure NetApp Files para aplicaciones SAP

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=storage&regions=all
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2002167]: https://launchpad.support.sap.com/#/notes/2002167
[2009879]: https://launchpad.support.sap.com/#/notes/2009879
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability-rhel.md
[glusterfs-ha]:high-availability-guide-rhel-glusterfs.md

En este artículo se describe cómo implementar y configurar las máquinas virtuales, y cómo instalar el marco del clúster y un sistema SAP NetWeaver 7.50 de alta disponibilidad, mediante [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction/).
En las configuraciones, comandos de instalación, etc. de ejemplo, La instancia de ASCS tiene el número 00, la de ERS el 01, la de la aplicación principal (PAS) el 02 y la de la aplicación (AAS) el 03. Se usan los controles de calidad de los identificadores del sistema SAP. 

El nivel de la base de datos no se trata en detalle en este artículo.  

Lea primero las notas y los documentos de SAP siguientes:

* [Documentación sobre Azure NetApp Files][anf-azure-doc] 
* Nota de SAP [1928533], que incluye:
  * La lista de tamaños de máquina virtual de Azure que se admiten para la implementación del software de SAP
  * Información importante sobre la capacidad para los tamaños de máquina virtual de Azure
  * Software de SAP admitido y combinaciones de sistema operativo y base de datos
  * Versión del kernel de SAP requerida para Windows y Linux en Microsoft Azure

* La nota de SAP [2015553] enumera los requisitos previos para las implementaciones de software de SAP admitidas por SAP en Azure.
* La nota de SAP [2002167] recomienda la configuración del sistema operativo para Red Hat Enterprise Linux
* La nota de SAP [2009879] contiene las instrucciones de SAP HANA para Red Hat Enterprise Linux
* La nota de SAP [2178632] contiene información detallada sobre todas las métricas de supervisión notificadas para SAP en Azure.
* La nota de SAP [2191498] incluye la versión de SAP Host Agent necesaria para Linux en Azure.
* La nota de SAP [2243692] incluye información acerca de las licencias de SAP en Linux en Azure.
* La nota de SAP [1999351] contiene más soluciones de problemas de la extensión de supervisión mejorada de Azure para SAP.
* La [WIKI de la comunidad SAP](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) contiene todas las notas de SAP que se necesitan para Linux.
* [Planeación e implementación de Azure Virtual Machines para SAP en Linux][planning-guide]
* [Implementación de Azure Virtual Machines para SAP en Linux][deployment-guide]
* [Implementación de DBMS de Azure Virtual Machines para SAP en Linux][dbms-guide]
* [SAP Netweaver en el clúster de Pacemaker](https://access.redhat.com/articles/3150081)
* Documentación general de RHEL
  * [Introducción al complemento de alta disponibilidad](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Administración del complemento de alta disponibilidad](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Referencia del complemento de alta disponibilidad](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
  * [Configuración de ASCS/ERS para SAP Netweaver con recursos independientes en RHEL 7.5](https://access.redhat.com/articles/3569681)
  * [Configuración de SAP S/4HANA ASCS/ERS con el servidor 2 de puesta en cola independiente (ENSA2) en Pacemaker en RHEL ](https://access.redhat.com/articles/3974941)
* Documentación de RHEL específica para Azure:
  * [Directivas de compatibilidad para clústeres de alta disponibilidad RHEL: instancias de Microsoft Azure Virtual Machines como miembros del clúster](https://access.redhat.com/articles/3131341)
  * [Instalación y configuración de un clúster de alta disponibilidad de Red Hat Enterprise Linux 7.4 (y versiones posteriores) en Microsoft Azure](https://access.redhat.com/articles/3252491)
* [Aplicaciones de NetApp SAP en Microsoft Azure que usan Azure NetApp Files][anf-sap-applications-azure]

## <a name="overview"></a>Información general

La alta disponibilidad de los servicios centrales de SAP Netweaver requiere almacenamiento compartido.
Para lograrlo en Red Hat Linux hasta ahora era necesario crear el clúster independiente GlusterFS de alta disponibilidad. 

Ahora ya es posible lograr alta disponibilidad en SAP NetWeaver mediante el uso de almacenamiento compartido implementado en Azure NetApp Files. Mediante el uso de Azure NetApp Files para el almacenamiento compartido se elimina la necesidad del [clúster GlusterFS](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs) adicional. Pacemaker sigue siendo necesario para lograr una alta disponibilidad de los servicios centrales de SAP NetWeaver (ASCS/SCS).

![Información general sobre la alta disponibilidad de SAP NetWeaver](./media/high-availability-guide-rhel/high-availability-guide-rhel-anf.png)

SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS y la base de datos SAP HANA usan direcciones IP virtuales y el nombre de host virtual. En Azure, se requiere un equilibrador de carga para usar una dirección IP virtual. En la lista siguiente se muestra la configuración del equilibrador de carga con direcciones IP de front-end para (A)SCS y ERS.

> [!IMPORTANT]
> La agrupación en clústeres de varios SID de SAP ASCS/ERS con Red Hat Linux como sistema operativo invitado en las VM de Azure **NO se admite**. La agrupación en clústeres de varios SID describe la instalación de varias instancias de SAP ASCS/ERS con SID diferentes en un clúster de Pacemaker.

### <a name="ascs"></a>(A)SCS

* Configuración de front-end
  * Dirección IP 192.168.14.9
* Configuración de back-end
  * Se conecta a interfaces de red principales de todas las máquinas que deben ser parte del clúster (A)SCS/ERS
* Puerto de sondeo
  * Puerto 620<strong>&lt;nr&gt;</strong>
* Reglas de equilibrio de carga
  * 32<strong>&lt;nr&gt;</strong> TCP
  * 36<strong>&lt;nr&gt;</strong> TCP
  * 39<strong>&lt;nr&gt;</strong> TCP
  * 81<strong>&lt;nr&gt;</strong> TCP
  * 5<strong>&lt;nr&gt;</strong>13 TCP
  * 5<strong>&lt;nr&gt;</strong>14 TCP
  * 5<strong>&lt;nr&gt;</strong>16 TCP

### <a name="ers"></a>ERS

* Configuración de front-end
  * Dirección IP 192.168.14.10
* Configuración de back-end
  * Se conecta a interfaces de red principales de todas las máquinas que deben ser parte del clúster (A)SCS/ERS
* Puerto de sondeo
  * Puerto 621<strong>&lt;nr&gt;</strong>
* Reglas de equilibrio de carga
  * 32<strong>&lt;nr&gt;</strong> TCP
  * 33<strong>&lt;nr&gt;</strong> TCP
  * 5<strong>&lt;nr&gt;</strong>13 TCP
  * 5<strong>&lt;nr&gt;</strong>14 TCP
  * 5<strong>&lt;nr&gt;</strong>16 TCP

## <a name="setting-up-the-azure-netapp-files-infrastructure"></a>Configuración de la infraestructura de Azure NetApp Files 

SAP NetWeaver requiere un almacenamiento compartido para el directorio de transporte y perfil.  Antes de continuar con la configuración de la infraestructura de Azure NetApp Files, familiarícese con la [documentación correspondiente][anf-azure-doc]. Compruebe si la región de Azure seleccionada ofrece Azure NetApp Files. El siguiente vínculo muestra la disponibilidad de Azure NetApp Files por región de Azure: [Disponibilidad de Azure NetApp Files por región de Azure][anf-avail-matrix].

Azure NetApp Files está disponible en varias [regiones de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=netapp). Antes de implementar Azure NetApp Files, solicite la incorporación a este siguiendo las [instrucciones para registrarse en Azure NetApp Files][anf-register]. 

### <a name="deploy-azure-netapp-files-resources"></a>Implementación de recursos de Azure NetApp Files  

En estos pasos se supone que ya ha implementado [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). Los recursos de Azure NetApp Files y las máquinas virtuales en las que esos recursos se montarán se deben implementar en la misma red virtual de Azure o en redes virtuales emparejadas.  

1. Si aún no lo ha hecho, solicite la [incorporación a Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register).  
2. Cree la cuenta de NetApp en la región de Azure seleccionada mediante las [instrucciones para crear la cuenta de NetApp](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-netapp-account).  
3. Configure el grupo de capacidad de Azure NetApp Files con las [instrucciones correspondientes](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-set-up-capacity-pool).  
La arquitectura de SAP Netweaver que se presenta en este artículo usa un único grupo de capacidad de Azure NetApp Files, SKU Premium. Se recomienda usar SKU Premium de Azure NetApp Files para cargas de trabajo de aplicaciones de SAP NetWeaver en Azure.  
4. Delegue una subred en Azure NetApp Files como se describe en las [instrucciones correspondientes](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet).  

5. Implemente los volúmenes de Azure NetApp Files mediante las [instrucciones correspondientes](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes). Implemente los volúmenes en la [subred](https://docs.microsoft.com/rest/api/virtualnetwork/subnets) de Azure NetApp Files designada. Tenga en cuenta que los recursos de Azure NetApp Files y las máquinas virtuales de Azure deben estar en la misma red virtual de Azure o en redes virtuales emparejadas. En este ejemplo se usan dos volúmenes de Azure NetApp Files: sap<b>QAS</b> y transSAP. Las rutas de acceso de los archivos que se montan en los correspondientes puntos de montaje son /usrsap<b>qas</b>/sapmnt<b>QAS</b>, /usrsap<b>qas</b>/usrsap<b>QAS</b>sys, etc.  

   1. volumen sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/sapmnt<b>QAS</b>)
   2. volumen sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>ascs)
   3. volumen sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>sys)
   4. volumen sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>ers)
   5. volumen transSAP (nfs://192.168.24.4/transSAP)
   6. volumen sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>pas)
   7. volumen sap<b>QAS</b> (nfs://192.168.24.5/usrsap<b>qas</b>/usrsap<b>QAS</b>aas)
  
En este ejemplo, hemos usado Azure NetApp Files para todos los sistemas de archivos de SAP Netweaver para demostrar cómo se puede usar Azure NetApp Files. Los sistemas de archivos SAP que no es necesario montar mediante NFS se pueden también implementar en [Azure Disk Storage](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd). En este ejemplo, <b>a-e</b> deben estar en Azure NetApp Files y <b>f-g</b> (es decir, /usr/sap/<b>QAS</b>/D<b>02</b>, /usr/sap/<b>QAS</b>/D<b>03</b>) se podrían implementar en Azure Disk Storage. 

### <a name="important-considerations"></a>Consideraciones importantes

A la hora de considerar Azure NetApp Files para SAP Netweaver en la arquitectura de alta disponibilidad de SUSE, tenga en cuenta los siguientes aspectos importantes:

- El grupo de capacidad mínimo es de 4 TiB. El tamaño del grupo de capacidad debe ser un múltiplo de 4 TiB.
- El volumen mínimo es de 100 GiB
- Azure NetApp Files y todas las máquinas virtuales en las que los volúmenes de Azure NetApp Files se montarán, se deben implementar en la misma red virtual de Azure o en [redes virtuales emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) de la misma región. Ahora también se admite el acceso de Azure NetApp Files mediante emparejamiento de red virtual en la misma región. Aún no se admite el acceso de Azure NetApp a través del emparejamiento global.
- La red virtual seleccionada debe tener una subred delegada en Azure NetApp Files.
- Azure NetApp Files solo admite NFSv3 actualmente. 
- Azure NetApp Files ofrece la [directiva de exportación](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy): puede controlar los clientes permitidos, el tipo de acceso (lectura y escritura, solo lectura, etc.). 
- La característica Azure NetApp Files no depende aún de la zona. En la actualidad, la característica Azure NetApp Files no se implementa en todas las zonas de disponibilidad de una región de Azure. Tenga en cuenta las posibles implicaciones de latencia en algunas regiones de Azure. 

## <a name="setting-up-ascs"></a>Configuración de (A)SCS

En este ejemplo, los recursos se implementaron manualmente mediante [Azure Portal](https://portal.azure.com/#home).

### <a name="deploy-linux-manually-via-azure-portal"></a>Implementación manual de Linux mediante Azure Portal

Primero deberá crear los volúmenes de Azure NetApp Files. Implementar las VM. Después, creará un equilibrador de carga y usará las máquinas virtuales de los grupos de servidores back-end.

1. Creación de un equilibrador de carga (interno)  
   1. Creación de las direcciones IP de front-end
      1. Dirección IP 192.168.14.9 de ASCS
         1. Abra el equilibrador de carga, seleccione el grupo de direcciones IP de front-end y haga clic en Agregar
         1. Escriba el nombre del nuevo grupo de direcciones IP de front-end (por ejemplo, **frontend.QAS.ASCS**).
         1. Establezca Asignación en Estática y escriba la dirección IP (por ejemplo, **192.168.14.9**).
         1. Haga clic en Aceptar
      1. Dirección IP 192.168.14.10 para ASCS ERS
         * Repita los pasos anteriores a partir de "a" para crear una dirección IP para el ERS (por ejemplo, **192.168.14.10** y **frontend.QAS.ERS**).
   1. Creación de los grupos de servidores back-end
      1. Creación de un grupo de servidores back-end para ASCS
         1. Abra el equilibrador de carga, seleccione los grupos de back-end y haga clic en Agregar
         1. Escriba el nombre del nuevo grupo de servidores back-end (por ejemplo, **backend.QAS**).
         1. Haga clic en Agregar una máquina virtual.
         1. Seleccione el conjunto de disponibilidad que creó anteriormente para ASCS 
         1. Seleccione las máquinas virtuales del clúster (A)SCS.
         1. Haga clic en Aceptar
   1. Creación de los sondeos de estado
      1. Puerto 620**00** para ASCS
         1. Abra el equilibrador de carga, seleccione los sondeos de estado y haga clic en Agregar
         1. Escriba el nombre del sondeo de estado nuevo (por ejemplo **health.QAS.ASCS**).
         1. Seleccione TCP como protocolo, puerto 620**00**, y mantenga el intervalo de 5 y el umbral incorrecto 2.
         1. Haga clic en Aceptar
      1. Puerto 621**01** para ASCS ERS
            * Repita los pasos anteriores a partir de "c" para crear un sondeo de estado para ERS (por ejemplo 621**01** y **health.QAS.ERS**).
   1. Reglas de equilibrio de carga
      1. TCP 32**00** para ASCS
         1. Abra el equilibrador de carga, seleccione las reglas de equilibrio de carga y haga clic en Agregar.
         1. Escriba el nombre de la nueva regla del equilibrador de carga (por ejemplo, **lb.QAS.ASCS.3200**).
         1. Seleccione la dirección IP de front-end para ASCS, el grupo de servidores back-end y el sondeo de estado que creó anteriormente (por ejemplo, **frontend.QAS.ASCS**).
         1. Conserve el protocolo **TCP** y escriba el puerto **3200**.
         1. Aumente el tiempo de espera de inactividad a 30 minutos
         1. **Asegúrese de habilitar la dirección IP flotante**
         1. Haga clic en Aceptar
      1. Puertos adicionales para ASCS
         * Repita los pasos anteriores para los puertos 36**00**, 39**00**, 81**00**, 5**00**13, 5**00**14, 5**00**16 y TCP para ASCS
      1. Puertos adicionales para ASCS ERS
         * Repita los pasos anteriores a partir de "d" para los puertos 32**01**, 33**01**, 5**01**13, 5**01**14, 5**01**16 y TCP para ASCS ERS


> [!IMPORTANT]
> No habilite las marcas de tiempo TCP en VM de Azure que se encuentren detrás de Azure Load Balancer. Si habilita las marcas de tiempo TCP provocará un error en los sondeos de estado. Establezca el parámetro **net.ipv4.tcp_timestamps** a **0**. Lea [Sondeos de estado de Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview) para obtener más información.

### <a name="create-pacemaker-cluster"></a>Creación del clúster de Pacemaker

Siga los pasos de [Configuración de Pacemaker en Red Hat Enterprise Linux en Azure](high-availability-guide-rhel-pacemaker.md) para crear un clúster de Pacemaker básico para este servidor (A)SCS.

### <a name="prepare-for-sap-netweaver-installation"></a>Preparación de la instalación de SAP NetWeaver

Los elementos siguientes tienen el prefijo **[A]** : aplicable a todos los nodos, **[1]** : aplicable solo al nodo 1 o **[2]** : aplicable solo al nodo 2.

1. **[A]** Configure la resolución nombres de host

   Puede usar un servidor DNS o modificar /etc/hosts en todos los nodos. En este ejemplo se muestra cómo utilizar el archivo /etc/hosts.
   Reemplace la dirección IP y el nombre de host en los siguientes comandos

    ```
    sudo vi /etc/hosts
    ```

   Inserte las siguientes líneas en /etc/hosts. Cambie la dirección IP y el nombre de host para que coincida con su entorno

    ```
    # IP address of cluster node 1
    192.168.14.5    anftstsapcl1
    # IP address of cluster node 2
    192.168.14.6     anftstsapcl2
    # IP address of the load balancer frontend configuration for SAP Netweaver ASCS
    192.168.14.9    anftstsapvh
    # IP address of the load balancer frontend configuration for SAP Netweaver ERS
    192.168.14.10    anftstsapers
    ```

1. **[1]**  Cree directorios de SAP en el volumen de Azure NetApp Files.  
   Monte el volumen de Azure NetApp Files temporalmente en una de las máquinas virtuales y cree los directorios de SAP (rutas de acceso de archivos).  

    ```
     #mount temporarily the volume
     sudo mkdir -p /saptmp
     sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=3,tcp 192.168.24.5:/sapQAS /saptmp
     # create the SAP directories
     sudo cd /saptmp
     sudo mkdir -p sapmntQAS
     sudo mkdir -p usrsapQASascs
     sudo mkdir -p usrsapQASers
     sudo mkdir -p usrsapQASsys
     sudo mkdir -p usrsapQASpas
     sudo mkdir -p usrsapQASaas
     # unmount the volume and delete the temporary directory
     sudo cd ..
     sudo umount /saptmp
     sudo rmdir /saptmp

1. **[A]** Create the shared directories

   ```
   sudo mkdir -p /sapmnt/QAS sudo mkdir -p /usr/sap/trans sudo mkdir -p /usr/sap/QAS/SYS sudo mkdir -p /usr/sap/QAS/ASCS00 sudo mkdir -p /usr/sap/QAS/ERS01
   
   sudo chattr +i /sapmnt/QAS sudo chattr +i /usr/sap/trans sudo chattr +i /usr/sap/QAS/SYS sudo chattr +i /usr/sap/QAS/ASCS00 sudo chattr +i /usr/sap/QAS/ERS01
   ```

1. **[A]** Install NFS client and other requirements

   ```
   sudo yum -y install nfs-utils resource-agents resource-agents-sap
   ```

1. **[A]** Check version of resource-agents-sap

   Make sure that the version of the installed resource-agents-sap package is at least 3.9.5-124.el7
   ```
   sudo yum info resource-agents-sap
   
   # <a name="loaded-plugins-langpacks-product-id-search-disabled-repos"></a>Loaded plugins: langpacks, product-id, search-disabled-repos
   # <a name="repodata-is-over-2-weeks-old-install-yum-cron-or-run-yum-makecache-fast"></a>Repodata is over 2 weeks old. Install yum-cron? Or run: yum makecache fast
   # <a name="installed-packages"></a>Installed Packages
   # <a name="name---------resource-agents-sap"></a>Name        : resource-agents-sap
   # <a name="arch---------x8664"></a>Arch        : x86_64
   # <a name="version------395"></a>Version     : 3.9.5
   # <a name="release------124el7"></a>Release     : 124.el7
   # <a name="size---------100-k"></a>Size        : 100 k
   # <a name="repo---------installed"></a>Repo        : installed
   # <a name="from-repo----rhel-sap-for-rhel-7-server-rpms"></a>From repo   : rhel-sap-for-rhel-7-server-rpms
   # <a name="summary------sap-cluster-resource-agents-and-connector-script"></a>Summary     : SAP cluster resource agents and connector script
   # <a name="url----------httpsgithubcomclusterlabsresource-agents"></a>URL         : https://github.com/ClusterLabs/resource-agents
   # <a name="license------gplv2"></a>License     : GPLv2+
   # <a name="description--the-sap-resource-agents-and-connector-script-interface-with"></a>Description : The SAP resource agents and connector script interface with
   #          <a name="-pacemaker-to-allow-sap-instances-to-be-managed-in-a-cluster"></a>: Pacemaker to allow SAP instances to be managed in a cluster
   #          <a name="-environment"></a>: environment.
   ```


1. **[A]** Add mount entries

   ```
   sudo vi /etc/fstab
   
   # <a name="add-the-following-lines-to-fstab-save-and-exit"></a>Add the following lines to fstab, save and exit
    192.168.24.5:/sapQAS/sapmntQAS /sapmnt/QAS nfs rw,hard,rsize=65536,wsize=65536,vers=3  192.168.24.5:/sapQAS/usrsapQASsys /usr/sap/QAS/SYS nfs rw,hard,rsize=65536,wsize=65536,vers=3  192.168.24.4:/transSAP /usr/sap/trans nfs rw,hard,rsize=65536,wsize=65536,vers=3
   ```

   Mount the new shares

   ```
   sudo mount -a  
   ```

1. **[A]** Configure SWAP file

   ```
   sudo vi /etc/waagent.conf
   
   # <a name="set-the-property-resourcediskenableswap-to-y"></a>Set the property ResourceDisk.EnableSwap to y
   # <a name="create-and-use-swapfile-on-resource-disk"></a>Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=y
   
   # <a name="set-the-size-of-the-swap-file-with-property-resourcediskswapsizemb"></a>Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # <a name="the-free-space-of-resource-disk-varies-by-virtual-machine-size-make-sure-that-you-do-not-set-a-value-that-is-too-big-you-can-check-the-swap-space-with-command-swapon"></a>The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # <a name="size-of-the-swapfile"></a>Size of the swapfile.
   ResourceDisk.SwapSizeMB=2000
   ```

   Restart the Agent to activate the change

   ```
   sudo service waagent restart
   ```

1. **[A]** RHEL configuration

   Configure RHEL as described in SAP Note [2002167]

### Installing SAP NetWeaver ASCS/ERS

1. **[1]** Create a virtual IP resource and health-probe for the ASCS instance

   ```
   sudo pcs node standby anftstsapcl2
   
   sudo pcs resource create fs_QAS_ASCS Filesystem device='192.168.24.5:/sapQAS/usrsapQASascs' \
     directory='/usr/sap/QAS/ASCS00' fstype='nfs' \
     --group g-QAS_ASCS
   
   sudo pcs resource create vip_QAS_ASCS IPaddr2 \
     ip=192.168.14.9 cidr_netmask=24 \
     --group g-QAS_ASCS
   
   sudo pcs resource create nc_QAS_ASCS azure-lb port=62000 \
     --group g-QAS_ASCS
   ```

   Make sure that the cluster status is ok and that all resources are started. It is not important on which node the resources are running.

   ```
   sudo pcs status
   
   # <a name="node-anftstsapcl2-standby"></a>Node anftstsapcl2: standby
   # <a name="online--anftstsapcl1-"></a>Online: [ anftstsapcl1 ]
   #
   # <a name="full-list-of-resources"></a>Full list of resources:
   #
   # <a name="rscstazure----stonithfenceazurearm------started-anftstsapcl1"></a>rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1
   #  <a name="resource-group-g-qasascs"></a>Resource Group: g-QAS_ASCS
   #      <a name="fsqasascs--------ocfheartbeatfilesystem----started-anftstsapcl1"></a>fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
   #      <a name="ncqasascs--------ocfheartbeatazure-lb------started-anftstsapcl1"></a>nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
   #      <a name="vipqasascs-------ocfheartbeatipaddr2-------started-anftstsapcl1"></a>vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
   ```

1. **[1]** Install SAP NetWeaver ASCS  

   Install SAP NetWeaver ASCS as root on the first node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ASCS, for example <b>anftstsapvh</b>, <b>192.168.14.9</b> and the instance number that you used for the probe of the load balancer, for example <b>00</b>.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   # <a name="allow-access-to-swpm-this-rule-is-not-permanent-if-you-reboot-the-machine-you-have-to-run-the-command-again"></a>Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again.
   sudo firewall-cmd --zone=public  --add-port=4237/tcp
   
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin SAPINST_USE_HOSTNAME=<virtual_hostname>
   ```

   If the installation fails to create a subfolder in /usr/sap/**QAS**/ASCS**00**, try setting the owner and group of the ASCS**00** folder and retry.

   ```
   sudo chown qasadm /usr/sap/QAS/ASCS00 sudo chgrp sapsys /usr/sap/QAS/ASCS00
   ```

1. **[1]** Create a virtual IP resource and health-probe for the ERS instance

   ```
   sudo pcs node unstandby anftstsapcl2 sudo pcs node standby anftstsapcl1
   
   sudo pcs resource create fs_QAS_AERS Filesystem device='192.168.24.5:/sapQAS/usrsapQASers' \
     directory='/usr/sap/QAS/ERS01' fstype='nfs' \
    --group g-QAS_AERS

   sudo pcs resource create vip_QAS_AERS IPaddr2 \
     ip=192.168.14.10 cidr_netmask=24 \
    --group g-QAS_AERS
   
   sudo pcs resource create nc_QAS_AERS azure-lb port=62101 \
    --group g-QAS_AERS
   ```
 
   Make sure that the cluster status is ok and that all resources are started. It is not important on which node the resources are running.

   ```
   sudo pcs status
   
   # <a name="node-anftstsapcl1-standby"></a>Node anftstsapcl1: standby
   # <a name="online--anftstsapcl2-"></a>Online: [ anftstsapcl2 ]
   #
   # <a name="full-list-of-resources"></a>Full list of resources:
   #
   # <a name="rscstazure----stonithfenceazurearm------started-anftstsapcl2"></a>rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl2
   #  <a name="resource-group-g-qasascs"></a>Resource Group: g-QAS_ASCS
   #      <a name="fsqasascs--------ocfheartbeatfilesystem----started-anftstsapcl2"></a>fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
   #      <a name="ncqasascs--------ocfheartbeatazure-lb------started-anftstsapcl2"></a>nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2<
   #      <a name="vipqasascs-------ocfheartbeatipaddr2-------started-anftstsapcl2"></a>vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
   #  <a name="resource-group-g-qasaers"></a>Resource Group: g-QAS_AERS
   #      <a name="fsqasaers--------ocfheartbeatfilesystem----started-anftstsapcl2"></a>fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
   #      <a name="ncqasaers--------ocfheartbeatazure-lb------started-anftstsapcl2"></a>nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
   #      <a name="vipqasaers-------ocfheartbeatipaddr2-------started-anftstsapcl2"></a>vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
   ```

1. **[2]** Install SAP NetWeaver ERS  

   Install SAP NetWeaver ERS as root on the second node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ERS, for example <b>anftstsapers</b>, <b>192.168.14.10</b> and the instance number that you used for the probe of the load balancer, for example <b>01</b>.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   # <a name="allow-access-to-swpm-this-rule-is-not-permanent-if-you-reboot-the-machine-you-have-to-run-the-command-again"></a>Allow access to SWPM. This rule is not permanent. If you reboot the machine, you have to run the command again.
   sudo firewall-cmd --zone=public  --add-port=4237/tcp

   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin SAPINST_USE_HOSTNAME=<virtual_hostname>
   ```

   If the installation fails to create a subfolder in /usr/sap/**QAS**/ERS**01**, try setting the owner and group of the ERS**01** folder and retry.

   ```
   sudo chown qaadm /usr/sap/QAS/ERS01 sudo chgrp sapsys /usr/sap/QAS/ERS01
   ```

1. **[1]** Adapt the ASCS/SCS and ERS instance profiles

   * ASCS/SCS profile

   ```
   sudo vi /sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh
   
   # <a name="change-the-restart-command-to-a-start-command"></a>Change the restart command to a start command
   #<a name="restartprogram01--local-en-pfpf"></a>Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)
   
   # <a name="add-the-keep-alive-parameter"></a>Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   ```

   * ERS profile

   ```
   sudo vi /sapmnt/QAS/profile/QAS_ERS01_anftstsapers
   
   # <a name="change-the-restart-command-to-a-start-command"></a>Change the restart command to a start command
   #<a name="restartprogram00--local-er-pfpfl-nrscsid"></a>Restart_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   Start_Program_00 = local $(_ER) pf=$(_PFL) NR=$(SCSID)
   
   # <a name="remove-autostart-from-ers-profile"></a>remove Autostart from ERS profile
   # <a name="autostart--1"></a>Autostart = 1
   ```


1. **[A]** Configure Keep Alive

   The communication between the SAP NetWeaver application server and the ASCS/SCS is routed through a software load balancer. The load balancer disconnects inactive connections after a configurable timeout. To prevent this, you need to set a parameter in the SAP NetWeaver ASCS/SCS profile and change the Linux system settings. Read [SAP Note 1410736][1410736] for more information.

   The ASCS/SCS profile parameter enque/encni/set_so_keepalive was already added in the last step.

   ```
   # <a name="change-the-linux-system-configuration"></a>Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   ```

1. **[A]** Update the /usr/sap/sapservices file

   To prevent the start of the instances by the sapinit startup script, all instances managed by Pacemaker must be commented out from /usr/sap/sapservices file. Do not comment out the SAP HANA instance if it will be used with HANA SR.

   ```
   sudo vi /usr/sap/sapservices
   
   # <a name="on-the-node-where-you-installed-the-ascs-comment-out-the-following-line"></a>On the node where you installed the ASCS, comment out the following line
   # <a name="ldlibrarypathusrsapqasascs00exeldlibrarypath-export-ldlibrarypath-usrsapqasascs00exesapstartsrv-pfusrsapqassysprofileqasascs00anftstsapvh--d--u-qasadm"></a>LD_LIBRARY_PATH=/usr/sap/QAS/ASCS00/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/QAS/ASCS00/exe/sapstartsrv pf=/usr/sap/QAS/SYS/profile/QAS_ASCS00_anftstsapvh -D -u qasadm
   
   # <a name="on-the-node-where-you-installed-the-ers-comment-out-the-following-line"></a>On the node where you installed the ERS, comment out the following line
   # <a name="ldlibrarypathusrsapqasers01exeldlibrarypath-export-ldlibrarypath-usrsapqasers01exesapstartsrv-pfusrsapqasers01profileqasers01anftstsapers--d--u-qasadm"></a>LD_LIBRARY_PATH=/usr/sap/QAS/ERS01/exe:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH; /usr/sap/QAS/ERS01/exe/sapstartsrv pf=/usr/sap/QAS/ERS01/profile/QAS_ERS01_anftstsapers -D -u qasadm
   ```

1. **[1]** Create the SAP cluster resources

   If using enqueue server 1 architecture (ENSA1), define the resources as follows:

   ```
   sudo pcs property set maintenance-mode=true
   
    sudo pcs resource create rsc_sap_QAS_ASCS00 SAPInstance \
    InstanceName=QAS_ASCS00_anftstsapvh START_PROFILE="/sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 migration-threshold=1 \
    --group g-QAS_ASCS
   
    sudo pcs resource create rsc_sap_QAS_ERS01 SAPInstance \
    InstanceName=QAS_ERS01_anftstsapers START_PROFILE="/sapmnt/QAS/profile/QAS_ERS01_anftstsapers" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    --group g-QAS_AERS
      
    sudo pcs constraint colocation add g-QAS_AERS with g-QAS_ASCS -5000  sudo pcs constraint location rsc_sap_QAS_ASCS00 rule score=2000 runs_ers_QAS eq 1  sudo pcs constraint order g-QAS_ASCS then g-QAS_AERS kind=Optional symmetrical=false
    
    sudo pcs node unstandby anftstsapcl1  sudo pcs property set maintenance-mode=false
    ```

   SAP introduced support for enqueue server 2, including replication, as of SAP NW 7.52. Starting with ABAP Platform 1809, enqueue server 2 is installed by default. See SAP note [2630416](https://launchpad.support.sap.com/#/notes/2630416) for enqueue server 2 support.
   If using enqueue server 2 architecture ([ENSA2](https://help.sap.com/viewer/cff8531bc1d9416d91bb6781e628d4e0/1709%20001/en-US/6d655c383abf4c129b0e5c8683e7ecd8.html)), install resource agent resource-agents-sap-4.1.1-12.el7.x86_64 or newer and define the resources as follows:

    ```
    sudo pcs property set maintenance-mode=true
    
    sudo pcs resource create rsc_sap_QAS_ASCS00 SAPInstance \
    InstanceName=QAS_ASCS00_anftstsapvh START_PROFILE="/sapmnt/QAS/profile/QAS_ASCS00_anftstsapvh" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 \
    --group g-QAS_ASCS
   
    sudo pcs resource create rsc_sap_QAS_ERS01 SAPInstance \
    InstanceName=QAS_ERS01_anftstsapers START_PROFILE="/sapmnt/QAS/profile/QAS_ERS01_anftstsapers" \
    AUTOMATIC_RECOVER=false IS_ERS=true \
    --group g-QAS_AERS
      
    sudo pcs constraint colocation add g-QAS_AERS with g-QAS_ASCS -5000  sudo pcs constraint order g-QAS_ASCS then g-QAS_AERS kind=Optional symmetrical=false
   
    sudo pcs node unstandby anftstsapcl1  sudo pcs property set maintenance-mode=false
    ```

   If you are upgrading from an older version and switching to enqueue server 2, see SAP note [2641322](https://launchpad.support.sap.com/#/notes/2641322). 

   Make sure that the cluster status is ok and that all resources are started. It is not important on which node the resources are running.

    ```
    sudo pcs status
    
    # <a name="online--anftstsapcl1-anftstsapcl2-"></a>Online: [ anftstsapcl1 anftstsapcl2 ]
    #
    # <a name="full-list-of-resources"></a>Full list of resources:
    #
    # <a name="rscstazure----stonithfenceazurearm------started-anftstsapcl2"></a>rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl2
    #  <a name="resource-group-g-qasascs"></a>Resource Group: g-QAS_ASCS
    #      <a name="fsqasascs--------ocfheartbeatfilesystem----started-anftstsapcl2"></a>fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2
    #      <a name="ncqasascs--------ocfheartbeatazure-lb------started-anftstsapcl2"></a>nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2
    #      <a name="vipqasascs-------ocfheartbeatipaddr2-------started-anftstsapcl2"></a>vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2
    #      <a name="rscsapqasascs00-ocfheartbeatsapinstance---started-anftstsapcl2"></a>rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
    #  <a name="resource-group-g-qasaers"></a>Resource Group: g-QAS_AERS
    #      <a name="fsqasaers--------ocfheartbeatfilesystem----started-anftstsapcl1"></a>fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1
    #      <a name="ncqasaers--------ocfheartbeatazure-lb------started-anftstsapcl1"></a>nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1
    #      <a name="vipqasaers-------ocfheartbeatipaddr2-------started-anftstsapcl1"></a>vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1
    #      <a name="rscsapqasers01--ocfheartbeatsapinstance---started-anftstsapcl1"></a>rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   ```

1. **[A]** Add firewall rules for ASCS and ERS on both nodes
   Add the firewall rules for ASCS and ERS on both nodes.
   ```
   # <a name="probe-port-of-ascs"></a>Probe Port of ASCS
   sudo firewall-cmd --zone=public --add-port=62000/tcp --permanent sudo firewall-cmd --zone=public --add-port=62000/tcp sudo firewall-cmd --zone=public --add-port=3200/tcp --permanent sudo firewall-cmd --zone=public --add-port=3200/tcp sudo firewall-cmd --zone=public --add-port=3600/tcp --permanent sudo firewall-cmd --zone=public --add-port=3600/tcp sudo firewall-cmd --zone=public --add-port=3900/tcp --permanent sudo firewall-cmd --zone=public --add-port=3900/tcp sudo firewall-cmd --zone=public --add-port=8100/tcp --permanent sudo firewall-cmd --zone=public --add-port=8100/tcp sudo firewall-cmd --zone=public --add-port=50013/tcp --permanent sudo firewall-cmd --zone=public --add-port=50013/tcp sudo firewall-cmd --zone=public --add-port=50014/tcp --permanent sudo firewall-cmd --zone=public --add-port=50014/tcp sudo firewall-cmd --zone=public --add-port=50016/tcp --permanent sudo firewall-cmd --zone=public --add-port=50016/tcp
   # <a name="probe-port-of-ers"></a>Probe Port of ERS
   sudo firewall-cmd --zone=public --add-port=62101/tcp --permanent sudo firewall-cmd --zone=public --add-port=62101/tcp sudo firewall-cmd --zone=public --add-port=3301/tcp --permanent sudo firewall-cmd --zone=public --add-port=3301/tcp sudo firewall-cmd --zone=public --add-port=50113/tcp --permanent sudo firewall-cmd --zone=public --add-port=50113/tcp sudo firewall-cmd --zone=public --add-port=50114/tcp --permanent sudo firewall-cmd --zone=public --add-port=50114/tcp sudo firewall-cmd --zone=public --add-port=50116/tcp --permanent sudo firewall-cmd --zone=public --add-port=50116/tcp
   ```

## <a name="2d6008b0-685d-426c-b59e-6cd281fd45d7"></a>SAP NetWeaver application server preparation

   Some databases require that the database instance installation is executed on an application server. Prepare the application server virtual machines to be able to use them in these cases.  

   The steps bellow assume that you install the application server on a server different from the ASCS/SCS and HANA servers. Otherwise some of the steps below (like configuring host name resolution) are not needed.  

   The following items are prefixed with either **[A]** - applicable to both PAS and AAS, **[P]** - only applicable to PAS or **[S]** - only applicable to AAS.  

1. **[A]** Setup host name resolution
   You can either use a DNS server or modify the /etc/hosts on all nodes. This example shows how to use the /etc/hosts file.
   Replace the IP address and the hostname in the following commands:  

   ```
   sudo vi /etc/hosts
   ```

   Insert the following lines to /etc/hosts. Change the IP address and hostname to match your environment.

   ```
   # <a name="ip-address-of-the-load-balancer-frontend-configuration-for-sap-netweaver-ascs"></a>IP address of the load balancer frontend configuration for SAP NetWeaver ASCS
   192.168.14.9 anftstsapvh
   # <a name="ip-address-of-the-load-balancer-frontend-configuration-for-sap-netweaver-ascs-ers"></a>IP address of the load balancer frontend configuration for SAP NetWeaver ASCS ERS
   192.168.14.10 anftstsapers 192.168.14.7 anftstsapa01 192.168.14.8 anftstsapa02
   ```

1. **[A]** Create the sapmnt directory
   Create the sapmnt directory.
   ```
   sudo mkdir -p /sapmnt/QAS sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/QAS sudo chattr +i /usr/sap/trans
   ```

1. **[A]** Install NFS client and other requirements  

   ```
   sudo yum -y install nfs-utils uuidd
   ```

1. **[A]** Add mount entries  

   ```
   sudo vi /etc/fstab
   
   # <a name="add-the-following-lines-to-fstab-save-and-exit"></a>Add the following lines to fstab, save and exit
   192.168.24.5:/sapQAS/sapmntQAS /sapmnt/QAS nfs rw,hard,rsize=65536,wsize=65536,vers=3 192.168.24.4:/transSAP /usr/sap/trans nfs rw,hard,rsize=65536,wsize=65536,vers=3
   ```

   Mount the new shares

   ```
   sudo mount -a
   ```

1. **[P]** Create and mount the PAS directory  

   ```
   sudo mkdir -p /usr/sap/QAS/D02 sudo chattr +i /usr/sap/QAS/D02
   
   sudo vi /etc/fstab
   # <a name="add-the-following-line-to-fstab"></a>Add the following line to fstab
   92.168.24.5:/sapQAS/usrsapQASpas /usr/sap/QAS/D02 nfs rw,hard,rsize=65536,wsize=65536,vers=3
   
   # <a name="mount"></a>Montaje
   sudo mount -a
   ```

1. **[S]** Create and mount the AAS directory  

   ```
   sudo mkdir -p /usr/sap/QAS/D03 sudo chattr +i /usr/sap/QAS/D03
   
   sudo vi /etc/fstab
   # <a name="add-the-following-line-to-fstab"></a>Add the following line to fstab
   92.168.24.5:/sapQAS/usrsapQASaas /usr/sap/QAS/D03 nfs rw,hard,rsize=65536,wsize=65536,vers=3
   
   # <a name="mount"></a>Montaje
   sudo mount -a
   ```


1. **[A]** Configure SWAP file
 
   ```
   sudo vi /etc/waagent.conf
   
   # <a name="set-the-property-resourcediskenableswap-to-y"></a>Set the property ResourceDisk.EnableSwap to y
   # <a name="create-and-use-swapfile-on-resource-disk"></a>Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=y
   
   # <a name="set-the-size-of-the-swap-file-with-property-resourcediskswapsizemb"></a>Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # <a name="the-free-space-of-resource-disk-varies-by-virtual-machine-size-make-sure-that-you-do-not-set-a-value-that-is-too-big-you-can-check-the-swap-space-with-command-swapon"></a>The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # <a name="size-of-the-swapfile"></a>Size of the swapfile.
   ResourceDisk.SwapSizeMB=2000
   ```

   Restart the Agent to activate the change

   ```
   sudo service waagent restart
   ```

## Install database

In this example, SAP NetWeaver is installed on SAP HANA. You can use every supported database for this installation. For more information on how to install SAP HANA in Azure, see [High availability of SAP HANA on Azure VMs on Red Hat Enterprise Linux][sap-hana-ha]. For a list of supported databases, see [SAP Note 1928533][1928533].

1. Run the SAP database instance installation

   Install the SAP NetWeaver database instance as root using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the database.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin
   ```

## SAP NetWeaver application server installation

Follow these steps to install an SAP application server.

1. Prepare application server

   Follow the steps in the chapter [SAP NetWeaver application server preparation](high-availability-guide-rhel.md#2d6008b0-685d-426c-b59e-6cd281fd45d7) above to prepare the application server.

1. Install SAP NetWeaver application server

   Install a primary or additional SAP NetWeaver applications server.

   You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.

   ```
   sudo <swpm>/sapinst SAPINST_REMOTE_ACCESS_USER=sapadmin
   ```

1. Update SAP HANA secure store

   Update the SAP HANA secure store to point to the virtual name of the SAP HANA System Replication setup.

   Run the following command to list the entries as \<sapsid>adm

   ```
   hdbuserstore List
   ```

   This should list all entries and should look similar to
   ```
   DATA FILE       : /home/qasadm/.hdb/anftstsapa01/SSFS_HDB.DAT KEY FILE        : /home/qasadm/.hdb/anftstsapa01/SSFS_HDB.KEY
   
   KEY DEFAULT   ENV : 192.168.14.4:30313   USER: SAPABAP1   DATABASE: QAS
   ```

   The output shows that the IP address of the default entry is pointing to the virtual machine and not to the load balancer's IP address. This entry needs to be changed to point to the virtual hostname of the load balancer. Make sure to use the same port (**30313** in the output above) and database name (**QAS** in the output above)!

   ```
   su - qasadm hdbuserstore SET DEFAULT qasdb:30313@QAS SAPABAP1 <password of ABAP schema>
   ```

## Test the cluster setup

1. Manually migrate the ASCS instance

   Resource state before starting the test:

   ```
    rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

   Run the following commands as root to migrate the ASCS instance.

   ```
   [root@anftstsapcl1 ~]# pcs resource move rsc_sap_QAS_ASCS00
   
   [root@anftstsapcl1 ~]# pcs resource clear rsc_sap_QAS_ASCS00
   
   # <a name="remove-failed-actions-for-the-ers-that-occurred-as-part-of-the-migration"></a>Remove failed actions for the ERS that occurred as part of the migration
   [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   ```

1. Simulate node crash

   Resource state before starting the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   ```

   Run the following command as root on the node where the ASCS instance is running

   ```
   [root@anftstsapcl2 ~]# echo b > /proc/sysrq-trigger
   ```

   The status after the node is started again should look like this.

   ```
   Online: [ anftstsapcl1 anftstsapcl2 ]
   
   Full list of resources:
   
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   
   Failed Actions:
   * rsc_sap_QAS_ERS01_monitor_11000 on anftstsapcl1 'not running' (7): call=45, status=complete, exitreason='',
   ```

   Use the following command to clean the failed resources.

   ```
   [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

1. Kill message server process

   Resource state before starting the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```
   
   Run the following commands as root to identify the process of the message server and kill it.

   ```
   [root@anftstsapcl1 ~]# pgrep ms.sapQAS | xargs kill -9
   ```

   If you only kill the message server once, it will be restarted by `sapstart`. If you kill it often enough, Pacemaker will eventually move the ASCS instance to the other node. Run the following commands as root to clean up the resource state of the ASCS and ERS instance after the test.

   ```
   [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ASCS00 [root@anftstsapcl1 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   ```

1. Kill enqueue server process

   Resource state before starting the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl2  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl1
   ```

   Run the following commands as root on the node where the ASCS instance is running to kill the enqueue server.

   ```
   [root@anftstsapcl2 ~]# pgrep en.sapQAS | xargs kill -9
   ```

   The ASCS instance should immediately fail over to the other node. The ERS instance should also fail over after the ASCS instance is started. Run the following commands as root to clean up the resource state of the ASCS and ERS instance after the test.

   ```
   [root@anftstsapcl2 ~]# pcs resource cleanup rsc_sap_QAS_ASCS00 [root@anftstsapcl2 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

1. Kill enqueue replication server process

   Resource state before starting the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

   Run the following command as root on the node where the ERS instance is running to kill the enqueue replication server process.

   ```
   [root@anftstsapcl2 ~]# pgrep er.sapQAS | xargs kill -9
   ```

   If you only run the command once, `sapstart` will restart the process. If you run it often enough, `sapstart` will not restart the process and the resource will be in a stopped state. Run the following commands as root to clean up the resource state of the ERS instance after the test.

   ```
   [root@anftstsapcl2 ~]# pcs resource cleanup rsc_sap_QAS_ERS01
   ```

   Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

1. Kill enqueue sapstartsrv process

   Resource state before starting the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

   Run the following commands as root on the node where the ASCS is running.

   ```
   [root@anftstsapcl1 ~]# pgrep -fl ASCS00.*sapstartsrv
   # <a name="59545-sapstartsrv"></a>59545 sapstartsrv
   
   [root@anftstsapcl1 ~]# kill -9 59545
   ```

   The sapstartsrv process should always be restarted by the Pacemaker resource agent as part of the monitoring. Resource state after the test:

   ```
   rsc_st_azure    (stonith:fence_azure_arm):      Started anftstsapcl1  Resource Group: g-QAS_ASCS      fs_QAS_ASCS        (ocf::heartbeat:Filesystem):    Started anftstsapcl1      nc_QAS_ASCS        (ocf::heartbeat:azure-lb):      Started anftstsapcl1      vip_QAS_ASCS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl1      rsc_sap_QAS_ASCS00 (ocf::heartbeat:SAPInstance):   Started anftstsapcl1  Resource Group: g-QAS_AERS      fs_QAS_AERS        (ocf::heartbeat:Filesystem):    Started anftstsapcl2      nc_QAS_AERS        (ocf::heartbeat:azure-lb):      Started anftstsapcl2      vip_QAS_AERS       (ocf::heartbeat:IPaddr2):       Started anftstsapcl2      rsc_sap_QAS_ERS01  (ocf::heartbeat:SAPInstance):   Started anftstsapcl2
   ```

## Next steps

* [Azure Virtual Machines planning and implementation for SAP][planning-guide]
* [Azure Virtual Machines deployment for SAP][deployment-guide]
* [Azure Virtual Machines DBMS deployment for SAP][dbms-guide]
* To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).
* To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure VMs, see [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha]

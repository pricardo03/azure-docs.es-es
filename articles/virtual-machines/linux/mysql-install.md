---
title: Configuración de MySQL en una VM de Linux en Azure | Microsoft Docs
description: Aprenda a instalar la pila de MySQL en una máquina virtual Linux (sistema operativo de la familia Ubuntu o Red Hat) en Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: f9e0582a1338bcae7b330c7ece7c3d8cc8593cfa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60543946"
---
# <a name="how-to-install-mysql-on-azure"></a>Instalación de MySQL en Azure
En este artículo aprenderá a instalar y configurar MySQL en una máquina virtual de Azure con Linux.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a>Instalación de MySQL en la máquina virtual
> [!NOTE]
> Debe tener una máquina virtual de Microsoft Azure en la que se ejecuta Linux para completar este tutorial. Antes de continuar, consulte el [tutorial sobre máquinas virtuales Linux de Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para crear y configurar una máquina virtual Linux con `mysqlnode` como nombre de máquina virtual y `azureuser` como usuario.
> 
> 

En este caso, use el puerto 3306 como puerto MySQL.  

Conéctese a la máquina virtual Linux que creó mediante putty. Si es la primera vez que usa una máquina virtual Linux de Azure, vea cómo usar putty para conectarse a una máquina virtual Linux [aquí](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Usaremos el paquete de repositorio para instalar MySQL5.6 como un ejemplo de este artículo. En realidad, MySQL5.6 tiene un mejor rendimiento MySQL5.5.  Puede encontrar más información [aquí](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/):

### <a name="how-to-install-mysql56-on-ubuntu"></a>Instalación de MySQL5.6 en Ubuntu
Usaremos aquí la máquina virtual de Linux con Ubuntu de Azure.

* Paso 1: instale el modificador de MySQL Server 5.6 en el usuario `root`:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Instalación de mysql-server 5.6:
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    Durante la instalación, aparecerá una ventana de cuadro de diálogo para pedirle que establezca la contraseña raíz de MySQL a continuación, y debe establecer la contraseña aquí.
  
    ![image](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    Vuelva a escribir la contraseña para confirmar.

    ![image](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* Paso 2: inicie sesión en el servidor MySQL
  
    Una vez finalizada la instalación del servidor MySQL, el servicio MySQL se inicia automáticamente. Puede iniciar sesión en el servidor MySQL con el usuario `root` .
    Use el siguiente comando para la contraseña de inicio de sesión y la entrada.
  
             #[root@mysqlnode ~]# mysql -uroot -p
* Paso 3: administre el servicio MySQL en ejecución
  
    (a) Obtener el estado del servicio MySQL
  
             #[root@mysqlnode ~]# service mysql status
  
    (b) Iniciar el servicio MySQL
  
             #[root@mysqlnode ~]# service mysql start
  
    (c) Detener el servicio MySQL
  
             #[root@mysqlnode ~]# service mysql stop
  
    (d) Reiniciar el servicio MySQL
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Instalación de MySQL en la familia de sistemas operativos Red Hat, como Oracle Linux o CentOS
Aquí usaremos la máquina virtual de Linux con CentOS u Oracle Linux.

* Paso 1: agregue el modificador del repositorio de MySQL Yum en el usuario `root`:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Descargue e instale el paquete de la versión de MySQL:
  
            #[root@mysqlnode ~]# wget https://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* Paso 2: edite el archivo siguiente y habilite el repositorio de MySQL para descargar el paquete MySQL5.6.
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    Actualizar cada valor de este archivo como se muestra a continuación:
  
        \# *Enable to use MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* Paso 3: instale MySQL desde el repositorio de MySQL Install MySQL:
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    Se instalará el paquete RPM MySQL y todos los paquetes relacionados.
* Paso 4: administre el servicio MySQL en ejecución
  
    (a) Comprobar el estado del servicio del servidor MySQL:
  
           #[root@mysqlnode ~]#service mysqld status
  
    (b) Comprobar si el puerto predeterminado de MySQL Server se está ejecutando:
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    (c) Iniciar el servidor de MySQL:

           #[root@mysqlnode ~]#service mysqld start

    (d) Detener el servidor de MySQL:

           #[root@mysqlnode ~]#service mysqld stop

    (e) Establecer MySQL para iniciarse con el arranque del sistema:

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-to-install-mysql-on-suse-linux"></a>Instalación de MySQL en SUSE Linux
Aquí usaremos la máquina virtual de Linux con OpenSUSE.

* Paso 1: descargue e instale el servidor MySQL
  
    Cambiar a usuario `root` a través de comando siguiente:  
  
           #sudo su -
  
    Descargar e instalar el paquete MySQL:
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* Paso 2: administre el servicio MySQL en ejecución
  
    (a) Comprobar el estado del servidor de MySQL:
  
           #[root@mysqlnode ~]# rcmysql status
  
    (b) Comprobar si el puerto predeterminado del servidor de MySQL se está ejecutando:
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    (c) Iniciar el servidor de MySQL:

           #[root@mysqlnode ~]# rcmysql start

    (d) Detener el servidor de MySQL:

           #[root@mysqlnode ~]# rcmysql stop

    (e) Establecer MySQL para iniciarse con el arranque del sistema:

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a>siguiente paso
Buscar más información de uso y sobre MySQL [aquí](https://www.mysql.com/).


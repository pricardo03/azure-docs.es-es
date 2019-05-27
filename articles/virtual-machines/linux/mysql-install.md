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
ms.openlocfilehash: 21ad3f9baf4b8e117f881d9a36fc606af04e17a5
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158439"
---
# <a name="how-to-install-mysql-on-azure"></a>Instalación de MySQL en Azure
En este artículo aprenderá a instalar y configurar MySQL en una máquina virtual de Azure con Linux.


> [!NOTE]
> Debe tener una máquina virtual de Microsoft Azure en la que se ejecuta Linux para completar este tutorial. Antes de continuar, consulte el [tutorial sobre máquinas virtuales Linux de Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para crear y configurar una máquina virtual Linux con `mysqlnode` como nombre de máquina virtual y `azureuser` como usuario.
> 
> 

En este caso, use el puerto 3306 como puerto MySQL.  

Usaremos el paquete de repositorio para instalar MySQL5.6 como un ejemplo de este artículo. En realidad, MySQL5.6 tiene un mejor rendimiento MySQL5.5.  Puede encontrar más información [aquí](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/):

## <a name="install-mysql56-on-ubuntu"></a>Instalación de MySQL5.6 en Ubuntu
Usaremos una VM de Linux con Ubuntu.


### <a name="install-mysql"></a>Instalar MySQL

Instalar MySQL Server 5.6 cambiando a la `root` usuario:

```bash  
sudo su -
```

Instalación de mysql-server 5.6:

```bash  
apt-get update
apt-get -y install mysql-server-5.6
```

  
Durante la instalación, aparecerá una ventana de cuadro de diálogo para pedirle que establezca la contraseña raíz de MySQL a continuación, y debe establecer la contraseña aquí.
  
![image](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

Vuelva a escribir la contraseña para confirmar.

![image](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

### <a name="sign-in"></a>Iniciar sesión
  
Una vez finalizada la instalación del servidor MySQL, el servicio MySQL se inicia automáticamente. Puede iniciar sesión en el servidor MySQL con la `root` usuario y escriba la contraseña.

```bash  
mysql -uroot -p
```


### <a name="manage-the-mysql-service"></a>Administrar el servicio MySQL

Obtener el estado del servicio de MySQL

```bash   
service mysql status
```
  
Iniciar el servicio MySQL

```bash  
service mysql start
```
  
Detener el servicio MySQL

```bash  
service mysql stop
```
  
Reinicie el servicio MySQL

```bash  
service mysql restart
```

## <a name="install-mysql-on-red-hat-os-centos-oracle-linux"></a>Instalación de MySQL en el sistema operativo de Red Hat, CentOS, Oracle Linux
Aquí usaremos la máquina virtual de Linux con CentOS u Oracle Linux.

### <a name="add-the-mysql-yum-repository"></a>Agregue el repositorio de MySQL yum
    
Cambie a `root` usuario:

```bash  
sudo su -
```

Descargue e instale el paquete de la versión de MySQL:

```bash  
wget https://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
yum localinstall -y mysql-community-release-el6-5.noarch.rpm
```

### <a name="enable-the-mysql-repository"></a>Habilitación del repositorio de MySQL
edite el archivo siguiente y habilite el repositorio de MySQL para descargar el paquete MySQL5.6.

```bash  
vim /etc/yum.repos.d/mysql-community.repo
```

  
Actualizar cada valor de este archivo como se muestra a continuación:

```  
\# *Enable to use MySQL 5.6*
  
[mysql56-community]
name=MySQL 5.6 Community Server
  
baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
enabled=1
  
gpgcheck=1
  
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

### <a name="install-mysql"></a>Instalar MySQL 

Instalar MySQL desde el repositorio.

```bash  
yum install mysql-community-server
```
  
Se instalará el paquete RPM MySQL y todos los paquetes relacionados.


## <a name="manage-the-mysql-service"></a>Administrar el servicio MySQL
  
Compruebe el estado del servicio del servidor MySQL:

```bash  
service mysqld status\
```
  
Compruebe si se está ejecutando el servidor de puerto de MySQL predeterminada:

```bash  
netstat  –tunlp|grep 3306
```

Iniciar el servidor MySQL:

```bash
service mysqld start
```

Detener el servidor MySQL:

```bash
service mysqld stop
```

Establecer MySQL para iniciarse cuando el sistema arranque copia de seguridad:

```bash
chkconfig mysqld on
```

## <a name="install-mysql-on-suse-linux"></a>Instalar MySQL en SUSE Linux

Aquí usaremos la máquina virtual de Linux con OpenSUSE.

### <a name="download-and-install-mysql-server"></a>descargue e instale el servidor MySQL
  
Cambiar a usuario `root` a través de comando siguiente:  

```bash  
sudo su -
```
  
Descargar e instalar el paquete MySQL:

```bash  
zypper update
zypper install mysql-server mysql-devel mysql
```

### <a name="manage-the-mysql-service"></a>Administrar el servicio MySQL
  
Compruebe el estado del servidor MySQL:

```bash  
rcmysql status
```
  
Compruebe si el puerto predeterminado del servidor MySQL:

```bash  
netstat  –tunlp|grep 3306
```

Iniciar el servidor MySQL:

```bash
rcmysql start
```

Detener el servidor MySQL:

```bash
rcmysql stop
```

Establecer MySQL para iniciarse cuando el sistema arranque copia de seguridad:

```bash
insserv mysql
```

## <a name="next-step"></a>Paso siguiente
Para obtener más información, consulte el [MySQL](https://www.mysql.com/) sitio Web.


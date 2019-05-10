---
title: Creación de un servidor de Ubuntu NFS (Network File System) para su uso con pods de Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo crear manualmente un volumen de servidor de NFS Ubuntu Linux para su uso con pods en Azure Kubernetes Service (AKS)
services: container-service
author: ozboms
ms.service: container-service
ms.topic: article
ms.date: 4/25/2019
ms.author: obboms
ms.openlocfilehash: 55eb5b0b98a4097d2f300bacabbfef3b0a32b27b
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65468505"
---
# <a name="manually-create-and-use-an-nfs-network-file-system-linux-server-volume-with-azure-kubernetes-service-aks"></a>Crear manualmente y usar un volumen de servidor de NFS (Network File System) de Linux con Azure Kubernetes Service (AKS)
Compartir datos entre contenedores a menudo es un componente necesario de las aplicaciones y servicios basados en contenedor. Suelen tener varios pods que necesitan tener acceso a la misma información en un volumen persistente externo.    
Mientras que los archivos de Azure son una opción, la creación de un servidor NFS en una máquina virtual de Azure es otra forma de almacenamiento compartido persistente. 

En este artículo le mostrará cómo crear un servidor NFS en una máquina virtual Ubuntu. Y también dar al acceso de los contenedores AKS a este sistema de archivos compartido.

## <a name="before-you-begin"></a>Antes de empezar
En este artículo se supone que tiene un clúster de AKS existente. Si necesita un clúster de AKS, consulte la Guía de inicio rápido AKS [mediante la CLI de Azure] [ aks-quickstart-cli] o [mediante Azure portal][aks-quickstart-portal].

El clúster de AKS debe residir en las redes virtuales emparejadas o mismas como el servidor NFS. El clúster debe crearse en una red virtual existente, lo que puede ser la misma red virtual que la máquina virtual.

Se describen los pasos para configurar una red virtual existente en la documentación: [crear clúster de AKS en la red virtual existente] [ aks-virtual-network] y [conectar redes virtuales con emparejamiento de VNET][peer-virtual-networks]

También se supone que ha creado una máquina Virtual de Linux de Ubuntu (por ejemplo, 18.04 LTS). Configuración y el tamaño pueden ser a su gusto y pueden implementarse a través de Azure. Para el inicio rápido de Linux, consulte [administración de máquinas virtuales de linux][linux-create].

Si implementa el clúster de AKS en primer lugar, Azure automáticamente rellenará el campo de la red virtual al implementar la máquina Ubuntu, haciéndolos en vivo dentro de la misma red virtual. Pero si desea trabajar con redes emparejadas en su lugar, consulte la documentación anterior.

## <a name="deploying-the-nfs-server-onto-a-virtual-machine"></a>Implementar el servidor de NFS en una máquina Virtual
Este es el script para configurar un servidor NFS dentro de la máquina virtual de Ubuntu:
```bash
#!/bin/bash

# This script should be executed on Linux Ubuntu Virtual Machine

EXPORT_DIRECTORY=${1:-/export/data}
DATA_DIRECTORY=${2:-/data}
AKS_SUBNET=${3:-*}

echo "Updating packages"
apt-get -y update

echo "Installing NFS kernel server"

apt-get -y install nfs-kernel-server

echo "Making data directory ${DATA_DIRECTORY}"
mkdir -p ${DATA_DIRECTORY}

echo "Making new directory to be exported and linked to data directory: ${EXPORT_DIRECTORY}"
mkdir -p ${EXPORT_DIRECTORY}

echo "Mount binding ${DATA_DIRECTORY} to ${EXPORT_DIRECTORY}"
mount --bind ${DATA_DIRECTORY} ${EXPORT_DIRECTORY}

echo "Giving 777 permissions to ${EXPORT_DIRECTORY} directory"
chmod 777 ${EXPORT_DIRECTORY}

parentdir="$(dirname "$EXPORT_DIRECTORY")"
echo "Giving 777 permissions to parent: ${parentdir} directory"
chmod 777 $parentdir

echo "Appending bound directories into fstab"
echo "${DATA_DIRECTORY}    ${EXPORT_DIRECTORY}   none    bind  0  0" >> /etc/fstab

echo "Appending localhost and Kubernetes subnet address ${AKS_SUBNET} to exports configuration file"
echo "/export        ${AKS_SUBNET}(rw,async,insecure,fsid=0,crossmnt,no_subtree_check)" >> /etc/exports
echo "/export        localhost(rw,async,insecure,fsid=0,crossmnt,no_subtree_check)" >> /etc/exports

nohup service nfs-kernel-server restart
```
El servidor se reiniciará (debido a la secuencia de comandos) y es posible montar el servidor NFS para AKS

>[!IMPORTANT]  
>No olvide reemplazar el **AKS_SUBNET** por la correcta desde el clúster o bien "*" se abrirá el servidor NFS para todos los puertos y las conexiones.

Después de crear la máquina virtual, copie el script anterior en un archivo. A continuación, puede moverlo desde el equipo local, o siempre que sea la secuencia de comandos, en la máquina virtual mediante: 
```console
scp /path/to/script_file username@vm-ip-address:/home/{username}
```
Una vez que la secuencia de comandos en la máquina virtual, que puede acceder mediante ssh en la máquina virtual y ejecútelo mediante el comando:
```console
sudo ./nfs-server-setup.sh
```
Si se produce un error en su ejecución debido a un error de permiso denegado, establezca el permiso de ejecución a través del comando:
```console
chmod +x ~/nfs-server-setup.sh
```

## <a name="connecting-aks-cluster-to-nfs-server"></a>Clúster de AKS conectando al servidor NFS
El servidor NFS, podemos conectarnos a nuestro clúster mediante el aprovisionamiento de un volumen persistente y notificación de volumen persistente que especifica cómo tener acceso al volumen.  
Conectar los dos servicios en las redes virtuales emparejadas o mismos es necesario. Instrucciones para configurar el clúster en la misma red virtual ya están aquí: [crear clúster de AKS en la red virtual existente][aks-virtual-network]

Una vez que están en la misma red virtual (o emparejada), debe aprovisionar que un volumen persistente y un volumen persistente de notificación en el clúster de AKS. Los contenedores, a continuación, pueden montar la unidad NFS en su directorio local.

Este es un ejemplo de definición de kubernetes para el volumen persistente (esta definición se supone que el clúster y la máquina virtual están en la misma red virtual):

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: <NFS_NAME>
  labels:
    type: nfs
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: <NFS_INTERNAL_IP>
    path: <NFS_EXPORT_FILE_PATH>
```
Reemplace **NFS_INTERNAL_IP**, **NFS_NAME** y **NFS_EXPORT_FILE_PATH** con información del servidor NFS.

También necesitará un archivo de notificación de volumen persistente. Este es un ejemplo de lo que debe incluir:

>[!IMPORTANT]  
>**"storageClassName"** debe permanecer vacío no funcionará la cadena o la notificación.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <NFS_NAME>
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  resources:
    requests:
      storage: 1Gi
  selector: 
    matchLabels:
      type: nfs
```

## <a name="troubleshooting"></a>solución de problemas
Si no se puede conectar al servidor de un clúster, un problema podría ser el directorio exportado o su elemento primario, no tiene permisos suficientes para tener acceso al servidor.

Compruebe que el directorio de exportación y su directorio primario tienen 777 permisos.

Para comprobar los permisos, ejecute el comando siguiente y deben tener los directorios *'drwxrwxrwx'* permisos:
```console
ls -l
```

## <a name="more-information"></a>Más información
Para obtener un tutorial completo o para ayudarle a depurar el programa de instalación del servidor NFS, este es un tutorial exhaustivo sobre:
  - [Tutorial NFS][nfs-tutorial]

## <a name="next-steps"></a>Pasos siguientes

Para las prácticas recomendadas asociadas, consulte [procedimientos recomendados para el almacenamiento y copias de seguridad en AKS][operator-best-practices-storage].

<!-- LINKS - external -->
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[linux-create]: https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm
[nfs-tutorial]: https://help.ubuntu.com/community/SettingUpNFSHowTo#Pre-Installation_Setup
[aks-virtual-network]: https://docs.microsoft.com/azure/aks/configure-kubenet#create-an-aks-cluster-in-the-virtual-network
[peer-virtual-networks]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[operator-best-practices-storage]: operator-best-practices-storage.md

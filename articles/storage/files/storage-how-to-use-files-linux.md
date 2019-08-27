---
title: Uso de Azure Files con Linux | Microsoft Docs
description: Aprenda a montar un recurso compartido de archivos de Azure mediante SMB en Linux.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 03/29/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 9674df346ff0b5c046df26fe85bf7ba0bbb802e6
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69900307"
---
# <a name="use-azure-files-with-linux"></a>Uso de Azure Files con Linux

[Azure Files](storage-files-introduction.md) es el sencillo sistema de archivos en la nube de Microsoft. Los recursos compartidos de archivos de Azure se pueden montar en distribuciones de Linux mediante el [cliente kernel de SMB](https://wiki.samba.org/index.php/LinuxCIFS). En este artículo se muestran dos maneras de montar un recurso compartido de archivos de Azure: a petición, con el comando `mount` y al inicio, mediante la creación de una entrada en `/etc/fstab`.

> [!NOTE]  
> Para montar un recurso compartido de archivos de Azure fuera de la región de Azure en la que se hospeda, bien sea en local o en una región distinta de Azure, el sistema operativo debe admitir la funcionalidad de cifrado de SMB 3.0.

## <a name="prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Requisitos previos para el montaje de un recurso compartido de archivos de Azure con Linux y el paquete cifs-utils
<a id="smb-client-reqs"></a>

* **Una cuenta de Azure Storage existente y un recurso compartido de archivos**: para completar los procedimientos de este artículo, debe tener una cuenta de almacenamiento y un recurso compartido de archivos. Si aún no ha creado uno, vea uno de nuestros inicios rápidos sobre el tema: [Creación de recursos compartidos de archivos: CLI](storage-how-to-use-files-cli.md).

* **El nombre y la clave de la cuenta de almacenamiento** Necesitará el nombre y la clave de la cuenta de almacenamiento para completar los procedimientos de este artículo. Si los creó con el inicio rápido de la CLI, ya debería tenerlos; de lo contrario, vea el inicio rápido de la CLI del vínculo anterior para obtener información sobre cómo recuperar la clave de la cuenta de almacenamiento.

* **Seleccione una distribución de Linux para satisfacer sus necesidades de montaje.**  
      Azure Files se puede montar ya sea a través de SMB 2.1 o de SMB 3.0. Para las conexiones procedentes de clientes locales o de otras regiones de Azure, debe usar SMB 3.0; Azure Files rechazará SMB 2.1 (o SMB 3.0 sin cifrado). Si accede al recurso compartido de archivos de Azure desde una VM dentro de la misma región de Azure, puede tener acceso a este con SMB 2.1 solo si la opción *Se requiere transferencia segura* está deshabilitada para la cuenta de almacenamiento que hospeda el recurso compartido de archivos de Azure. Siempre se recomienda requerir transferencia segura y usar solo SMB 3.0 con cifrado.

    La compatibilidad con el cifrado de SMB 3.0 se introdujo en el kernel de Linux versión 4.11 y se ha modificado para poder utilizarla con versiones anteriores del kernel para las distribuciones más populares de Linux. En el momento de la publicación de este documento, las siguientes distribuciones de la galería de Azure admiten la opción de montaje especificada en los encabezados de tabla. 

### <a name="minimum-recommended-versions-with-corresponding-mount-capabilities-smb-version-21-vs-smb-version-30"></a>Versiones mínimas recomendadas con funcionalidades de montaje correspondientes (SMB versión 2.1 frente a SMB versión 3.0)

|   | SMB 2.1 <br>(Puede montar en máquinas virtuales dentro de la misma región de Azure) | SMB 3.0 <br>(Puede montar desde el nivel local a entre regiones) |
| --- | :---: | :---: |
| Ubuntu Server | 14.04+ | 16.04 (o posterior) |
| RHEL | 7 (o posterior) | 7.5 (o posterior) |
| CentOS | 7 (o posterior) |  7.5 (o posterior) |
| Debian | 8 (o posterior) | 9+<sup>*</sup> |
| openSUSE | 13.2 (o posterior) | 42.3+ |
| SUSE Linux Enterprise Server | 12 | 12 SP3 (o posterior) |

<sup>*</sup>Es posible que se deba usar un kernel modificado con versiones anteriores. Puede instalarla con `sudo apt-get install linux-image-cloud-amd64`.

Si la distribución de Linux no aparece aquí, puede comprobar la versión del kernel de Linux con el siguiente comando:

```bash
uname -r
```

* <a id="install-cifs-utils"></a>**El paquete cifs-utils está instalado**.  
    El paquete cifs-utils se puede instalar con el administrador de paquetes en la distribución de Linux de su elección. 

    En distribuciones de **Ubuntu** y **Debian**, use el administrador de paquetes `apt-get`:

    ```bash
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    En **RHEL** y **CentOS**, use el administrador de paquetes `yum`:

    ```bash
    sudo yum install cifs-utils
    ```

    En **openSUSE**, use el administrador de paquetes `zypper`:

    ```bash
    sudo zypper install cifs-utils
    ```

    En otras distribuciones, use el administrador de paquetes apropiado o [compile desde el origen](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download)

* **Decidir sobre los permisos de archivo o directorio del recurso compartido montado**: en los ejemplos siguientes, se usa el permiso `0777` para proporcionar permisos de lectura, escritura y ejecución a todos los usuarios. Puede reemplazarlo por otros [permisos chmod](https://en.wikipedia.org/wiki/Chmod) según sea necesario, aunque esto puede conllevar algunas restricciones de acceso. Si usa otros permisos, debe considerar el uso también de uid y gid para seguir teniendo acceso a los usuarios locales y a los grupos de su elección.

> [!NOTE]
> Si no asigna expresamente permisos de archivo y de directorio con dir_mode y file_mode, se establecerán de forma predeterminada en 0755.

* **Asegúrese de que el puerto 445 está abierto**: SMB se comunica a través del puerto TCP 445: compruebe que el firewall no bloquea el puerto TCP 445 en el equipo cliente.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Montaje del recurso compartido de archivos de Azure a petición con `mount`

1. **[Instale el paquete cifs-utils correspondiente a su distribución de Linux](#install-cifs-utils)** .

1. **Cree una carpeta para un punto de montaje**: La carpeta para el punto de montaje se puede crear en cualquier parte del sistema de archivos, pero es habitual crearla en una carpeta nueva. Por ejemplo, con el siguiente comando se crea un directorio; reemplace **<storage_account_name>** y **<file_share_name>** por la información de su entorno correspondiente:

    ```bash
    mkdir -p <storage_account_name>/<file_share_name>
    ```

1. **Use el comando siguiente para montar el recurso compartido de archivos de Azure**: No olvide reemplazar **<storage_account_name>** , **<share_name>** , **<smb_version>** , **<storage_account_key>** y **<mount_point>** por la información de su entorno correspondiente. Si la distribución de Linux es compatible con SMB 3.0 con cifrado (vea [Comprender los requisitos de cliente de SMB](#smb-client-reqs) para más información), use **3.0** en **<smb_version>** . Para las distribuciones de Linux que no sean compatibles con SMB 3.0 con cifrado, use **2.1** en **<smb_version>** . Solo se puede montar un recurso compartido de archivos de Azure fuera de una región de Azure (incluidos en local o en una región distinta de Azure) con SMB 3.0. Si lo desea, puede cambiar los permisos de archivo y de directorio del recurso compartido montado, pero esto significaría restringir el acceso.

    ```bash
    sudo mount -t cifs //<storage_account_name>.file.core.windows.net/<share_name> <mount_point> -o vers=<smb_version>,username=<storage_account_name>,password=<storage_account_key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Cuando haya terminado de usar el recurso compartido de archivos de Azure, puede usar `sudo umount <mount_point>` para desmontarlo.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Creación de un punto de montaje persistente para el recurso compartido de archivos de Azure con `/etc/fstab`

1. **[Instale el paquete cifs-utils correspondiente a su distribución de Linux](#install-cifs-utils)** .

1. **Cree una carpeta para un punto de montaje**: La carpeta para el punto de montaje se puede crear en cualquier parte del sistema de archivos, pero es habitual crearla en una carpeta nueva. Siempre que la cree, tenga en cuenta la ruta de acceso absoluta de la carpeta. Por ejemplo, con el siguiente comando se crea un directorio; reemplace **<storage_account_name>** y **<file_share_name>** por la información de su entorno correspondiente.

    ```bash
    sudo mkdir -p <storage_account_name>/<file_share_name>
    ```

1. **Cree un archivo de credenciales para almacenar el nombre de usuario (el nombre de cuenta de almacenamiento) y la contraseña (la clave de cuenta de almacenamiento) para el recurso compartido de archivos.** Reemplace **<storage_account_name>** y **<storage_account_key>** por la información de su entorno correspondiente.

    ```bash
    if [ ! -d "/etc/smbcredentials" ]; then
    sudo mkdir /etc/smbcredentials
    fi
    if [ ! -f "/etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred" ]; then
    sudo bash -c 'echo "username=<STORAGE ACCOUNT NAME>" >> /etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred'
    sudo bash -c 'echo "password=7wRbLU5ea4mgc<DRIVE LETTER>PIpUCNcuG9gk2W4S2tv7p0cTm62wXTK<DRIVE LETTER>CgJlBJPKYc4VMnwhyQd<DRIVE LETTER>UT<DRIVE LETTER>yR5/RtEHyT/EHtg2Q==" >> /etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred'
    fi
    ```

1. **Cambie los permisos en el archivo de credenciales para que solo la raíz pueda leer o modificar el archivo de contraseña.** Puesto que la clave de la cuenta de almacenamiento es básicamente una contraseña de superadministrador para la cuenta de almacenamiento, el establecimiento de los permisos en el archivo de forma que solo la raíz pueda acceder a ella es importante para que los usuarios con privilegios reducidos no puedan recuperarla.   

    ```bash
    sudo chmod 600 /etc/smbcredentials/<storage_account_name>.cred
    ```

1. **Use el siguiente comando para anexar la siguiente línea a `/etc/fstab`** : No olvide reemplazar **<storage_account_name>** , **<share_name>** , **<smb_version>** y **<mount_point>** por la información de su entorno correspondiente. Si la distribución de Linux es compatible con SMB 3.0 con cifrado (vea [Comprender los requisitos de cliente de SMB](#smb-client-reqs) para más información), use **3.0** en **<smb_version>** . Para las distribuciones de Linux que no sean compatibles con SMB 3.0 con cifrado, use **2.1** en **<smb_version>** . Solo se puede montar un recurso compartido de archivos de Azure fuera de una región de Azure (incluidos en local o en una región distinta de Azure) con SMB 3.0.

    ```bash
    sudo bash -c 'echo "//<STORAGE ACCOUNT NAME>.file.core.windows.net/<FILE SHARE NAME> /mount/<STORAGE ACCOUNT NAME>/<FILE SHARE NAME> cifs _netdev,nofail,vers=3.0,credentials=/etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'

    sudo mount /mount/<STORAGE ACCOUNT NAME>/<FILE SHARE NAME>
    ```

> [!Note]  
> Puede usar `sudo mount -a` para montar el recurso compartido de archivos de Azure después de editar `/etc/fstab` en lugar de reiniciar el sistema.

## <a name="feedback"></a>Comentarios

Usuarios de Linux: nos gustaría conocer su opinión.

El grupo de usuarios de Azure Files para Linux ofrece un foro donde puede compartir sus comentarios a medida que evalúa y adopta File Storage en Linux. Envíe un correo electrónico a [Usuarios de Azure Files para Linux](mailto:azurefileslinuxusers@microsoft.com) para unirse al grupo de usuarios.

## <a name="next-steps"></a>Pasos siguientes

Consulte los vínculos siguientes para más información sobre Azure Files:

* [Planeamiento de una implementación de Azure Files](storage-files-planning.md)
* [P+F](../storage-files-faq.md)
* [Solución de problemas](storage-troubleshoot-linux-file-connection-problems.md)

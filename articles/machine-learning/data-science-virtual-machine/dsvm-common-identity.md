---
title: Configuración de una identidad común para Data Science Virtual Machine en Azure | Microsoft Docs
description: Obtenga información sobre cómo crear cuentas de usuario comunes que se pueden usar en varias instancias de Data Science Virtual Machine. Puede usar Azure Active Directory o una instancia local de Active Directory para autenticar usuarios en Data Science Virtual Machine.
keywords: aprendizaje profundo, AI, herramientas de ciencia de datos, data science virtual machine, análisis geoespacial, proceso de ciencia de datos en equipo
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 0146ee6ee37c2eb9e98d831b54df2218d7de5b62
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60502388"
---
# <a name="set-up-a-common-identity-on-the-data-science-virtual-machine"></a>Configuración de una identidad común en Data Science Virtual Machine

En una máquina virtual (VM) de Azure, que incluye Data Science Virtual Machine (DSVM), debe crear cuentas de usuario locales al aprovisionar la máquina virtual. Los usuarios se autentican en la máquina virtual mediante estas credenciales. Si tiene varias máquinas virtuales a las que necesita acceder, este enfoque puede volverse complicado al administrar las credenciales. Las cuentas de usuario comunes y la administración mediante un proveedor de identidades basado en estándares le permiten usar un único conjunto de credenciales para acceder a varios recursos de Azure, lo que incluye varias DSVM. 

Active Directory es un conocido proveedor de identidades y se admite tanto en Azure como servicio como en el entorno local. Puede usar Azure Active Directory (Azure AD) o Active Directory local para autenticar a los usuarios en una máquina virtual DSVM independiente o un clúster de DSVM en un conjunto de escalado de máquinas virtuales de Azure. Para ello, debe unir las instancias de DSVM a un dominio de Active Directory. 

Si ya tiene una instancia de Active Directory para administrar las identidades, puede usarla como proveedor de identidad común. Si no tiene Active Directory, puede ejecutar una instancia de Active Directory administrada en Azure mediante un servicio llamado [Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/) (Azure AD DS). 

En la documentación de [Azure AD](https://docs.microsoft.com/azure/active-directory/) se proporcionan [instrucciones de administración](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution), incluidas las de conexión de Azure AD al directorio local, si tiene uno. 

En este artículo se describen los pasos necesarios para configurar un servicio de dominio de Active Directory completamente administrado en Azure con Azure AD DS. A continuación, puede unir sus máquinas virtuales DSVM al dominio de Active Directory administrado para permitir que los usuarios accedan a un grupo de DSVM (y otros recursos de Azure) utilizando una cuenta de usuario común y sus credenciales. 

## <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Configuración de un dominio de Active Directory completamente administrado en Azure

Azure AD DS simplifica el proceso de administrar las identidades al proporcionar un servicio completamente administrado en Azure. En este dominio de Active Directory, se administran usuarios y grupos. Los pasos para configurar un dominio de Active Directory hospedado en Azure y las cuentas de usuarios de su directorio son:

1. En Azure Portal, agregue el usuario a Active Directory: 

    a. Inicie sesión en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com) con una cuenta que tenga el rol de administrador global en el directorio.
    
   b. Seleccione **Azure Active Directory** y, después, **Usuarios y grupos**.
    
   c. En **Usuarios y grupos**, seleccione **Todos los usuarios** y **Nuevo usuario**.
   
      Se abre el panel **Usuario**.
      
      ![Panel "Usuario"](./media/add-user.png)
    
   d. Especifique los detalles del usuario, como el **nombre** y el **nombre de usuario**. La parte del nombre de dominio del nombre de usuario debe ser el nombre de dominio del nombre de dominio predeterminado inicial "[nombre de dominio].onmicrosoft.com" o un [nombre de dominio personalizado](../../active-directory/add-custom-domain.md) comprobado y no federado, como "contoso.com".
    
   e. Copie o anote la contraseña de usuario generada para poder proporcionársela al usuario cuando finalice este proceso.
    
   f. También puede abrir y rellenar la información de **Perfil**, **Grupos** o **Rol del directorio** del usuario. 
    
   g. En **Usuario**, seleccione **Crear**.
    
   h. Distribuya de manera segura la contraseña generada al nuevo usuario para que pueda iniciar sesión.

1. Cree una instancia de Azure AD DS. Siga las instrucciones del artículo [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) (tareas de 1 a 5). Es importante actualizar las contraseñas de usuario existentes en Active Directory para que la contraseña de Azure AD DS esté sincronizada. También es importante agregar DNS a Azure AD DS, como se indica en la tarea 4 del artículo anterior. 

1. Cree una subred de DSVM independiente en la red virtual creada en la tarea 2 del paso anterior.
1. Cree una o varias instancias de Data Science VM en la subred de DSVM. 
1. Siga las [instrucciones](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) para agregar DSVM a Active Directory. 
1. Monte un recurso compartido de Azure Files para hospedar su directorio particular o de cuaderno de forma que el área de trabajo se pueda montar en cualquier máquina. (Si necesita permisos de nivel de archivo estrictos, necesitará que NFS se ejecute en una o varias máquinas virtuales).

    a. [Cree un recurso compartido de Azure Files](../../storage/files/storage-how-to-create-file-share.md).
    
   b. Móntelo en la DSVM de Linux. Al hacer clic en el botón **Conectar** para el recurso compartido de Azure Files en la cuenta de almacenamiento en Azure Portal, se muestra el comando para la ejecución en el shell de Bash en la máquina virtual DSVM de Linux. El comando tiene este aspecto:
   
   ```
   sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
   ```
1. Supongamos que ha montado el recurso compartido de Azure Files en /data/workspace, por ejemplo. Ahora cree directorios para cada uno de los usuarios en el recurso compartido: /data/workspace/user1, /data/workspace/user2 y así sucesivamente. Cree un directorio `notebooks` en el área de trabajo de cada usuario. 
1. Cree vínculos simbólicos para `notebooks` en `$HOME/userx/notebooks/remote`.   

Ahora tendrá los usuarios en la instancia de Active Directory hospedada en Azure. Con las credenciales de Active Directory, los usuarios pueden iniciar sesión en cualquier máquina virtual DSVM (SSH o JupyterHub) unida a Azure AD DS. Dado que el área de trabajo del usuario se encuentra en un recurso compartido de Azure Files, los usuarios tendrán acceso a sus cuadernos y demás trabajo desde cualquier DSVM al usar Jupyterhub. 

Para el escalado automático, puede usar un conjunto de escalado de máquinas virtuales para crear un grupo de máquinas virtuales, todas ellas unidas al dominio de esta forma y con el disco compartido montado. Los usuarios pueden iniciar sesión en cualquier máquina disponible en el conjunto de escalado de máquinas virtuales y tener acceso al disco compartido donde se guardan sus cuadernos. 

## <a name="next-steps"></a>Pasos siguientes

* [Almacenamiento seguro de las credenciales para acceder a recursos en la nube](dsvm-secure-access-keys.md)




---
title: Usuarios de administración de Azure Data Box Gateway | Microsoft Docs
description: Describe cómo usar Azure Portal para administrar usuarios en Azure Data Box Gateway.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/09/2018
ms.author: alkohli
ms.openlocfilehash: 3bd368cf19edf1370aaeef54d3ba0e4fd26ac239
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49070811"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-gateway"></a>Uso de Azure Portal para administrar usuarios en Azure Data Box Gateway 

En este artículo se describe cómo administrar usuarios en Azure Data Box Gateway. Azure Data Box Gateway se puede administrar a través de Azure Portal o de la interfaz de usuario web local. Usar Azure Portal para agregar, modificar o eliminar usuarios.

> [!IMPORTANT]
> - Data Box Gateway está en versión preliminar. Antes de solicitar e implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Agregar un usuario
> * Modificación de un usuario
> * Eliminar un usuario 

## <a name="about-users"></a>Acerca de los usuarios

Los usuarios pueden ser de solo lectura o tener privilegios totales. Como indican los nombres, los usuarios de solo lectura solo pueden ver los datos de los recursos compartidos. Los usuarios con privilegios totales pueden leer datos de los recursos compartidos, escribir en estos recursos compartidos y modificar o eliminar los datos de los recursos compartidos. 

 - **Usuario con privilegios totales**: un usuario local con acceso completo. 
 - **Usuario de solo lectura**: un usuario local con acceso de solo lectura. Estos usuarios están asociados a los recursos compartidos que permiten operaciones de solo lectura.

Los permisos del usuario se definen por primera vez cuando se crea el usuario durante la creación de los recursos compartidos. Una vez que se definen los permisos asociados a un usuario, se pueden modificar mediante el Explorador de archivos. 


## <a name="add-a-user"></a>Adición de un usuario

Para agregar un usuario siga estos pasos en Azure Portal.

1. En Azure Portal, vaya al recurso Data Box Gateway y, después, a **Información general**. Haga clic en **+ Agregar usuario** en la barra de comandos.

    ![Hacer clic en agregar usuario](media/data-box-gateway-manage-users/add-user-1.png)

2. Especifique el nombre de usuario y la contraseña del usuario que desea agregar. Confirme la contraseña y haga clic en **Agregar**.

    ![Hacer clic en agregar usuario](media/data-box-gateway-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Estos usuarios los reserva el sistema y no deben usarse: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Recibirá una notificación tanto cuando comience la creación del usuario como cuando se complete. Una vez que se crea el usuario, en la barra de comandos, haga clic en **Actualizar** para ver la lista actualizada de usuarios.


## <a name="modify-user"></a>Modificación de un usuario

Una vez que se crea un usuario es posible cambiar la contraseña asociada con él. Seleccione y haga clic en la lista de usuarios. Indique y confirme la nueva contraseña. Guarde los cambios.
 
![Modificación de un usuario](media/data-box-gateway-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Eliminación de un usuario

Siga estos pasos en Azure Portal para eliminar un usuario.

1. Seleccione un usuario en la lista de usuarios, haga clic en él y, después, en **Eliminar**.  

   ![Eliminar un usuario](media/data-box-gateway-manage-users/delete-user-1.png)

2. Cuando se le solicite, confirme la eliminación. 

   ![Eliminar un usuario](media/data-box-gateway-manage-users/delete-user-2.png)

La lista de usuarios se actualiza para reflejar el usuario eliminado.

![Eliminar un usuario](media/data-box-gateway-manage-users/delete-user-3.png)


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar el ancho de banda](data-box-gateway-manage-bandwidth-schedules.md).

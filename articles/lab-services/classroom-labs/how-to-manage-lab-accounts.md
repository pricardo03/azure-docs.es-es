---
title: Administración de cuentas de laboratorio en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo crear o eliminar una cuenta de laboratorio y cómo ver todas las cuentas de laboratorio de una suscripción de Azure.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: ff2968f8e2fa9a705817b020f2daa6582d78029c
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225309"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Administración de cuentas de laboratorio en Azure Lab Services 
En Azure Lab Services, una cuenta de laboratorio es un contenedor para laboratorios administrados, como los laboratorios de clase. Los administradores configuran una cuenta de laboratorio con Azure Lab Services y proporcionan acceso a los propietarios del laboratorio que pueden crear laboratorios en la cuenta. En este artículo se describe cómo crear o eliminar una cuenta de laboratorio y cómo ver todas las cuentas de laboratorio.

## <a name="create-a-lab-account"></a>Creación de una cuenta de laboratorio
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En el menú principal del lado izquierdo, seleccione **Crear un recurso**.
3. Busque **Lab Services** en Azure Marketplace y seleccione **Lab Services** en la lista desplegable. 
4. Seleccione **Lab Services (versión preliminar)** en la lista filtrada de servicios. 
5. En la ventana **Create a lab account** (Crear una cuenta de laboratorio), seleccione **Crear**.
7. En la ventana **Lab account** (Cuenta de laboratorio), realice las acciones siguientes: 
    1. En **Lab account name** (Nombre de la cuenta de laboratorio), escriba un nombre. 
    2. Seleccione la **suscripción de Azure** donde desea crear la cuenta de laboratorio.
    3. En **Grupo de recursos**, seleccione **Crear nuevo** y escriba un nombre para el grupo de recursos.
    4. En **Ubicación**, seleccione la ubicación o región en la que desea crear la cuenta de laboratorio. 
    5. Seleccione **Crear**. 

        ![Ventana de creación de una cuenta de laboratorio](../media/how-to-manage-lab-accounts/lab-account-settings.png)
5. Si no ve la página de la cuenta de laboratorio, seleccione el botón **Notificaciones** y haga clic en el botón **Ir al recurso** en las notificaciones. 

    ![Ventana de creación de una cuenta de laboratorio](../media/how-to-manage-lab-accounts/notification-go-to-resource.png)    
6. Verá la siguiente página de a **cuenta de laboratorio**:

    ![Página de la cuenta de laboratorio](../media/how-to-manage-lab-accounts/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Incorporación de un usuario al rol Creador de laboratorio
Para configurar un laboratorio de clase en una cuenta de laboratorio, el usuario debe ser miembro del rol **Creador de laboratorio** en la cuenta de laboratorio. La cuenta que usó para crear la cuenta de laboratorio se agrega automáticamente a este rol. Si pretende usar la misma cuenta de usuario para crear un laboratorio de clase, puede omitir este paso. Para usar otra cuenta de usuario para crear un laboratorio de clase, siga los pasos siguientes: 

1. En la página de la **cuenta de laboratorio**, seleccione **Control de acceso (IAM)** y haga clic en **+ Agregar** en la barra de herramientas. 

    ![Página de la cuenta de laboratorio](../media/tutorial-setup-lab-account/access-control.png)
2. En la página **Agregar permisos**, seleccione **Creador de laboratorio** en **Rol**, seleccione el usuario que desea agregar al rol Creador de laboratorio y seleccione **Guardar**. 

    ![Incorporación del usuario al rol Creador de laboratorio](../media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)

## <a name="specify-marketplace-images-available-to-lab-owners"></a>Especificación de las imágenes de Marketplace disponibles para los propietarios de laboratorio
En esta sección, especificará las imágenes de Marketplace que los propietarios de laboratorio pueden usar para crear laboratorios de clase. 

1. Seleccione **Imágenes de Marketplace** en el menú a la izquierda. De forma predeterminada, aparecerá la lista completa de las imágenes (habilitadas y deshabilitadas). Puede filtrar la lista para ver solamente las imágenes habilitadas o deshabilitadas seleccionando la opción **Enabled only**(Solo habilitadas)/**Disabled only** (Solo deshabilitadas) en la lista desplegable en la parte superior. 

    ![Página de imágenes de Marketplace](../media/tutorial-setup-lab-account/marketplace-images-page.png)
2. Para **deshabilitar** una imagen de Marketplace que se ha habilitado, realice una de las siguientes acciones: 
    1. Seleccione **... (puntos suspensivos)**  en la última columna y seleccione **Disable image** (Deshabilitar imagen). 

        ![Deshabilitar una imagen](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Seleccione una o más imágenes en la lista seleccionando las casillas que hay antes de los nombres de imagen en la lista y seleccione **Disable selected images** (Deshabilitar imágenes seleccionadas). 

        ![Deshabilitar varias imágenes](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. De forma similar, para **habilitar** una imagen de Marketplace que se ha habilitado, realice una de las siguientes acciones: 
    1. Seleccione **... (puntos suspensivos)**  en la última columna y seleccione **Enable image** (Habilitar imagen). 
    2. Seleccione una o más imágenes en la lista seleccionando las casillas que hay antes de los nombres de imagen en la lista y seleccione **Enable selected images** (Habilitar imágenes seleccionadas). 

## <a name="view-lab-accounts"></a>Vista de las cuentas de laboratorio
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los recursos** en el menú. 
3. Seleccione **Servicios de laboratorio** como el **tipo**. 
    También puede filtrar por suscripción, grupo de recursos, ubicaciones y etiquetas. 

## <a name="delete-a-lab-account"></a>Eliminación de una cuenta de laboratorio
Siga las instrucciones que aparecen en la sección anterior que muestra una lista de las cuentas de laboratorio. Use las instrucciones siguientes para eliminar una cuenta de laboratorio: 

1. Seleccione la **cuenta de laboratorio** que desee eliminar. 
2. Seleccione **Eliminar** en la barra de herramientas. 
3. Escriba **Sí** para confirmar.
4. Seleccione **Eliminar**. 

## <a name="next-steps"></a>Pasos siguientes
Introducción a la configuración de un laboratorio con Azure Lab Services:

- [Configuración de un laboratorio educativo](tutorial-setup-classroom-lab.md)
- [Configuración de un laboratorio](../tutorial-create-custom-lab.md)

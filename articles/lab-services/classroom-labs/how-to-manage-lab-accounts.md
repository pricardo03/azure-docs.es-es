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
ms.date: 02/07/2018
ms.author: spelluru
ms.openlocfilehash: f1194d8385d1e7ddcb906d0c8c3a2b56648e2547
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60696391"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Administración de cuentas de laboratorio en Azure Lab Services 
En Azure Lab Services, una cuenta de laboratorio es un contenedor para los tipos de laboratorio administrado, como laboratorios educativos. Los administradores configuran una cuenta de laboratorio con Azure Lab Services y proporcionan acceso a los propietarios del laboratorio que pueden crear laboratorios en la cuenta. En este artículo se describe cómo crear o eliminar una cuenta de laboratorio y cómo ver todas las cuentas de laboratorio.

## <a name="create-a-lab-account"></a>Creación de una cuenta de laboratorio
Los pasos siguientes muestran cómo usar Azure Portal para crear una cuenta de laboratorio con Azure Lab Services. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el menú de la izquierda. Seleccione **Cuentas de laboratorio** en la sección **DEVOPS**. Si selecciona la estrella (`*`) junto a **Cuentas de laboratorio**, se agrega a la sección **FAVORITOS** en el menú de la izquierda. A partir de la próxima vez, seleccione **Cuentas de laboratorio** en **FAVORITOS**.

    ![Todos los servicios -> Cuentas de laboratorio](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. En la página **Cuentas de laboratorio**, seleccione **Agregar** en la barra de herramientas. 

    ![Seleccione Agregar en la página Cuentas de laboratorio](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. En la página **Cuenta de laboratorio**, realice las acciones siguientes: 
    1. En **Lab account name** (Nombre de la cuenta de laboratorio), escriba un nombre. 
    2. Seleccione la **suscripción de Azure** donde desea crear la cuenta de laboratorio.
    3. En **Grupo de recursos**, seleccione **Crear nuevo** y escriba un nombre para el grupo de recursos.
    4. En **Ubicación**, seleccione la ubicación o región en la que desea crear la cuenta de laboratorio. 
    5. Para **Asociar red virtual**, seleccione una red virtual del mismo nivel como red de laboratorio. Los laboratorios creados en esta cuenta se conectan a la red virtual seleccionada y tienen acceso a los recursos de esta. 
    7. En el campo **Allow lab creator to pick lab location** (Permitir que el creador del laboratorio seleccione su ubicación), especifique si quiere que los creadores del laboratorio puedan seleccionar una ubicación para el laboratorio. De forma predeterminada, la opción está deshabilitada. Cuando está deshabilitada, los creadores del laboratorio no pueden especificar una ubicación para el laboratorio que crean. Los laboratorios se crean en la ubicación geográfica más cercana a la cuenta del laboratorio. Cuando está habilitada, el creador del laboratorio puede seleccionar una ubicación en el momento de su creación.      
    8. Seleccione **Crear**. 

        ![Ventana de creación de una cuenta de laboratorio](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Seleccione el **icono de campana** en la barra de herramientas (**Notificaciones**), confirme que la implementación se realizó correctamente y, a continuación, seleccione **Ir al recurso**. 

    Como alternativa, seleccione **Actualizar** en la página **Cuentas de laboratorio** y seleccione la cuenta de laboratorio que creó. 

    ![Ventana de creación de una cuenta de laboratorio](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Verá la siguiente página de a **cuenta de laboratorio**:

    ![Página de la cuenta de laboratorio](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Incorporación de un usuario al rol Creador de laboratorio
Para configurar un laboratorio de clase en una cuenta de laboratorio, el usuario debe ser miembro del rol **Creador de laboratorio** en la cuenta de laboratorio. La cuenta que usó para crear la cuenta de laboratorio se agrega automáticamente a este rol. Si pretende usar la misma cuenta de usuario para crear un laboratorio de clase, puede omitir este paso. Para usar otra cuenta de usuario para crear un laboratorio de clase, siga los pasos siguientes: 

Para proporcionar a los educadores el permiso para crear laboratorios para sus clases agréguelos al rol **Creador de laboratorios**:

1. En la página de la **cuenta de laboratorio**, seleccione **Control de acceso (IAM)** y haga clic en **+ Agregar asignación de rol** en la barra de herramientas. 

    ![Control de acceso -> botón Agregar asignación de roles](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. En la página **Agregar asignación de roles**, seleccione **Creador de laboratorio** en **Rol**, seleccione el usuario que quiere agregar al rol Creador de laboratorio y seleccione **Guardar**. 

    ![Incorporación del creador de laboratorio](../media/tutorial-setup-lab-account/add-lab-creator.png)


## <a name="specify-marketplace-images-available-to-lab-creators"></a>Especificación de las imágenes de Marketplace disponibles para los creadores de laboratorios
Como propietario de una cuenta de laboratorio, puede especificar las imágenes de Marketplace que los creadores de laboratorios pueden usar para este fin en la cuenta de laboratorio. 

1. Seleccione **Imágenes de Marketplace** en el menú a la izquierda. De forma predeterminada, aparecerá la lista completa de las imágenes (habilitadas y deshabilitadas). Puede filtrar la lista para ver solamente las imágenes habilitadas o deshabilitadas seleccionando la opción **Enabled only**(Solo habilitadas)/**Disabled only** (Solo deshabilitadas) en la lista desplegable en la parte superior. 
    
    ![Página de imágenes de Marketplace](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    Las imágenes de Marketplace que se muestran en la lista son las únicas que cumplen las condiciones siguientes:
        
    - Crea una sola máquina virtual.
    - Utiliza Azure Resource Manager para aprovisionar las máquinas virtuales.
    - No requiere la compra de un plan de licencias adicional.
2. Para **deshabilitar** una imagen de Marketplace que se ha habilitado, realice una de las siguientes acciones: 
    1. Seleccione **... (puntos suspensivos)**  en la última columna y seleccione **Disable image** (Deshabilitar imagen). 

        ![Deshabilitar una imagen](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Seleccione una o más imágenes en la lista seleccionando las casillas que hay antes de los nombres de imagen en la lista y seleccione **Disable selected images** (Deshabilitar imágenes seleccionadas). 

        ![Deshabilitar varias imágenes](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. De forma similar, para **habilitar** una imagen de Marketplace, realice una de las siguientes acciones: 
    1. Seleccione **... (puntos suspensivos)**  en la última columna y seleccione **Enable image** (Habilitar imagen). 
    2. Seleccione una o más imágenes en la lista seleccionando las casillas que hay antes de los nombres de imagen en la lista y seleccione **Enable selected images** (Habilitar imágenes seleccionadas). 

## <a name="configure-the-lab-account"></a>Configurar la cuenta de laboratorio
1. En el **cuenta de laboratorio** página, seleccione **configuración Labs** en el menú izquierdo.

    ![Página de configuración de laboratorios](../media/how-to-manage-lab-accounts/labs-configuration-page.png) 
1. Para **red virtual del mismo nivel**, seleccione **habilitado** o **deshabilitado**. El valor predeterminado es **deshabilitado**. Para habilitar la red virtual del mismo nivel, realice los pasos siguientes: 
    1. Seleccione **Habilitado**.
    2. Seleccione el **VNet** en la lista desplegable. 
    3. Seleccione **Guardar** en la barra de herramientas. 
    
        Laboratorio creado en esta cuenta se conecta a la red virtual seleccionada. Pueden acceder a los recursos de la red virtual seleccionada. 
3. Para el **permitir creador de laboratorio para elegir la ubicación de laboratorio**, seleccione **habilitado** si desea que el creador de laboratorio para que pueda seleccionar una ubicación para el laboratorio. Si está deshabilitado, los laboratorios se crean automáticamente en la misma ubicación en el que existe la cuenta de laboratorio. 

## <a name="view-lab-accounts"></a>Vista de las cuentas de laboratorio
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los recursos** en el menú. 
3. Seleccione **Cuentas de laboratorio** como **tipo**. 
    También puede filtrar por suscripción, grupo de recursos, ubicaciones y etiquetas. 

    ![Todos los recursos -> Cuentas de laboratorio](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)

## <a name="view-and-manage-labs-in-the-lab-account"></a>Visualización y administración de laboratorios en la cuenta de laboratorio

1. En la página **Cuenta de laboratorio**, seleccione **Laboratorios** en el menú de la izquierda.

    ![Laboratorios en la cuenta](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. Verá una **lista de laboratorios** en la cuenta con la siguiente información: 
    1. Nombre del laboratorio.
    2. La fecha en que se creó el laboratorio. 
    3. Dirección de correo electrónico del usuario que creó el laboratorio. 
    4. Máximo número de usuarios permitidos en el laboratorio. 
    5. Estado del laboratorio. 



## <a name="delete-a-lab-in-the-lab-account"></a>Eliminación de un laboratorio en la cuenta de laboratorio
Siga las instrucciones que aparecen en la sección anterior para ver una lista de los laboratorios de la cuenta de laboratorio.

1. Seleccione **... (puntos suspensivos)**  y después **Eliminar**. 

    ![Botón de eliminación de un laboratorio](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. Seleccione **Sí** en el mensaje de advertencia. 

    ![Confirmar eliminación de laboratorio](../media/how-to-manage-lab-accounts/confirm-lab-delete.png)

## <a name="delete-a-lab-account"></a>Eliminación de una cuenta de laboratorio
Siga las instrucciones que aparecen en la sección anterior que muestra una lista de las cuentas de laboratorio. Use las instrucciones siguientes para eliminar una cuenta de laboratorio: 

1. Seleccione la **cuenta de laboratorio** que desee eliminar. 
2. Seleccione **Eliminar** en la barra de herramientas. 

    ![Cuentas de laboratorio -> botón Eliminar](../media/how-to-manage-lab-accounts/delete-button.png)
1. Escriba **Sí** para confirmar.
1. Seleccione **Eliminar**. 

    ![Eliminar cuenta de laboratorio - confirmación](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)



## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como propietario del laboratorio, configurar y controlar el uso de un laboratorio](how-to-configure-student-usage.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)
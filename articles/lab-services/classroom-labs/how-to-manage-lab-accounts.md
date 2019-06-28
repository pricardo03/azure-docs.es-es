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
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: 6f283ce007e96547e01a01a3753ddcb60574bfc3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65412800"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Administración de cuentas de laboratorio en Azure Lab Services 
En Azure Lab Services, una cuenta de laboratorio es un contenedor para tipos de laboratorios administrados, como los laboratorios de clase. Los administradores configuran una cuenta de laboratorio con Azure Lab Services y proporcionan acceso a los propietarios del laboratorio que pueden crear laboratorios en la cuenta. En este artículo se describe cómo crear o eliminar una cuenta de laboratorio y cómo ver todas las cuentas de laboratorio.

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
    5. Seleccione una **galería de imágenes compartidas** existente o créela. Puede guardar la plantilla de máquina virtual en la galería de imágenes compartidas para que otros usuarios puedan usarla. Para más información acerca de las galerías de imágenes compartidas, consulte [Use una galería de imágenes compartidas en Azure Lab Services](how-to-use-shared-image-gallery.md).
    6. Para **Asociar red virtual**, seleccione una red virtual del mismo nivel como red de laboratorio. Los laboratorios creados en esta cuenta se conectan a la red virtual seleccionada y tienen acceso a los recursos de esta. 
    7. Especifique un **rango de direcciones** para las máquinas virtuales en el laboratorio. Dicho rango debe estar en la notación de enrutamiento de interdominios sin clases (CIDR) (por ejemplo: 10.20.0.0/23). Las máquinas virtuales del laboratorio se crearán en este rango de direcciones. Para más información, consulte el apartado [Especifique un intervalo de direcciones para las máquinas virtuales en el laboratorio](how-to-configure-lab-accounts.md#specify-an-address-range-for-vms-in-the-lab).    
    8. En el campo **Allow lab creator to pick lab location** (Permitir que el creador del laboratorio seleccione su ubicación), especifique si quiere que los creadores del laboratorio puedan seleccionar una ubicación para el laboratorio. De forma predeterminada, la opción está deshabilitada. Cuando está deshabilitada, los creadores del laboratorio no pueden especificar una ubicación para el laboratorio que crean. Los laboratorios se crean en la ubicación geográfica más cercana a la cuenta del laboratorio. Cuando está habilitada, el creador del laboratorio puede seleccionar una ubicación en el momento de su creación.      
    9. Seleccione **Crear**. 

        ![Ventana de creación de una cuenta de laboratorio](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Seleccione el **icono de campana** en la barra de herramientas (**Notificaciones**), confirme que la implementación se realizó correctamente y, a continuación, seleccione **Ir al recurso**. 

    Como alternativa, seleccione **Actualizar** en la página **Cuentas de laboratorio** y seleccione la cuenta de laboratorio que creó. 

    ![Ventana de creación de una cuenta de laboratorio](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Verá la siguiente página de a **cuenta de laboratorio**:

    ![Página de la cuenta de laboratorio](../media/tutorial-setup-lab-account/lab-account-page.png)

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
Consulte el artículo siguiente: [Configurar las cuentas de laboratorio](how-to-configure-lab-accounts.md).
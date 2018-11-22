---
title: Configuración de una cuenta de laboratorio con Azure Lab Services | Microsoft Docs
description: En este tutorial, va a configurar una cuenta de laboratorio con Azure Lab Services.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 11/15/2018
ms.author: spelluru
ms.openlocfilehash: 72acc4fe0febaa09d6deb82043795b6003af683f
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706786"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Tutorial: Configuración de una cuenta de laboratorio con Azure Lab Services
En Azure Lab Services, una cuenta de laboratorio sirve como cuenta central en la que se administran los laboratorios de una organización. En su cuenta de laboratorio, puede conceder permiso a otros usuarios para crear laboratorios y establecer las directivas que se aplican a todos los laboratorios de la cuenta de laboratorio. En este tutorial, aprenderá a crear una cuenta de laboratorio como administrador de laboratorio. 

En este tutorial realizará lo siguiente:

> [!div class="checklist"]
> * Creación de una cuenta de laboratorio
> * Incorporación de un usuario al rol Creador de laboratorio
> * Especificación de las imágenes de Marketplace disponibles para los propietarios de laboratorio

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-a-lab-account"></a>Creación de una cuenta de laboratorio
Los pasos siguientes muestran cómo usar Azure Portal para crear una cuenta de laboratorio con Azure Lab Services. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En el menú principal del lado izquierdo, seleccione **Crear un recurso**.
3. Busque **Lab Services** en Azure Marketplace y seleccione **Lab Services** en la lista desplegable. 
4. Seleccione **Lab Services (versión preliminar)** en la lista filtrada de servicios. 
1. En la ventana **Create a lab account** (Crear una cuenta de laboratorio), seleccione **Crear**.
2. En la ventana **Lab account** (Cuenta de laboratorio), realice las acciones siguientes: 
    1. En **Lab account name** (Nombre de la cuenta de laboratorio), escriba un nombre. 
    2. Seleccione la **suscripción de Azure** donde desea crear la cuenta de laboratorio.
    3. En **Grupo de recursos**, seleccione **Crear nuevo** y escriba un nombre para el grupo de recursos.
    4. En **Ubicación**, seleccione la ubicación o región en la que desea crear la cuenta de laboratorio. 
    5. Seleccione **Crear**. 

        ![Ventana de creación de una cuenta de laboratorio](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Si no ve la página de la cuenta de laboratorio, seleccione el botón **Notificaciones** y haga clic en el botón **Ir al recurso** en las notificaciones. 

    ![Ventana de creación de una cuenta de laboratorio](../media/tutorial-setup-lab-account/notification-go-to-resource.png)    
6. Verá la siguiente página de a **cuenta de laboratorio**:

    ![Página de la cuenta de laboratorio](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Incorporación de un usuario al rol Creador de laboratorio
Para configurar un laboratorio de clase en una cuenta de laboratorio, el usuario debe ser miembro del rol **Creador de laboratorio** en la cuenta de laboratorio. La cuenta que usó para crear la cuenta de laboratorio se agrega automáticamente a este rol. Si pretende usar la misma cuenta de usuario para crear un laboratorio de clase, puede omitir este paso. Para usar otra cuenta de usuario para crear un laboratorio de clase, siga los pasos siguientes: 

Para proporcionar a los educadores el permiso para crear laboratorios para sus clases agréguelos al rol **Creador de laboratorios**:

1. En la página de la **cuenta de laboratorio**, seleccione **Control de acceso (IAM)** y haga clic en **+ Agregar** en la barra de herramientas. 

    ![Página de la cuenta de laboratorio](../media/tutorial-setup-lab-account/access-control.png)
2. En la página **Agregar permisos**, seleccione **Creador de laboratorio** en **Rol**, seleccione el usuario que desea agregar al rol Creador de laboratorio y seleccione **Guardar**. 

    ![Incorporación del usuario al rol Creador de laboratorio](../media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)

## <a name="specify-marketplace-images-available-to-lab-owners"></a>Especificación de las imágenes de Marketplace disponibles para los propietarios de laboratorio
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

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha creado una cuenta de laboratorio. Para aprender a crear un laboratorio como profesor, pase al siguiente tutorial:

> [!div class="nextstepaction"]
> [Configuración de un laboratorio de clase](tutorial-setup-classroom-lab.md)


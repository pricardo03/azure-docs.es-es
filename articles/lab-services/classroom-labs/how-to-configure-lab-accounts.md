---
title: Configurar las cuentas de laboratorio en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo configurar una cuenta de laboratorio después de crearlo.
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
ms.openlocfilehash: ba469c038f04a31a57e798b97b5120bec573feae
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65414037"
---
# <a name="configure-lab-accounts-in-azure-lab-services"></a>Configurar las cuentas de laboratorio en Azure Lab Services 
En Azure Lab Services, una cuenta de laboratorio es un contenedor para los tipos de laboratorio administrado, como laboratorios educativos. Los administradores configuran una cuenta de laboratorio con Azure Lab Services y proporcionan acceso a los propietarios del laboratorio que pueden crear laboratorios en la cuenta. En este artículo se describe cómo crear o eliminar una cuenta de laboratorio y cómo ver todas las cuentas de laboratorio.

## <a name="connect-with-a-peer-virtual-network"></a>Conectar con una red virtual del mismo nivel
Para conectar una red virtual como una red punto a red virtual del laboratorio, siga estos pasos:

1. En el **cuenta de laboratorio** página, seleccione **configuración Labs** en el menú izquierdo.

    ![Página de configuración de laboratorios](../media/how-to-manage-lab-accounts/labs-configuration-page.png) 
1. Para **red virtual del mismo nivel**, seleccione **habilitado** o **deshabilitado**. El valor predeterminado es **deshabilitado**. Para habilitar la red virtual del mismo nivel, realice los pasos siguientes: 
    1. Seleccione **Habilitado**.
    2. Seleccione el **VNet** en la lista desplegable. 
3. Seleccione **Guardar** en la barra de herramientas. 

Laboratorio creado en esta cuenta se conecta a la red virtual seleccionada. Pueden acceder a los recursos de la red virtual seleccionada. Para obtener más información, consulte [conexión de red de su laboratorio con una red virtual del mismo nivel en Azure Lab Services](how-to-connect-peer-virtual-network.md).

Al seleccionar una red virtual para la **red virtual del mismo nivel** campo, el **permitir creador de laboratorio para elegir la ubicación de laboratorio** opción está deshabilitada. Es porque laboratorios en la cuenta de laboratorio deben estar en la misma región que la cuenta de laboratorio para que puedan conectarse con los recursos de la red virtual del mismo nivel. 

## <a name="allow-lab-creator-to-pick-location-for-the-lab"></a>Permitir que el creador de laboratorio elegir la ubicación del laboratorio de pruebas
Puede permitir que el creador de laboratorio crear laboratorios en una ubicación diferente a la ubicación de la cuenta de laboratorio, siga estos pasos: 

1. En el **cuenta de laboratorio** página, seleccione **configuración Labs** en el menú izquierdo.
2. Para el **permitir creador de laboratorio para elegir la ubicación de laboratorio**, seleccione **habilitado** si desea que el creador de laboratorio para que pueda seleccionar una ubicación para el laboratorio. Si está deshabilitado, los laboratorios se crean automáticamente en la misma ubicación en el que existe la cuenta de laboratorio. 
    
    Este campo está deshabilitado cuando se selecciona una red virtual para la **red virtual del mismo nivel** campo. Es porque laboratorios en la cuenta de laboratorio deben estar en la misma región que la cuenta de laboratorio para que puedan tener acceso a recursos en la red virtual del mismo nivel. 
1. Seleccione **Guardar** en la barra de herramientas. 

    ![Configurar la ubicación de laboratorio](../media/how-to-manage-lab-accounts/labs-configuration-page-lab-location.png)


## <a name="specify-an-address-range-for-vms-in-the-lab"></a>Especifique un intervalo de direcciones para las máquinas virtuales en el laboratorio
El procedimiento siguiente tiene pasos para especificar un intervalo de direcciones para las máquinas virtuales en el laboratorio. Si actualiza el intervalo que especificó anteriormente, el intervalo de direcciones modificada se aplica solo a las máquinas virtuales que se crean después de que se realizó el cambio. 

Estas son algunas restricciones al especificar el intervalo de direcciones que debe tener en cuenta. 

- El prefijo debe ser menor o igual a 23. 
- Si una red virtual se empareja con la cuenta de laboratorio, el intervalo de direcciones proporcionado no puede coincidir con el intervalo de direcciones de red virtual emparejada.

1. En el **cuenta de laboratorio** página, seleccione **configuración Labs** en el menú izquierdo.
2. Para el **intervalo de direcciones** , especifique el intervalo de direcciones para las máquinas virtuales que se creará en el laboratorio. El intervalo de direcciones debe estar en la notación de enrutamiento de interdominios sin clases (CIDR) (ejemplo: 10.20.0.0/23). Las máquinas virtuales en el laboratorio se crearán en este intervalo de direcciones.
3. Seleccione **Guardar** en la barra de herramientas. 

    ![Configurar el intervalo de direcciones](../media/how-to-manage-lab-accounts/labs-configuration-page-address-range.png)

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




## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como propietario del laboratorio, configurar y controlar el uso de un laboratorio](how-to-configure-student-usage.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)

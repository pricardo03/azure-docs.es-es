---
title: Configuración de cuentas de laboratorio en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo configurar una cuenta de laboratorio después de crearla.
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65414037"
---
# <a name="configure-lab-accounts-in-azure-lab-services"></a>Configuración de cuentas de laboratorio en Azure Lab Services 
En Azure Lab Services, una cuenta de laboratorio es un contenedor para tipos de laboratorios administrados, como los laboratorios de clase. Los administradores configuran una cuenta de laboratorio con Azure Lab Services y proporcionan acceso a los propietarios del laboratorio que pueden crear laboratorios en la cuenta. En este artículo se describe cómo crear o eliminar una cuenta de laboratorio y cómo ver todas las cuentas de laboratorio.

## <a name="connect-with-a-peer-virtual-network"></a>Conexión a una red virtual del mismo nivel
Para conectar una red virtual como una red del mismo nivel a la red virtual del laboratorio, siga estos pasos:

1. En la página **Cuenta de laboratorio**, seleccione **Configuración de laboratorios** en el menú de la izquierda.

    ![Página Configuración de laboratorios](../media/how-to-manage-lab-accounts/labs-configuration-page.png) 
1. En **Red virtual del mismo nivel**, seleccione **Habilitado** o **Deshabilitado**. El valor predeterminado es **Deshabilitado**. Para habilitar la red virtual del mismo nivel, siga estos pasos: 
    1. Seleccione **Habilitado**.
    2. Seleccione la **red virtual** en la lista desplegable. 
3. Seleccione **Guardar** en la barra de herramientas. 

Los laboratorios creados en esta cuenta se conectan a la red virtual seleccionada. Pueden acceder a los recursos de la red virtual seleccionada. Para más información, vea [Conexión de la red del laboratorio con una red virtual del mismo nivel en Azure Lab Services](how-to-connect-peer-virtual-network.md).

Al seleccionar una red virtual para el campo **Red virtual del mismo nivel**, se deshabilita la opción **Permitir al creador del laboratorio seleccionar la ubicación correspondiente**. Se debe a que los laboratorios de la cuenta de laboratorio deben estar en la misma región que esta para que se puedan conectar con los recursos de la red virtual del mismo nivel. 

## <a name="allow-lab-creator-to-pick-location-for-the-lab"></a>Permitir al creador del laboratorio seleccionar la ubicación correspondiente
Para permitir que el creador del laboratorio cree laboratorios en una ubicación distinta a la de la cuenta de laboratorio, siga estos pasos: 

1. En la página **Cuenta de laboratorio**, seleccione **Configuración de laboratorios** en el menú de la izquierda.
2. En el campo **Permitir al creador del laboratorio seleccionar la ubicación correspondiente**, seleccione **Habilitado** si quiere que el creador del laboratorio pueda seleccionar una ubicación para el laboratorio. Si se deshabilita, los laboratorios se crean de forma automática en la misma ubicación en la que existe la cuenta de laboratorio. 
    
    Este campo está deshabilitado cuando se selecciona una red virtual para el campo **Red virtual del mismo nivel**. Se debe a que los laboratorios de la cuenta de laboratorio deben estar en la misma región que esta para que puedan acceder a los recursos de la red virtual del mismo nivel. 
1. Seleccione **Guardar** en la barra de herramientas. 

    ![Configuración de la ubicación del laboratorio](../media/how-to-manage-lab-accounts/labs-configuration-page-lab-location.png)


## <a name="specify-an-address-range-for-vms-in-the-lab"></a>Especificación de un intervalo de direcciones para las máquinas virtuales del laboratorio
El procedimiento siguiente tiene pasos para especificar un intervalo de direcciones para las máquinas virtuales del laboratorio. Si actualiza el intervalo que ha especificado antes, el intervalo de direcciones modificado se aplica solo a las máquinas virtuales que se crean después de realizar el cambio. 

Estas son algunas restricciones al especificar el intervalo de direcciones que debe tener en cuenta. 

- El prefijo debe ser menor o igual a 23. 
- Si una red virtual se empareja con la cuenta de laboratorio, el intervalo de direcciones proporcionado no puede coincidir con el de la red virtual emparejada.

1. En la página **Cuenta de laboratorio**, seleccione **Configuración de laboratorios** en el menú de la izquierda.
2. Para el campo **Intervalo de direcciones**, especifique el intervalo de direcciones para las máquinas virtuales que se van a crear en el laboratorio. Dicho rango debe estar en la notación de enrutamiento de interdominios sin clases (CIDR) (por ejemplo: 10.20.0.0/23). Las máquinas virtuales del laboratorio se crearán en este rango de direcciones.
3. Seleccione **Guardar** en la barra de herramientas. 

    ![Configuración del intervalo de direcciones](../media/how-to-manage-lab-accounts/labs-configuration-page-address-range.png)

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

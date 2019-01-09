---
title: Tutorial sobre la preparación de Azure Portal para la implementación de Data Box Edge | Microsoft Docs
description: El primer tutorial para implementar Azure Data Box Edge trata sobre la preparación de Azure Portal.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 25f68b011d1fcba450903e9a691b98dfe9e87281
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53726126"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge"></a>Tutorial: Preparación para la implementación de Azure Data Box Edge  


Este es el primer tutorial de la serie de tutoriales de implementación necesarios para implementar completamente Azure Data Box Edge. En este tutorial se describe cómo preparar Azure Portal para implementar un recurso de Data Box Edge. 

Para completar el proceso de instalación y configuración se necesitan privilegios de administrador. La preparación del portal dura menos de 10 minutos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un nuevo recurso
> * Obtención de la clave de activación

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.


> [!IMPORTANT]
> Data Box Edge se encuentra en versión preliminar. Antes de solicitar e implementar esta solución, revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).  

### <a name="get-started"></a>Introducción

Para implementar Data Box Edge, consulte los siguientes tutoriales en el orden indicado.

| **#** | **En este paso** | **Use estos documentos** |
| --- | --- | --- | 
| 1. |**[Preparación de Azure Portal para Data Box Edge](data-box-edge-deploy-prep.md)** |Cree y configure el recurso de Data Box Edge antes de instalar un dispositivo físico de Data Box Edge. |
| 2. |**[Instalación de Data Box Edge](data-box-edge-deploy-install.md)**|Desempaquete, monte en el bastidor y realice el cableado del dispositivo físico de Data Box Edge.  |
| 3. |**[Conexión, configuración y activación de Data Box Edge](data-box-edge-deploy-connect-setup-activate.md)** |Conéctese a la interfaz de usuario web local, complete la configuración del dispositivo y active el dispositivo. El dispositivo está listo para configurar recursos compartidos de SMB o NFS.  |
| 4. |**[Transferencia de datos con Data Box Edge](data-box-edge-deploy-add-shares.md)** |Agregue recursos compartidos y conéctese a ellos mediante SMB o NFS. |
| 5. |**[Transformación de datos con Data Box Edge](data-box-edge-deploy-configure-compute.md)** |Configure los módulos de Edge en el dispositivo para que transformen los datos a medida que estos se mueven a Azure. |

Ya puede empezar a configurar Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

A continuación, encontrará los requisitos previos para configurar su recurso de Data Box Edge, el dispositivo Data Box Edge y la red de centros de datos.

### <a name="for-the-data-box-edge-resource"></a>Para el recurso de Data Box Edge

Antes de comenzar, asegúrese de que:

* Su suscripción de Microsoft Azure debe estar habilitada para el recurso de Data Box Edge.
* Tiene una cuenta de almacenamiento de Microsoft Azure con credenciales de acceso.

### <a name="for-the-data-box-edge-device"></a>Para el dispositivo de Data Box Edge

Antes de implementar un dispositivo físico, asegúrese de que:
- Tiene una ranura 1 U disponible en un bastidor estándar de 19" en su centro de datos para montar el dispositivo. 
- Tiene acceso a una superficie de trabajo plana, estable y nivelada en la que el dispositivo pueda apoyarse con seguridad.
- La ubicación en la que desea efectuar la instalación del dispositivo dispone de corriente alterna estándar de una fuente independiente o una unidad de distribución de energía (PDU) en bastidor con un sistema de alimentación ininterrumpida (UPS).
- Tiene acceso a un dispositivo físico.


### <a name="for-the-datacenter-network"></a>Para la red del centro de datos

Antes de comenzar, asegúrese de que:

* La red del centro de datos está configurada según los requisitos de red para el dispositivo de Data Box Edge. Para más información, consulte [Requisitos del sistema de Data Box Edge](data-box-gateway-system-requirements.md).

* El recurso de Data Box Edge tiene un ancho de banda de Internet de 20 Mbps (o más) disponible en todo momento. Este ancho de banda no debe compartirse con otras aplicaciones. Si usa la limitación de red para limitar el trabajo, se recomienda usar un ancho de banda de Internet de 32 Mbps o más.

## <a name="create-a-new-resource"></a>Crear un nuevo recurso

Realice los siguientes pasos para crear un nuevo recurso de Data Box Edge. 

Si ya tiene un recurso de Data Box Edge para administrar el dispositivo físico, omita este paso y vaya a [Obtención de la clave de activación](#get-the-activation-key).

Siga estos pasos en Azure Portal para crear un recurso de Data Box Edge.

1. Use sus credenciales de Microsoft Azure para iniciar sesión en el Portal de vista previa de Azure, en esta dirección URL: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Seleccione la suscripción que desea usar para la versión preliminar de Data Box Edge. Seleccione la región donde desea implementar el recurso Data Box Edge. En la opción **Data Box Edge**, haga clic en **Crear**.

    ![Búsqueda del servicio Data Box Edge](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Escriba o seleccione la siguiente información para el nuevo recurso.
    
    |Configuración  |Valor  |
    |---------|---------|
    |Nombre del recurso   | Nombre descriptivo que identifique el recurso.<br>El nombre del recurso tiene entre 2 y 50 caracteres y contiene letras, números y guiones.<br> El nombre comienza y termina con una letra o un número.        |
    |Subscription    |La suscripción está vinculada a la cuenta de facturación. |
    |Grupos de recursos  |Cree un nuevo grupo o seleccione uno existente.<br>Más información sobre los [grupos de recursos de Azure](../azure-resource-manager/resource-group-overview.md).     |
    |Ubicación     |En esta versión están disponibles Europa Occidental, Este de EE. UU., Oeste de EE.UU. 2, Sudeste Asiático y Europa Occidental. <br> Elija la ubicación más cercana a la región geográfica donde quiera implementar el dispositivo.|
    
    ![Creación de un recurso de Data Box Edge](media/data-box-edge-deploy-prep/data-box-edge-resource.png)
    
4. Seleccione **Aceptar**.
 
Se tarda unos minutos en crear el recurso. Recibirá una notificación apropiada cuando el servicio se cree correctamente.


## <a name="get-the-activation-key"></a>Obtención de la clave de activación

Cuando el recurso de Data Box Edge esté en funcionamiento, deberá obtener la clave de activación. Esta clave se utiliza para activar y conectar el dispositivo de Data Box Edge con el recurso. Puede obtener esta clave ahora mientras está en Azure Portal.

1. Seleccione el recurso que ha creado y, a continuación, seleccione **Información general**.

2. Haga clic en **Generar clave** para crear una nueva clave de activación. Seleccione el icono de copia para copiar la clave y guárdela para su uso posterior.

    ![Obtención de la clave de activación](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - La clave de activación expira tres días después de generarla. 
> - Si la clave ha expirado, genere una nueva clave. La clave antigua no es válida.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Edge, como:

> [!div class="checklist"]
> * Creación de un nuevo recurso
> * Obtención de la clave de activación

En el siguiente tutorial aprenderá a instalar un dispositivo de Data Box Edge. 

> [!div class="nextstepaction"]
> [Instalación de Data Box Edge](./data-box-edge-deploy-install.md)




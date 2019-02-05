---
title: Tutorial sobre la preparación de Azure portal para la implementación de Data Box Gateway | Microsoft Docs
description: El primer tutorial para implementar Azure Data Box Gateway trata sobre la preparación de Azure Portal.
services: databox-edge-gateway
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: alkohli
ms.openlocfilehash: 6f47606e91ec55bae624527bace81d947c5ea4f9
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55091553"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway-preview"></a>Tutorial: Preparación de la implementación de Azure Data Box Gateway (versión preliminar)


Este es el primer tutorial de la serie de tutoriales de implementación necesarios para implementar completamente Azure Data Box Gateway. En este tutorial se describe cómo preparar Azure Portal para implementar recursos de Data Box Gateway. 

Para completar el proceso de instalación y configuración se necesitan privilegios de administrador. La preparación del portal dura menos de 10 minutos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un nuevo recurso
> * Descarga de la imagen del dispositivo virtual
> * Obtención de la clave de activación

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.


> [!IMPORTANT]
> - Data Box Gateway está en versión preliminar. Antes de solicitar e implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

### <a name="get-started"></a>Introducción

Para implementar Data Box Gateway, consulte los siguientes tutoriales en el orden indicado.

| **#** | **En este paso** | **Use estos documentos** |
| --- | --- | --- | 
| 1. |**[Preparación de Azure Portal para Data Box Gateway](data-box-gateway-deploy-prep.md)** |Cree y configure el recurso de Data Box Gateway antes de aprovisionar un dispositivo virtual de Data Box Gateway. |
| 2. |**[Aprovisionamiento de Azure Data Box Gateway en Hyper-V](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[Aprovisionamiento de Azure Data Box Gateway en VMware](data-box-gateway-deploy-provision-vmware.md)**|Para Hyper-V, aprovisione un dispositivo virtual de Data Box Gateway en un sistema host que ejecuta Hyper-V en Windows Server 2016 o Windows Server 2012 R2, y conéctese a él. <br><br><br> En VMware, aprovisione un dispositivo virtual de Data Box Gateway en un sistema host que ejecuta VMware ESXi 6.0, 6.5 o 6.7, y conéctese a él.<br></br> |
| 3. |**[Conexión, configuración y activación de Data Box Gateway](data-box-gateway-deploy-connect-setup-activate.md)** |Conéctese a la interfaz de usuario web local, complete la configuración del dispositivo y active el dispositivo. A continuación, puede aprovisionar los recursos compartidos de SMB.  |
| 4. |**[Transferencia de datos con Data Box Gateway](data-box-gateway-deploy-add-shares.md)** |Agregue recursos compartidos y conéctese a ellos mediante SMB o NFS. |

Ya puede empezar a configurar Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Aquí encontrará los requisitos previos para configurar su recurso de Data Box Gateway, el dispositivo Data Box Gateway y la red de centros de datos.

### <a name="for-the-data-box-gateway-resource"></a>Para el recurso Data Box Gateway

Antes de comenzar, asegúrese de que:

* Su suscripción de Microsoft Azure debe estar habilitada para el recurso de Data Box Gateway.
* Tiene una cuenta de almacenamiento de Microsoft Azure con credenciales de acceso.

### <a name="for-the-data-box-gateway-device"></a>Para el dispositivo de Data Box Gateway

Antes de implementar un dispositivo virtual, asegúrese de que:

* Tiene acceso a un sistema host que ejecuta Hyper-V en Windows Server 2012 R2 o versiones posteriores o VMware (ESXi 6.0, 6.5 o 6.7) que puede usarse para aprovisionar un dispositivo.
* El sistema host puede dedicar los recursos siguientes para aprovisionar el dispositivo virtual Data Box:
  
  * Un mínimo de 4 núcleos.
  * Al menos 8 GB de RAM. 
  * Una interfaz de red.
  * Un disco de sistema operativo de 250 GB.
  * Un disco virtual de 2 TB para datos del sistema.

### <a name="for-the-datacenter-network"></a>Para la red del centro de datos

Antes de comenzar, asegúrese de que:

* La red del centro de datos está configurada según los requisitos de red para el dispositivo de Data Box Gateway. Para más información, consulte [Requisitos del sistema de Data Box Gateway](data-box-gateway-system-requirements.md).

* El recurso de Data Box Gateway tiene un ancho de banda de Internet de 20 Mbps (o más) disponible en todo momento. Este ancho de banda no debe compartirse con otras aplicaciones. Si usa la limitación de red para limitar el trabajo, se recomienda usar un ancho de banda de Internet de 32 Mbps o más.

## <a name="create-a-new-resource"></a>Crear un nuevo recurso

Realice los siguientes pasos para crear un nuevo recurso de Data Box Gateway. 

Si ya tiene un recurso de Data Box Gateway para administrar los dispositivos virtuales, omita este paso y vaya a [Obtención de la clave de activación](#get-the-activation-key).

Siga estos pasos en Azure Portal para crear un recurso de Data Box.
1. Use sus credenciales de Microsoft Azure para iniciar sesión en el Portal de vista previa de Azure, en esta dirección URL: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Seleccione la suscripción que desea usar para la versión preliminar de Data Box Edge. Seleccione la región donde desea implementar el recurso Data Box Edge. En la opción **Data Box Gateway**, haga clic en **Crear**.

    ![Busque el servicio Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-edge-sku.png)

3. Escriba o seleccione la siguiente información para el nuevo recurso.
    
    |Configuración  |Valor  |
    |---------|---------|
    |Nombre del recurso   | Nombre descriptivo que identifique el recurso.<br>El nombre tiene entre 2 y 50 caracteres que contiene letras, números y guiones.<br> El nombre comienza y termina con una letra o un número.        |
    |Subscription    |La suscripción está vinculada a la cuenta de facturación. |
    |Grupos de recursos  |Cree un nuevo grupo o seleccione uno existente.<br>Más información sobre los [grupos de recursos de Azure](../azure-resource-manager/resource-group-overview.md).     |
    |Ubicación     |En esta versión están disponibles Europa Occidental, Este de EE. UU., Oeste de EE.UU. 2, Sudeste Asiático y Europa Occidental. <br> Elija la ubicación más cercana a la región geográfica donde quiera implementar el dispositivo.|
    
    ![Creación de un recurso de Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-resource.png)
    
4. Haga clic en **OK**.
 
Se tarda unos minutos en crear el recurso. Recibirá una apropiada cuando el servicio se cree correctamente.


## <a name="download-the-virtual-device-image"></a>Descarga de la imagen del dispositivo virtual

Una vez creado el recurso de Data Box Gateway, descargue la imagen correspondiente al dispositivo virtual para aprovisionar un dispositivo virtual en el sistema host. Las imágenes de dispositivos virtuales son específicas del sistema operativo y pueden descargarse de la hoja **Inicio rápido** en Azure Portal.

> [!IMPORTANT]
> El software que se ejecuta en Data Box Gateway solo puede usarse con el recurso Data Box Gateway.


Siga estos pasos en [Azure Portal](https://portal.azure.com/).

1. Haga clic en el recurso que ha creado y, a continuación, haga clic en **Información general**. Si ya tiene un recurso de Azure Data Box Gateway, haga clic en él y vaya a **Información general**.

    ![Nuevo recurso de Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

4. En el inicio rápido del panel derecho, haga clic en el vínculo correspondiente a la imagen que quiere descargar. Los archivos de imagen tienen un tamaño aproximado de 4,8 GB.
   
   * [VHD para Hyper-V en Windows Server 2012 R2 y versiones posteriores](https://aka.ms/dbe-vhdx-2012).
   * [VMDK para VMWare ESXi 6.0, 6.5 o 6.7](https://aka.ms/dbe-vmdk).

5. Descargue y descomprima el archivo en una unidad local y tome nota de dónde se encuentra el archivo sin comprimir.


## <a name="get-the-activation-key"></a>Obtención de la clave de activación

Cuando el recurso de Data Box Gateway esté en funcionamiento, deberá obtener la clave de activación. Esta clave se utiliza para activar y conectar el dispositivo de Data Box Gateway con el recurso.

La clave de activación se usa para registrar todos los dispositivos de Data Box Gateway que se deben activar con el recurso de Data Box Gateway. Puede obtener esta clave ahora mientras está en Azure Portal.

1. Haga clic en el recurso que ha creado y, a continuación, haga clic en **Información general**.

    ![Nuevo recurso de Data Box Gateway](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. Haga clic en **Generar clave** para crear una nueva clave de activación. Haga clic en el icono de copia para copiar la clave y guárdela para su uso posterior.

    ![Obtención de la clave de activación](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - La clave de activación expira tres días después de generarla. 
> - Si la clave ha expirado, genere una nueva. La clave antigua no es válida.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Gateway, como:

> [!div class="checklist"]
> * Crear un nuevo recurso
> * Descarga de la imagen del dispositivo virtual
> * Obtención de la clave de activación

Pase al siguiente tutorial para aprender cómo aprovisionar una máquina virtual para Data Box Gateway. Según el sistema operativo host, consulte las instrucciones detalladas en:

> [!div class="nextstepaction"]
> [Aprovisionamiento de Data Box Gateway en Hyper-V](./data-box-gateway-deploy-provision-hyperv.md)

OR

> [!div class="nextstepaction"]
> [Aprovisionamiento de Data Box Gateway en VMware](./data-box-gateway-deploy-provision-vmware.md)



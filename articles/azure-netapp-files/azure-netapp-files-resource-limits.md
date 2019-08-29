---
title: Límites de recursos para Azure NetApp Files| Microsoft Docs
description: Describe los límites de recursos para Azure NetApp Files, incluidos los límites de las cuentas de NetApp, los grupos de capacidad, los volúmenes, instantáneas y la subred delegada.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: b-juche
ms.openlocfilehash: 4ce40fdf36f7d66e60e15955318e43f1f24f275f
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69515849"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Límites de recursos para Azure NetApp Files

Entender los límites de recursos de Azure NetApp Files ayuda a administrar los volúmenes.

## <a name="resource-limits"></a>Límites de recursos

En la tabla siguiente se describen los límites de recursos de Azure NetApp Files:

|  Recurso  |  Límite predeterminado  |  Ajustable a través de la solicitud de soporte técnico  |
|----------------|---------------------|--------------------------------------|
|  Número de cuentas de NetApp por suscripción de Azure   |  10    |  Sí   |
|  Número de grupos de capacidad por cuenta de NetApp   |    25     |   Sí   |
|  Número de volúmenes por grupo de capacidad     |    500   |    Sí     |
|  Número máximo de instantáneas por volumen       |    255     |    Sin        |
|  Número de subredes que se delegan a Azure NetApp Files (Microsoft.NetApp/volumes) por cada red virtual de Azure    |   1   |    Sin    |
|  Número de direcciones IP simultáneas en una red virtual (incluidas las redes virtuales emparejadas) con acceso a Azure NetApp Files   |    1000   |    Sin   |
|  Tamaño mínimo de un único grupo de capacidades   |  4 TiB     |    Sin  |
|  Tamaño máximo de un único grupo de capacidades    |  500 TiB   |   Sin   |
|  Tamaño mínimo de un único volumen    |    100 GiB    |    Sin    |
|  Tamaño máximo de un único volumen     |    100 TiB    |    Sin       |
|  Número máximo de archivos (inodes) por volumen     |    50 millones    |    Sin    |    

## <a name="request-limit-increase"></a>Solicitud de aumento del límite 

Puede crear una solicitud de soporte técnico de Azure para aumentar los límites ajustables de la tabla anterior. 

Desde el plano de navegación de Azure Portal: 

1. Haga clic en **Ayuda y soporte técnico**.
2. Haga clic en **Nueva solicitud de soporte técnico**.
3. En la pestaña Básico, especifique la siguiente información: 
    1. Tipo de problema: Seleccione **Límites de servicio y suscripción (cuotas)** .
    2. Suscripciones: Seleccione la suscripción del recurso que necesite aumentar la cuota.
    3. Tipo de cuota: Seleccione **Storage: Límites de Azure NetApp Files**.
    4. Haga clic en **Siguiente: Soluciones**.
4. Haga clic en la pestaña Detalles:
    1. En el cuadro Descripción, proporcione la siguiente información para el tipo de recurso correspondiente:

        |  Recurso  |    Recursos primarios      |    Nuevos límites solicitados     |    Motivo de aumento de cuota       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Cuenta |  *Subscription ID*   |  *Nuevo número máximo de **cuentas** solicitado*    |  *¿Qué escenario o caso de uso motivó la solicitud?*  |
        |  grupo    |  *Identificador de suscripción y URI de la cuenta*  |  *Nuevo número máximo de **grupos** solicitado*   |  *¿Qué escenario o caso de uso motivó la solicitud?*  |
        |  Volumen  |  *Identificador de suscripción, URI y URI de grupo*   |  *Nuevo número máximo de **volúmenes** solicitado*     |  *¿Qué escenario o caso de uso motivó la solicitud?*  |

    2. Especifique admite el método de soporte técnico adecuado y proporcione la información del contrato.

    3. Haga clic en **Siguiente: Revisar y crear** para crear la solicitud. 


## <a name="next-steps"></a>Pasos siguientes  

- [Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
- [Modelo de costo de Azure NetApp Files](azure-netapp-files-cost-model.md)

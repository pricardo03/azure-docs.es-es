---
title: Delegación de una subred en Azure NetApp Files | Microsoft Docs
description: Se describe cómo delegar una subred en Azure NetApp Files.
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
ms.topic: get-started-article
ms.date: 11/13/2018
ms.author: b-juche
ms.openlocfilehash: 8ec41c6db8c8e5c62d15dc0638762f2649c637b8
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53631658"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Delegación de una subred en Azure NetApp Files 

Debe delegar una subred en Azure NetApp Files.   Cuando se crea un volumen, debe especificar la subred delegada.

## <a name="about-this-task"></a>Acerca de esta tarea
* El asistente para crear una nueva subred se establece de manera predeterminada en una máscara de red /24, que ofrece 251 direcciones IP disponibles. Con una máscara de red /28, que ofrece 16 direcciones IP utilizables, es suficiente para el servicio.
* No se puede designar un grupo de seguridad de red o punto de conexión de servicio en la subred delegada. Si lo hace, se producirá un error en la delegación de subred.
* En cada red virtual de Azure (VNet), solo puede delegarse una subred a Azure NetApp Files.
* Actualmente no se admite el acceso a un volumen desde una red virtual emparejada.

## <a name="steps"></a>Pasos 
1.  Vaya a la hoja **Redes virtuales** de Azure Portal y seleccione la red virtual que desea usar para Azure NetApp Files.    

1. Seleccione **Subredes** en la hoja de la red virtual y haga clic en el botón **+ Subred**. 

1. Cree una nueva subred que se usará para Azure NetApp Files completando los siguientes campos obligatorios en la página Agregar subred:
    * **Nombre**: Especifique el nombre de la subred.
    * **Intervalo de direcciones**: Especifique el intervalo de direcciones IP.
    * **Delegación de subred**: Seleccione **Microsoft.NetApp/volumes**. 

      ![Delegación de subred](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
También puede crear y delegar una subred al [crear un volumen de Azure NetApp Files](azure-netapp-files-create-volumes.md). 

## <a name="next-steps"></a>Pasos siguientes  
* [Creación de un volumen de Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Obtenga información sobre la integración de redes virtuales para los servicios de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)



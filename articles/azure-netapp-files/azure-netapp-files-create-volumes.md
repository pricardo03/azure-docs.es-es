---
title: Creación de un volumen de Azure NetApp Files | Microsoft Docs
description: Describe cómo crear un volumen de Azure NetApp Files.
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
ms.date: 12/17/2018
ms.author: b-juche
ms.openlocfilehash: 0d4629651e17c917720196c1e5f6152c50c2a33a
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57770273"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Creación de un volumen de Azure NetApp Files

El consumo de la capacidad de un volumen se descuenta de la capacidad aprovisionada de su grupo.  Puede crear varios volúmenes en un grupo de capacidad, pero el consumo de la capacidad total de los volúmenes no puede superar el tamaño del grupo. 

## <a name="before-you-begin"></a>Antes de empezar 
Debe haber configurado un grupo de capacidad.   
[Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)   
Debe haber una subred delegada en Azure NetApp Files.  
[Delegación de una subred en Azure NetApp Files](azure-netapp-files-delegate-subnet.md)


## <a name="steps"></a>Pasos 
1.  Haga clic en la hoja **Volúmenes** de la hoja Manage Capacity Pools (Administración de grupos de capacidad). 
2.  Haga clic en **+ Agregar volumen** para crear un volumen.  
    Aparece la ventana Nuevo volumen.

3.  En la ventana Nuevo volumen, haga clic en **Crear** y proporcione la información para los campos siguientes:   
    * **Nombre**      
        Especifique el nombre para el volumen que va a crear.   

        El nombre debe ser único dentro de un grupo de recursos. Debe tener tres caracteres de longitud, como mínimo.  Puede usar cualquier carácter alfanumérico.

    * **Ruta de acceso del archivo**  
        Especifique la ruta de acceso de archivo que se usará para crear la ruta de acceso de exportación para el nuevo volumen. La ruta de acceso de exportación se usa para montar el volumen y tener acceso a él.   
     
        El nombre de la ruta de acceso de archivo solo puede contener letras, números y guiones ("-"). El nombre debe tener entre 16 y 40 caracteres.  

    * **Cuota**  
        Especifique la cantidad de almacenamiento lógico que se asigna al volumen.  

        El campo **Cuota disponible** muestra la cantidad de espacio no utilizado en el grupo de capacidad elegido que puede usar para crear un nuevo volumen. El tamaño del volumen nuevo no debe superar la cuota disponible.  

    * **Red virtual**  
        Especifique la red virtual de Azure (Vnet) desde la que desea tener acceso al volumen.  

        La red virtual que especifique debe tener una subred delegada en Azure NetApp Files. Solo puede tener acceso al servicio Azure NetApp Files desde la misma red virtual o desde una red virtual que se encuentre en la misma ubicación que el volumen mediante el emparejamiento de redes virtuales. También puede tener acceso al volumen desde la red local a través de Express Route.   

    * **Subred**  
        Especifique la subred que desea usar para el volumen.  
        La red virtual que especifique debe estar delegada en Azure NetApp Files. 
        
        Si no ha delegado una subred, haga clic en **Crear nuevo** en el volumen de creación de un volumen. A continuación, en la página de creación de la subred, especifique la información de la subred y seleccione **Microsoft.NetApp/volumes** para delegar la subred para Azure NetApp Files. Tenga en cuenta que, en cada red virtual, solo puede delegarse una subred a Azure NetApp Files.   
 
        ![Nuevo volumen](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Creación de una subred](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)


4.  Haga clic en **OK**. 
 
Un volumen hereda los atributos de ubicación, la suscripción y el grupo de recursos de su grupo de capacidad. Para supervisar el estado de la implementación del volumen, puede usar la pestaña Notificaciones.

## <a name="next-steps"></a>Pasos siguientes  
* [Configuración de la directiva de exportación para un volumen (opcional)](azure-netapp-files-configure-export-policy.md)
* [Obtenga información sobre la integración de redes virtuales para los servicios de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


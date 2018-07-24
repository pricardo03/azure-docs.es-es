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
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 42e475f4da2102bb8b010daec6e6451ba7f13848
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011099"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Creación de un volumen de Azure NetApp Files

El consumo de la capacidad de un volumen se descuenta de la capacidad aprovisionada de su grupo.  Puede crear varios volúmenes en un grupo de capacidad, pero el consumo de la capacidad total de los volúmenes no puede superar el tamaño del grupo. 

## <a name="before-you-begin"></a>Antes de empezar 
Debe haber configurado un grupo de capacidad.  

[Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)

## <a name="steps"></a>Pasos 
1.  Haga clic en la hoja **Volúmenes** de la hoja Manage Capacity Pools (Administración de grupos de capacidad). 
2.  Haga clic en **+ Agregar volumen** para crear un volumen.  
    Aparece la ventana Nuevo volumen.

3.  En la ventana Nuevo volumen, haga clic en **Crear** y proporcione la información para los campos siguientes:   
    * **Nombre**      
        Especifique el nombre para el volumen que va a crear.   
        Debe ser único dentro de un grupo de recursos. Debe tener tres caracteres de longitud, como mínimo.  Puede usar cualquier carácter alfanumérico.

    * **Ruta de acceso del archivo**  
        Especifique la ruta de acceso de archivo que se usará para crear la ruta de acceso de exportación para el nuevo volumen. La ruta de acceso de exportación se usa para montar el volumen y acceder a él.   
        Un destino de montaje es el punto de conexión de la dirección IP del servicio NFS. Se genera automáticamente.    
        El nombre de la ruta de acceso de archivo solo puede contener letras, números y guiones ("-"). El nombre debe tener entre 16 y 40 caracteres.  

    * **Cuota**  
        Especifique la cantidad de almacenamiento lógico que se asigna al volumen.  
        El campo **Cuota disponible** muestra la cantidad de espacio no utilizado en el grupo de capacidad elegido que puede usar para crear un nuevo volumen. El tamaño del volumen nuevo no debe superar la cuota disponible.  

    * **Red virtual**  
        Especifique la red virtual de Azure (Vnet) desde la que desea tener acceso al volumen. La red virtual que especifique debe tener configurado Azure NetApp Files. Solo puede tener acceso al servicio Azure NetApp Files desde una red virtual que se encuentre en la misma ubicación que el volumen.   

    ![Nuevo volumen](../media/azure-netapp-files/azure-netapp-files-new-volume.png)

4.  Haga clic en **OK**. 
 
Un volumen hereda los atributos de ubicación, la suscripción y el grupo de recursos de su grupo de capacidad. Para supervisar el estado de la implementación del volumen, puede usar la pestaña Notificaciones.

## <a name="next-steps"></a>Pasos siguientes  
[Configuración de la directiva de exportación para un volumen (opcional)](azure-netapp-files-configure-export-policy.md)


---
title: Administración de instantáneas mediante Azure NetApp Files | Microsoft Docs
description: Describe cómo crear una instantánea a petición para un volumen o restaurar desde una instantánea a un nuevo volumen mediante Azure NetApp Files.
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
ms.topic: how-to-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 48cb88b9815ba723d93c18caf63f33b50eea850c
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011983"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Administración de instantáneas mediante Azure NetApp Files
Puede usar Azure NetApp Files para crear una instantánea a petición para un volumen o restaurar desde una instantánea a un nuevo volumen.

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Creación de una instantánea a petición para un volumen
Solo puede crear instantáneas a petición.  Las directivas de instantánea no se admiten actualmente.  
1.  En la hoja Administración de volumen, haga clic en **Instantáneas** y, a continuación, en **+ Agregar instantánea** para crear una instantánea a petición para un volumen.

2.  En la ventana Nueva instantánea, proporcione un nombre para la nueva instantánea que va a crear.   

3. Haga clic en **OK**. 


## <a name="restore-a-snapshot-to-a-new-volume"></a>Restauración de una instantánea a un nuevo volumen
Actualmente, puede restaurar una instantánea solo a un nuevo volumen. 
1. Vaya a la hoja **Administración de instantáneas** desde la hoja Volumen para mostrar la lista de instantáneas. 
2. Seleccione una instantánea para restaurarla.  
3. Haga clic con el botón derecho en el nombre de la instantánea y seleccione **Restaurar al nuevo volumen** en la opción de menú.  

    ![Restauración de la instantánea al nuevo volumen](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. En la ventana Nuevo volumen, proporcione información para el nuevo volumen:  
    * **Nombre**   
        Especifique el nombre para el volumen que va a crear.  
        
        El nombre debe ser único dentro de un grupo de recursos. Debe tener tres caracteres de longitud, como mínimo.  Puede usar cualquier carácter alfanumérico.

    * **Ruta de acceso del archivo**     
        Especifique la ruta de acceso de archivo que se usará para crear la ruta de acceso de exportación para el nuevo volumen. La ruta de acceso de exportación se usa para montar el volumen y tener acceso a él.   
        
        Un destino de montaje es el punto de conexión de la dirección IP del servicio NFS. Se genera automáticamente.   
        
        El nombre de la ruta de acceso de archivo solo puede contener letras, números y guiones ("-"). El nombre debe tener entre 16 y 40 caracteres. 

    * **Cuota**  
        Especifique la cantidad de almacenamiento lógico que se asigna al volumen.  

        El campo **Cuota disponible** muestra la cantidad de espacio no utilizado en el grupo de capacidad elegido que puede usar para crear un nuevo volumen. El tamaño del volumen nuevo no debe superar la cuota disponible.

    *   **Red virtual**  
        Especifique la red virtual de Azure (Vnet) desde la que desea tener acceso al volumen. 
        
        La red virtual que especifique debe tener configurado Azure NetApp Files. Solo puede tener acceso al servicio Azure NetApp Files desde una red virtual que se encuentre en la misma ubicación que el volumen.  

    ![Nuevo volumen restaurado](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
    
5. Haga clic en **OK**.   
    El nuevo volumen al que se restaura la instantánea aparece en la hoja Volúmenes.


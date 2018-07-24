---
title: Configuración de un grupo de capacidad de Azure NetApp Files | Microsoft Docs
description: Describe cómo configurar un grupo de capacidad para poder crear volúmenes en él.
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
ms.openlocfilehash: 0e9203f5b4e2a9043e242b804c82017cf6fc3ee1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010810"
---
# <a name="set-up-a-capacity-pool"></a>Configuración de un grupo de capacidad
Configurar un grupo de capacidad le permite crear volúmenes en él.  

## <a name="before-you-begin"></a>Antes de empezar 
Debe haber creado ya una cuenta de NetApp.   

[Creación de una cuenta de NetApp](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>Pasos 

1. Vaya a la hoja de administración de su cuenta de NetApp y, a continuación, en el panel de navegación, seleccione **Grupos de capacidad**.

2. Haga clic en **+ Agregar grupos** para crear un nuevo grupo de capacidad.   
    Aparecerá el cuadro de diálogo Nuevo grupo de capacidad.

3. Proporcione la siguiente información para el nuevo grupo de capacidad:  
  * **Name**  
    Especifique el nombre para el grupo de capacidad.  
    El nombre del grupo de capacidad debe ser único para cada cuenta de NetApp.

  * **Nivel de servicio**   
    Este campo muestra el rendimiento de destino para el grupo de capacidad.  
    Actualmente, solo está disponible el nivel de servicio Premium. 

  *  **Tamaño**     
      Especifique el tamaño del grupo de capacidad que va a adquirir.        
      El tamaño mínimo del grupo de capacidad es de 4 TiB. Puede crear un grupo con un tamaño que sea múltiplo de 4 TiB.   
      
      ![Nuevo grupo de capacidad](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. Haga clic en **OK**.

## <a name="next-steps"></a>Pasos siguientes 

1. [Creación de un volumen de Azure NetApp Files](azure-netapp-files-create-volumes.md)
2. [Configuración de la directiva de exportación para un volumen (opcional)](azure-netapp-files-configure-export-policy.md)


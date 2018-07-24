---
title: Configuración de la directiva de exportación para un volumen de Azure NetApp Files | Microsoft Docs
description: Describe cómo configurar la directiva de exportación para controlar el acceso a un volumen de Azure NetApp Files
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
ms.openlocfilehash: 08de7e2ca8cd993a0931f5b16cb9d6c9a04e53dc
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010963"
---
# <a name="configure-export-policy-for-a-volume-optional"></a>Configuración de la directiva de exportación para un volumen (opcional)

Si lo desea, puede configurar la directiva de exportación para controlar el acceso a un volumen de Azure NetApp Files. 

## <a name="steps"></a>Pasos 

1.  Haga clic en la hoja **Create Export Policy** (Crear directiva de exportación) en la hoja Manage Volume (Administrar volúmenes). 

2.  Especifique la información de los campos siguientes para crear una regla de directiva de exportación:   
    *  **Índice**   
        Especifique el número de índice para la regla.  
        Una directiva de exportación puede constar de hasta cinco reglas. Las reglas se evalúan según el orden en la lista de números de índice. Primero se evalúan las reglas con números de índice inferior. Por ejemplo, la regla con el número de índice 1 se evalúa antes que la que tenga el número de índice 2. 

    * **Clientes permitidos**   
        Especifique el valor en uno de los siguientes formatos:  
        * Dirección IPv4, por ejemplo, `10.1.12.24` 
        * Dirección IPv4 con una máscara de subred expresada como un número de bits, por ejemplo, `10.1.12.10/4`

    * **Access**  
        Seleccione uno de los tipos de accesos siguientes:  
        * Sin acceso 
        * Lectura y escritura
        * Solo lectura

    * **Protocolos**   
        Especifique el protocolo que se usará para la directiva de exportación.   
        Actualmente, Azure NetApp Files solo admite NFSv3.

    ![Directiva de exportación](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 


## <a name="next-steps"></a>Pasos siguientes 
* [Administración de volúmenes](azure-netapp-files-manage-volumes.md)
* [Instalación o desinstalación de un volumen para máquinas virtuales](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Administración de instantáneas](azure-netapp-files-manage-snapshots.md)

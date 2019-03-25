---
title: Configurar la directiva de exportación para un volumen NFS con Azure Files de NetApp | Microsoft Docs
description: Describe cómo configurar la directiva de exportación para controlar el acceso a un volumen NFS con Azure Files de NetApp
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
ms.date: 03/20/2019
ms.author: b-juche
ms.openlocfilehash: 8cda5921a1aec86d28beabbd9cea5b07a203a0e8
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401484"
---
# <a name="configure-export-policy-for-an-nfs-volume"></a>Configurar la directiva de exportación para un volumen NFS

Si lo desea, puede configurar la directiva de exportación para controlar el acceso a un volumen de Azure NetApp Files. Directiva de exportación es compatible sólo con volúmenes NFS. 

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

    * **Acceder**  
        Seleccione uno de los tipos de accesos siguientes:  
        * Sin acceso 
        * Leer y escribir
        * Sólo lectura

    * **Protocolos**   
        Especifique el protocolo que se usará para la directiva de exportación.   
        Actualmente, la directiva de exportación de archivos de NetApp Azure admite solo NFSv3.

    ![Exportar directiva](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 


## <a name="next-steps"></a>Pasos siguientes 
* [Administración de volúmenes](azure-netapp-files-manage-volumes.md)
* [Instalación o desinstalación de un volumen para máquinas virtuales](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Administración de instantáneas](azure-netapp-files-manage-snapshots.md)

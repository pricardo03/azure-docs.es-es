---
title: Montaje o desmontaje un volumen de Azure NetApp Files para máquinas virtuales Windows o Linux | Microsoft Docs
description: Describe cómo montar o desmontar un volumen para máquinas virtuales o máquinas virtuales Linux.
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
ms.date: 03/07/2019
ms.author: b-juche
ms.openlocfilehash: 76e01055043932f2c7e7d57bd7eed6265d666a8c
ms.sourcegitcommit: bd4198a3f2a028f0ce0a63e5f479242f6a98cc04
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2019
ms.locfileid: "72302777"
---
# <a name="mount-or-unmount-a-volume-for-windows-or-linux-virtual-machines"></a>Montaje o desmontaje de un volumen para máquinas virtuales Linux 

Puede montar o desmontar un volumen para máquinas virtuales Windows o Linux según sea necesario.  Las instrucciones de montaje para las máquinas virtuales Linux están disponibles en Azure NetApp Files.  

1. Haga clic en la hoja **Volúmenes** y seleccione el volumen para el que desea realizar el montaje. 
2. Haga clic en **Instrucciones de montaje** en el volumen seleccionado y siga las instrucciones para montar el volumen. 

    ![Instrucciones de montaje NFS](../media/azure-netapp-files/azure-netapp-files-mount-instructions-nfs.png)

    ![Instrucciones de montaje SMB](../media/azure-netapp-files/azure-netapp-files-mount-instructions-smb.png)
    
Si usa NFSv4.1, utilice el siguiente comando para montar el sistema de archivos: `sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=4.1,tcp,sec=sys $MOUNTTARGETIPADDRESS:/$VOLUMENAME $MOUNTPOINT`

---
title: Configuración de Microsoft Azure Data Box Disk | Microsoft Docs
description: Use este tutorial para saber cómo configurar Azure Data Box Disk
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/28/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 6fcc7823a7e2f2f1e280622a1fa05d4417a71546
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143489"
---
# <a name="tutorial-unpack-connect-and-unlock-azure-data-box-disk"></a>Tutorial: Desempaquetado, conexión y desbloqueo de Azure Data Box Disk

Este tutorial describe cómo desempaquetar, conectar y desbloquear Azure Data Box Disk.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Desempaquetado de Data Box Disk
> * Conéctese y desbloquee Data Box Disk.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que:

1. Ha completado el [Tutorial: Pedido de Azure Data Box Disk](data-box-disk-deploy-ordered.md).
2. Ha recibido los discos y el estado del trabajo en el portal se actualiza a **Delivered** (Entregado).
3. Tiene un equipo host en el que puede instalar la herramienta de desbloqueo de Data Box Disk. El equipo host debe:
    - Ejecutar un [sistema operativo admitido](data-box-disk-system-requirements.md).
    - Tener [instalado Windows PowerShell 4](https://www.microsoft.com/download/details.aspx?id=40855).
    - Tener [instalado .NET Framework 4.5.1](https://www.microsoft.com/download/details.aspx?id=30653).
    - Tener [BitLocker habilitado](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server).
    - Tener [instalado Windows Management Framework 4](https://www.microsoft.com/en-us/download/details.aspx?id=40855). 

## <a name="unpack-your-disks"></a>Desempaquetado de los discos

 Realice los pasos siguientes para desempaquetar los discos.

1. Data Box Disk se envía por correo en una caja pequeña. Abra la caja cuadro y extraiga su contenido. Compruebe que la caja contiene de 1 a 5 discos de estado sólido (SSD) y un cable USB de conexión por cada disco. Inspeccione la caja para comprobar si hay alguna evidencia de que haya sido alterada o de que haya sufrido algún otro daño evidente. 

    ![Paquete de envío de Data Box Disk](media/data-box-disk-deploy-set-up/data-box-disk-ship-package1.png)

2. Si la caja de envío ha sido manipulada o ha sufrido daños graves, no la abra. Póngase en contacto con el Soporte técnico de Microsoft para que le ayude a evaluar si los discos están en buenas condiciones y si debe devolverlos para que se los cambien.
3. Compruebe que la caja tiene una funda clara que contiene una etiqueta de envío (bajo la etiqueta actual) para el envío de devolución. Si esta etiqueta se pierde o se daña, siempre puede descargar e imprimir una nueva desde Azure Portal. 

    ![Etiqueta de envío de Data Box Disk](media/data-box-disk-deploy-set-up/data-box-disk-package-ship-label.png)

4. Guarde la caja y la espuma de embalaje para el envío de devolución de los discos.

## <a name="connect-and-unlock-your-disks"></a>Conexión y desbloqueo de los discos

Realice los pasos siguientes para conectarse a los discos y para desbloquearlos.

1. Utilice el cable incluido para conectar el disco a un equipo Windows que ejecute un sistema operativo compatible como se indica en los requisitos previos. 

    ![Conexión de Data Box Disk](media/data-box-disk-deploy-set-up/data-box-disk-connect-unlock.png)    
    
2. En Azure Portal, vaya a **General > Detalles del dispositivo**. 
3. Haga clic en **Download Data Box Disk unlock tool** (Descargar herramienta de desbloqueo de Data Box Disk). 

    ![Descarga de la herramienta de desbloqueo de Data Box Disk](media/data-box-disk-deploy-set-up/data-box-disk-unload1.png)     

4. Extraiga la herramienta en el mismo equipo que va a utilizar para copiar los datos.
5. Abra una ventana del símbolo del sistema o ejecute Windows PowerShell como administrador en el mismo equipo.
6. (Opcional) Para comprobar que el equipo que va a usar para desbloquear el disco cumple los requisitos del sistema operativo, ejecute el comando de comprobación del sistema. A continuación se muestra una salida de ejemplo. 

    ```powershell
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /SystemCheck
    Successfully verified that the system can run the tool.
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ``` 

7. En Azure Portal, vaya a **General > Detalles del dispositivo**. Use el icono de copia para copiar la clave de paso.
8. Ejecute `DataBoxDiskUnlock.exe` y proporcione la clave de paso. Se muestra la letra de unidad asignada en el disco. A continuación se muestra una salida de ejemplo.

    ```powershell
    PS C:\WINDOWS\system32> cd C:\DataBoxDiskUnlockTool\DiskUnlock
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe
    Enter the passkeys (format: passkey1;passkey2;passkey3):
    testpasskey1
    
    Following volumes are unlocked and verified.
    Volume drive letters: D:
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```

9. Repita los pasos 6 a 8 cuando vuelva a insertar discos. Use el comando de ayuda si necesita ayuda con la herramienta de desbloqueo de Data Box Disk.   

    ```powershell
    PS C:\DataBoxDiskUnlockTool\DiskUnlock> .\DataBoxDiskUnlock.exe /help
    USAGE:
    DataBoxUnlock /PassKeys:<passkey_list_separated_by_semicolon>
    
    Example: DataBoxUnlock /PassKeys:<your passkey>
    Example: DataBoxUnlock /SystemCheck
    Example: DataBoxUnlock /Help
    
    /PassKeys:       Get this passkey from Azure DataBox Disk order. The passkey unlocks your disks.
    /SystemCheck:    This option checks if your system meets the requirements to run the tool.
    /Help:           This option provides help on cmdlet usage and examples.
    
    PS C:\DataBoxDiskUnlockTool\DiskUnlock>
    ```  
10. Una vez que el disco está desbloqueado, puede ver su contenido.    

    ![Contenido de Data Box Disk](media/data-box-disk-deploy-set-up/data-box-disk-content.png) 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido sobre temas relacionados con Azure Data Box Disk; por ejemplo:

> [!div class="checklist"]
> * Desempaquetado de Data Box Disk
> * Conexión y desbloqueo de Data Box Disk


Pase al siguiente tutorial para aprender a copiar datos en Data Box Disk.

> [!div class="nextstepaction"]
> [Copia de datos en Data Box Disk](./data-box-disk-deploy-copy-data.md)


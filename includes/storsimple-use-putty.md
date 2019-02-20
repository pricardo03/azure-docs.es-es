---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: a5c62c67cd401c043352b06e6e6070a7fc0f1296
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55889709"
---
#### <a name="to-connect-through-the-serial-console"></a>Para conectarse a través de la consola serie
1. Conecte el cable serie al dispositivo (directamente o a través de un adaptador USB-serie).
2. Abra el **Panel de control** y, a continuación, abra el **Administrador de dispositivos**.
3. Identifique el puerto COM como se muestra en la siguiente ilustración.
   
     ![Conexión a través de la consola serie](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. Inicie PuTTY. 
5. En el panel derecho, cambie el **Tipo de conexión** a **Serie**.
6. En el panel derecho, escriba el puerto COM adecuado. Asegúrese de que los parámetros de configuración en serie se establecen como sigue:
   
   * Velocidad: 115 200
   * Bits de datos: 8
   * Bits de parada: 1
   * Paridad: None
   * Control de flujo: None
     
      Esta configuración se muestra en la siguiente ilustración.
     
     ![Configuración de PuTTY](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > Si la opción de control de flujo predeterminado no funciona, pruebe a establecer el control de flujo en XON/XOFF.
     > 
     > 
7. Haga clic en **Abrir** para iniciar una sesión en serie.


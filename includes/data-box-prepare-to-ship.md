---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: a8070d25e2606d8ad72ac231a0a208072c612c5c
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59737291"
---
El paso final consiste en preparar el dispositivo para el envío. En este paso, se desconectan todos los recursos compartidos de dispositivo. Una vez que empieza este proceso, no se puede acceder a los recursos compartidos.

> [!IMPORTANT]
> La preparación para el envío es necesario, ya que marca datos que no cumplen las convenciones de nomenclatura de Azure. Si omite este paso, podría provocar un error de carga en los datos por falta de cumplimiento.

1. Vaya a **Prepare to ship** (Preparación para el envío) y haga clic en **Start preparation** (Iniciar preparación). De forma predeterminada, las sumas de comprobación se procesan alineadas durante la acción de preparar para enviar. El proceso de la suma de comprobación puede tardar de horas a días, dependiendo del tamaño de los datos. 
   
    ![Preparación para el envío 1](media/data-box-prepare-to-ship/prepare-to-ship1.png)

    Si por algún motivo desea detener la preparación del dispositivo, haga clic en **Detener preparación**. Puede reanudar la preparación para el envío más adelante.
        
    ![Preparación para el envío 2](media/data-box-prepare-to-ship/prepare-to-ship2.png)
    
2. La preparación para el envío se inicia y se desconectan los recursos compartidos del dispositivo. Verá un aviso para descargar la etiqueta de envío una vez que el dispositivo esté listo.

    ![Aviso de descarga de la etiqueta de envío](media/data-box-prepare-to-ship/download-shipping-label-reminder.png)

3. Una vez completada la preparación del dispositivo, su estado se actualiza a *Ready to ship* (Listo para enviar) y el dispositivo se bloquea.
        
    ![Preparación para el envío 3](media/data-box-prepare-to-ship/prepare-to-ship3.png)

    Si desea copiar más datos en el dispositivo, puede desbloquearlo, copiar más datos y ejecutar la preparación para el envío de nuevo.

    Si hay errores en este paso, deberá descargar el registro de errores y resolverlos. Una vez hecho esto, ejecute **Preparación para el envío**.

4. Una vez completada correctamente (sin errores) la preparación para el envío, descargue la lista de archivos (también conocida como el manifiesto) que se copiaron en este proceso. Más adelante, puede usar esta lista para comprobar los archivos se cargaron en Azure.
        
    ![Preparación para el envío 1](media/data-box-prepare-to-ship/prepare-to-ship4.png)

5. Apague el dispositivo. Vaya a la página **Shut down or restart** (Apagar o reiniciar) y haga clic en **Shut down** (Apagar). Cuando se le pida confirmación, haga clic en **OK** (Aceptar) para continuar.

6. Retire los cables. El siguiente paso es enviar el dispositivo a Microsoft.

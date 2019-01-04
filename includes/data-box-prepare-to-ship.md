---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 11/13/2018
ms.author: alkohli
ms.openlocfilehash: 4efaaa0e3a1439a94a693aed13bf5c60563b4b29
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53549782"
---
El paso final consiste en preparar el dispositivo para el envío. En este paso, se desconectan todos los recursos compartidos de dispositivo. Una vez que se empiece a preparar el dispositivo para su envío, no se podrá acceder a los recursos compartidos.
1. Vaya a **Prepare to ship** (Preparación para el envío) y haga clic en **Start preparation** (Iniciar preparación). 
   
    ![Preparación para el envío 1](media/data-box-prepare-to-ship/prepare-to-ship1.png)

2. De forma predeterminada, las sumas de comprobación se procesan alineadas durante la acción de preparar para enviar. El proceso de la suma de comprobación puede tardar un tiempo en función del tamaño de los datos. Haga clic en **Start preparation** (Iniciar preparación).
    1. Los recursos compartidos de dispositivo se desconectan y el dispositivo se bloquea cuando se prepara para el envío.
        
        ![Preparación para el envío 1](media/data-box-prepare-to-ship/prepare-to-ship2.png) 
   
    2. Una vez completada la preparación del dispositivo, su estado se actualiza a *Ready to ship* (Listo para enviar). 
        
        ![Preparación para el envío 1](media/data-box-prepare-to-ship/prepare-to-ship3.png)

    3. Descargue la lista de archivos (manifiesto) que se copiaron en este proceso. Más adelante, puede usar esta lista para comprobar los archivos se cargaron en Azure.
        
        ![Preparación para el envío 1](media/data-box-prepare-to-ship/prepare-to-ship4.png)

3. Apague el dispositivo. Vaya a la página **Shut down or restart** (Apagar o reiniciar) y haga clic en **Shut down** (Apagar). Cuando se le pida confirmación, haga clic en **OK** (Aceptar) para continuar.
4. Retire los cables. El siguiente paso es enviar el dispositivo a Microsoft.

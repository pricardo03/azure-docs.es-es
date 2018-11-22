---
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 11/09/2018
ms.author: danlep
ms.openlocfilehash: a8ea55a40f1ee4681b6aec147e02b7bce6f1d7cf
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279915"
---
### <a name="retrieve-output-files"></a>Recuperación de archivos de salida

Puede usar Azure Portal para descargar los archivos MP3 de salida generados por las tareas de ffmpeg. 

1. Haga clic en **Todos los servicios** > **Cuentas de almacenamiento** y haga clic en el nombre de la cuenta de almacenamiento.
2. Haga clic en **Blobs** > *salida*.
3. Haga clic con el botón derecho en uno de los archivos MP3 de salida y, después, haga clic en **Descargar**. Para abrir o guardar el archivo, siga las indicaciones del explorador.

![Descargar un archivo de salida](./media/batch-common-tutorial-download/download.png)

Aunque no se muestra en este ejemplo, los archivos también se pueden descargar mediante programación desde los nodos de proceso o desde el contenedor de almacenamiento.

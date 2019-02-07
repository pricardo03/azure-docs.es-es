---
title: Crear y ejecutar trabajos en la aplicación de Azure IoT Central | Microsoft Docs
description: Los trabajos de Azure IoT Central permiten funcionalidades de administración de dispositivos de forma masiva, como actualizar una propiedad de dispositivo, configuración o ejecutar un comando.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 02/04/2019
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: e418ec7d22622c341abd972763d78ac2f0df46d9
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55772835"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Crear y ejecutar un trabajo en la aplicación de Azure IoT Central

Puede usar Microsoft Azure IoT Central para administrar los dispositivos conectados a escala mediante trabajos. La funcionalidad de trabajos le permite realizar actualizaciones masivas a las propiedades del dispositivo, la configuración y los comandos. Este artículo le guiará a través de cómo empezar a usar trabajos en su propia aplicación.

## <a name="create-and-run-a-job"></a>Crear y ejecutar un trabajo

En esta sección se muestra cómo crear y ejecutar un trabajo. Cada paso se pasa por un ejemplo que muestra cómo ejecutar un trabajo para los dispositivos de máquinas expendedoras refrigerados que deben aumentar la velocidad del ventilador.

1. Navegar a los trabajos desde el panel de navegación.

1. Haga clic en **+ Nuevo** para empezar a crear un nuevo trabajo.

    ![Crear nuevo trabajo](./media/howto-run-a-job-experimental/createnewjob.png)

1. Escriba un nombre y una descripción que le ayudarán a identificar el trabajo que va a crear.

1. Seleccione el conjunto de dispositivos al que desea que se aplique el trabajo. Después de seleccionar el conjunto de dispositivos, podrá ver que el lado derecho se llena con los dispositivos dentro del conjunto de dispositivos seleccionado. Si selecciona un conjunto de dispositivos roto, no se mostrará ningún dispositivo y verá un mensaje que explica que el conjunto de dispositivos está roto.

1. Después, elija el tipo de trabajo que se definirá (una configuración, una propiedad o un comando). Haga clic en **+** junto al tipo de trabajo seleccionado y agregue las operaciones deseadas.

    ![Configurar trabajo](./media/howto-run-a-job-experimental/configurejob.png)

1. En el lado derecho, elija los dispositivos en los que desea ejecutar el trabajo. Al hacer clic en la casilla de verificación superior, se seleccionan todos los dispositivos en todo el conjunto de dispositivos. Al hacer clic en la casilla de verificación junto a Nombre, se seleccionan todos los dispositivos en la página actual.

1. Una vez que se hayan seleccionado los dispositivos deseados, elija **Ejecutar**. El trabajo aparecerá ahora en la página **Trabajos** principal. En esta vista, podrá ver su trabajo en ejecución actualmente y el historial de los trabajos ejecutados anteriormente. Su trabajo en ejecución siempre se mostrará en la parte superior de la lista.

    ![Ver trabajo](./media/howto-run-a-job-experimental/viewjob.png)

    > [!NOTE]
    > Podrá ver el historial de los trabajos ejecutados en los últimos 30 días.

1. Para obtener información general de su trabajo, haga clic en el nombre del trabajo que desea ver en la lista. Esta información general contiene detalles del trabajo, los dispositivos y los estados de dispositivo.

    ![Ver el estado del dispositivo](./media/howto-run-a-job-experimental/viewdevicestatus.png)

### <a name="stop-a-running-job"></a>Detener un trabajo en ejecución

Si desea detener un trabajo que se está ejecutando, haga clic en el nombre del trabajo en ejecución que desea detener. Elija el botón **Detener** en el panel. Podrá ver que el estado del trabajo ha cambiado para reflejar que el trabajo se ha detenido.

   ![Detención de trabajo](./media/howto-run-a-job-experimental/stopjob.png)

### <a name="run-a-stopped-job"></a>Ejecución de un trabajo detenido

Si desea ejecutar un trabajo que se está detenido, haga clic en el nombre del trabajo en ejecución que desea ejecutar. Elija el botón **Ejecutar** en el panel. Verá que el estado del trabajo ha cambiado para reflejar que se ha ejecutado de nuevo.

   ![Trabajo reanudado](./media/howto-run-a-job-experimental/resumejob.png)

## <a name="view-the-job-status"></a>Ver el estado del trabajo

Una vez creado un trabajo, la columna **Estado** se actualizará con el mensaje de estado más reciente del trabajo. Los mensajes de estado significan lo siguiente:

| Mensaje de estado       | Significado de estado                                          |
| -------------------- | ------------------------------------------------------- |
| Completed            | Este trabajo se ha ejecutado en todos los dispositivos.              |
| Con error               | Este trabajo tiene errores y no se ejecutó completamente en los dispositivos.  |
| Pending              | Este trabajo no ha comenzado aún a ejecutarse en los dispositivos.        |
| En ejecución              | Este trabajo se está ejecutando actualmente en los dispositivos.             |
| Stopped              | El usuario ha detenido manualmente este trabajo.           |

El mensaje de estado va seguido de una visión general de los dispositivos dentro del trabajo. Estos estados de dispositivo significan lo siguiente:

| Mensaje de estado       | Significado de estado                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Succeeded            | El número de dispositivos en los que se ha ejecutado correctamente el trabajo.  |
| Con error               | El número de dispositivos en los que no se ha ejecutado correctamente el trabajo.      |

### <a name="view-the-device-status"></a>Ver el estado del dispositivo

Para ver el estado de cada dispositivo en el trabajo, haga clic en el nombre del trabajo. Aquí verá los detalles del trabajo y todos los dispositivos que forman parte de este trabajo específico. Junto a cada nombre de dispositivo, verá uno de los mensajes de estado siguientes:

| Mensaje de estado       | Significado de estado                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Completed            | Se ha ejecutado el trabajo en este dispositivo.                                     |
| Con error               | No se ha podido ejecutar el trabajo en este dispositivo. El mensaje de error mostrará más información.  |
| Pending              | Aún no se ha ejecutado el trabajo en este dispositivo.                                  |

> [!NOTE]
> Si se ha eliminado un dispositivo, no podrá seleccionar el dispositivo y se mostrará como eliminado en el identificador de dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear trabajos en la aplicación de Azure IoT Central, estos son algunos de los pasos siguientes:

- [Uso de conjuntos de dispositivos](howto-use-device-sets-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Administración de dispositivos](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Versión de la plantilla de dispositivo](howto-version-devicetemplate.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)

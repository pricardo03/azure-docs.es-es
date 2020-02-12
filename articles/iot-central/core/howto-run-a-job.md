---
title: Crear y ejecutar trabajos en la aplicación de Azure IoT Central | Microsoft Docs
description: Los trabajos de Azure IoT Central permiten funcionalidades de administración de dispositivos de forma masiva, como actualizar una propiedad de dispositivo, configuración o ejecutar un comando.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 07/08/2019
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 68d4dbff364f8d3fda72fc2377722031e9cccc3d
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77018897"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Crear y ejecutar trabajos en la aplicación de Azure IoT Central

Puede usar Microsoft Azure IoT Central para administrar los dispositivos conectados a escala mediante trabajos. Los trabajos le permiten realizar actualizaciones masivas de los comandos y las propiedades de los dispositivos. Este artículo le guiará en el proceso de cómo empezar a usar trabajos en su propia aplicación.


## <a name="create-and-run-a-job"></a>Crear y ejecutar un trabajo

En esta sección se muestra cómo crear y ejecutar un trabajo. Muestra cómo aumentar la velocidad del ventilador para varias máquinas expendedoras refrigeradas.

1. Navegar a los trabajos desde el panel de navegación.

2. Haga clic en **+ New** (+ Nuevo) para crear un nuevo trabajo.

    ![Crear nuevo trabajo](./media/howto-run-a-job/createnewjob.png)

3. Escriba un nombre y una descripción para identificar el trabajo que va a crear.

4. Seleccione el grupo de dispositivos al que desee que se aplique el trabajo. Puede ver el número de dispositivos a los que se aplicará la configuración del trabajo en la sección Resumen. 

5. Después, elija el tipo de trabajo para definir (una propiedad o un comando). Establezca la configuración del trabajo mediante la selección de la propiedad y establezca los nuevos valores o elija un comando. Es posible agregar varias propiedades a la vez.

    ![Configurar trabajo](./media/howto-run-a-job/configurejob.png)

6. En el lado derecho, elija los dispositivos en los que desee ejecutar el trabajo. Al activar la casilla superior, se seleccionan todos los dispositivos en todo el conjunto de dispositivos. Al activar la casilla junto a **Nombre**, se seleccionan todos los dispositivos en la página actual.

7. Después de seleccionar los dispositivos, elija **Ejecutar** o **Guardar**. El trabajo aparecerá ahora en la página principal **Trabajos**. En esta vista, podrá ver su trabajo en ejecución actualmente y el historial de los trabajos ejecutados anteriormente. Su trabajo en ejecución siempre se mostrará en la parte superior de la lista. El trabajo guardado se puede abrir de nuevo en cualquier momento para continuar con la edición o para ejecutarlo.

    ![Ver trabajo](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > Podrá ver el historial de los trabajos ejecutados en los últimos 30 días.

7. Para ver una descripción general del trabajo, seleccione el trabajo que ver en la lista. Esta información general contiene detalles del trabajo, los dispositivos y los valores de estado del dispositivo. En esta descripción general también puede seleccionar **Descargar detalles del trabajo** para descargar un archivo .csv de los detalles del trabajo, incluidos los dispositivos y sus valores de estado. Esta información puede ser útil para solucionar problemas.

    ![Ver el estado del dispositivo](./media/howto-run-a-job/downloaddetails.png)

### <a name="stop-a-running-job"></a>Detener un trabajo en ejecución

Para detener un trabajo en ejecución, selecciónelo y elija **Detener**. El estado del trabajo cambia para reflejar que el trabajo se ha detenido.

   ![Detención de trabajo](./media/howto-run-a-job/stopjob.png)

### <a name="run-a-stopped-job"></a>Ejecución de un trabajo detenido

Para ejecutar un trabajo que esté detenido actualmente, seleccione el trabajo detenido. Elija **ejecutar** en el panel. El estado del trabajo cambia para reflejar que el trabajo está de nuevo en ejecución.

   ![Trabajo reanudado](./media/howto-run-a-job/resumejob.png)

## <a name="copy-a-job"></a>Copia de un trabajo

Para copiar un trabajo existente que ha creado, abra un trabajo que se haya creado y seleccione **Copiar**. Se abrirá una nueva copia de la configuración del trabajo para que la pueda editar. Puede guardar o ejecutar el trabajo de nuevo. 

   ![Copia de trabajo](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>Ver el estado del trabajo

Una vez creado un trabajo, la columna **Estado** se actualizará con el mensaje de estado más reciente del trabajo. En la tabla siguiente se muestran los valores de estado posibles:

| Mensaje de estado       | Significado de estado                                          |
| -------------------- | ------------------------------------------------------- |
| Completed            | Este trabajo se ha ejecutado en todos los dispositivos.              |
| Con error               | Este trabajo tiene errores y no se ejecutó completamente en los dispositivos.  |
| Pending              | Este trabajo no ha comenzado a ejecutarse en los dispositivos aún.         |
| En ejecución              | Este trabajo se está ejecutando actualmente en los dispositivos.             |
| Detenido              | El usuario ha detenido manualmente este trabajo.           |

El mensaje de estado va seguido de una descripción general de los dispositivos en el trabajo. En la tabla siguiente se muestran los valores de estado del dispositivo posibles:

| Mensaje de estado       | Significado de estado                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Correcto            | El número de dispositivos en los que se ha ejecutado correctamente el trabajo.       |
| Con error               | El número de dispositivos en los que no se ha ejecutado correctamente el trabajo.       |

### <a name="view-the-device-status"></a>Ver el estado del dispositivo

Para ver el estado del trabajo y todos los dispositivos afectados, seleccione el trabajo. Seleccione **Descargar detalles del trabajo** para descargar un archivo .csv con los detalles del trabajo, incluida la lista de dispositivos y sus valores de estado. Verá uno de los mensajes de estado siguientes junto a cada nombre de dispositivo:

| Mensaje de estado       | Significado de estado                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Completed            | Se ha ejecutado el trabajo en este dispositivo.                                     |
| Con error               | No se ha podido ejecutar el trabajo en este dispositivo. El mensaje de error muestra más información.  |
| Pending              | Aún no se ha ejecutado el trabajo en este dispositivo.                                   |

> [!NOTE]
> Si se ha eliminado un dispositivo, no podrá seleccionar el dispositivo y se mostrará como eliminado en el identificador de dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear trabajos en la aplicación de Azure IoT Central, estos son algunos de los pasos siguientes:

- [Administración de dispositivos](howto-manage-devices.md)
- [Versión de la plantilla de dispositivo](howto-version-device-template.md)

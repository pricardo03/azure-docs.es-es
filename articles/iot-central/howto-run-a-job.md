---
title: Crear y ejecutar trabajos en la aplicación de Azure IoT Central | Microsoft Docs
description: Los trabajos de Azure IoT Central permiten funcionalidades de administración de dispositivos de forma masiva, como actualizar una propiedad de dispositivo, configuración o ejecutar un comando.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 03/18/2019
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: bb5eb2904ae969c3ffcd4ee8e365a51853add2fd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182241"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Crear y ejecutar un trabajo en la aplicación de Azure IoT Central

Puede usar Microsoft Azure IoT Central para administrar los dispositivos conectados a escala mediante trabajos. La funcionalidad de trabajos le permite realizar actualizaciones masivas a las propiedades del dispositivo, la configuración y los comandos. Este artículo le guiará a través de cómo empezar a usar los trabajos en su propia aplicación.

## <a name="create-and-run-a-job"></a>Crear y ejecutar un trabajo

En esta sección se muestra cómo crear y ejecutar un trabajo. Cada paso se pasa por un ejemplo que muestra cómo ejecutar un trabajo para los dispositivos de máquinas expendedoras refrigerados que deben aumentar la velocidad del ventilador.

1. Navegar a los trabajos desde el panel de navegación.

1. Seleccione **+ nuevo** con el fin de empezar a crear un nuevo trabajo.

    ![Crear nuevo trabajo](./media/howto-run-a-job/createnewjob.png)

1. Escriba un nombre y una descripción que le ayudarán a identificar el trabajo que va a crear.

1. Seleccione el conjunto de dispositivos al que desea que se aplique el trabajo. Después de seleccionar el dispositivo establecido, verá el lado derecho rellenar con los dispositivos dentro del conjunto de dispositivo seleccionado. Si selecciona un conjunto de dispositivos rota, no mostrar ningún dispositivo y verá un mensaje que explica que el conjunto de dispositivos se ha interrumpido.

1. A continuación, elija el tipo de trabajo para definir (una configuración, una propiedad o un comando). Seleccione **+** junto al tipo de trabajo seleccionado y agregar las operaciones deseadas.

    ![Configurar trabajo](./media/howto-run-a-job/configurejob.png)

1. En el lado derecho, elija los dispositivos en los que desea ejecutar el trabajo. Si activa la casilla de verificación superior, se seleccionan todos los dispositivos en el conjunto de todo el dispositivo. Activando la casilla de verificación junto a **nombre**, se seleccionan todos los dispositivos en la página actual.

1. Después de seleccionar los dispositivos, elija **ejecutar** o **guardar**. El trabajo aparece ahora en la oficina principal **trabajos** página. En esta vista, puede ver su trabajo en ejecución actualmente y el historial de cualquier ejecutados anteriormente. Su trabajo en ejecución siempre aparece en la parte superior de la lista. Su trabajo guardado se puede abrir de nuevo en cualquier momento para continuar con la edición o para ejecutar.

    ![Ver trabajo](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > Puede ver el historial de los trabajos ejecutados con anterioridad hasta 30 días.

1. Para obtener una visión general de su trabajo, seleccione el nombre del trabajo que desea ver en la lista. Esta información general contiene los detalles del trabajo, dispositivos y los valores de estado de dispositivo. En esta introducción, también puede seleccionar **descargar los detalles del trabajo** para descargar un archivo .csv de los detalles del trabajo, incluidos los dispositivos y sus valores de estado. Esta información puede ser útil para solucionar problemas.

    ![Ver el estado del dispositivo](./media/howto-run-a-job/downloaddetails.png)

### <a name="stop-a-running-job"></a>Detener un trabajo en ejecución

Si desea detener un trabajo que se está ejecutando actualmente, seleccione el nombre del trabajo en ejecución que desea detener. Elija el botón **Detener** en el panel. Podrá ver que el estado del trabajo ha cambiado para reflejar que el trabajo se ha detenido.

   ![Detención de trabajo](./media/howto-run-a-job/stopjob.png)

### <a name="run-a-stopped-job"></a>Ejecución de un trabajo detenido

Para ejecutar un trabajo que esté detenido, seleccione el nombre del trabajo detenido que desea ejecutar. Elija el botón **Ejecutar** en el panel. El estado del trabajo cambia para reflejar que el trabajo ahora se ejecuta de nuevo.

   ![Trabajo reanudado](./media/howto-run-a-job/resumejob.png)

## <a name="copy-a-job"></a>Un trabajo de copia

Para copiar un trabajo existente que ha creado, selecciónela en la página principal de trabajos y seleccione **copia**. Se abrirá una nueva copia de la configuración del trabajo para que pueda editar. Puede guardar o ejecutar el trabajo de nuevo. Si se han realizado cambios en el conjunto de dispositivo seleccionado, éstos aparecen en este trabajo copiado para que pueda editar.

   ![trabajo de copia](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>Ver el estado del trabajo

Una vez creado un trabajo, el **estado** las actualizaciones de la columna con el mensaje de estado más reciente del trabajo. Los mensajes de estado significan lo siguiente:

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

Para ver el estado de cada dispositivo en el trabajo, seleccione el nombre del trabajo. Consulte los detalles del trabajo y todos los dispositivos que formaban parte de él. Puede seleccionar **descargar los detalles del trabajo** para descargar un archivo .csv que incluye los detalles del trabajo, los dispositivos y sus valores de estado. Junto a cada nombre de dispositivo, consulte uno de los mensajes de estado siguientes:

| Mensaje de estado       | Significado de estado                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Completed            | Se ha ejecutado el trabajo en este dispositivo.                                     |
| Con error               | No se ha podido ejecutar el trabajo en este dispositivo. El mensaje de error muestra más información.  |
| Pending              | Aún no se ha ejecutado el trabajo en este dispositivo.                                  |

> [!NOTE]
> Si se ha eliminado un dispositivo, no se puede seleccionar el dispositivo y lo muestra como eliminado con el identificador de dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear trabajos en la aplicación de Azure IoT Central, estos son algunos de los pasos siguientes:

- [Uso de conjuntos de dispositivos](howto-use-device-sets.md)
- [Administración de dispositivos](howto-manage-devices.md)
- [Versión de la plantilla de dispositivo](howto-version-devicetemplate.md)

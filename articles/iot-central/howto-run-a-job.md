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
ms.openlocfilehash: ec7033719316bb186408ea78f6dabac43c383491
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60519356"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Crear y ejecutar un trabajo en la aplicación de Azure IoT Central

Puede usar Microsoft Azure IoT Central para administrar los dispositivos conectados a escala mediante trabajos. Los trabajos le permiten las actualizaciones de las propiedades del dispositivo, la configuración y los comandos de forma masiva. Este artículo le guiará a través de cómo empezar a usar los trabajos en su propia aplicación.

## <a name="create-and-run-a-job"></a>Crear y ejecutar un trabajo

En esta sección se muestra cómo crear y ejecutar un trabajo. Muestra cómo aumentar la velocidad del ventilador para varias máquinas expendedoras refrigeradas.

1. Navegar a los trabajos desde el panel de navegación.

1. Seleccione **+ nuevo** para crear un nuevo trabajo.

    ![Crear nuevo trabajo](./media/howto-run-a-job/createnewjob.png)

1. Escriba un nombre y una descripción para identificar el trabajo que se va a crear.

1. Seleccione el conjunto de dispositivos que desea que se aplican a su trabajo. Después de seleccionar el dispositivo establecido, verá el lado derecho rellenar con los dispositivos en el conjunto de dispositivos. Si selecciona un conjunto de dispositivos roto, no hay dispositivos mostrarán y verá un mensaje que se ha interrumpido el conjunto de dispositivos.

1. A continuación, elija el tipo de trabajo para definir (una configuración, una propiedad o un comando). Seleccione **+** junto al tipo de trabajo seleccionado y las operaciones de agregar.

    ![Configurar trabajo](./media/howto-run-a-job/configurejob.png)

1. En el lado derecho, seleccione los dispositivos que desea ejecutar el trabajo. Si activa la casilla de verificación superior, se seleccionan todos los dispositivos en el conjunto de todo el dispositivo. Activando la casilla de verificación junto a **nombre**, se seleccionan todos los dispositivos en la página actual.

1. Después de seleccionar los dispositivos, elija **ejecutar** o **guardar**. El trabajo aparece ahora en la oficina principal **trabajos** página. En esta vista, puede ver su trabajo en ejecución actualmente y el historial de cualquier ejecutados anteriormente. Su trabajo en ejecución siempre aparece en la parte superior de la lista. Su trabajo guardado se puede abrir de nuevo en cualquier momento para continuar con la edición o para ejecutar.

    ![Ver trabajo](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > Puede ver el historial de los trabajos ejecutados con anterioridad hasta 30 días.

1. Para obtener una visión general de su trabajo, seleccione el trabajo para ver en la lista. Esta información general contiene los detalles del trabajo, dispositivos y los valores de estado de dispositivo. En esta introducción, también puede seleccionar **descargar los detalles del trabajo** para descargar un archivo .csv de los detalles del trabajo, incluidos los dispositivos y sus valores de estado. Esta información puede ser útil para solucionar problemas.

    ![Ver el estado del dispositivo](./media/howto-run-a-job/downloaddetails.png)

### <a name="stop-a-running-job"></a>Detener un trabajo en ejecución

Para detener un trabajo en ejecución, selecciónelo y elija **detener** en el panel. El estado del trabajo cambia para reflejar que el trabajo se ha detenido.

   ![Detención de trabajo](./media/howto-run-a-job/stopjob.png)

### <a name="run-a-stopped-job"></a>Ejecución de un trabajo detenido

Para ejecutar un trabajo que esté detenido, seleccione el trabajo detenido. Elija **ejecutar** en el panel. El estado del trabajo cambia para reflejar el trabajo se está ejecutando ahora a intentarlo.

   ![Trabajo reanudado](./media/howto-run-a-job/resumejob.png)

## <a name="copy-a-job"></a>Un trabajo de copia

Para copiar un trabajo existente que ha creado, selecciónela en la página principal de trabajos y seleccione **copia**. Se abre una nueva copia de la configuración del trabajo para que pueda editar. Puede guardar o ejecutar el trabajo de nuevo. Si se han realizado cambios en el conjunto de dispositivo seleccionado, está refleja en este trabajo copiado para que pueda editar.

   ![trabajo de copia](./media/howto-run-a-job/copyjob.png)

## <a name="view-the-job-status"></a>Ver el estado del trabajo

Después de crea un trabajo, el **estado** las actualizaciones de la columna con el mensaje de estado más reciente del trabajo. En la tabla siguiente se enumera los valores de estado posibles:

| Mensaje de estado       | Significado de estado                                          |
| -------------------- | ------------------------------------------------------- |
| Completed            | Este trabajo se ha ejecutado en todos los dispositivos.              |
| Con error               | Este trabajo tiene errores y no se ejecutó completamente en los dispositivos.  |
| Pending              | Este trabajo aún no ha comenzado a ejecutar en los dispositivos.         |
| En ejecución              | Este trabajo se está ejecutando actualmente en los dispositivos.             |
| Stopped              | El usuario ha detenido manualmente este trabajo.           |

El mensaje de estado va seguido de una visión general de los dispositivos en el trabajo. En la tabla siguiente se enumera los valores de estado de dispositivo posibles:

| Mensaje de estado       | Significado de estado                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Succeeded            | El número de dispositivos que el trabajo se ejecutó correctamente en.       |
| Con error               | El número de dispositivos que se ha podido ejecutar en el trabajo.       |

### <a name="view-the-device-status"></a>Ver el estado del dispositivo

Para ver el estado del trabajo y todos los dispositivos afectados, seleccione el trabajo. Para descargar un archivo .csv que incluye los detalles del trabajo, incluida la lista de dispositivos y sus valores de estado, seleccione **descargar los detalles del trabajo**. Junto a cada nombre de dispositivo, consulte uno de los mensajes de estado siguientes:

| Mensaje de estado       | Significado de estado                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Completed            | Se ha ejecutado el trabajo en este dispositivo.                                     |
| Con error               | No se ha podido ejecutar el trabajo en este dispositivo. El mensaje de error muestra más información.  |
| Pending              | Aún no se ha ejecutado el trabajo en este dispositivo.                                   |

> [!NOTE]
> Si se ha eliminado un dispositivo, no se puede seleccionar el dispositivo y lo muestra como eliminado con el identificador de dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear trabajos en la aplicación de Azure IoT Central, estos son algunos de los pasos siguientes:

- [Uso de conjuntos de dispositivos](howto-use-device-sets.md)
- [Administración de dispositivos](howto-manage-devices.md)
- [Versión de la plantilla de dispositivo](howto-version-devicetemplate.md)

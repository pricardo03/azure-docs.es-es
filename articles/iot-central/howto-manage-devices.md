---
title: Administración de dispositivos en una aplicación de Azure IoT Central | Microsoft Docs
description: Como operador, aprenda a administrar dispositivos en la aplicación de Azure IoT Central.
author: ellenfosborne
ms.author: elfarber
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 364bd4dd0781c5fd74d0e4bdbfe3b4372a3d3ca0
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876012"
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Administración de dispositivos en la aplicación de Azure IoT Central

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

En este artículo se describe cómo administrar dispositivos en la aplicación de Azure IoT Central como operador. Como operador, puede hacer lo siguiente:

- Usar la página **Device Explorer** para ver, agregar y eliminar los dispositivos conectados a la aplicación de Azure IoT Central.
- Mantener un inventario actualizado de los dispositivos.
- Mantener actualizados los metadatos del dispositivo cambiando los valores almacenados en las propiedades del dispositivo.
- Controlar el comportamiento de los dispositivos mediante la actualización de una configuración en un dispositivo específico desde la página **Configuración**.

## <a name="view-your-devices"></a>Visualización de los dispositivos

Para ver un dispositivo determinado:

1. Elija **Device Explorer** en el menú de navegación izquierdo. Aquí verá una lista de las [plantillas de dispositivo](howto-set-up-template.md).

1. Elija una plantilla de dispositivo de la lista de **Plantillas**.

1. En el panel derecho de la página **Device Explorer**, verá una lista de dispositivos creados con esa plantilla de dispositivo. Elija un dispositivo para ver la página de detalles de ese dispositivo:

    ![Página Detalles del dispositivo](./media/howto-manage-devices/devicelist.png)

## <a name="add-a-device"></a>Agregar un dispositivo

Para agregar un dispositivo real a una aplicación de Azure IoT Central:

1. Elija **Device Explorer** en el menú de navegación izquierdo.

1. Elija la plantilla de dispositivo desde la que va a crear un dispositivo.

1. Elija + **Nuevo**.

1. Elija **Real** o **Simulado**. Un dispositivo real hace referencia a un dispositivo físico que se conecta a la aplicación de Azure IoT Central. Un dispositivo simulado tiene datos de ejemplo generados por Azure IoT Central, que los pone a su disposición.

## <a name="import-devices"></a>Importación de dispositivos

Para conectar un gran número de dispositivos a la aplicación, puede importar dispositivos en bloque desde un archivo CSV. El archivo CSV debe tener las siguientes columnas y encabezados:

* **IOTC_DeviceID**: el identificador de dispositivo debe estar en minúscula.
* **IOTC_DeviceName**: esta columna es opcional.

Para registrar dispositivos en la aplicación en bloque:

1. Elija **Device Explorer** en el menú de navegación izquierdo.

1. En el panel izquierdo, elija la plantilla de dispositivo para la que desea crear dispositivos en bloque.

    > [!NOTE]
    > Si aún no tiene una plantilla de dispositivo, puede importar dispositivos en **Unassociated devices** (Dispositivos no asociados) y registrarlos sin ninguna plantilla. Una vez que se han importado, puede asociarlos con una plantilla.

1. Seleccione **Import** (Importar).

    ![Acción de importación](./media/howto-manage-devices/bulkimport1a.png)

1. Seleccione el archivo CSV que tiene la lista de identificadores de dispositivo que desea importar.

1. La importación de dispositivos se inicia una vez que se ha cargado el archivo. Puede realizar un seguimiento del estado de la importación en la parte superior de la cuadrícula de dispositivo.

1. Una vez finalizada la importación, se muestra un mensaje de confirmación en la cuadrícula de dispositivos.

    ![Importación correcta](./media/howto-manage-devices/bulkimport3a.png)

Si se produce un error en la operación de importación de dispositivos, aparece un mensaje de error en la cuadrícula de dispositivos. Se genera un archivo de registro descargable con todos los errores.

**Asociación de dispositivos con una plantilla**

Si registra dispositivos mediante la importación en **Unassociated devices** (Dispositivos no asociados), los dispositivos se crean sin asociación con una plantilla de dispositivo. Los dispositivos deben estar asociados con una plantilla para explorar los datos y otros detalles sobre ellos. Siga estos pasos para asociar dispositivos con una plantilla:

1. Elija **Device Explorer** en el menú de navegación izquierdo.

1. En el panel izquierdo, elija **Unassociated devices** (Dispositivos no asociados):

    ![Dispositivos no asociados](./media/howto-manage-devices/unassociateddevices1a.png)

1. Seleccione los dispositivos que quiere asociar con una plantilla:

1. Seleccione **Asociar**:

    ![Asociar dispositivos](./media/howto-manage-devices/unassociateddevices2a.png)

1. Elija la plantilla de la lista de plantillas disponibles y seleccione **Asociar**.

1. Los dispositivos seleccionados están asociados con la plantilla de dispositivo que eligió.

> [!NOTE]
> Una vez que un dispositivo se ha asociado con una plantilla, no se puede desasociar ni asociar con una plantilla diferente.

## <a name="export-devices"></a>Exportación de dispositivos

Para conectar un dispositivo real a IoT Central, necesita su cadena de conexión. Puede exportar los detalles del dispositivo de forma masiva para obtener la información que necesita para crear las cadenas de conexión del dispositivo. El proceso de exportación crea un archivo CSV con la identidad, el nombre y las claves de todos los dispositivos seleccionados.

Para realizar la exportación masiva de dispositivos desde la aplicación:

1. Elija **Device Explorer** en el menú de navegación izquierdo.

1. En el panel izquierdo, elija la plantilla de dispositivo para la que desea exportar los dispositivos.

1. Seleccione los dispositivos que desea exportar y la acción **Exportar**.

    ![Exportación](./media/howto-manage-devices/export1a.png)

1. Se inicia el proceso de exportación. Puede realizar un seguimiento del estado en la parte superior de la cuadrícula.

1. Cuando la exportación finaliza, se muestra un mensaje de confirmación junto con un vínculo para descargar el archivo generado.

1. Seleccione el **mensaje de operación correcta** para descargar el archivo en una carpeta local en el disco.

    ![Exportación correcta](./media/howto-manage-devices/export2a.png)

1. El archivo CSV exportado contiene las siguientes columnas: Id. de dispositivo, nombre del dispositivo, claves de dispositivo y huellas digitales de certificado X509:

    * IOTC_DEVICEID
    * IOTC_DEVICENAME
    * IOTC_SASKEY_PRIMARY
    * IOTC_SASKEY_SECONDARY
    * IOTC_X509THUMBPRINT_PRIMARY
    * IOTC_X509THUMBPRINT_SECONDARY

Consulte [Conectividad de dispositivos en Azure IoT Central](concepts-connectivity.md) para más información sobre las cadenas de conexión y la conexión de dispositivos reales a la aplicación de IoT Central.

## <a name="delete-a-device"></a>Eliminar un dispositivo

Para eliminar ya sea un dispositivo real o simulado de la aplicación de Azure IoT Central:

1. Elija **Device Explorer** en el menú de navegación.

1. Elija la plantilla de dispositivo del dispositivo que desea eliminar.

1. Active la casilla situada junto al dispositivo que desea eliminar.

1. Elija **Eliminar**.

## <a name="change-a-device-setting"></a>Cambio de la configuración de un dispositivo

La configuración controla el comportamiento de un dispositivo. En otras palabras, le permite proporcionar entradas para el dispositivo. Puede ver y actualizar la configuración del dispositivo en la página **Detalles del dispositivo**.

1. Elija **Device Explorer** en el menú de navegación.

1. Elija la plantilla de dispositivo del dispositivo cuya configuración desea modificar.

1. Elija la pestaña **Configuración** . Aquí verá toda la configuración que tiene el dispositivo y sus valores actuales. Para cada valor, puede ver si el dispositivo se está sincronizando todavía.

1. Modifique la configuración según los valores que necesita. Puede modificar varias opciones de configuración a la vez y actualizarlas todas al mismo tiempo.

1. Seleccione **Actualizar**. Los valores se envían al dispositivo. Cuando el dispositivo confirma el cambio de configuración, el estado de la configuración se cambia de nuevo a **sincronizado**.

## <a name="change-a-property"></a>Cambio de una propiedad

Las propiedades son los metadatos de dispositivo asociados al dispositivo, como la ciudad y el número de serie. Puede ver y actualizar las propiedades en la página **Detalles del dispositivo**.

1. Elija **Device Explorer** en el menú de navegación.

1. Elija la plantilla de dispositivo del dispositivo cuyas propiedades desea modificar.

1. Elija la pestaña **Propiedades**, donde verá todas las propiedades.

1. Modifique las propiedades de la aplicación con los valores que necesite. Puede modificar varias propiedades a la vez y actualizarlas todas al mismo tiempo. Seleccione **Actualizar**.

> [!NOTE]
> No se puede cambiar el valor de las _propiedades del dispositivo_. Las propiedades del dispositivo se establecen con el dispositivo y son de solo lectura en la aplicación de Azure IoT Central.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar dispositivos en la aplicación de Azure IoT Central, le sugerimos el paso siguiente:

> [!div class="nextstepaction"]
> [How to use device sets](howto-use-device-sets.md) (Uso de conjuntos de dispositivos)

<!-- Next how-tos in the sequence -->

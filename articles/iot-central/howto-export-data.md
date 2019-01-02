---
title: Exportación de los datos a Azure IoT Central | Microsoft Docs
description: Exportación de datos desde la aplicación de Azure IoT Central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/07/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: cba0bad2e81ffddedfc4ca04e82e17e4286b389b
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53312126"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportación de los datos a Azure IoT Central

*Este tema se aplica a administradores*.

En este artículo se describe cómo usar la característica de exportación continua de datos de Azure IoT Central para exportar los datos a sus propias instancias de **Azure Blob Storage**, **Azure Event Hubs** y **Azure Service Bus**. Puede exportar **medidas**, **dispositivos** y **plantillas de dispositivo** a su propio destino para analizar las rutas de acceso activas e inactivas. Puede exportar datos a Blob Storage para ejecutar análisis de tendencias a largo plazo en Microsoft Power BI, o exportar datos a Event Hubs y Service Bus para transformar y aumentar los datos casi en tiempo real con Azure Logic Apps o Azure Functions.

> [!Note]
> Al activar la exportación de datos continua, solo obtendrá los datos a partir de ese momento. Actualmente, los datos no se pueden recuperar durante un tiempo cuando la exportación de datos continua estaba desactivada. Para conservar más datos históricos, activa la exportación continua de datos al principio.

## <a name="prerequisites"></a>Requisitos previos

- Ser administrador en la aplicación de IoT Central

## <a name="export-to-blob-storage"></a>Exportación a Blob Storage

Los datos de medidas, dispositivos y plantillas de dispositivo se exportan a la cuenta de almacenamiento una vez por minuto, y cada archivo contiene el lote de cambios desde el último archivo exportado. Los datos exportados están en formato [Apache AVRO](https://avro.apache.org/docs/current/index.html).

Aprenda más sobre la [exportación a Blob Storage](howto-export-data-blob-storage.md).

## <a name="export-to-event-hubs-and-service-bus"></a>Exportación a Event Hubs y Service Bus

Los datos de medidas, dispositivos y plantillas de dispositivo se exportan a su centro de eventos o cola o tema de Service Bus. Los datos de medidas exportados llegan casi en tiempo real y contienen la totalidad del mensaje que los dispositivos enviaron a IoT Central, no solo los valores de las propias medidas. Los datos de dispositivo exportados llegan en lotes una vez por minuto y contienen los cambios en las propiedades y la configuración de todos los dispositivos; las plantillas de dispositivo exportadas contienen los cambios en todas las plantillas de dispositivo.


Aprenda más sobre la [exportación a Event Hubs y Service Bus](howto-export-data-event-hubs-service-bus.md).

## <a name="set-up-export-destination"></a>Configuración del destino de exportación

Si no tiene una instancia existente de Storage, Event Hubs o Service Bus a la que exportar, siga estos pasos:

### <a name="create-storage-account"></a>Crear cuenta de almacenamiento

1. Cree una [cuenta de almacenamiento en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Puede encontrar más información en los [documentos de Azure Storage](https://aka.ms/blobdocscreatestorageaccount).
2. Para el tipo de cuenta, elija **Uso general** o **Blob Storage**.
3. Elija una suscripción. 

    > [!Note] 
    > Ahora puede exportar datos a otras suscripciones que **no sean las mismas** que las de la aplicación de IoT Central de pago por uso. En este caso se conectará mediante una cadena de conexión.

4. Cree un contenedor en la cuenta de almacenamiento. Vaya a la cuenta de almacenamiento. En **Blob Service** seleccione **Examinar blobs**. Seleccione **+ Contenedor** en la parte superior para crear un contenedor.

### <a name="create-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs

1. Cree un [espacio de nombres de Event Hubs en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). Puede encontrar más información en los [documentos de Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).
2. Elija una suscripción. 

    > [!Note] 
    > Ahora puede exportar datos a otras suscripciones que **no sean las mismas** que las de la aplicación de IoT Central de pago por uso. En este caso se conectará mediante una cadena de conexión.
3. Cree un centro de eventos en el espacio de nombres de Event Hubs Vaya al espacio de nombres y seleccione **+ Centro de eventos** en la parte superior para crear una instancia de centro de eventos.

### <a name="create-service-bus-namespace"></a>Creación de un espacio de nombres de Service Bus

1. Cree un [espacio de nombres de Service Bus en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5) . Puede encontrar más información en los [documentos de Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal).
2. Elija una suscripción. 

    > [!Note] 
    > Ahora puede exportar datos a otras suscripciones que **no sean las mismas** que las de la aplicación de IoT Central de pago por uso. En este caso se conectará mediante una cadena de conexión.

3. Vaya al espacio de nombres de Service Bus y seleccione **+ Cola** o **+ Tema** en la parte superior para crear una cola o un tema como destino de exportación.

## <a name="set-up-continuous-data-export"></a>Configuración de la exportación continua de datos

Ahora que tiene un destino de Storage, Event Hubs o Service Bus al que exportar los datos, siga estos pasos para configurar la exportación de datos continua. 

1. Inicie sesión en su aplicación de IoT Central.

2. En el menú izquierdo, haga clic en **Exportación de datos continua**.

    > [!Note]
    > Si no ve la exportación de datos continua en el menú izquierdo, significa que no es un administrador de la aplicación. Hable con un administrador para configurar la exportación de datos.

    ![Creación de un centro de eventos](media/howto-export-data/export_menu.PNG)

3. Haga clic en el botón **+ Nuevo** en la esquina superior derecha. Elija **Azure Blob Storage**, **Azure Event Hubs** o **Azure Service Bus** como destino de la exportación. 

    > [!NOTE] 
    > El número máximo de exportaciones por aplicación es cinco. 

    ![Creación de una exportación de datos continua](media/howto-export-data/export_new.PNG)

4. En el cuadro de lista desplegable, seleccione su **cuenta de almacenamiento, espacio de nombres de Event Hubs o espacio de nombres de Service Bus**. También puede elegir la última opción de la lista que es **Escriba una cadena de conexión**. 

    > [!NOTE] 
    > Solo verá las cuentas de almacenamiento, los espacios de nombres de Event Hubs o los espacios de nombres de Service Bus de la **misma suscripción que la de la aplicación de IoT Central**. Si quiere exportar a un destino fuera de esta suscripción, elija **Escriba una cadena de conexión** y vea el paso 5.

    > [!NOTE] 
    > En las aplicaciones de prueba de siete días, la única manera de configurar la exportación de datos continua es mediante una cadena de conexión. El motivo es que estas aplicaciones no tienen una suscripción de Azure asociada.

    ![Creación de un centro de eventos](media/howto-export-data/export_create.PNG)

5. (Opcional) Si eligió **Escriba una cadena de conexión**, aparece un nuevo cuadro para que pegue la cadena de conexión. Para obtener la cadena de conexión para su:
    - Cuenta de almacenamiento, vaya a la cuenta de almacenamiento en Azure Portal.
        - En **Configuración**, haga clic en **Claves de acceso**.
        - Copie la cadena de conexión key1 o la cadena de conexión key2.
    - Event Hubs o Service Bus, vaya al espacio de nombres en Azure Portal.
        - En **Configuración**, haga clic en **Directivas de acceso compartido**.
        - Elija el valor predeterminado **RootManageSharedAccessKey** o cree uno.
        - Copie la cadena de conexión principal o la secundaria.
 
6. Elija un contenedor, un centro de eventos, una cola o un tema del cuadro de lista desplegable.

7. En **Datos para exportar**, especifique cada tipo de datos para exportar y establezca el tipo en **Activado**.

6. Para activar la exportación de datos continua, asegúrese de que la opción **Exportación de datos** esté **activada**. Seleccione **Guardar**.

  ![Configuración de la exportación continua de datos](media/howto-export-data/export_list.PNG)

7. Transcurridos unos minutos, los datos aparecen en el destino elegido.

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo exportar los datos, continúe con el paso siguiente:

> [!div class="nextstepaction"]
> [Exportación a Azure Blob Storage](howto-export-data-blob-storage.md)

> [!div class="nextstepaction"]
> [Exportación de datos a Azure Event Hubs y Azure Service Bus](howto-export-data-event-hubs-service-bus.md)

> [!div class="nextstepaction"]
> [Procedimiento para visualizar los datos en Power BI](howto-connect-powerbi.md)

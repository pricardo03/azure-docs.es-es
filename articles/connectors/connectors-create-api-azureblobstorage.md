---
title: 'Conectarse a Azure blob storage: Azure Logic Apps'
description: Creación y administración de blobs en Azure Storage con Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/21/2018
tags: connectors
ms.openlocfilehash: ea3e97db9ec560306788943d92a7670025f38bdc
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58310377"
---
# <a name="create-and-manage-blobs-in-azure-blob-storage-with-azure-logic-apps"></a>Creación y administración de blobs en Azure Blob Storage con Azure Logic Apps

En este artículo se muestra cómo se puede acceder y administrar los archivos almacenados como blobs en una cuenta de Azure Storage desde dentro de una aplicación de lógica con el conector de Azure Blob Storage. De este modo, puede crear aplicaciones lógicas que automatizan tareas y flujos de trabajo para administrar los archivos. Por ejemplo, puede crear aplicaciones lógicas que creen, obtengan, actualicen y eliminen archivos en su cuenta de almacenamiento.

Imagine que tiene una herramienta que se actualiza en un sitio web de Azure, que actúa como desencadenador para su aplicación lógica. Cuando se produce este evento, la aplicación lógica puede actualizar algunos archivos en el contenedor de Blob Storage, que es una acción de la aplicación lógica.

> [!NOTE]
> Logic Apps no admite la conexión directa a las cuentas de Azure Storage a través de firewalls. Para acceder a estas cuentas de almacenamiento, use cualquiera de estas opciones:
>
> * Cree un [entorno de servicio de integración](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) que pueda conectarse a los recursos de una red virtual de Azure.
>
> * Si ya usa API Management, puede usar este servicio para el escenario. Para obtener más información, vea [Arquitectura de integración empresarial sencilla](https://aka.ms/aisarch).

Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Para obtener información técnica específica del conector, consulte la <a href="https://docs.microsoft.com/connectors/azureblobconnector/" target="blank">referencia sobre el conector de Azure Blob Storage</a>.

## <a name="prerequisites"></a>Requisitos previos

* Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

* Una [cuenta de Azure Storage y un contenedor de almacenamiento](../storage/blobs/storage-quickstart-blobs-portal.md)

* La aplicación lógica en la que necesita acceso a la cuenta de Azure Blob Storage. Para iniciar la aplicación lógica con un desencadenador de Azure Blob Storage, necesita una [aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>Agregar un desencadenador de Blob Storage

En Azure Logic Apps, cada aplicación lógica debe comenzar con un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que se activa cuando sucede un evento específico o cuando se cumple una condición determinada. Cada vez que el desencadenador se activa, el motor de Logic Apps crea una instancia de aplicación lógica y empieza a ejecutar el flujo de trabajo de la aplicación.

En este ejemplo se muestra cómo se puede iniciar un flujo de trabajo de una aplicación lógica con el desencadenador **Azure Blob Storage: cuando se agrega o modifica un blob (solo propiedades)** cuando las propiedades de un blob se agregan o se actualizan en un contenedor de almacenamiento. 

1. En Azure Portal o Visual Studio, cree una aplicación lógica en blanco que abre el diseñador de aplicaciones lógicas. En este ejemplo se usa Azure Portal.

2. En el cuadro de búsqueda, escriba "azure blob" como filtro. En la lista de desencadenadores, seleccione el que desee.

   Este ejemplo utiliza este desencadenador: **Azure Blob Storage: cuando se agrega o modifica (solo propiedades) de un blob**

   ![Seleccionar un desencadenador](./media/connectors-create-api-azureblobstorage/azure-blob-trigger.png)

3. Si se le piden los detalles de la conexión, [cree ahora su conexión de Blob Storage](#create-connection). O bien, si la conexión ya existe, especifique la información necesaria para el desencadenador.

   Para este ejemplo, seleccione el contenedor y la carpeta que desea supervisar.

   1. En el cuadro **Contenedor**, seleccione el icono de la carpeta.

   2. En la lista de carpetas, elija el corchete angular derecho (**>**) y, después, desplácese hasta que encuentre y seleccione la carpeta que desee.

      ![Seleccionar carpeta](./media/connectors-create-api-azureblobstorage/trigger-select-folder.png)

   3. Seleccione el intervalo y la frecuencia con la que desea que el desencadenador compruebe si hay cambios en la carpeta.

4. Cuando esté listo, elija **Guardar** en la barra de herramientas del diseñador.

5. Ahora, agregue a la aplicación lógica una o varias acciones, en función de las tareas que desea realizar con los resultados del desencadenador.

<a name="add-action"></a>

## <a name="add-blob-storage-action"></a>Agregar un acción a Blob Storage

En Azure Logic Apps, una [acción](../logic-apps/logic-apps-overview.md#logic-app-concepts) es un paso del flujo de trabajo que sigue a un desencadenador u otra acción. En este ejemplo, la aplicación lógica se inicia con el [desencadenador Periodicidad](../connectors/connectors-native-recurrence.md).

1. En Azure Portal o Visual Studio, abra la aplicación lógica en el diseñador de aplicaciones lógicas. En este ejemplo se usa Azure Portal.

2. En el Diseñador de Logic Apps, en el desencadenador o la acción, elija **Nuevo paso** > **Agregar una acción**.

   ![Agregar una acción](./media/connectors-create-api-azureblobstorage/add-action.png) 

   Para agregar una acción entre los pasos existentes, mueva el mouse sobre la flecha de conexión. 
   Elija el signo más (**+**) que aparece y, luego, elija **Agregar una acción**.

3. En el cuadro de búsqueda, escriba "azure blob" como filtro. En la lista de acciones, seleccione la acción que desee.

   Este ejemplo usa esta acción: **Azure Blob Storage: obtener el contenido de blob**

   ![Acción Select](./media/connectors-create-api-azureblobstorage/azure-blob-action.png) 

4. Si se le piden los detalles de la conexión, [cree ahora su conexión de Azure Blob Storage](#create-connection). O bien, si la conexión ya existe, especifique la información necesaria para la acción.

   En este ejemplo, seleccione el archivo que desee.

   1. En el cuadro **Blob**, seleccione el icono de la carpeta.
  
      ![Seleccionar carpeta](./media/connectors-create-api-azureblobstorage/action-select-folder.png)

   2. Busque el archivo que desee por el número de **identificador** del blob y selecciónelo. Dicho número de **identificador** se puede encontrar en los metadatos del blob que devuelve el desencadenador de almacenamiento del blob descrito anteriormente.

5. Cuando esté listo, elija **Guardar** en la barra de herramientas del diseñador.
Para probar la aplicación lógica, asegúrese de que la carpeta seleccionada contiene un blob.

En este ejemplo solo se obtiene el contenido de un blob. Para ver dicho contenido, agregue otra acción que cree un archivo con el blob mediante otro conector. Por ejemplo, agregue una acción de OneDrive que cree un archivo basándose en el contenido del blob.

<a name="create-connection"></a>

## <a name="connect-to-storage-account"></a>Conectar con la cuenta de almacenamiento

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos, como desencadenadores, acciones y los límites, como se describe en Open API del conector (anteriormente Swagger) de archivos, consulte el [página de referencia del conector](/connectors/azureblobconnector/).

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)

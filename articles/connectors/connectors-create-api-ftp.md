---
title: Conexión a un servidor FTP
description: Creación, supervisión y administración de archivos en un servidor FTP con Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 06/19/2019
tags: connectors
ms.openlocfilehash: c7b8c1ac94fd35a4a0cb30ad32d8c6ce39edc058
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789786"
---
# <a name="create-monitor-and-manage-ftp-files-by-using-azure-logic-apps"></a>Creación, supervisión y administración de archivos FTP mediante Azure Logic Apps

Con Azure Logic Apps y el conector FTP, puede crear tareas y flujos de trabajo automatizados que crea, supervisan, envían y reciben archivos mediante su cuenta en un servidor FTP, junto con otras acciones, por ejemplo:

* Supervisar cuándo se agregan o cambian archivos
* Obtener, crear, copiar, actualizar, enumerar y eliminar archivos
* Obtener contenido de archivos y metadatos
* Extraer archivos en carpetas

Puede usar desencadenadores que obtengan respuestas de su servidor FTP y permitan que la salida esté disponible para otras acciones. Puede usar las acciones de ejecución en las aplicaciones lógicas para administrar los archivos del servidor FTP. También puede hacer que otras acciones usen la salida de las acciones de FTP. Por ejemplo, si recupera archivos del servidor FTP habitualmente, puede enviar un correo electrónico sobre esos archivos y su contenido mediante el conector Office 365 Outlook o el conector Outlook.com. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="limits"></a>límites

* El conector de FTP solo admite FTP por SSL (FTPS) explícito y no es compatible con FTPS implícito.

* De forma predeterminada, las acciones de FTP pueden leer o escribir archivos con un tamaño de *50 MB o menos*. Para procesar archivos mayores de 50 MB, las acciones de FTP admiten la [fragmentación de mensajes](../logic-apps/logic-apps-handle-large-messages.md). La acción **Obtener contenido de archivo** usa implícitamente la fragmentación.

* Los desencadenadores de FTP no admiten la fragmentación. Cuando se solicita el contenido del archivo, los desencadenadores seleccionan solo los archivos que tienen un tamaño de 50 MB o menos. Para obtener archivos de más de 50 MB, siga este patrón:

  * Use un desencadenador de FTP que devuelva las propiedades de archivo, como **Cuando se agrega o modifica un archivo (solo propiedades)** .

  * Siga el desencadenador con la acción **Obtener contenido de archivo** de FTP, que lee el archivo completo y usa implícitamente la fragmentación.

## <a name="how-ftp-triggers-work"></a>Funcionamiento de desencadenadores de FTP

Los desencadenadores de FTP funcionan al sondear el sistema de archivos FTP y buscar archivos que se hayan modificado desde el último sondeo. Algunas herramientas le permiten conservar la marca de tiempo cuando cambian los archivos. En estos casos, tiene que deshabilitar esta característica para que el desencadenador funcione. Estas son algunas opciones de configuración habituales:

| Cliente SFTP | . |
|-------------|--------|
| WinSCP | Vaya a **Options** > **Preferences** > **Transfer** > **Edit** > **Preserve timestamp** > **Disable** (Opciones > Preferencias > Transferir > Editar > Conservar marca de tiempo >Deshabilitar). |
| FileZilla | Vaya a **Transfer** > **Preserve timestamps of transferred files** > **Disable** (Transferir > Conservar marca de tiempo de archivos transferidos > Deshabilitar). |
|||

Cuando un desencadenador encuentra un nuevo archivo, el desencadenador comprueba que el nuevo archivo se ha escrito por completo y no parcialmente. Por ejemplo, un archivo podría tener los cambios en curso cuando el desencadenador comprueba el servidor de archivos. Para evitar devolver un archivo parcialmente escrito, el desencadenador anota la marca de tiempo del archivo que tiene cambios recientes, pero no devuelve inmediatamente dicho archivo. El desencadenador devuelve el archivo solo al volver a sondear el servidor. En ocasiones, este comportamiento puede provocar un retraso de hasta dos veces el intervalo de sondeo del desencadenador.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Las credenciales de la cuenta y la dirección del servidor host FTP

  El conector de FTP requiere que el servidor FTP sea accesible desde Internet y que esté configurado para poder funcionar en modo *pasivo*. Las credenciales permiten a la aplicación lógica crear una conexión a la cuenta de FTP y acceder a ella.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica desde donde quiere acceder a la cuenta de FTP. Para comenzar con un desencadenador de FTP, [cree una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar una acción de FTP, inicie la aplicación lógica con otro desencadenador, por ejemplo, el desencadenador **Recurrence**.

## <a name="connect-to-ftp"></a>Conexión a un FTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Para las aplicaciones lógicas en blanco, en el cuadro de búsqueda, escriba "ftp" como filtro. En la lista de desencadenadores, seleccione el que desee.

   O bien

   Para las aplicaciones lógicas existentes, en el último paso donde quiere agregar una acción, elija **Nuevo paso** y seleccione **Agregar una acción**. En el cuadro de búsqueda, escriba "ftp" como filtro. En la lista de acciones, seleccione la que desee.

   Para agregar una acción entre un paso y otro, mueva el puntero sobre la flecha entre ellos. Elija el signo más ( **+** ) que aparece y seleccione **Agregar una acción**.

1. Proporcione la información necesaria para la conexión y, a continuación, seleccione **Crear**:

1. Proporcione los detalles necesarios para el desencadenador o la acción seleccionados y continúe con la compilación del flujo de trabajo de la aplicación lógica.

## <a name="examples"></a>Ejemplos

<a name="file-added-modified"></a>

### <a name="ftp-trigger-when-a-file-is-added-or-modified"></a>Desencadenador FTP: When a file is added or modified (Cuando se agrega o modifica un archivo)

Este desencadenador inicia un flujo de trabajo de aplicación lógica cuando detecta que se ha agregado o cambiado un archivo en un servidor FTP. Por ejemplo, puede agregar una condición que compruebe el contenido del archivo y decida si obtener ese contenido, en función de si cumple una condición especificada. Por último, puede agregar una acción que obtenga el contenido del archivo y lo coloque en una carpeta en el servidor SFTP.

**Ejemplo Enterprise**: Puede usar este desencadenador para supervisar nuevos archivos en una carpeta de SFTP que describen los pedidos de los clientes. Luego, puede usar una acción de FTP, como **Obtener contenido de archivo** para obtener el contenido del pedido para su posterior procesamiento y almacenar ese pedido en una base de datos de pedidos.

Este es un ejemplo que muestra este desencadenador: **When a file is added or modified**

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Para las aplicaciones lógicas en blanco, en el cuadro de búsqueda, escriba "ftp" como filtro. En la lista de desencadenadores, seleccione este desencadenador: **When a filed is added or modified - FTP**

   ![Busque y seleccione el desencadenador de FTP](./media/connectors-create-api-ftp/select-ftp-trigger.png)  

1. Proporcione la información necesaria para la conexión y, a continuación, seleccione **Crear**:

   De forma predeterminada, este conector transfiere los archivos en formato de texto. Para transferir archivos en formato binario, por ejemplo, cuando se use la codificación, seleccione **Transporte binario**.

   ![Creación de una conexión al servidor FTP](./media/connectors-create-api-ftp/create-ftp-connection-trigger.png)  

1. Junto al cuadro **Carpeta**, seleccione el icono de carpeta para que aparezca una lista. Para buscar la carpeta donde quiere supervisar los archivos nuevos o modificados, seleccione la flecha de ángulo derecho ( **>** ), vaya a esa carpeta y, luego, seleccione la carpeta.

   ![Busque y seleccione la carpeta para supervisar](./media/connectors-create-api-ftp/select-folder.png)  

   La carpeta seleccionada aparece en el cuadro **Carpeta**.

   ![Carpeta seleccionada](./media/connectors-create-api-ftp/selected-folder.png)  

Ahora que la aplicación lógica tiene un desencadenador, agregue las acciones que quiera ejecutar cuando la aplicación lógica encuentre un archivo nuevo o modificado. En este ejemplo, puede agregar una acción de FTP que obtiene el contenido nuevo o actualizado.

<a name="get-content"></a>

### <a name="ftp-action-get-content"></a>Acción de FTP: Obtener contenido

Esta acción obtiene el contenido de un archivo en un servidor FTP cuando ese archivo se agrega o actualiza. Por ejemplo, puede agregar el desencadenador del ejemplo anterior y una acción que obtiene el contenido del archivo después de que se agregue o modifique el archivo.

Este es un ejemplo que muestra esta acción: **Obtener contenido**

1. En el desencadenador o en cualquiera de las otras acciones, elija **Nuevo paso**.

1. En el cuadro de búsqueda, escriba "ftp" como filtro. En la lista de acciones, seleccione esta acción: **Get file content - FTP**

   ![Selección de la acción de FTP](./media/connectors-create-api-ftp/select-ftp-action.png)  

1. Si ya tiene una conexión con el servidor FTP y la cuenta, vaya al paso siguiente. De lo contrario, proporcione la información necesaria para la conexión y, luego, seleccione **Crear**.

   ![Creación de una conexión al servidor FTP](./media/connectors-create-api-ftp/create-ftp-connection-action.png)

1. Después de que se abra la acción **Obtener contenido de archivo**, haga clic dentro del cuadro **Archivo** para que aparezca la lista de contenido dinámico. Ahora puede seleccionar propiedades para las salidas de los pasos anteriores. En la lista de contenido dinámico, seleccione la propiedad **Contenido del archivo**, que tiene el contenido del archivo agregado o actualizado.  

   ![Buscar y seleccionar el archivo](./media/connectors-create-api-ftp/ftp-action-get-file-content.png)

   La propiedad **Contenido del archivo** aparece ahora en el cuadro **Archivo**.

   ![Propiedad "Contenido del archivo" seleccionada](./media/connectors-create-api-ftp/ftp-action-selected-file-content-property.png)

1. Guarde la aplicación lógica. Para probar el flujo de trabajo, agregue un archivo a la carpeta FTP que ahora supervisa la aplicación lógica.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener información técnica acerca de los desencadenadores, las acciones y los límites, que se detallan en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia de los conectores](/connectors/ftpconnector/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
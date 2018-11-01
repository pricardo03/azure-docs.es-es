---
title: Conexión a una cuenta de SFTP desde Azure Logic Apps | Microsoft Docs
description: Automatice tareas y flujos de trabajo que supervisan, crean, administran, envían y reciben archivos en un servidor SFTP mediante Azure Logic Apps.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
tags: connectors
ms.date: 09/24/2018
ms.openlocfilehash: 2250c6952aeac7b10dcb1a1a35419941e5cad507
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233215"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps-and-sftp-ssh-connector"></a>Supervisión, creación y administración de archivos SFTP mediante Azure Logic Apps y el conector SFTP-SSH

Con Azure Logic Apps y el conector SFTP-SSH, puede crear tareas y flujos de trabajo automatizados que supervisan, crean, envían y reciben archivos mediante su cuenta en un servidor [SFTP](https://www.ssh.com/ssh/sftp/), junto con otras acciones, por ejemplo:

* Supervisar cuándo se agregan o cambian archivos
* Obtener, crear, copiar, cambiar el nombre, actualizar, enumerar y eliminar archivos.
* Crear una carpeta.
* Obtener contenido de archivos y metadatos
* Extraer archivos en carpetas

Puede usar desencadenadores que obtengan respuestas de su servidor SFTP y permitan que la salida esté disponible para otras acciones. Puede usar las acciones en las aplicaciones lógicas para realizar tareas con archivos en el servidor SFTP. También puede hacer que otras acciones usen la salida de las acciones de SFTP. Por ejemplo, si recupera archivos del servidor SFTP con regularidad, puede enviar un correo electrónico sobre esos archivos y su contenido mediante el conector Office 365 Outlook o el conector Outlook.com.
Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="sftp-ssh-versus-sftp"></a>SFTP-SSH frente a SFTP

Estas son las pocas diferencias claves entre el conector SFTP-SSH y el conector [SFTP](../connectors/connectors-create-api-sftp.md). El conector de SFTP-SSH proporciona estas funcionalidades:

* Usa la biblioteca <a href="https://github.com/sshnet/SSH.NET" target="_blank">**SSH.NET**</a>, que es una biblioteca de Secure Shell (SSH) de código abierto para .NET.

* Proporciona compatibilidad para archivos de gran tamaño de hasta **1 GB**. El conector puede leer o escribir archivos de hasta 1 GB de tamaño.

* Proporciona la acción **Crear carpeta**, que crea una carpeta en la ruta de acceso especificada en el servidor SFTP.

* Proporciona la acción **Cambiar nombre de archivo**, que cambia el nombre de un archivo en el servidor SFTP.

* Almacena en caché la conexión al servidor SFTP, lo que mejora el rendimiento y reduce el número de intentos de conexión en el servidor. 

  Puede controlar la duración que se usa para almacenar en caché la conexión configurando la propiedad <a href="http://man.openbsd.org/sshd_config#ClientAliveInterval" target="_blank">**ClientAliveInterval**</a> del servidor SFTP. 

## <a name="how-trigger-polling-works"></a>Cómo funciona la desencadenación mediante sondeo

El método de funcionamiento de los desencadenadores SFTP-SSH es sondear el sistema de archivos SFTP y buscar cualquier archivo que se haya cambiado desde el último sondeo. Algunas herramientas le permiten conservar la marca de tiempo cuando cambian los archivos, por lo que en estos casos, deberá deshabilitar esta característica para que funcione el desencadenador. Estas son algunas opciones de configuración habituales:

| Cliente SFTP | . | 
|-------------|--------| 
| WinSCP | Opciones → Preferencias… → Transferencia → Editar… → Conservar marca de tiempo → Deshabilitar |
| FileZilla | Transferencia → Conservar marca de tiempo en archivos transferidos → Deshabilitar | 
||| 

Cuando un desencadenador encuentra un nuevo archivo, el desencadenador comprueba que el nuevo archivo se ha escrito por completo y no parcialmente. Por ejemplo, un archivo podría tener los cambios en curso cuando el desencadenador comprueba el servidor de archivos. Para evitar devolver un archivo parcialmente escrito, el desencadenador anota la marca de tiempo del archivo que tiene cambios recientes, pero no devuelve inmediatamente dicho archivo. El desencadenador devuelve el archivo solo al volver a sondear el servidor. En ocasiones, este comportamiento puede provocar un retraso de hasta dos veces el intervalo de sondeo del desencadenador. 

Cuando se solicita el contenido del archivo, el desencadenador no recupera los archivos mayores de 50 MB. Para seleccionar archivos de más de 50 MB, siga este patrón:

* Use un desencadenador que devuelva las propiedades de archivo, como **Cuando se agrega o modifica un archivo (solo propiedades)**. 
* Siga el desencadenador con una acción que lee el archivo completo, como **Obtener contenido del archivo mediante ruta de acceso**.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

* La dirección de servidor host de SFTP y las credenciales, que autorizan a la aplicación lógica a crear una conexión con la cuenta de SFTP y acceder a ella.

  Copie y pegue el contenido completo de la clave privada de SSH en la propiedad **Clave privada de SSH** siguiendo el formato multilínea. 
  Aquí tiene pasos de ejemplo que muestran cómo proporcionar la clave privada SSH mediante Notepad.exe:
    
  1. Abra el archivo de clave privada de SSH en Notepad.exe
  2. En el menú **Editar**, seleccione **Seleccionar todo**.
  3. Seleccione **Editar** > **Copiar**.
  4. Al crear la conexión, en la propiedad **Clave privada de SSH**, pegue la clave. No edite manualmente la propiedad **Clave privada de SSH**.

     El conector utiliza la biblioteca SSH.NET, que admite estos formatos de clave privada de SSH y solo la huella dactilar de MD5:

     * RSA 
     * DSA

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica desde donde quiere acceder a la cuenta de SFTP. Para comenzar con un desencadenador de SFTP, [cree una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar una acción de SFTP, inicie la aplicación lógica con otro desencadenador, por ejemplo, el desencadenador **Recurrence**.

## <a name="connect-to-sftp"></a>Conexión a SFTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Para las aplicaciones lógicas en blanco, en el cuadro de búsqueda, escriba "sftp" como filtro. En la lista de desencadenadores, seleccione el que desee. 

   O bien

   Para las aplicaciones lógicas existentes, en el último paso donde desea agregar una acción, elija **Nuevo paso**. 
   En el cuadro de búsqueda, escriba "sftp" como filtro. 
   En la lista de acciones, seleccione la acción que desee.

   Para agregar una acción entre un paso y otro, mueva el puntero sobre la flecha entre ellos. 
   Elija el signo más (**+**) que aparece y seleccione **Agregar una acción**.

1. Proporcione la información necesaria para la conexión y, a continuación, seleccione **Crear**:

1. Proporcione los detalles necesarios para el desencadenador o la acción seleccionados y continúe con la compilación del flujo de trabajo de la aplicación lógica.

## <a name="examples"></a>Ejemplos

### <a name="sftp-trigger-when-a-file-is-added-or-modified"></a>Desencadenador de SFTP: When a file is added or modified

Este desencadenador inicia un flujo de trabajo de aplicación lógica cuando detecta que se ha agregado o cambiado un archivo en un servidor SFTP. Por ejemplo, puede agregar una condición que compruebe el contenido del archivo y decida si obtener ese contenido, en función de si cumple una condición especificada. Por último, puede agregar una acción que obtenga el contenido del archivo y lo coloque en una carpeta en el servidor SFTP. 

**Ejemplo empresarial**: puede usar este desencadenador para supervisar nuevos archivos en una carpeta de SFTP que representan los pedidos de los clientes. Luego, puede usar una acción de SFTP, como **Get file content** para obtener el contenido del pedido para su posterior procesamiento y almacenar ese pedido en una base de datos de pedidos.

### <a name="sftp-action-get-content"></a>Acción de SFTP: Get content

Esta acción obtiene el contenido de un archivo en un servidor SFTP. Por ejemplo, puede agregar el desencadenador del ejemplo anterior y una condición que debe cumplir el contenido del archivo. Si la condición es verdadera, se puede ejecutar la acción que obtiene el contenido. 

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos acerca de desencadenadores, acciones y límites, que se describen en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](/connectors/sftpconnector/) del conector.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
---
title: 'Conexión al servidor SFTP con SSH: Azure Logic Apps | Microsoft Docs'
description: Automatice las tareas que supervisan, crean, administran, envían y reciben archivos en un servidor SFTP mediante SSH y Azure Logic Apps.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
tags: connectors
ms.date: 10/31/2018
ms.openlocfilehash: 336288aaf3817fe267d58a225249bf54cca691bc
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979104"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-ssh-and-azure-logic-apps"></a>Supervisión, creación y administración de archivos SFTP mediante SSH y Azure Logic Apps

Para automatizar las tareas que supervisan, crean, envían y reciben archivos en un servidor [Secure File Transfer Protocol (SFTP)](https://www.ssh.com/ssh/sftp/) mediante el protocolo [Secure Shell (SSH)](https://www.ssh.com/ssh/protocol/), puede crear y automatizar los flujos de trabajo de integración mediante Azure Logic Apps y el conector SFTP-SSH. SFTP es un protocolo de red que proporciona acceso a archivos, transferencia de archivos y administración de archivos a través de cualquier flujo de datos confiable. Estas son algunas tareas de ejemplo que se pueden automatizar: 

* Supervisar cuándo se agregan o cambian archivos
* Obtener, crear, copiar, cambiar el nombre, actualizar, enumerar y eliminar archivos.
* Crear carpetas.
* Obtener contenido de archivos y metadatos
* Extraer archivos en carpetas

En comparación con el [conector SFTP](../connectors/connectors-create-api-sftp.md), el conector SFTP-SSH puede leer o escribir archivos con un tamaño de hasta *1 GB*. Con archivos de más de 1 GB, puede usar el conector SFTP-SSH más la [fragmentación para el tratamiento de mensajes de gran tamaño](../logic-apps/logic-apps-handle-large-messages.md). Si quiere ver más diferencias, revise [Comparación de SFTP-SSH y SFTP](#comparison) más adelante en este artículo.

Puede usar desencadenadores que supervisen eventos en el servidor SFTP y permitir que la salida esté disponible para otras acciones. Puede usar acciones que realicen diversas tareas en el servidor SFTP. También puede hacer que otras acciones de la aplicación lógica usen la salida de las acciones SFTP. Por ejemplo, si recupera regularmente archivos del servidor SFTP, puede enviar por correo electrónico alertas sobre esos archivos y su contenido mediante el conector Office 365 Outlook o el conector Outlook.com.
Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

<a name="comparison"></a>

## <a name="compare-sftp-ssh-versus-sftp"></a>Comparación de SFTP-SSH y SFTP

Estas son otras diferencias importantes entre el conector SFTP-SSH y el conector SFTP donde el conector SFTP-SSH tiene estas funcionalidades:

* Usa la biblioteca <a href="https://github.com/sshnet/SSH.NET" target="_blank">**SSH.NET**</a>, que es una biblioteca de Secure Shell (SSH) de código abierto que admite .NET. 

  > [!NOTE]
  >
  > El conector SFTP-SSH *solo* admite estas claves privadas, formatos, algoritmos y huellas digitales:
  > 
  > * **Formatos de clave privada**: claves RSA (Rivest Shamir Adleman) y DSA (Digital Signature Algorithm) en formato OpenSSH y ssh.com
  > * **Algoritmos de cifrado**: DES-EDE3-CBC, DES-EDE3-CFB, DES-CBC, AES-128-CBC, AES-192-CBC y AES-256-CBC
  > * **Huella digital**: MD5

* Lee o escribe archivos de hasta *1 GB* de tamaño en comparación con el conector SFTP. Con archivos de más de 1 GB, use la [fragmentación para el tratamiento de mensajes de gran tamaño](../logic-apps/logic-apps-handle-large-messages.md). 

* Proporciona la acción **Crear carpeta**, que crea una carpeta en la ruta de acceso especificada en el servidor SFTP.

* Proporciona la acción **Cambiar nombre de archivo**, que cambia el nombre de un archivo en el servidor SFTP.

* Almacena en caché la conexión al servidor SFTP *durante hasta 1 hora*, lo que mejora el rendimiento y reduce el número de intentos de conexión al servidor. Para establecer la duración de este comportamiento de almacenamiento en caché, modifique la propiedad <a href="http://man.openbsd.org/sshd_config#ClientAliveInterval" target="_blank">**ClientAliveInterval**</a> en la configuración de SSH del servidor SFTP. 

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

* La dirección del servidor SFTP y las credenciales de cuenta, que permiten que la aplicación lógica acceda a la cuenta de SFTP. También necesita acceso a una clave privada SSH y a la contraseña de clave privada SSH. 

  > [!IMPORTANT]
  >
  > El conector SFTP-SSH *solo* admite estas claves privadas, formatos, algoritmos y huellas digitales:
  > 
  > * **Formatos de clave privada**: claves RSA (Rivest Shamir Adleman) y DSA (Digital Signature Algorithm) en formato OpenSSH y ssh.com
  > * **Algoritmos de cifrado**: DES-EDE3-CBC, DES-EDE3-CFB, DES-CBC, AES-128-CBC, AES-192-CBC y AES-256-CBC
  > * **Huella digital**: MD5
  >
  > Al crear la aplicación lógica, después de agregar el desencadenador o la acción SFTP-SSH que desee, deberá proporcionar información de conexión para el servidor SFTP. 
  > Si usa una clave privada SSH, asegúrese de ***copiar*** la clave del archivo de clave privada SSH y ***pegar*** esa clave en los datos de conexión. ***No especifique ni edite manualmente la clave***, ya que podría producirse un error en la conexión. 
  > Para más información, consulte los pasos más adelante en este artículo.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica desde donde quiere acceder a la cuenta de SFTP. Para comenzar con un desencadenador SFTP-SSH, [cree una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar una acción SFTP-SSH, inicie la aplicación lógica con otro desencadenador, por ejemplo, el desencadenador **Recurrence**.

## <a name="connect-to-sftp-with-ssh"></a>Conexión a SFTP con SSH

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Para las aplicaciones lógicas en blanco, en el cuadro de búsqueda, escriba "sftp ssh" como filtro. En la lista de desencadenadores, seleccione el que desee. 

   O bien

   Para las aplicaciones lógicas existentes, en el último paso donde desea agregar una acción, elija **Nuevo paso**. 
   En el cuadro de búsqueda, escriba "sftp ssh" como filtro. 
   En la lista de acciones, seleccione la que desee.

   Para agregar una acción entre un paso y otro, mueva el puntero sobre la flecha entre ellos. 
   Elija el signo más (**+**) que aparece y seleccione **Agregar una acción**.

1. Proporcione la información necesaria para la conexión.

   > [!IMPORTANT] 
   >
   > Al escribir la clave privada SSH en la propiedad **SSH private key**, siga estos pasos adicionales para asegurarse de proporcionar el valor completo y correcto para esta propiedad. 
   > Una clave no válida provoca un error de conexión.
   
   Aunque puede usar cualquier editor de texto, estos son los pasos de ejemplo que muestran cómo copiar y pegar correctamente la clave mediante Notepad.exe.
    
   1. Abra el archivo de clave privada SSH en un editor de texto. 
   Estos pasos usan Bloc de notas como ejemplo.

   1. En el menú **Edición** del Bloc de notas, seleccione **Seleccionar todo**.

   1. Seleccione **Editar** > **Copiar**.

   1. En el desencadenador o la acción SFTP-SSH que ha agregado, pegue la clave *completa* que copió en la propiedad **SSH private key**, que admite varias líneas. 
   ***Asegúrese de pegar*** la clave. ***No especifique ni edite la clave manualmente***.

1. Cuando haya terminado de especificar los detalles de conexión, elija **Crear**.

1. Ahora, proporcione los detalles necesarios para el desencadenador o la acción seleccionados y continúe con la compilación del flujo de trabajo de la aplicación lógica.

## <a name="trigger-limits"></a>Límites de desencadenador

El método de funcionamiento de los desencadenadores SFTP-SSH es sondear el sistema de archivos SFTP y buscar cualquier archivo que se haya cambiado desde el último sondeo. Algunas herramientas le permiten conservar la marca de tiempo cuando cambian los archivos. En estos casos, tiene que deshabilitar esta característica para que el desencadenador funcione. Estas son algunas opciones de configuración habituales:

| Cliente SFTP | . | 
|-------------|--------| 
| WinSCP | Vaya a **Options** > **Preferences** > **Transfer** > **Edit** > **Preserve timestamp** > **Disable** (Opciones > Preferencias > Transferir > Editar > Conservar marca de tiempo >Deshabilitar). |
| FileZilla | Vaya a **Transfer** > **Preserve timestamps of transferred files** > **Disable** (Transferir > Conservar marca de tiempo de archivos transferidos > Deshabilitar). | 
||| 

Cuando un desencadenador encuentra un nuevo archivo, el desencadenador comprueba que el nuevo archivo se ha escrito por completo y no parcialmente. Por ejemplo, un archivo podría tener los cambios en curso cuando el desencadenador comprueba el servidor de archivos. Para evitar devolver un archivo parcialmente escrito, el desencadenador anota la marca de tiempo del archivo que tiene cambios recientes, pero no devuelve inmediatamente dicho archivo. El desencadenador devuelve el archivo solo al volver a sondear el servidor. En ocasiones, este comportamiento puede provocar un retraso de hasta dos veces el intervalo de sondeo del desencadenador. 

Cuando se solicita el contenido del archivo, el desencadenador no recupera los archivos mayores de 50 MB. Para obtener archivos de más de 50 MB, siga este patrón:

* Use un desencadenador que devuelva las propiedades de archivo, como **Cuando se agrega o modifica un archivo (solo propiedades)**. 
* Siga el desencadenador con una acción que lee el archivo completo, como **Obtener contenido del archivo mediante ruta de acceso**.

## <a name="examples"></a>Ejemplos

### <a name="sftp---ssh-trigger-when-a-file-is-added-or-modified"></a>Desencadenador SFTP - SSH: When a file is added or modified

Este desencadenador inicia un flujo de trabajo de aplicación lógica cuando se agrega o se modifica un archivo en un servidor SFTP. Por ejemplo, puede agregar una condición que compruebe el contenido del archivo y obtenga el contenido en función de si cumple una condición especificada. Luego, puede agregar una acción que obtenga el contenido del archivo y lo coloque en una carpeta en el servidor SFTP. 

**Ejemplo empresarial**: puede usar este desencadenador para supervisar nuevos archivos en una carpeta de SFTP que representan los pedidos de los clientes. Seguidamente, puede usar una acción SFTP, como **Get file content** para obtener el contenido del pedido para su posterior procesamiento y almacenar ese pedido en una base de datos de pedidos.

### <a name="sftp---ssh-action-get-content"></a>Acción SFTP - SSH: Get content

Esta acción obtiene el contenido de un archivo en un servidor SFTP. Por ejemplo, puede agregar el desencadenador del ejemplo anterior y una condición que debe cumplir el contenido del archivo. Si la condición es verdadera, se puede ejecutar la acción que obtiene el contenido. 

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos acerca de desencadenadores, acciones y límites, que se describen en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](/connectors/sftpconnector/) del conector.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
---
title: Introducción al Explorador de Storage | Microsoft Docs
description: Administre recursos de Azure Storage con el Explorador de Storage.
services: storage
author: cawaMS
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.date: 04/22/2019
ms.author: cawa
ms.openlocfilehash: 2b923a381be9d6cdb1a83f582a541c9e17e117a4
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086625"
---
# <a name="get-started-with-storage-explorer"></a>Introducción al Explorador de Storage

## <a name="overview"></a>Información general

Explorador de Microsoft Azure Storage es una aplicación independiente que facilita el trabajo con los datos de Azure Storage en Windows, macOS y Linux. En este artículo aprenderá varias maneras de conectarse a cuentas de Azure Storage y de administrarlas.

![Explorador de Microsoft Azure Storage][0]

## <a name="prerequisites"></a>Requisitos previos

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

El Explorador de Storage es compatible con las siguientes versiones de Windows:

* Windows 10 (recomendado)
* Windows 8
* Windows 7

Para todas las versiones de Windows, se requiere .NET Framework 4.6.2 o posterior.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

El Explorador de Storage es compatible con las siguientes versiones de macOS:

* macOS 10.12 "Sierra" y versiones posteriores

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

El Explorador de Storage está disponible en [Snap Store](https://snapcraft.io/storage-explorer) para las distribuciones más comunes de Linux y es el método recomendado de instalación. El complemento Explorador de Storage instala automáticamente todas sus dependencias y actualizaciones cuando se publican nuevas versiones en Snap Store.

Para obtener una lista de las distribuciones admitidas, vaya a la [página de instalación acoplada](https://snapcraft.io/docs/installing-snapd).

El Explorador de Storage requiere el uso de un administrador de contraseñas que puede que tenga que conectar manualmente para que el Explorador de Storage funcione correctamente. Puede conectar el Explorador de Storage al administrador de contraseñas del sistema mediante la ejecución del siguiente comando:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

El Explorador de Storage también está disponible como una descarga .tar.gz, pero tendrá que instalar las dependencias manualmente. La instalación .tar.gz es compatible con las siguientes distribuciones de Linux:

* Ubuntu 18.04 x64
* Ubuntu 16.04 x64
* Ubuntu 14.04 x64

La instalación .tar.gz podría funcionar en otras distribuciones, pero solo se admiten oficialmente las que aparecen en la lista anterior.

Para obtener ayuda para instalar el Explorador de Storage en Linux, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting#linux-dependencies).

---

## <a name="download-and-install"></a>Descargar e instalar

[Descarga e instalación del Explorador de Storage](https://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Conexión a una cuenta de almacenamiento o servicio

El Explorador de Microsoft Azure Storage proporciona varias maneras de conectar con las cuentas de almacenamiento. En general, puede:

* [Iniciar sesión en Azure para tener acceso a las suscripciones y sus recursos](#sign-in-to-azure)
* [Vincular un recurso específico de Storage o CosmosDB](#attach-a-specific-resource)

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

> [!NOTE]
> Para obtener acceso total a los recursos tras el inicio de sesión, el Explorador de Storage requiere permisos de administración (Azure Resource Manager) y en el nivel de datos. Esto significa que necesita permisos de Azure Active Directory que proporcionen acceso a la cuenta de Storage, los contenedores de la cuenta y los datos de los contenedores. Si solo tiene permisos en el nivel de datos, considere la posibilidad de [agregar un recurso mediante Azure AD](#add-a-resource-via-azure-ad). Para más información sobre los permisos específicos que requiere el Explorador de Storage, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#role-based-access-control-permission-issues).

1. En el Explorador de Storage, seleccione **Administrar cuentas** para ir al panel Administración de cuentas.

    ![Administración de cuentas][1]

2. En el panel izquierdo se muestran ahora todas las cuentas de Azure en las que ha iniciado sesión. Para conectarse a otra cuenta, seleccione **Agregar una cuenta**.

3. Si quiere iniciar sesión en una nube nacional o en una instancia de Azure Stack, seleccione la lista desplegable **Entorno de Azure** para elegir qué nube de Azure quiere usar. Cuando haya elegido el entorno, seleccione el botón **Iniciar sesión**. Para más información, consulte [Conexión del Explorador de Storage a una suscripción de Azure Stack](/azure-stack/user/azure-stack-storage-connect-se).

    ![Opción de inicio de sesión][2]

4. Después de iniciar sesión correctamente con una cuenta de Azure, la cuenta y las suscripciones de Azure asociadas con esa cuenta se agregan al panel izquierdo. Elija las suscripciones de Azure con las que desea trabajar y luego seleccione **Aplicar**. (Al seleccionar **Todas las suscripciones**, se alterna la selección entre todas o ninguna de las suscripciones de Azure que aparecen).

    ![Selección de suscripciones de Azure][3]

    El panel izquierdo muestra ahora las cuentas de almacenamiento asociadas a las suscripciones de Azure seleccionadas.

    ![Suscripciones de Azure seleccionadas][4]

### <a name="attach-a-specific-resource"></a>Vincular un recurso específico

Hay varias maneras de asociar un recurso al Explorador de Storage:

* [Agregar un recurso a través de Azure AD](#add-a-resource-via-azure-ad). Si solo tiene permisos en el nivel de datos, puede usar esta opción para agregar un contenedor de blobs o un contenedor de almacenamiento de blobs de Azure Data Lake Storage Gen2.
* [Usar una cadena de conexión](#use-a-connection-string). Use esta opción si tiene una cadena de conexión a una cuenta de Storage. El Explorador de Storage admite la clave las cadenas de conexión de [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) y la clave.
* [Usar un URI de SAS](#use-a-sas-uri). Si tiene un [URI de SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) en un contenedor de blobs, recurso compartido de archivos, cola o tabla úselo para asociarlo al recurso. Para obtener un URI de SAS, puede usar el [Explorador de Storage](#generate-a-sas-in-storage-explorer) o [Azure Portal](https://portal.azure.com).
* [Usar un nombre y una clave](#use-a-name-and-key). Si conoce cualquiera de las claves de cuenta para la cuenta de Storage, puede usar esta opción para conectarse rápidamente. Las claves de la cuenta de Storage se encuentran en el panel **Claves de acceso** de la cuenta de Storage en [Azure Portal](https://portal.azure.com).
* [Asociar a un emulador local](#attach-to-a-local-emulator). Si usa uno de los emuladores de Azure Storage disponibles, use esta opción para conectarse fácilmente al emulador.
* [Conectarse a una cuenta de Azure Cosmos DB mediante una cadena de conexión](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string). Use esta opción si tiene una cadena de conexión para una instancia de Cosmos DB.
* [Conectarse a Azure Data Lake Store mediante el URI](#connect-to-azure-data-lake-store-by-uri). Use esta opción si tiene un URI para Azure Data Lake Store.

#### <a name="add-a-resource-via-azure-ad"></a>Agregar un recurso a través de Azure AD

1. Para abrir el cuadro de diálogo **Conectar**, seleccione el botón **Conectar** en la barra de herramientas vertical situada en la parte izquierda:

    ![Opción de conexión a Almacenamiento de Azure][9]

2. Si aún no lo ha hecho, use la opción **Agregar una cuenta de Azure** para iniciar sesión en la cuenta de Azure que tiene acceso al recurso. Después de iniciar sesión, vuelva al cuadro de diálogo **Conectar**.

3. Seleccione la opción **Agregar un recurso a través de Azure Active Directory (Azure AD)** y seleccione **Siguiente**.

4. Seleccione la cuenta de Azure y el inquilino que tiene acceso al recurso de Storage al que desea asociarse. Seleccione **Next** (Siguiente).

5. Elija el tipo de recurso que quiere vincular y, a continuación, escriba la información necesaria para conectarse. La información que escriba en esta página dependerá del tipo de recurso que va a agregar. Asegúrese de elegir el tipo de recurso correcto. Cuando haya especificado toda la información necesaria, seleccione **Siguiente**.

6. Revise el resumen de la conexión y asegúrese de que toda la información es correcta. Si es así, seleccione **Conectar**. De lo contrario, use el botón **Atrás** para volver a las páginas anteriores para corregir cualquier información incorrecta.

Una vez agregada la conexión correctamente, el árbol de recursos irá automáticamente al nodo que representa la conexión. Si no es así, busque en **Local & Attached (Local y asociado)**  > **Cuentas de almacenamiento** >  **(Contenedores asociados)**  > **Contenedores de blob**. Si el Explorador de Storage no pudo agregar la conexión o si no puede acceder a los datos después de agregar correctamente la conexión, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting).

#### <a name="use-a-connection-string"></a>Usar una cadena de conexión

1. Para abrir el cuadro de diálogo **Conectar**, seleccione el botón **Conectar** en la barra de herramientas vertical situada en la parte izquierda:

    ![Opción de conexión a Almacenamiento de Azure][9]

2. Seleccione **Usar una cadena de conexión** y, a continuación, seleccione **Siguiente**.

3. Elija un nombre para mostrar para la conexión y escríbalo en la cadena de conexión. Después, seleccione **Siguiente**.

4. Revise el resumen de la conexión y asegúrese de que toda la información es correcta. Si es así, seleccione **Conectar**. De lo contrario, use el botón **Atrás** para volver a las páginas anteriores para corregir cualquier información incorrecta.

Una vez agregada la conexión correctamente, el árbol de recursos irá automáticamente al nodo que representa la conexión. Si no es así, busque en **Local & Attached (Local y asociado)**  > **Cuentas de almacenamiento**. Si el Explorador de Storage no pudo agregar la conexión o si no puede acceder a los datos después de agregar correctamente la conexión, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting).

#### <a name="use-a-sas-uri"></a>Usar un URI de SAS

1. Para abrir el cuadro de diálogo **Conectar**, seleccione el botón **Conectar** en la barra de herramientas vertical situada en la parte izquierda:

    ![Opción de conexión a Almacenamiento de Azure][9]

2. Seleccione **Usar un URI de firma de acceso compartido (SAS)** y, a continuación, seleccione **Siguiente**.

3. Elija un nombre para mostrar para la conexión y escriba el URI de SAS. El punto de conexión de servicio para el tipo de recurso que se va a asociar debería autorrellenarse. Si utiliza un punto de conexión personalizado, es posible que no lo haga. Seleccione **Next** (Siguiente).

4. Revise el resumen de la conexión y asegúrese de que toda la información es correcta. Si es así, seleccione **Conectar**. De lo contrario, use el botón **Atrás** para volver a las páginas anteriores para corregir cualquier información incorrecta.

Una vez agregada la conexión correctamente, el árbol de recursos irá automáticamente al nodo que representa la conexión. Si no es así, busque en **Local & Attached (Local y asociado)**  > **Cuentas de almacenamiento** >  **(Contenedores asociados)**  > *el nodo de servicio del tipo de contenedor que asoció*. Si el Explorador de Storage no pudo agregar la conexión o si no puede acceder a los datos después de agregar correctamente la conexión, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting).

#### <a name="use-a-name-and-key"></a>Usar un nombre y una clave

1. Para abrir el cuadro de diálogo **Conectar**, seleccione el botón **Conectar** en la barra de herramientas vertical situada en la parte izquierda:

    ![Opción de conexión a Almacenamiento de Azure][9]

2. Seleccione **Usar un nombre y clave de la cuenta de almacenamiento** y, a continuación, seleccione **Siguiente**.

3. Elija un nombre para mostrar para la conexión.

4. Escriba el nombre de la cuenta de Storage y cualquiera de sus claves de acceso.

5. Elija el **dominio de Storage** que va a usar y, a continuación, seleccione **Siguiente**.

6. Revise el resumen de la conexión y asegúrese de que toda la información es correcta. Si es así, seleccione **Conectar**. De lo contrario, use el botón **Atrás** para volver a las páginas anteriores para corregir cualquier información incorrecta.

Una vez agregada la conexión correctamente, el árbol de recursos irá automáticamente al nodo que representa la conexión. Si no es así, busque en **Local & Attached (Local y asociado)**  > **Cuentas de almacenamiento**. Si el Explorador de Storage no pudo agregar la conexión o si no puede acceder a los datos después de agregar correctamente la conexión, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting).

#### <a name="attach-to-a-local-emulator"></a>Vincular un emulador local

Actualmente, el Explorador de Storage admite dos emuladores de almacenamiento oficiales:
* [Emulador de Azure Storage](storage/common/storage-use-emulator.md) (solo Windows)
* [Azurite](https://github.com/azure/azurite) (Windows, macOS o Linux)

Si el emulador escucha en los puertos predeterminados, puede usar el nodo **Emulador: puertos predeterminados** [que se encuentra en **Local & Attached (Local y vinculado)]**  > **Cuentas de almacenamiento** para acceder rápidamente al emulador.

Si quiere usar otro nombre para la conexión o si el emulador no se está ejecutando en los puertos predeterminados, siga estos pasos:

1. Inicie el emulador. Cuando lo haga, tome nota de qué puertos está escuchando el emulador para cada tipo de servicio.

   > [!IMPORTANT]
   > Explorador de Storage no inicia automáticamente el emulador. Debe iniciarlo manualmente.

2. Para abrir el cuadro de diálogo **Conectar**, seleccione el botón **Conectar** en la barra de herramientas vertical situada en la parte izquierda:

    ![Opción de conexión a Almacenamiento de Azure][9]

3. Seleccione **Asociar a un emulador local** y **Siguiente**.

4. Elija un nombre para mostrar para la conexión y escríbalo en los puertos que está escuchando el emulador para cada tipo de servicio. Los cuadros de texto empezarán por los valores del puerto predeterminado para la mayoría de los emuladores. El cuadro **Puerto de archivos** se deja en blanco ya que ninguno de los emuladores oficiales admite el servicio Files actualmente. Si el emulador que está utilizando es compatible con Files, puede especificar el puerto que se está usando. Después, seleccione **Siguiente**.

5. Revise el resumen de la conexión y asegúrese de que toda la información es correcta. Si es así, seleccione **Conectar**. De lo contrario, use el botón **Atrás** para volver a las páginas anteriores para corregir cualquier información incorrecta.

Una vez agregada la conexión correctamente, el árbol de recursos irá automáticamente al nodo que representa la conexión. Si no es así, busque en **Local & Attached (Local y asociado)**  > **Cuentas de almacenamiento**. Si el Explorador de Storage no pudo agregar la conexión o si no puede acceder a los datos después de agregar correctamente la conexión, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting).

#### <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Conexión a una cuenta de Azure Cosmos DB mediante una cadena de conexión

Además de administrar cuentas de Azure Cosmos DB mediante una suscripción de Azure, una manera alternativa de conectarse a Azure Cosmos DB es usar una cadena de conexión. Para ello, siga estos pasos.

1. En la parte izquierda del árbol de recursos, expanda **Local and Attached** (Local y asociado), haga clic con el botón derecho en **Cuentas de Azure Cosmos DB** y seleccione **Connect to Azure Cosmos DB** (Conectar a Azure Cosmos DB).

    ![Conectarse a Azure Cosmos DB mediante una cadena de conexión][21]

2. Seleccione la API de Azure Cosmos DB, escriba los datos de la **cadena de conexión** y, a continuación, seleccione **Aceptar** para conectarse a la cuenta de Azure Cosmos DB. Para más información sobre cómo recuperar la cadena de conexión, consulte [Obtener la cadena de conexión](https://docs.microsoft.com/azure/cosmos-db/manage-account).

    ![Cadena de conexión][22]

#### <a name="connect-to-azure-data-lake-store-by-uri"></a>Conexión a Azure Data Lake Store mediante el URI

Si desea acceder a un recurso que no está en su suscripción, necesitará alguien que pueda acceder a ese recurso para que le proporcione el identificador URI del recurso. Después de iniciar sesión, puede conectarse a Data Lake Store con el URI. Para ello, siga estos pasos.

1. Abra el Explorador de Storage.
2. En el panel izquierdo, expanda **Local and Attached** (Local y asociada).
3. Haga clic con el botón derecho en **Data Lake Store**. En el menú contextual, seleccione **Connect to Data Lake Store** (Conectar a Data Lake Store).

    ![Menú contextual Connect to Data Lake Store (Conectar a Data Lake Store)](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Escriba el URI. La herramienta va a la ubicación de la dirección URL que acaba de escribir.

    ![Cuadro de diálogo Connect to Data Lake Store (Conectar a Data Lake Store)](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![Resultado de Connect to Data Lake Store (Conectar a Data Lake Store)](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)


## <a name="generate-a-sas-in-storage-explorer"></a>Generar una SAS en el Explorador de Storage

### <a name="account-level-sas"></a>SAS de nivel de cuenta

1. Haga clic con el botón derecho en la cuenta de Storage que quiere compartir y seleccione **Obtener firma de acceso compartido**.

    ![Opción de menú contextual para obtener la SAS][14]

2. En el cuadro de diálogo **Generate Shared Access Signature** (Generar firma de acceso compartido), especifique el período y los permisos que desee para la cuenta y, luego, seleccione **Crear**.

    ![Cuadro de diálogo Get SAS (Obtener SAS)][15]

3. Ya puede copiar la **cadena de conexión** o la **cadena de consulta** sin formato en el Portapapeles.

### <a name="service-level-sas"></a>SAS de nivel de servicio

[Obtención de la SAS para un contenedor de blobs en el Explorador de Storage](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)

## <a name="search-for-storage-accounts"></a>Búsqueda de cuentas de almacenamiento

Si necesita encontrar un recurso de almacenamiento y no sabe dónde está, puede usar el cuadro de búsqueda en la parte superior del panel izquierdo para buscar el recurso.

A medida que escribe en el cuadro de búsqueda, en el panel izquierdo se muestran todos los recursos que coinciden con el valor de búsqueda especificado hasta ese punto. Por ejemplo, en la siguiente captura de pantalla se muestra una búsqueda de **puntos de conexión**:

![Búsqueda de cuenta de almacenamiento][23]

> [!NOTE]
> Para mejorar la velocidad de la búsqueda, use el panel Administración de cuentas para anular la selección de las suscripciones que no contengan el elemento que busca. También puede hacer clic con el botón derecho en un nodo y seleccionar **Buscar desde aquí** para comenzar a buscar desde un nodo específico.
>
>

## <a name="next-steps"></a>Pasos siguientes

* [Administración de recursos de Azure Blob Storage con el Explorador de Storage](vs-azure-tools-storage-explorer-blobs.md)
* [Administración de Azure Cosmos DB en el Explorador de Storage (versión preliminar)](./cosmos-db/storage-explorer.md)
* [Administración de recursos de Azure Data Lake Store en el Explorador de Storage](./data-lake-store/data-lake-store-in-storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-SignInSelected.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccountPanel.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/PortalAccessKeys.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccessKeys.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-AddWithKeySelected.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-NameAndKeyPage.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount-Detach.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/GetSharedAccessSignature.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SharedAccessSignatureDialog.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-WithConnStringOrSASSelected.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-1.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithSASAccount.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-2.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ServiceAttachedWithSAS.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-db-by-connection-string.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Search.png

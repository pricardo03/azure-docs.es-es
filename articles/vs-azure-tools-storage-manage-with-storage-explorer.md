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
ms.openlocfilehash: f7dd6d3d30f34ba2c69b40111bb28d484ce572e7
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508743"
---
# <a name="get-started-with-storage-explorer"></a>Introducción al Explorador de Storage

## <a name="overview"></a>Información general

El Explorador de Azure Storage es una aplicación independiente que permite trabajar fácilmente con datos de Azure Storage en Windows, macOS y Linux. En este artículo aprenderá varias maneras de conectarse a cuentas de Azure Storage y de administrarlas.

![Explorador de Microsoft Azure Storage][0]

## <a name="prerequisites"></a>Requisitos previos

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

El Explorador de Azure Storage es compatible con las siguientes versiones de Windows:

* Windows 10 (recomendado)
* Windows 8
* Windows 7

Para todas las versiones de Windows, se requiere .NET Framework 4.6.2 o posterior.

[Descarga e instalación del Explorador de Storage](https://www.storageexplorer.com)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

El Explorador de Azure Storage es compatible con las siguientes versiones de macOS:

* macOS 10.12 "Sierra" y versiones posteriores

[Descarga e instalación del Explorador de Storage](https://www.storageexplorer.com)

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

El Explorador de Azure Storage es compatible con las siguientes distribuciones de Linux:

* Ubuntu 18.04 x64
* Ubuntu 16.04 x64
* Ubuntu 14.04 x64

El Explorador de Azure Storage puede funcionar en otras distribuciones, pero las mencionadas anteriormente son las únicas oficialmente compatibles.

Para obtener ayuda para instalar el Explorador de Storage en Linux, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting#linux-dependencies).

Las [notas de la versión](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) del Explorador de Azure Storage contienen pasos específicos para algunas distribuciones.

[Descarga e instalación del Explorador de Storage](https://www.storageexplorer.com)

---

## <a name="connect-to-a-storage-account-or-service"></a>Conexión a una cuenta de almacenamiento o servicio

El Explorador de Microsoft Azure Storage proporciona varias maneras de conectar con las cuentas de almacenamiento. En general, puede:

* [Iniciar sesión en Azure para tener acceso a las suscripciones y sus recursos](#sign-in-to-azure)
* [Vincular un recurso específico de Storage o CosmosDB](#attach-a-specific-resource)

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

> [!NOTE]
> Para obtener acceso total a los recursos tras el inicio de sesión, el Explorador de Storage requiere permisos de administración (ARM) y de capa de datos. Esto significa que necesita permisos de Azure AD que proporcionan acceso a la cuenta de Storage, los contenedores de la cuenta y los datos de los contenedores. Si solo tiene permisos en la capa de datos, considere la opción de [vincular con Azure AD](#add-a-resource-via-azure-ad). Para obtener más información sobre los permisos exactos que requiere el Explorador de Storage, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#role-based-access-control-permission-issues).
>
>

1. En el Explorador de Storage, seleccione **Manage Accounts** (Administrar cuentas) para ir a **Account Management Panel** (Panel de administración de cuentas).

    ![Administración de cuentas][1]

2. En el panel izquierdo se muestran ahora todas las cuentas de Azure en las que ha iniciado sesión. Para conectarse a otra cuenta, seleccione **Add an account** (Agregar una cuenta).

3. Si quiere iniciar sesión en una nube nacional o en una instancia de Azure Stack, haga clic en la lista desplegable **Azure environment** (Entorno de Azure) para seleccionar qué nube de Azure quiere usar. Cuando haya elegido el entorno, haga clic en el botón **Sign in...** (Iniciar sesión...). Si va a iniciar sesión en Azure Stack, consulte [Conexión del Explorador de Storage a una suscripción de Azure Stack](/azure-stack/user/azure-stack-storage-connect-se) para más información.

    ![Opción de inicio de sesión][2]

4. Después de iniciar sesión correctamente con una cuenta de Azure, la cuenta y las suscripciones de Azure asociadas con esa cuenta se agregan al panel izquierdo. Seleccione las suscripciones de Azure con las que quiere trabajar y, luego, seleccione **Apply** (Aplicar) (al seleccionar **All subscriptions:** (Todas las suscripciones:) se alterna entre seleccionar todas o ninguna de las suscripciones de Azure enumeradas).

    ![Selección de suscripciones de Azure][3]

    El panel izquierdo muestra ahora las cuentas de almacenamiento asociadas a las suscripciones de Azure seleccionadas.

    ![Suscripciones de Azure seleccionadas][4]

### <a name="attach-a-specific-resource"></a>Vincular un recurso específico
    
Hay una variedad de opciones para vincular un recurso al Explorador de Storage. Puede:

* [Agregar un recurso a través de Azure AD](#add-a-resource-via-azure-ad): Si solo tiene permisos en la capa de datos, puede usar esta opción para agregar un contenedor de blobs o un contenedor de blobs de ADLS Gen2.
* [Usar una cadena de conexión](#use-a-connection-string): Si tiene una cadena de conexión para una cuenta de Storage. El Explorador de Storage admite la clave las cadenas de conexión de [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) y la clave.
* [Usar un URI de SAS](#use-a-sas-uri): Si tiene un URI de [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) en un contenedor de blobs, recurso compartido de archivos, cola o tabla. Para obtener un URI de SAS, puede usar el [Explorador de Storage](#generate-a-sas-in-storage-explorer) o [Azure Portal](https://portal.azure.com).
* [Usar un nombre y una clave](#use-a-name-and-key): Si conoce cualquiera de las claves de cuenta para la cuenta de Storage, puede usar esta opción para conectarse rápidamente. Las claves para la cuenta de Storage se encuentran en la hoja **Claves de acceso** de la cuenta de Storage en [Azure Portal](https://portal.azure.com).
* [Vincular un emulador local](#attach-to-a-local-emulator): Si usa uno de los emuladores de Azure Storage disponibles, use esta opción para conectarse fácilmente al emulador.
* [Conexión a una cuenta de Azure Cosmos DB mediante una cadena de conexión](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string): Si tiene una cadena de conexión para una instancia de CosmosDB.
* [Conectarse a Azure Data Lake Store mediante el URI](#connect-to-azure-data-lake-store-by-uri): Si tiene un URI para Azure Data Lake Store.

#### <a name="add-a-resource-via-azure-ad"></a>Agregar un recurso a través de Azure AD

1. Para abrir el **cuadro de diálogo Conectar**, haga clic en el **botón Conectar** en la barra de herramientas vertical situada en la parte izquierda.

    ![Opción de conexión a Almacenamiento de Azure][9]

2. Si aún no lo ha hecho, use la opción **Agregar una cuenta de Azure** para iniciar sesión en la cuenta de Azure que tiene acceso al recurso. Después de iniciar sesión, vuelva al **cuadro de diálogo Conectar**.

3. Seleccione la opción **Agregar un recurso a través de Azure Active Directory (Azure AD)** y haga clic en **Siguiente**.

4. Seleccione la cuenta de Azure que tiene acceso al recurso de Storage que quiere vincular y la suscripción en la que se encuentra el recurso y, a continuación, haga clic en **Siguiente**.

5. Elija el tipo de recurso que quiere vincular y, a continuación, escriba la información necesaria para conectarse. Las entradas en esta página cambiarán según el tipo de recurso que agregue. Asegúrese de elegir el tipo de recurso correcto. Después de haber rellenado la información necesaria, haga clic en **Siguiente**.

6. Revise el resumen de la conexión y asegúrese de que toda la información es correcta. Si toda la información es correcta, a continuación, haga clic en **Conectar**; en caso contrario, vuelva a las páginas anteriores con el botón **Atrás** para corregir la información incorrecta.

Una vez agregada la conexión correctamente, el árbol de recursos navegará automáticamente hasta el nodo que representa la conexión. Si por algún motivo no es así, busque en **Local & Attached** (Local y vinculado) → **Cuentas de almacenamiento** → **(Attached Containers)** (Contenedores vinculados) → **Contenedores de blob**. Si el Explorador de Storage no pudo agregar la conexión o si no puede acceder a los datos después de agregar correctamente la conexión, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) para obtener ayuda.

#### <a name="use-a-connection-string"></a>Usar una cadena de conexión

1. Para abrir el **cuadro de diálogo Conectar**, haga clic en el **botón Conectar** en la barra de herramientas vertical situada en la parte izquierda.

    ![Opción de conexión a Almacenamiento de Azure][9]

2. Seleccione la opción **Usar una cadena de conexión** y haga clic en **Siguiente**.

3. Elija un nombre para mostrar para la conexión y escríbalo en la cadena de conexión. A continuación, haga clic en **Siguiente**.

4. Revise el resumen de la conexión y asegúrese de que toda la información es correcta. Si toda la información es correcta, a continuación, haga clic en **Conectar**; en caso contrario, vuelva a las páginas anteriores con el botón **Atrás** para corregir la información incorrecta.

Una vez agregada la conexión correctamente, el árbol de recursos navegará automáticamente hasta el nodo que representa la conexión. Si por algún motivo no es así, busque en **Local & Attached** (Local y vinculado) → **Cuentas de almacenamiento**. Si el Explorador de Storage no pudo agregar la conexión o si no puede acceder a los datos después de agregar correctamente la conexión, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) para obtener ayuda.

#### <a name="use-a-sas-uri"></a>Usar un URI de SAS

1. Para abrir el **cuadro de diálogo Conectar**, haga clic en el **botón Conectar** en la barra de herramientas vertical situada en la parte izquierda.

    ![Opción de conexión a Almacenamiento de Azure][9]

2. Seleccione la opción **Usar un URI de firma de acceso compartido (SAS)** y haga clic en **Siguiente**.

3. Elija un nombre para mostrar para la conexión y escríbalo en el URI de SAS. El punto de conexión de servicio para el tipo de recurso que se va a vincular debería autorrellenarse. Si está usando un extremo personalizado, es posible que no lo haga. Haga clic en **Next**.

4. Revise el resumen de la conexión y asegúrese de que toda la información es correcta. Si toda la información es correcta, a continuación, haga clic en **Conectar**; en caso contrario, vuelva a las páginas anteriores con el botón **Atrás** para corregir la información incorrecta.

Una vez agregada la conexión correctamente, el árbol de recursos navegará automáticamente hasta el nodo que representa la conexión. Si por algún motivo no es así, busque en **Local & Attached** (Local y vinculado) → **Cuentas de almacenamiento** → **(Attached Containers)** (Contenedores vinculados) → **el nodo de servicio para el tipo de contenedor que ha vinculado**. Si el Explorador de Storage no pudo agregar la conexión o si no puede acceder a los datos después de agregar correctamente la conexión, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) para obtener ayuda.

#### <a name="use-a-name-and-key"></a>Usar un nombre y una clave

1. Para abrir el **cuadro de diálogo Conectar**, haga clic en el **botón Conectar** en la barra de herramientas vertical situada en la parte izquierda.

    ![Opción de conexión a Almacenamiento de Azure][9]

2. Seleccione la opción **Usar un nombre y clave de la cuenta de almacenamiento** y haga clic en **Siguiente**.

3. Elija un nombre para mostrar para la conexión.

4. Escriba el nombre de la cuenta de Storage y cualquiera de sus claves de almacenamiento.

5. Elija el **dominio de Storage** que va a usar y, a continuación, haga clic en **Siguiente**.

4. Revise el resumen de la conexión y asegúrese de que toda la información es correcta. Si toda la información es correcta, a continuación, haga clic en **Conectar**; en caso contrario, vuelva a las páginas anteriores con el botón **Atrás** para corregir la información incorrecta.

Una vez agregada la conexión correctamente, el árbol de recursos navegará automáticamente hasta el nodo que representa la conexión. Si por algún motivo no es así, busque en **Local & Attached** (Local y vinculado) → **Cuentas de almacenamiento**. Si el Explorador de Storage no pudo agregar la conexión o si no puede acceder a los datos después de agregar correctamente la conexión, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) para obtener ayuda.

#### <a name="attach-to-a-local-emulator"></a>Vincular un emulador local

El Explorador de Storage es compatible con emuladores en todas las plataformas. Los dos emuladores oficiales que están disponibles actualmente son:
* [Emulador de Azure Storage](storage/common/storage-use-emulator.md) (solo Windows)
* [Azurite](https://github.com/azure/azurite) (Windows, macOS o Linux)

Si el emulador se ejecuta en los puertos predeterminados, puede usar el nodo **Emulador: puertos predeterminados**, que se puede encontrar siempre en **Local & Attached** (Local y vinculado) → **Cuentas de almacenamiento** para acceder rápidamente al emulador. Si quiere usar otro nombre para la conexión o si el emulador no se está ejecutando en los puertos predeterminados, siga los pasos que se indican a continuación.

1. Inicie el emulador. Cuando lo haga, tome nota de qué puertos está escuchando el emulador para cada tipo de servicio. Necesitará esta información posteriormente.

   > [!IMPORTANT]
   > Explorador de Storage no inicia automáticamente el emulador. Debe iniciarlo usted mismo.

2. Para abrir el **cuadro de diálogo Conectar**, haga clic en el **botón Conectar** en la barra de herramientas vertical situada en la parte izquierda.

    ![Opción de conexión a Almacenamiento de Azure][9]

3. Seleccione la opción **Attach to a local emulator** (Vincular a un emulador local) y haga clic en **Siguiente**.

4. Elija un nombre para mostrar para la conexión y escríbalo en los puertos que está escuchando el emulador para cada tipo de servicio. De manera predeterminada, los cuadros de texto incluirán los valores del puerto predeterminado para la mayoría de los emuladores. El **puerto de los archivos** también se deja en blanco de manera predeterminada ya que ninguno de los emuladores oficiales admite el servicio de archivos actualmente. Si, no obstante, el emulador que está usando lo admite, puede escribir en el puerto que se está usando. Haga clic en **Next**.

5. Revise el resumen de la conexión y asegúrese de que toda la información es correcta. Si toda la información es correcta, a continuación, haga clic en **Conectar**; en caso contrario, vuelva a las páginas anteriores con el botón **Atrás** para corregir la información incorrecta.

Una vez agregada la conexión correctamente, el árbol de recursos navegará automáticamente hasta el nodo que representa la conexión. Si por algún motivo no es así, busque en **Local & Attached** (Local y vinculado) → **Cuentas de almacenamiento**. Si el Explorador de Storage no pudo agregar la conexión o si no puede acceder a los datos después de agregar correctamente la conexión, consulte la [Guía de solución de problemas](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) para obtener ayuda.

#### <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Conexión a una cuenta de Azure Cosmos DB mediante una cadena de conexión

Además de administrar cuentas de Azure Cosmos DB a través de una suscripción de Azure, una manera alternativa de conectarse a Azure Cosmos DB es usar una cadena de conexión. Siga estos pasos para conectarse usando una cadena de conexión.

1. Busque **Local and Attached** (Locales y adjuntas) en el árbol de la izquierda, haga clic con el botón derecho en **Cuentas de Azure Cosmos DB**, elija **Connect to Azure Cosmos DB...** (Conectar a Azure Cosmos DB).

    ![Conexión a Azure Cosmos DB mediante una cadena de conexión][21]

2. Elija la API de Azure Cosmos DB, pegue su **cadena de conexión** y, a continuación, haga clic en **Aceptar** para conectarse a la cuenta de Azure Cosmos DB. Para más información sobre cómo recuperar la cadena de conexión, vea [Obtener la cadena de conexión](https://docs.microsoft.com/azure/cosmos-db/manage-account).

    ![connection-string][22]

#### <a name="connect-to-azure-data-lake-store-by-uri"></a>Conexión a Azure Data Lake Store mediante el URI

Si desea acceder a recursos que no existen en su suscripción, a cuyos URI le conceden acceso otros usuarios, se puede conectar a Data Lake Store con el URI una vez iniciada la sesión. Consulte los pasos siguientes.

1. Abra el Explorador de Storage.
2. En el panel izquierdo, expanda **Local and Attached** (Local y asociada).
3. Haga clic con el botón derecho en **Data Lake Store** y, en el menú contextual, seleccione **Connect to Data Lake Store...** (Conectar a Data Lake Store...).

    ![Menú contextual Connect to Data Lake Store... (Conectar a Data Lake Store...)](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Escriba el URI. La herramienta le lleva a la ubicación de la dirección URL que acaba de escribir.

    ![Cuadro de diálogo Connect to Data Lake Store... (Conectar a Data Lake Store...)](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![Resultado de Connect to Data Lake Store... (Conectar a Data Lake Store...)](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)


## <a name="generate-a-sas-in-storage-explorer"></a>Generar una SAS en el Explorador de Storage

### <a name="account-level-sas"></a>SAS de nivel de cuenta

1. Haga clic con el botón derecho en la cuenta de almacenamiento que quiere compartir y seleccione **Obtener firma de acceso compartido**.

    ![Opción de menú contextual para obtener la SAS][14]

2. En el cuadro de diálogo **Generate Shared Access Signature** (Generar firma de acceso compartido), especifique la franja de tiempo y los permisos que quiere para la cuenta y, luego, haga clic en el botón **Create** (Crear).

    ![Cuadro de diálogo Get SAS (Obtener SAS)][15]

3. Ya puede copiar la **cadena de conexión** o la **cadena de consulta** sin formato en el Portapapeles.

### <a name="service-level-sas"></a>SAS de nivel de servicio

[Obtención de la SAS para un contenedor de blobs en el Explorador de Storage](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)

## <a name="search-for-storage-accounts"></a>Búsqueda de cuentas de almacenamiento

Si necesita encontrar un recurso de almacenamiento y no sabe dónde está, puede usar el cuadro de búsqueda en la parte superior del panel izquierdo para buscar el recurso.

A medida que escribe en el cuadro de búsqueda, en el panel izquierdo se muestran todos los recursos que coinciden con el valor de búsqueda especificado hasta ese punto. Por ejemplo, en la siguiente captura de pantalla se muestra una búsqueda de **puntos de conexión**:

![Búsqueda de cuenta de almacenamiento][23]

> [!NOTE]
> Use **Account Management Panel** (Panel de administración de cuentas) para deseleccionar las suscripciones que no contengan el elemento que busca para mejorar el tiempo de ejecución de la búsqueda. También puede hacer clic con el botón derecho en un nodo y seleccionar **Search From Here** (Buscar desde aquí) para comenzar a buscar desde un nodo específico.
>
>

## <a name="next-steps"></a>Pasos siguientes

* [Administración de recursos de Azure Blob Storage con el Explorador de Storage](vs-azure-tools-storage-explorer-blobs.md)
* [Administración de Azure Cosmos DB en el Explorador de Azure Storage (versión preliminar)](./cosmos-db/storage-explorer.md)
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

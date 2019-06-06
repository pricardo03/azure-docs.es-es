---
title: archivo de inclusión
description: archivo de inclusión
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: a7c994f85d90e94d514bb4e4f91a6644ed45432c
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66455186"
---
1. Descargar al cliente inicios rápidos del SDK para las plataformas siguientes:
    
    [iOS (Objective-C)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS)  
    [iOS (Swift)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS-Swift)  
    [Android (Java)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/android)  
    [Xamarin.iOS](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.iOS)  
    [Xamarin.Android](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.android)  
    [Xamarin.Forms](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.forms)  
    [Cordova](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/cordova)  
    [Windows (C#)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/windows-uwp-cs)  

    > [!NOTE]
    > Si usa el proyecto de iOS necesita descargar "azuresdk-iOS -\*.zip" de [versión más reciente de GitHub](https://github.com/Azure/azure-mobile-apps-ios-client/releases/latest). Descomprima y agregue el `MicrosoftAzureMobile.framework` archivo a la raíz del proyecto.
    >

2. Tendrá que agregar una conexión de base de datos o conectarse a una conexión existente. En primer lugar, determine si podrá crear un almacén de datos o usar uno existente.

    - **Crear un nuevo almacén de datos**: Si vas a crear un almacén de datos, use la Guía de inicio rápido siguiente:

        [Inicio rápido: Introducción a las bases de datos únicas en Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-quickstart-guide)

    - **Origen de datos existente**: Siga las instrucciones siguientes, si desea usar una conexión de base de datos existente

        1. Formato de cadena de conexión de base de datos de SQL: `Data Source=tcp:{your_SQLServer},{port};Initial Catalog={your_catalogue};User ID={your_username};Password={your_password}`

           **{your_SQLServer}**  Nombre del servidor, esto puede encontrarse en la página de información general de la base de datos y suele ser en forma de "server_name.database.windows.net".
            **{port}**  normalmente 1433.
            **{your_catalogue}**  Nombre de la base de datos.
            **{your_username}**  Nombre de usuario para tener acceso a la base de datos.
            **{Neptuno}**  Contraseña para tener acceso a la base de datos.

            [Más información sobre el formato de cadena de conexión SQL](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-string-syntax#sqlclient-connection-strings)

        2. Agregue la cadena de conexión a su **aplicación móvil** en App Service, puede administrar las cadenas de conexión para la aplicación mediante el **configuración** opción en el menú.

            Para agregar una cadena de conexión:

            1. Haga clic en el **configuración de la aplicación** ficha.

            2. Haga clic en **[+] nueva cadena de conexión**.

            3. Deberá proporcionar **nombre**, **valor** y **tipo** para la cadena de conexión.

            4. Tipo **nombre** como `MS_TableConnectionString`

            5. Valor debe ser la cadena de conexión que tiene el formato en el paso antes.

            6. Elija si va a agregar una cadena de conexión a una base de datos de SQL Azure **SQLAzure** en **tipo**.

3. Azure Mobile Apps tiene SDK de back-ends .NET y Node.js.

   - **Back-end de Node.js**
    
     Si va a usar la aplicación de inicio rápido de Node.js, siga las instrucciones siguientes.

     1. En el portal de Azure, vaya a **Easy Tables**, verá esta pantalla.
      
        ![Nodo de tablas fáciles](./media/app-service-mobile-configure-new-backend/node-easy-tables.png)

     2. Asegúrese de que la cadena de conexión SQL ya está incorporada en el **configuración** ficha. A continuación, active la casilla de **reconozco que esta acción sobrescribirá el contenido del sitio Web** y haga clic en el **crear tabla TodoItem** botón.
     
        ![Configuración del nodo tablas fáciles](./media/app-service-mobile-configure-new-backend/node-easy-tables-configuration.png)

     3. En **Easy Tables**, haga clic en el **+ agregar** botón.
    
        ![Botón de agregar tablas fáciles de nodo](./media/app-service-mobile-configure-new-backend/node-easy-tables-add.png)

     4. Crear un `TodoItem` tabla con el acceso anónimo.
      
        ![Nodo de tablas fáciles Agregar tabla](./media/app-service-mobile-configure-new-backend/node-easy-tables-table-add.png)

   - **Back-end de .NET**
    
        Si va a usar la aplicación de inicio rápido. NET, siga las instrucciones siguientes.

        1. Descargue el proyecto de servidor .NET de Azure Mobile Apps desde la [repositorio azure-mobile-apps-guías de inicio rápido](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/backend/dotnet/Quickstart).

        2. Compile el proyecto de servidor .NET localmente en Visual Studio.

        3. En Visual Studio, abra el Explorador de soluciones, haga doble clic en `ZUMOAPPNAMEService` del proyecto, haga clic en **publicar**, verá un `Publish to App Service` ventana. Si está trabajando en Mac, consulte otras formas de implementar la aplicación [aquí](https://docs.microsoft.com/azure/app-service/deploy-local-git).
        
           ![Publicación de Visual studio](./media/app-service-mobile-configure-new-backend/visual-studio-publish.png)

        4. Seleccione **App Service** como destino de publicación, a continuación, haga clic en **seleccionar existente**, a continuación, haga clic en el **publicar** situado en la parte inferior de la ventana.

        5. Deberá iniciar sesión en Visual Studio con su suscripción de Azure en primer lugar. Seleccione el `Subscription`, `Resource Group`y, a continuación, seleccione el nombre de la aplicación. Cuando esté listo, haga clic en **Aceptar**, se implementará el proyecto de servidor de .NET que tiene localmente en el back-end de App Service. Cuando finalice la implementación, se le redirigirá a `http://{zumoappname}.azurewebsites.net/` en el explorador.
        
           ![Back-end está activo](./media/app-service-mobile-configure-new-backend/backend-is-up.png)

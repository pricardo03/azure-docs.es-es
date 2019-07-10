---
title: 'Protección de la conexión con SQL Database mediante una identidad administrada: Azure App Service | Microsoft Docs'
description: Aprenda a hacer que la conectividad con la base de datos sea más segura con una identidad administrada y también a aplicar esto a otros servicios de Azure.
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 06/21/2019
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 31535642526c608ad0ae29e5c0e3c93368e184ad
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481011"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>Tutorial: Protección de la conexión con Azure SQL Database desde App Service mediante una identidad administrada

[App Service](overview.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático en Azure. También proporciona una [identidad administrada](overview-managed-identity.md) para la aplicación, la cual constituye una solución inmediata para proteger el acceso a [Azure SQL Database](/azure/sql-database/) y a otros servicios de Azure. Las identidades administradas de App Service hacen que su aplicación sea más segura mediante la eliminación de los secretos de aplicación como, por ejemplo, las credenciales en las cadenas de conexión. En este tutorial, agregará una identidad administrada a la aplicación web ASP.NET de ejemplo que se creó en [Tutorial: Compilación de una aplicación ASP.NET en Azure con SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md). Cuando haya terminado, la aplicación de ejemplo se conectará a SQL Database de forma segura sin necesidad de nombres de usuario ni contraseñas.

> [!NOTE]
> Este escenario lo admite actualmente .NET Framework 4.7.2 y versiones posteriores. [.NET core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2) admite este escenario, pero todavía no se ha incluido en las imágenes predeterminadas de App Service. 
>

Aprenderá a:

> [!div class="checklist"]
> * Habilitar identidades administradas
> * Conceder a SQL Database acceso a la identidad administrada
> * Configurar Entity Framework para utilizar la autenticación de Azure AD con SQL Database
> * Conectarse a SQL Database desde Visual Studio mediante la autenticación de Azure AD

> [!NOTE]
>La autenticación de Azure AD es _distinta_ de la [autenticación integrada de Windows](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) en Active Directory (AD DS) local. AD DS y Azure AD utilizan protocolos de autenticación completamente diferentes. Para más información, consulte [Documentación de Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Este artículo continúa donde lo dejó en [Tutorial: Compilación de una aplicación ASP.NET en Azure con SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md). Si no lo ha hecho ya, siga este tutorial primero. Como alternativa, puede adaptar los pasos para su propia aplicación ASP.NET con SQL Database.

Para depurar la aplicación con SQL Database como back-end, asegúrese de [permitir la conexión de cliente desde el equipo](app-service-web-tutorial-dotnet-sqldatabase.md#allow-client-connection-from-your-computer).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="grant-azure-ad-user-access-to-database"></a>Concesión de acceso de usuario mediante Azure AD a la base de datos

Primero debe habilitar la autenticación de Azure AD para SQL Database mediante la asignación de un usuario de Azure AD como administrador de Active Directory para el servidor de SQL Database. Este usuario no es la cuenta Microsoft que usó para suscribirse a Azure. Debe ser un usuario que haya creado, importado, sincronizado o invitado en Azure AD. Para más información sobre los usuarios de Azure AD permitidos, consulte las [características de Azure AD y las limitaciones de en SQL Database](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations). 

Busque el identificador de objeto del usuario de Azure AD mediante [`az ad user list`](/cli/azure/ad/user?view=azure-cli-latest#az-ad-user-list) y reemplace *\<nombre-principal-de-usuario>* . El resultado se guardará en una variable.

```azurecli-interactive
azureaduser=$(az ad user list --filter "userPrincipalName eq '<user-principal-name>'" --query [].objectId --output tsv)
```
> [!TIP]
> Para ver la lista de todos los nombres principales de usuario de Azure AD, ejecute `az ad user list --query [].userPrincipalName`.
>

Agregue este usuario de Azure AD como administrador de Active Directory mediante el comando [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin-create) en Cloud Shell. En el siguiente comando, reemplace *\<nombre-del-servidor>* .

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server-name> --display-name ADMIN --object-id $azureaduser
```

Para más información sobre cómo agregar un administrador de Active Directory, consulte [Provision an Azure Active Directory administrator for your Azure SQL Database Server](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) (Aprovisionamiento de un administrador de Azure Active Directory para el servidor de Azure SQL Database)

## <a name="set-up-visual-studio"></a>Configuración de Visual Studio

Para habilitar el desarrollo y la depuración en Visual Studio, agregue el usuario de Azure AD en Visual Studio; para ello, seleccione **Archivo** > **Configuración de la cuenta** en el menú y haga clic en **Agregar un cuenta**.

Para establecer el usuario de Azure AD para la autenticación de servicio de Azure, seleccione **Herramientas** > **Opciones** en el menú y, después, **Azure Service Authentication** (Autenticación del servicio de Azure) > **Selección de cuentas**. Seleccione el usuario de Azure AD que agregó y haga clic en **Aceptar**.

Ahora está listo para desarrollar y depurar la aplicación con SQL Database como back-end y mediante la autenticación de Azure AD.

## <a name="modify-aspnet-project"></a>Modificación de un proyecto ASP.NET

En Visual Studio, abra la consola del administrador de paquetes y agregue el paquete NuGet [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication):

```powershell
Install-Package Microsoft.Azure.Services.AppAuthentication -Version 1.2.0
```

En *Web.config*, desde el principio del archivo, realice los siguientes cambios:

- En `<configSections>`, agregue la siguiente declaración de la sección:

    ```xml
    <section name="SqlAuthenticationProviders" type="System.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
    ```

- Bajo la etiqueta de cierre `</configSections>`, agregue el siguiente código XML para `<SqlAuthenticationProviders>`.

    ```xml
    <SqlAuthenticationProviders>
      <providers>
        <add name="Active Directory Interactive" type="Microsoft.Azure.Services.AppAuthentication.SqlAppAuthenticationProvider, Microsoft.Azure.Services.AppAuthentication" />
      </providers>
    </SqlAuthenticationProviders>
    ```    

- Busque la cadena de conexión denominada `MyDbConnection` y reemplace su valor `connectionString` por `"server=tcp:<server-name>.database.windows.net;database=<db-name>;UID=AnyString;Authentication=Active Directory Interactive"`. Reemplace _\<nombre-del-servidor>_ y _\<nombre-de-la-base-de-datos>_ por el nombre del servidor y el de la base de datos.

Escriba `Ctrl+F5` para ejecutar la aplicación de nuevo. Ahora, la misma aplicación CRUD del explorador se conectará a Azure SQL Database directamente con la autenticación de Azure AD. Esta configuración permite ejecutar migraciones de base de datos. Más adelante, al implementar los cambios en App Service, la misma configuración funciona con la identidad administrada de la aplicación.

## <a name="use-managed-identity-connectivity"></a>Uso de la conectividad de la identidad administrada

A continuación, configure la aplicación de App Service para conectarse a SQL Database con una identidad administrada asignada por el sistema.

### <a name="enable-managed-identity-on-app"></a>Habilitación de la identidad administrada en la aplicación

Para habilitar una identidad administrada para la aplicación de Azure, use el comando [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) de Cloud Shell. En el siguiente comando, reemplace *\<nombre-de-la-aplicación>* .

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app-name>
```

Este es un ejemplo de la salida:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

### <a name="add-managed-identity-to-an-azure-ad-group"></a>Incorporación de una identidad administrada a un grupo de Azure AD

Para conceder este acceso a la identidad a SQL Database, deberá agregarlo a un [grupo de Azure AD](../active-directory/fundamentals/active-directory-manage-groups.md). En Cloud Shell, agréguelo a un nuevo grupo denominado _myAzureSQLDBAccessGroup_, como se muestra en el siguiente script:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group myResourceGroup --name <app-name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

Si desea ver la salida JSON completa para cada comando, use los parámetros `--query objectId --output tsv`.

### <a name="grant-permissions-to-azure-ad-group"></a>Concesión de permisos a un grupo de Azure AD

En Cloud Shell, inicie sesión en SQL Database mediante el comando SQLCMD. Reemplace _\<nombre-del-servidor>_ por su nombre de servidor de SQL Database, _\<nombre-de-la-base-de-datos>_ por el nombre de la base de datos que utiliza la aplicación y _\<nombre-de-usuario-de-aad>_ y _\<contraseña-de-aad>_ por las credenciales de usuario de Azure AD.

```azurecli-interactive
sqlcmd -S <server-name>.database.windows.net -d <db-name> -U <aad-user-name> -P "<aad-password>" -G -l 30
```

En el símbolo del sistema de SQL para la base de datos que desee, ejecute los siguientes comandos para agregar el grupo de Azure AD y conceder los permisos que la aplicación necesita. Por ejemplo, 

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_ddladmin ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

Escriba `EXIT` para volver al símbolo del sistema de Cloud Shell.

### <a name="modify-connection-string"></a>Modificación de la cadena de conexión

Recuerde que los mismos cambios que haya realizado en `Web.config` funcionan con la identidad administrada, por lo que lo único necesario es eliminar la cadena de conexión existente de la aplicación que creó Visual Studio al implementar la aplicación inicialmente. En los siguientes comandos, reemplace *\<nombre-de-la-aplicación>* por el nombre de la aplicación.

```azurecli-interactive
az webapp config connection-string delete --resource-group myResourceGroup --name <app-name> --setting-names MyDbConnection
```

## <a name="publish-your-changes"></a>Publicación de los cambios

Ahora lo único que queda es publicar los cambios en Azure.

En el **Explorador de soluciones**, haga clic con el botón derecho en su proyecto **DotNetAppSqlDb** y seleccione **Publicar**.

![Publicar desde el Explorador de soluciones](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

En la página de publicación, haga clic en **Publicar**. Cuando la página web nueva muestra su lista de tareas pendientes, la aplicación se conecta a la base de datos mediante la identidad administrada.

![Aplicación de Azure después de Migraciones de Code First](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Ahora ya puede editar la lista de tareas pendientes como antes.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Pasos siguientes

¿Qué ha aprendido?

> [!div class="checklist"]
> * Habilitar identidades administradas
> * Conceder a SQL Database acceso a la identidad administrada
> * Configurar Entity Framework para utilizar la autenticación de Azure AD con SQL Database
> * Conectarse a SQL Database desde Visual Studio mediante la autenticación de Azure AD

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a una aplicación web.

> [!div class="nextstepaction"]
> [Asignación de un nombre DNS personalizado existente a Azure App Service](app-service-web-tutorial-custom-domain.md)

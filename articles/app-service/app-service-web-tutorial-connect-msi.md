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
ms.date: 11/30/2018
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6af6eb0dd6473b9fe947f7cc4939da4e0cbc77cb
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53718542"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>Tutorial: Protección de la conexión con Azure SQL Database desde App Service mediante una identidad administrada

[App Service](overview.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático en Azure. También proporciona una [identidad administrada](overview-managed-identity.md) para la aplicación, la cual constituye una solución inmediata para proteger el acceso a [Azure SQL Database](/azure/sql-database/) y a otros servicios de Azure. Las identidades administradas de App Service hacen que su aplicación sea más segura mediante la eliminación de los secretos de aplicación como, por ejemplo, las credenciales en las cadenas de conexión. En este tutorial, agregará una identidad administrada a la aplicación web ASP.NET de ejemplo que se creó en [Tutorial: Compilación de una aplicación ASP.NET en Azure con SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md). Cuando haya terminado, la aplicación de ejemplo se conectará a SQL Database de forma segura sin necesidad de nombres de usuario ni contraseñas.

> [!NOTE]
> Este escenario lo admite actualmente .NET Framework 4.6 y versiones superiores, pero no por [.NET Core 2.1](https://www.microsoft.com/net/learn/get-started/windows). [.NET core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2) admite este escenario, pero todavía no se ha incluido en las imágenes predeterminadas de App Service. 
>

Aprenderá a:

> [!div class="checklist"]
> * Habilitar identidades administradas
> * Conceder a SQL Database acceso a la identidad administrada
> * Configurar el código de la aplicación para autenticarse con SQL Database mediante la autenticación de Azure Active Directory
> * Conceder los privilegios mínimos a la identidad administrada en SQL Database

> [!NOTE]
>La autenticación de Azure Active Directory es _diferente_ de la [autenticación integrada de Windows](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) en Active Directory (AD DS) local. AD DS y Azure Active Directory utilizan protocolos de autenticación completamente diferentes. Para más información, consulte [Documentación de Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Este artículo continúa donde lo dejó en [Tutorial: Compilación de una aplicación ASP.NET en Azure con SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md). Si no lo ha hecho ya, siga este tutorial primero. Como alternativa, puede adaptar los pasos para su propia aplicación ASP.NET con SQL Database.

<!-- ![app running in App Service](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png) -->

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-managed-identities"></a>Habilitar identidades administradas

Para habilitar una identidad administrada para la aplicación de Azure, use el comando [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) de Cloud Shell. En el siguiente comando, reemplace *\<app name>*.

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app name>
```

Este es un ejemplo de la salida después de crear la identidad en Azure Active Directory:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

Usará el valor de `principalId` en el paso siguiente. Si desea ver los detalles de la nueva identidad en Azure Active Directory, ejecute el siguiente comando opcional con el valor de `principalId`:

```azurecli-interactive
az ad sp show --id <principalid>
```

## <a name="grant-database-access-to-identity"></a>Concesión del acceso de la base de datos a la identidad

A continuación, conceda a la base de datos acceso a la identidad administrada de la aplicación mediante el comando [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin_create) en Cloud Shell. En el siguiente comando, reemplace *\<server_name>* y <principalid_from_last_step>. Escriba un nombre de administrador para *\<admin_user>*.

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server_name> --display-name <admin_user> --object-id <principalid_from_last_step>
```

La identidad administrada ya tiene acceso al servidor de Azure SQL Database.

## <a name="modify-connection-string"></a>Modificación de la cadena de conexión

Modifique la conexión que estableció anteriormente para la aplicación mediante el comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) de Cloud Shell. En el siguiente comando, reemplace *\<app name>* por el nombre de la aplicación, y reemplace *\<server_name>* y *\<db_name>* por los nombres correspondientes de su instancia de SQL Database.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='Server=tcp:<server_name>.database.windows.net,1433;Database=<db_name>;' --connection-string-type SQLAzure
```

## <a name="modify-aspnet-code"></a>Modificación del código ASP.NET

En Visual Studio, abra la consola del administrador de paquetes y agregue el paquete NuGet [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication):

```PowerShell
Install-Package Microsoft.Azure.Services.AppAuthentication -Version 1.1.0-preview
```

Abra _Models\MyDatabaseContext.cs_ y agregue las siguientes instrucciones `using` en la parte superior del archivo:

```csharp
using System.Data.SqlClient;
using Microsoft.Azure.Services.AppAuthentication;
using System.Web.Configuration;
```

En la clase `MyDatabaseContext`, agregue el constructor siguiente:

```csharp
public MyDatabaseContext(SqlConnection conn) : base(conn, true)
{
    conn.ConnectionString = WebConfigurationManager.ConnectionStrings["MyDbConnection"].ConnectionString;
    // DataSource != LocalDB means app is running in Azure with the SQLDB connection string you configured
    if(conn.DataSource != "(localdb)\\MSSQLLocalDB")
        conn.AccessToken = (new AzureServiceTokenProvider()).GetAccessTokenAsync("https://database.windows.net/").Result;

    Database.SetInitializer<MyDatabaseContext>(null);
}
```

Este constructor configura un objeto SqlConnection personalizado para que use un token de acceso para Azure SQL Database desde App Service. Con el token de acceso, la aplicación App Service se autentica con Azure SQL Database con su identidad administrada. Para más información, consulte [Obtención de tokens para recursos de Azure](overview-managed-identity.md#obtaining-tokens-for-azure-resources). La instrucción `if` le permite seguir probando la aplicación localmente con LocalDB.

> [!NOTE]
> `SqlConnection.AccessToken` se admite actualmente solo en .NET Framework 4.6 y versiones posteriores, así como en [.NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2), pero no en [.NET Core 2.1](https://www.microsoft.com/net/learn/get-started/windows).
>

Para usar este nuevo constructor, abra `Controllers\TodosController.cs` y busque la línea `private MyDatabaseContext db = new MyDatabaseContext();`. El código existente usa el controlador `MyDatabaseContext` predeterminado para crear una base de datos mediante la cadena de conexión estándar que tenía el nombre de usuario y la contraseña en texto no cifrado antes de que [lo cambiara](#modify-connection-string).

Reemplace esta línea al completo por el siguiente código:

```csharp
private MyDatabaseContext db = new MyDatabaseContext(new System.Data.SqlClient.SqlConnection());
```

### <a name="publish-your-changes"></a>Publicación de los cambios

Ahora lo único que queda es publicar los cambios en Azure.

En el **Explorador de soluciones**, haga clic con el botón derecho en su proyecto **DotNetAppSqlDb** y seleccione **Publicar**.

![Publicar desde el Explorador de soluciones](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

En la página de publicación, haga clic en **Publicar**. Cuando la página web nueva muestra su lista de tareas pendientes, la aplicación se conecta a la base de datos mediante la identidad administrada.

![Aplicación de Azure después de Migraciones de Code First](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Ahora ya puede editar la lista de tareas pendientes como antes.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="grant-minimal-privileges-to-identity"></a>Concesión de privilegios mínimos a la identidad

Durante los pasos anteriores, probablemente ha observado que la identidad administrada está conectada a SQL Server como administrador de Azure AD. Para conceder privilegios mínimos a la identidad administrada, debe iniciar sesión en el servidor de Azure SQL Database como administrador de Azure AD y, a continuación, agregar un grupo de Azure Active Directory que contenga la identidad administrada. 

### <a name="add-managed-identity-to-an-azure-active-directory-group"></a>Incorporación de una identidad administrada a un grupo de Azure Active Directory

En Cloud Shell, agregue la identidad administrada de la aplicación en un nuevo grupo de Azure Active Directory denominado _myAzureSQLDBAccessGroup_, como se muestra en el siguiente script:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group <group_name> --name <app_name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

Si desea ver la salida JSON completa para cada comando, use los parámetros `--query objectId --output tsv`.

### <a name="reconfigure-azure-ad-administrator"></a>Reconfiguración del administrador de Azure AD

Anteriormente, asignó la identidad administrada como administrador de Azure AD para la instancia de SQL Database. No se puede usar esta identidad para el inicio de sesión interactivo (para agregar usuarios de la base de datos), por lo que deberá usar su usuario real de Azure AD. Para agregar su usuario de Azure AD, siga los pasos descritos en [Aprovisionamiento de un administrador de Azure Active Directory para el servidor de Azure SQL Database](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server). 

> [!IMPORTANT]
> Una vez agregado, no quite este administrador de Azure AD para SQL Database a menos que desee deshabilitar el acceso de Azure AD a SQL Database completamente (de todas las cuentas de Azure AD).
> 

### <a name="grant-permissions-to-azure-active-directory-group"></a>Concesión de permisos al grupo de Azure Active Directory

En Cloud Shell, inicie sesión en SQL Database mediante el comando SQLCMD. Reemplace _\<server\_name>_ por su nombre de servidor de SQL Database, _\<db\_name>_ por el nombre de la base de datos que utiliza la aplicación y _\<AADuser\_name>_ y _\<AADpassword>_ por las credenciales de usuario de Azure AD.

```azurecli-interactive
sqlcmd -S <server_name>.database.windows.net -d <db_name> -U <AADuser_name> -P "<AADpassword>" -G -l 30
```

En el símbolo del sistema de SQL para la base de datos que desee, ejecute los comandos siguientes para agregar el grupo de Azure Active Directory que ha creado antes y conceder los permisos que necesita la aplicación. Por ejemplo, 

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_ddladmin ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

Escriba `EXIT` para volver al símbolo del sistema de Cloud Shell. 

## <a name="next-steps"></a>Pasos siguientes

¿Qué ha aprendido?

> [!div class="checklist"]
> * Habilitar identidades administradas
> * Conceder a SQL Database acceso a la identidad administrada
> * Configurar el código de la aplicación para autenticarse con SQL Database mediante la autenticación de Azure Active Directory
> * Conceder los privilegios mínimos a la identidad administrada en SQL Database

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a una aplicación web.

> [!div class="nextstepaction"]
> [Asignación de un nombre DNS personalizado existente a Azure App Service](app-service-web-tutorial-custom-domain.md)

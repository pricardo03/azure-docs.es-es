---
title: ActiveDirectoryInteractive se conecta a SQL | Microsoft Docs
description: Ejemplo de código de C#, con explicaciones, para conectarse a Azure SQL Database mediante el modo SqlAuthenticationMethod.ActiveDirectoryInteractive.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: active directory
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: MirekS
ms.reviewer: GeneMi
ms.date: 03/12/2019
manager: craigg
ms.openlocfilehash: 7f151c45920a51b3c9a25f36de34b2c1800e4e71
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58520690"
---
# <a name="connect-to-azure-sql-database-with-azure-multi-factor-authentication"></a>Conectarse a Azure SQL Database con Azure Multi-factor Authentication

Este artículo se proporciona un C# programa que se conecta a Azure SQL Database. El programa utiliza la autenticación de modo interactivo, que es compatible con [Azure Multi-factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).

Para obtener más información sobre la compatibilidad con la autenticación multifactor para las herramientas de SQL, consulte [soporte técnico de Azure Active Directory en SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory).

## <a name="multi-factor-authentication-for-azure-sql-database"></a>Autenticación multifactor para Azure SQL Database

A partir de .NET Framework versión 4.7.2, la enumeración [ `SqlAuthenticationMethod` ](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlauthenticationmethod) tiene un nuevo valor: `ActiveDirectoryInteractive`. En un cliente C# programa, el valor de enumeración indica al sistema que use el modo interactivo de Azure Active Directory (Azure AD) que admite la autenticación multifactor para conectarse a una base de datos SQL de Azure. El usuario que ejecuta el programa ve los cuadros de diálogos siguientes:

* Un cuadro de diálogo que muestra el nombre de usuario de Azure AD y pide la contraseña del usuario.

   Si el dominio del usuario está federado con Azure AD, no aparece este cuadro de diálogo, ya que no se necesita ninguna contraseña.

   Si la directiva de Azure AD impone la autenticación multifactor en el usuario, se muestran los dos cuadros de diálogo.

* La primera vez que un usuario se desplaza a través de la autenticación multifactor, el sistema muestra un cuadro de diálogo que solicita un número de teléfono móvil enviar mensajes de texto a. Cada mensaje proporciona el *código de verificación* que el usuario debe especificar en el cuadro de diálogo siguiente.

* Un cuadro de diálogo que solicita un código de verificación autenticación multifactor, que el sistema ha enviado a un teléfono móvil.

Para obtener información acerca de cómo configurar Azure AD para requerir autenticación multifactor, consulte [Introducción a Azure Multi-factor Authentication en la nube](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-cloud).

Para capturas de pantalla de estos cuadros de diálogo, vea [configurar la autenticación multifactor para SQL Server Management Studio y Azure AD](sql-database-ssms-mfa-authentication-configure.md).

> [!TIP]
> Puede buscar la API de .NET Framework con la [página de la herramienta Explorador de API de .NET](https://docs.microsoft.com/dotnet/api/).
>
> También puede buscar directamente con el [opcional? término =&lt;Buscar valor&gt; parámetro](https://docs.microsoft.com/dotnet/api/?term=SqlAuthenticationMethod).

## <a name="configure-your-c-application-in-the-azure-portal"></a>Configuración de la aplicación de C# en Azure Portal

Antes de empezar, debe haber un [servidor de Azure SQL Database](sql-database-get-started-portal.md) disponible.

### <a name="register-your-app-and-set-permissions"></a>Registro de la aplicación y establecimiento de permisos

Para usar la autenticación de Azure AD, el programa de C# se debe registrar como una aplicación de Azure AD. Para registrar una aplicación, debe ser administrador de Azure AD o usuario asignado al rol *Desarrollador de aplicaciones* de Azure AD. Para obtener más información sobre la asignación de roles, consulte [asignar roles de administrador y que no sea de administrador a los usuarios con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

Completar un registro de aplicación genera y muestra un **Id. de aplicación**. El programa debe incluir este identificador para realizar la conexión.

Para registrar y establecer los permisos necesarios para la aplicación:

1. En Azure Portal, seleccione **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro de aplicaciones**.

    ![Registro de aplicación](media/active-directory-interactive-connect-azure-sql-db/image1.png)

    Una vez creado el registro de aplicación, el **Id. de aplicación** se genera y muestra el valor.

    ![Identificador de aplicación mostrado](media/active-directory-interactive-connect-azure-sql-db/image2.png)

2. Seleccione **aplicación registrada** > **configuración** > **permisos necesarios** > **agregar**.

    ![Configuración de permisos para la aplicación registrada](media/active-directory-interactive-connect-azure-sql-db/sshot-registered-app-settings-required-permissions-add-api-access-c32.png)

3. Seleccione **permisos necesarios** > **agregar** > **seleccionar una API** > **Azure SQL Database**.

    ![Adición de acceso a la API de Azure SQL Database](media/active-directory-interactive-connect-azure-sql-db/sshot-registered-app-settings-required-permissions-add-api-access-Azure-sql-db-d11.png)

4. Seleccione **acceso a la API** > **seleccionar permisos** > **permisos delegados**.

    ![Delegación de permisos en la API de Azure SQL Database](media/active-directory-interactive-connect-azure-sql-db/sshot-add-api-access-azure-sql-db-delegated-permissions-checkbox-e14.png)

### <a name="set-an-azure-ad-admin-for-your-sql-database-server"></a>Configuración de un administrador de Azure AD para un servidor de SQL Database

Para su C# programar para ejecutarse, un administrador del servidor SQL de Azure debe asignar un administrador de Azure AD para el servidor de base de datos SQL. 

En el **SQL Server** página, seleccione **Administrador de Active Directory** > **establecer administrador**.

Para obtener más información sobre los usuarios y administradores de Azure AD para Azure SQL Database, vea las capturas de pantalla [configurar y administrar la autenticación de Azure Active Directory con SQL Database](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server).

### <a name="add-a-non-admin-user-to-a-specific-database-optional"></a>Incorporación de un usuario no administrador en una base de datos específica (opcional)

El administrador de Azure AD de un servidor de SQL Database puede ejecutar el programa de ejemplo de C#. Un usuario de Azure AD puede ejecutar el programa si están en la base de datos. Un administrador de SQL de Azure AD o un usuario de Azure AD que ya existe en la base de datos y tiene el permiso`ALTER ANY USER` en la base de datos pueden agregar un usuario.

Puede agregar un usuario a la base de datos con el comando [`Create User`](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) de SQL. Un ejemplo es `CREATE USER [<username>] FROM EXTERNAL PROVIDER`.

Para más información, consulte [Uso de la autenticación de Azure Active Directory para autenticación con SQL Database, Instancia administrada o SQL Data Warehouse](sql-database-aad-authentication.md).

## <a name="new-authentication-enum-value"></a>Nuevo valor de enumeración de autenticación

El ejemplo de C# se basa en el espacio de nombres [`System.Data.SqlClient`](https://docs.microsoft.com/dotnet/api/system.data.sqlclient). De especial interés para la autenticación multifactor es la enumeración `SqlAuthenticationMethod`, que tiene los siguientes valores:

- `SqlAuthenticationMethod.ActiveDirectoryInteractive`

   Use este valor con un nombre de usuario de Azure AD para implementar la autenticación multifactor. Este valor es el enfoque del presente artículo. Produce una experiencia interactiva al mostrar cuadros de diálogo para la contraseña de usuario y, a continuación, para la validación de autenticación multifactor si Multi-factor Authentication se impone en el usuario. Este valor está disponible a partir de .NET Framework 4.7.2.

- `SqlAuthenticationMethod.ActiveDirectoryIntegrated`

  Use este valor para una cuenta *federada*. En una cuenta federada, el nombre del usuario se conoce en el dominio de Windows. Este método de autenticación no es compatible con autenticación multifactor.

- `SqlAuthenticationMethod.ActiveDirectoryPassword`

  Use este valor para la autenticación que requiere un nombre de usuario y contraseña de Azure AD. Azure SQL Database realiza la autenticación. Este método no es compatible con autenticación multifactor.

## <a name="set-c-parameter-values-from-the-azure-portal"></a>Establecimiento de los valores de parámetros de C# desde Azure Portal

Para que el programa de C# se ejecute correctamente, deberá asignar los valores correspondientes a los campos estáticos. Aquí se muestran campos con valores de ejemplo. También se muestran son las ubicaciones de portal de Azure donde puede obtener los valores necesarios.

| Nombre del campo estático | Valor de ejemplo | Dónde está en Azure Portal |
| :---------------- | :------------ | :-------------------- |
| Az_SQLDB_svrName | "my-sqldb-svr.database.windows.net" | **Servidores SQL** > **Filtrar por nombre** |
| AzureAD_UserID | "auser\@abc.onmicrosoft.com" | **Azure Active Directory** > **Usuario** > **Nuevo usuario invitado** |
| Initial_DatabaseName | "myDatabase" | **Servidores SQL** > **Bases de datos SQL** |
| ClientApplicationID | "a94f9c62-97fe-4d19-b06d-111111111111" | **Azure Active Directory** > **Registros de aplicaciones** > **Buscar por nombre** > **Identificador de aplicación** |
| RedirectUri | new Uri("https://mywebserver.com/") | **Azure Active Directory** > **registros de aplicaciones** > **buscar por nombre** > *[Your-App-registration]*  >  **Configuración** > **RedirectURIs**<br /><br />En este artículo, cualquier valor válido es correcto para RedirectUri, porque no se utiliza aquí. |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="verify-with-sql-server-management-studio"></a>Compruebe con SQL Server Management Studio

Antes de ejecutar el C# programa, es una buena idea comprobar que el programa de instalación y las configuraciones son correctas en SQL Server Management Studio (SSMS). De ese modo, cualquier error del programa de C# se puede restringir al código fuente.

### <a name="verify-sql-database-firewall-ip-addresses"></a>Verificación de las direcciones IP del firewall de SQL Database

Ejecute SSMS desde el mismo equipo, en el mismo edificio, donde planea ejecutar el programa de C#. Para esta prueba, cualquier **autenticación** modo es correcto. Si hay cualquier indicación de que el firewall del servidor de bases de datos no acepta su dirección IP, consulte [Reglas de firewall de nivel de servidor y de nivel de base de datos de Azure SQL Database](sql-database-firewall-configure.md) para obtener ayuda.

### <a name="verify-azure-active-directory-multi-factor-authentication"></a>Comprobar la autenticación multifactor de Azure Active Directory

Vuelva a ejecutar SSMS, en esta ocasión con la opción **Autenticación** establecida en **Active Directory - Universal compatible con MFA**. Esta opción requiere la versión 17.5 o posterior de SSMS.

Para obtener más información, consulte [configurar Multi-factor Authentication para SSMS y Azure AD](sql-database-ssms-mfa-authentication-configure.md).

> [!NOTE]
> Si es un usuario invitado en la base de datos, también deberá proporcionar el nombre de dominio de Azure AD para la base de datos: Seleccione **opciones** > **identificador de inquilino ni el nombre de dominio de AD**. Para buscar el nombre de dominio de Azure Portal, seleccione **Azure Active Directory** > **Nombres de dominio personalizados**. En el programa de C# de ejemplo, no es necesario brindar un nombre de dominio.

## <a name="c-code-example"></a>Ejemplo de código de C#

El programa de C# de ejemplo se basa en el ensamblado DLL de [*Microsoft.IdentityModel.Clients.ActiveDirectory*](https://docs.microsoft.com/dotnet/api/microsoft.identitymodel.clients.activedirectory).

Para instalar este paquete, en Visual Studio seleccione **Proyecto** > **Administrar paquetes NuGet**. Busque e instale **Microsoft.IdentityModel.Clients.ActiveDirectory**.

Este es un ejemplo de C# código fuente.

```csharp

using System;

// Reference to Azure AD authentication assembly
using Microsoft.IdentityModel.Clients.ActiveDirectory;

using DA = System.Data;
using SC = System.Data.SqlClient;
using AD = Microsoft.IdentityModel.Clients.ActiveDirectory;
using TX = System.Text;
using TT = System.Threading.Tasks;

namespace ADInteractive5
{
    class Program
    {
        // ASSIGN YOUR VALUES TO THESE STATIC FIELDS !!
        static public string Az_SQLDB_svrName = "<Your SQL DB server>";
        static public string AzureAD_UserID = "<Your User ID>";
        static public string Initial_DatabaseName = "<Your Database>";
        // Some scenarios do not need values for the following two fields:
        static public readonly string ClientApplicationID = "<Your App ID>";
        static public readonly Uri RedirectUri = new Uri("<Your URI>");

        public static void Main(string[] args)
        {
            var provider = new ActiveDirectoryAuthProvider();

            SC.SqlAuthenticationProvider.SetProvider(
                SC.SqlAuthenticationMethod.ActiveDirectoryInteractive,
                //SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated,  // Alternatives.
                //SC.SqlAuthenticationMethod.ActiveDirectoryPassword,
                provider);

            Program.Connection();
        }

        public static void Connection()
        {
            SC.SqlConnectionStringBuilder builder = new SC.SqlConnectionStringBuilder();

            // Program._  static values that you set earlier.
            builder["Data Source"] = Program.Az_SQLDB_svrName;
            builder.UserID = Program.AzureAD_UserID;
            builder["Initial Catalog"] = Program.Initial_DatabaseName;

            // This "Password" is not used with .ActiveDirectoryInteractive.
            //builder["Password"] = "<YOUR PASSWORD HERE>";

            builder["Connect Timeout"] = 15;
            builder["TrustServerCertificate"] = true;
            builder.Pooling = false;

            // Assigned enum value must match the enum given to .SetProvider().
            builder.Authentication = SC.SqlAuthenticationMethod.ActiveDirectoryInteractive;
            SC.SqlConnection sqlConnection = new SC.SqlConnection(builder.ConnectionString);

            SC.SqlCommand cmd = new SC.SqlCommand(
                "SELECT '******** MY QUERY RAN SUCCESSFULLY!! ********';",
                sqlConnection);

            try
            {
                sqlConnection.Open();
                if (sqlConnection.State == DA.ConnectionState.Open)
                {
                    var rdr = cmd.ExecuteReader();
                    var msg = new TX.StringBuilder();
                    while (rdr.Read())
                    {
                        msg.AppendLine(rdr.GetString(0));
                    }
                    Console.WriteLine(msg.ToString());
                    Console.WriteLine(":Success");
                }
                else
                {
                    Console.WriteLine(":Failed");
                }
                sqlConnection.Close();
            }
            catch (Exception ex)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Connection failed with the following exception...");
                Console.WriteLine(ex.ToString());
                Console.ResetColor();
            }
        }
    } // EOClass Program.

    /// <summary>
    /// SqlAuthenticationProvider - Is a public class that defines 3 different Azure AD
    /// authentication methods.  The methods are supported in the new .NET 4.7.2.
    ///  . 
    /// 1. Interactive,  2. Integrated,  3. Password
    ///  . 
    /// All 3 authentication methods are based on the Azure
    /// Active Directory Authentication Library (ADAL) managed library.
    /// </summary>
    public class ActiveDirectoryAuthProvider : SC.SqlAuthenticationProvider
    {
        // Program._ more static values that you set!
        private readonly string _clientId = Program.ClientApplicationID;
        private readonly Uri _redirectUri = Program.RedirectUri;

        public override async TT.Task<SC.SqlAuthenticationToken>
            AcquireTokenAsync(SC.SqlAuthenticationParameters parameters)
        {
            AD.AuthenticationContext authContext =
                new AD.AuthenticationContext(parameters.Authority);
            authContext.CorrelationId = parameters.ConnectionId;
            AD.AuthenticationResult result;

            switch (parameters.AuthenticationMethod)
            {
                case SC.SqlAuthenticationMethod.ActiveDirectoryInteractive:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,  // "https://database.windows.net/"
                        _clientId,
                        _redirectUri,
                        new AD.PlatformParameters(AD.PromptBehavior.Auto),
                        new AD.UserIdentifier(
                            parameters.UserId,
                            AD.UserIdentifierType.RequiredDisplayableId));
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_1 == '.ActiveDirectoryIntegrated'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserCredential());
                    break;

                case SC.SqlAuthenticationMethod.ActiveDirectoryPassword:
                    Console.WriteLine("In method 'AcquireTokenAsync', case_2 == '.ActiveDirectoryPassword'.");

                    result = await authContext.AcquireTokenAsync(
                        parameters.Resource,
                        _clientId,
                        new AD.UserPasswordCredential(
                            parameters.UserId,
                            parameters.Password));
                    break;

                default: throw new InvalidOperationException();
            }
            return new SC.SqlAuthenticationToken(result.AccessToken, result.ExpiresOn);
        }

        public override bool IsSupported(SC.SqlAuthenticationMethod authenticationMethod)
        {
            return authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryIntegrated
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryInteractive
                || authenticationMethod == SC.SqlAuthenticationMethod.ActiveDirectoryPassword;
        }
    } // EOClass ActiveDirectoryAuthProvider.
} // EONamespace.  End of entire program source code.

```

&nbsp;

Este es un ejemplo de la C# resultados de pruebas.

```
[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>> ADInteractive5.exe
In method 'AcquireTokenAsync', case_0 == '.ActiveDirectoryInteractive'.
******** MY QUERY RAN SUCCESSFULLY!! ********

:Success

[C:\Test\VSProj\ADInteractive5\ADInteractive5\bin\Debug\]
>>
```

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de PowerShell de Azure Resource Manager es compatible aún con Azure SQL Database, pero todo el desarrollo futuro es para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo de Az y en los módulos AzureRm son esencialmente idénticos.

- [Get-AzSqlServerActiveDirectoryAdministrator](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserveractivedirectoryadministrator)
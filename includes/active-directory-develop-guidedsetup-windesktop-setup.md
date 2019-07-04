---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: ae6d590cdada24638ec2d24c83609b8e6addfaf0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186490"
---
## <a name="set-up-your-project"></a>Configurar su proyecto

En esta sección, creará un nuevo proyecto para demostrar cómo integrar una aplicación .NET de escritorio de Windows (XAML) con el *inicio de sesión en Microsoft* para que la aplicación pueda consultar las API web que requieren un token.

La aplicación que se crea con esta guía muestra un botón que se utiliza para llamar a un gráfico, un área para mostrar los resultados en la pantalla y un botón de cierre de sesión.

> [!NOTE]
> ¿Prefiere descargar este proyecto de Visual Studio de ejemplo en su lugar? [Descargue un proyecto](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/msal3x.zip) y vaya al [paso de configuración](#register-your-application) para configurar el código de ejemplo antes de ejecutarlo.
>

Para crear la aplicación, lleva a cabo los siguientes pasos:

1. Abra Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto**.
2. En **Plantillas**, seleccione **Visual C#** .
3. Seleccione **Aplicación de WPF (.NET Framework)** , en función de la versión de Visual Studio que use.

## <a name="add-msal-to-your-project"></a>Adición de MSAL al proyecto

1. En Visual Studio, seleccione **Herramientas** > **Administrador de paquetes NuGet**> **Consola del administrador de paquetes**.
2. En la ventana de la Consola del administrador de paquetes, pegue el siguiente comando de Azure PowerShell:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

    > [!NOTE] 
    > Este comando instala la biblioteca de autenticación de Microsoft. MSAL controla la adquisición, el almacenamiento en caché y la actualización de los tokens de usuario que se usan para acceder a las API protegidas por Azure Active Directory v2.0
    >

## <a name="add-the-code-to-initialize-msal"></a>Adición del código para inicializar MSAL

En este caso, creará una clase para administrar la interacción con MSAL, como la administración de tokens.

1. Abra el archivo *App.xaml.cs* y, a continuación, agregue la referencia de MSAL a la clase:

    ```csharp
    using Microsoft.Identity.Client;
    ```
   <!-- Workaround for Docs conversion bug -->

2. Actualice la clase app a la siguiente:

    ```csharp
    public partial class App : Application
    {
        static App()
        {
            _clientApp = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(AzureCloudInstance.AzurePublic, Tenant)
                .Build();
        }

        // Below are the clientId (Application Id) of your app registration and the tenant information. 
        // You have to replace:
        // - the content of ClientID with the Application Id for your app registration
        // - the content of Tenant by the information about the accounts allowed to sign-in in your application:
        //   - For Work or School account in your org, use your tenant ID, or domain
        //   - for any Work or School accounts, use `organizations`
        //   - for any Work or School accounts, or Microsoft personal account, use `common`
        //   - for Microsoft Personal account, use consumers
        private static string ClientId = "0b8b0665-bc13-4fdc-bd72-e0227b9fc011";

        private static string Tenant = "common";

        private static IPublicClientApplication _clientApp ;

        public static IPublicClientApplication PublicClientApp { get { return _clientApp; } }
    }
    ```

## <a name="create-the-application-ui"></a>Creación de la interfaz de usuario de la aplicación

En esta sección se muestra cómo puede una aplicación consultar un servidor back-end protegido como Microsoft Graph. 

Debe crearse un archivo *MainWindow.xaml* automáticamente como parte de la plantilla de proyecto. Abra este archivo y, luego, reemplace el nodo *\<Grid>* de la aplicación por el código siguiente:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```

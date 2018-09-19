---
title: Creación de la primera función en Azure mediante Visual Studio Code
description: Cree y publique en Azure una función simple desencadenada por HTTP mediante la extensión de Azure Functions de Visual Studio Code.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure functions, funciones, procesamiento de eventos, proceso, arquitectura sin servidor
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 09/07/2018
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 614ac06e92efd906950dd7fac85095cc4acc4a53
ms.sourcegitcommit: 465ae78cc22eeafb5dfafe4da4b8b2138daf5082
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44325233"
---
# <a name="create-your-first-function-using-visual-studio-code"></a>Creación de la primera función mediante Visual Studio Code

Azure Functions permite ejecutar el código en un entorno [sin servidor](https://azure.microsoft.com/overview/serverless-computing/) sin necesidad de crear una máquina virtual o publicar una aplicación web.

En este artículo aprenderá a usar la [extensión de Azure Functions para Visual Studio Code] para crear y probar una función "hola mundo" en su equipo local mediante Microsoft Visual Studio Code. Posteriormente, puede publicar el código de la función en Azure desde Visual Studio Code.

![Código Azure Functions en un proyecto de Visual Studio](./media/functions-create-first-function-vs-code/functions-vscode-intro.png)

La extensión admite actualmente funciones de C#, JavaScript y Java. Los pasos descritos en este artículo pueden variar según el lenguaje que elija para el proyecto de Azure Functions. Actualmente, la extensión se encuentra en versión preliminar. Para más información, consulte la página de la [extensión de Azure Functions para Visual Studio Code].

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido:

* Instale [Visual Studio Code](https://code.visualstudio.com/) en una de las [plataformas compatibles](https://code.visualstudio.com/docs/supporting/requirements#_platforms). Este artículo se ha desarrollado y probado en un dispositivo que ejecutaba macOS (High Sierra).

* Instale la versión 2.x de [Azure Functions Core Tools](functions-run-local.md#v2), que se encuentra aún en versión preliminar.

* Instale los requisitos específicos para el lenguaje elegido:

    | Idioma | Extensión |
    | -------- | --------- |
    | **C#** | [C# para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)<br/>[Herramientas de la CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)*   |
    | **Java** | [Depurador para Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)<br/>[JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)<br/>[Maven 3+](https://maven.apache.org/) |
    | **JavaScript** | [Node 8.0+](https://nodejs.org/)  |

    \* Core Tools también lo requiere.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-the-azure-function-extension"></a>Instalación de la extensión de Azure Functions

La extensión de Azure Functions se usa para crear, probar e implementar funciones en Azure.

1. En Visual Studio Code, abra **Extensiones** y busque `azure functions`, o [abra este vínculo en Visual Studio Code](vscode:extension/ms-azuretools.vscode-azurefunctions).

1. Seleccione **Instalar** para instalar la extensión en Visual Studio Code. 

    ![Instalación de la extensión de Azure Functions](./media/functions-create-first-function-vs-code/vscode-install-extension.png)

1. Reinicie Visual Studio Code y seleccione el icono de Azure en la barra Actividad. Debería ver un área de Azure Functions en la barra lateral.

    ![Área de Azure Functions en la barra lateral](./media/functions-create-first-function-vs-code/azure-functions-window-vscode.png)

## <a name="create-an-azure-functions-project"></a>Creación de un proyecto de Azure Functions

La plantilla del proyecto de Azure Functions de Visual Studio Code crea un proyecto que se puede publicar en una aplicación de función en Azure. Una aplicación de función permite agrupar funciones como una unidad lógica para la administración, la implementación y el uso compartido de recursos.

1. En Visual Studio Code, seleccione el logotipo de Azure para que se muestre el área **Azure: Functions** y, a continuación, seleccione el icono Crear nuevo proyecto.

    ![Creación de un proyecto de aplicación de función](./media/functions-create-first-function-vs-code/create-function-app-project.png)

1. Elija una ubicación para el área de trabajo del proyecto y elija **Seleccionar**.

    > [!NOTE]
    > Este artículo se ha diseñado para completarse fuera de un área de trabajo. Por tanto, no seleccione una carpeta de proyecto que forme parte de un área de trabajo.

1. Seleccione el lenguaje para el proyecto de la aplicación de función. En este artículo, se usa JavaScript.
    ![Elegir el lenguaje del proyecto](./media/functions-create-first-function-vs-code/create-function-app-project-language.png)

1. Cuando se le solicite, elija **Agregar al área de trabajo**.

Visual Studio Code crea el proyecto de la aplicación de función en una nueva área de trabajo. Este proyecto contiene los archivos de configuración [host.json](functions-host-json.md) y [local.settings.json](functions-run-local.md#local-settings-file), además de los archivos de proyecto específicos del lenguaje. También obtendrá un nuevo repositorio de Git en la carpeta del proyecto.

## <a name="create-an-http-triggered-function"></a>Crear una función desencadenada mediante HTTP

1. En **Azure: Functions**, elija el icono Crear función.

    ![Creación de una función](./media/functions-create-first-function-vs-code/create-function.png)

1. Seleccione la carpeta con el proyecto de la aplicación de función y seleccione la plantilla de función del **desencadenador HTTP**.

    ![Elegir la plantilla del desencadenador HTTP](./media/functions-create-first-function-vs-code/create-function-choose-template.png)

1. Escriba `HTTPTrigger` como nombre de la función y presione ENTRAR y, a continuación, seleccione la opción de autenticación **Anónimo**.

    ![Elegir autenticación anónima](./media/functions-create-first-function-vs-code/create-function-anonymous-auth.png)

    Se crea una función en el lenguaje elegido mediante la plantilla para una función desencadenada por HTTP.

    ![Plantilla de función desencadenada por HTTP en Visual Studio Code](./media/functions-create-first-function-vs-code/new-function-full.png)

Puede agregar enlaces de entrada y salida a la función modificando el archivo function.json. Para más información, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).

Ahora que ha creado un proyecto de función y una función desencadenada por HTTP, puede probarla en el equipo local.

## <a name="test-the-function-locally"></a>Prueba local de la función

Azure Functions Core Tools le permite ejecutar un proyecto de Azure Functions en el equipo de desarrollo local. Se le solicita que instale estas herramientas la primera vez que inicie una función de Visual Studio Code.  

1. Para probar la función, establezca un punto de interrupción en el código de la función y presione F5 para iniciar el proyecto de la aplicación de función. La salida de Core Tools aparece en el panel **Terminal**.

1. En el panel **Terminal**, copie el punto de conexión de la dirección URL de la función desencadenada por HTTP.

    ![Salida local de Azure](./media/functions-create-first-function-vs-code/functions-vscode-f5.png)

1. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<yourname>` a esta dirección URL y ejecute la solicitud. La ejecución se pone en pausa cuando se alcanza el punto de interrupción.

    ![Función que alcanza el punto de interrupción en Visual Studio Code](./media/functions-create-first-function-vs-code/function-debug-vscode-js.png)

1. Si continúa la ejecución, aquí se muestra la respuesta en el explorador a la solicitud GET:

    ![Respuesta de localhost de la función en el explorador](./media/functions-create-first-function-vs-code/functions-test-local-browser.png)

1. Para detener la depuración, presione Mayús + F1.

Después de comprobar que la función se ejecuta correctamente en el equipo local es el momento de publicar el proyecto en Azure.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Para poder publicar la aplicación, debe iniciar sesión en Azure.

1. En el área **Azure: Functions**, elija **Iniciar sesión en Azure...**. En caso de no tener ninguna cuenta, seleccione **Crear una cuenta de Azure gratis**.

    ![Respuesta de localhost de la función en el explorador](./media/functions-create-first-function-vs-code/functions-sign-into-azure.png)

1. Cuando se le solicite, seleccione **Copiar y abrir**, o copie el código que aparece y abra <https://aka.ms/devicelogin> en el explorador.

1. Pegue el código copiado en la página **Inicio de sesión del dispositivo**, compruebe el inicio de sesión de Visual Studio Code y luego seleccione **Continuar**.  

1. Termine el inicio de sesión mediante las credenciales de su cuenta de Azure. Cuando haya iniciado sesión correctamente, puede cerrar el explorador.

## <a name="publish-the-project-to-azure"></a>Publicar el proyecto en Azure

Visual Studio Code le permite publicar el proyecto de Functions directamente en Azure. En el proceso, puede crear una aplicación de función y los recursos relacionados en su suscripción de Azure. La aplicación de función proporciona un contexto de ejecución para sus funciones. El proyecto se empaqueta e implementa en la nueva aplicación de función en su suscripción de Azure. 

En este artículo se da por supuesto que va a crear una nueva aplicación de función. La publicación en una aplicación de función existente sobrescribe el contenido de esa aplicación en Azure.

1. En el área **Azure: Functions**, seleccione el icono de implementación en la aplicación de función.

    ![Configuración de Function App](./media/functions-create-first-function-vs-code/function-app-publish-project.png)

1. Elija la carpeta del proyecto que, en este caso, es el área de trabajo actual.

1. Si tiene más de una suscripción, elija la que desee para hospedar su aplicación de función y, a continuación, elija **+ Create New Function App** (+ Crear nueva aplicación de función).

1. Escriba un nombre único global que identifique la aplicación de función y presione ENTRAR. Los siguientes son caracteres válidos para un nombre de aplicación de función: `a-z`, `0-9` y `-`.

1. Elija **+ Crear un nuevo grupo de recursos**, escriba un nombre para el grupo de recursos, como `myResourceGroup` y presione ENTRAR. También puede usar un grupo de recursos existente.

1. Elija **+ Crear nueva cuenta de almacenamiento**, escriba un nombre único global para la nueva cuenta de almacenamiento que va a usar la aplicación de función y presione ENTRAR. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente.

1. Elija una ubicación en una [región](https://azure.microsoft.com/regions/) cercana a usted o a otros servicios a los que accedan las funciones.

    La creación de la aplicación de función se inicia después de elegir la ubicación. Una vez que se haya creado la aplicación de función se mostrará una notificación y se aplicará el paquete de implementación.

1. Seleccione **View Output** (Ver salida) en las notificaciones para ver la creación y los resultados de la implementación, incluidos los recursos de Azure que ha creado.

    ![Salida de la creación de aplicación de función](./media/functions-create-first-function-vs-code/function-create-notifications.png)

1. Anote la dirección URL de la nueva aplicación de función en Azure. Utilícela para probar la función después de que el proyecto se publique en Azure.

    ![Salida de la creación de aplicación de función](./media/functions-create-first-function-vs-code/function-create-output.png)

1. En el área **Azure: Functions** puede ver la nueva aplicación de función que aparece en su suscripción. Al expandir este nodo, puede ver las funciones de la aplicación de función, así como la configuración de la aplicación y los servidores proxy de la función.

    ![Configuración de Function App](./media/functions-create-first-function-vs-code/function-app-project-settings.png)

    En el nodo de la aplicación de función, escriba Ctrl y haga clic (clic con el botón derecho) para realizar diversas tareas de administración y configuración de la aplicación de función en Azure. También puede decidir ver la aplicación de función en Azure Portal.

## <a name="test-your-function-in-azure"></a>Prueba de una función en Azure

1. Copie la dirección URL del desencadenador HTTP del panel **Salida**. Como antes, asegúrese de que agrega la cadena de consulta `?name=<yourname>` al final de esta dirección URL y ejecute la solicitud.

    La dirección URL que llama a la función desencadenada por HTTP debería tener el formato siguiente:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

1. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. A continuación se muestra la respuesta en el explorador para la solicitud GET remota devuelta por la función: 

    ![Respuesta de la función en el explorador](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="next-steps"></a>Pasos siguientes

Ha usado Visual Studio Code para crear una aplicación de función con una función simple desencadenada por HTTP. Para más información sobre el desarrollo de funciones en un lenguaje específico, consulte las guías de referencia de lenguaje para [JavaScript](functions-reference-node.md), [.NET](functions-dotnet-class-library.md) o [Java](functions-reference-java.md).

A continuación, puede que desee más información sobre la prueba y depuración local desde el terminal o el símbolo del sistema mediante Azure Functions Core Tools.

> [!div class="nextstepaction"]
> [Codificación y pruebas locales](functions-run-local.md)

[Azure Functions Core Tools]: functions-run-local.md
[Extensión de Azure Functions para Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions

---
title: Definición de la configuración de Azure Function App | Microsoft Docs
description: Obtenga información sobre cómo definir la configuración de Azure Function App.
services: ''
documentationcenter: .net
author: ggailey777
manager: jeconnoc
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: azure-functions
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: glenga
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 92ca09040836dfc55a9d709b12a0ee01192d6bac
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65957395"
---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a>Administración de una Function App en Azure Portal 

En Azure Functions, una Function App ofrece el contexto de ejecución de funciones individuales. Los comportamientos de Function App se aplican a todas las funciones hospedadas en una Function App determinada. En este tema se describe cómo configurar y administrar Function App en Azure Portal.

Para comenzar, vaya a [Azure Portal](https://portal.azure.com) e inicie sesión en la cuenta de Azure. En la barra de búsqueda en la parte superior del portal, escriba el nombre de la Function App y selecciónela en la lista. Después de seleccionar la Function App, vea la siguiente página:

![Información general sobre Function App en Azure Portal](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

Puede navegar a todo lo que necesita para administrar Function App desde la página de información general, en particular la **[Configuración de la aplicación](#settings)** y las **[Características de la plataforma](#platform-features)** .

## <a name="settings"></a>Configuración de la aplicación

La pestaña **Configuración de la aplicación** mantiene la configuración de la aplicación que Function App utiliza.

![Configuración de Function App en Azure Portal.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

Esta configuración se almacena cifrada y debe seleccionar **Mostrar valores** para ver los valores en el portal.

Para agregar una configuración, seleccione **Nueva configuración de la aplicación** y agregue el nuevo par clave-valor.

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

Al desarrollar una instancia de Function App localmente, estos valores se mantienen en el archivo del proyecto local.settings.json.

## <a name="platform-features"></a>Características de la plataforma

![Pestaña Características de la plataforma de Function App](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Las Function App se ejecutan en la plataforma de Azure App Service, donde también se realiza su mantenimiento. Por tanto, Function App tiene acceso a la mayoría de las características de la plataforma de hospedaje web principal de Azure. En la pestaña **Características de la plataforma** puede acceder a muchas características de la plataforma de App Service que puede usar en las Function App. 

> [!NOTE]
> No todas las características de App Service están disponibles cuando una Function App se ejecuta con el plan de hospedaje de consumo.

El resto de este tema se centra en las siguientes características de App Service en Azure Portal que resultan útiles para Functions:

+ [Editor de App Service](#editor)
+ [Console](#console)
+ [Herramientas avanzadas (Kudu)](#kudu)
+ [Opciones de implementación](#deployment)
+ [CORS](#cors)
+ [Autenticación](#auth)
+ [Definición de la API](#swagger)

Para más información sobre cómo trabajar con la configuración de App Service, vea [Configuración de Azure App Service](../app-service/configure-common.md).

### <a name="editor"></a>Editor de App Service

| | |
|-|-|
| ![Editor de App Service de Function App](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | El Editor de App Service es un editor en portal avanzado que puede usar para modificar archivos de configuración JSON y archivos de código similares. Al seleccionar esta opción se inicia una pestaña de explorador independiente con un editor básico. Esto le permite realizar la integración con el repositorio Git, ejecutar y depurar código y modificar la configuración de Function App. Este editor proporciona un entorno de desarrollo mejorado para las funciones en comparación con la hoja de Function App predeterminada.    |

![Editor de App Service](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="console"></a>Consola

| | |
|-|-|
| ![Consola de Function App en Azure Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | La consola del portal es una herramienta ideal para desarrolladores si prefiere interactuar con Function App desde la línea de comandos. Los comandos comunes incluyen creación de archivos y directorios y navegación por los mismos, así como la ejecución de archivos y scripts por lotes. |

![Consola de Function App](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Herramientas avanzadas (Kudu)

| | |
|-|-|
| ![Kudu de Function App en Azure Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | Las herramientas avanzadas para App Service (también conocidas como Kudu) proporcionan acceso a las características administrativas avanzadas de la Function App. Con Kudu, puede administrar la información del sistema, la configuración de las aplicaciones, las variables del entorno, las extensiones del sitio, los encabezados HTTP y las variables del servidor. También puede iniciar **Kudu** si examina el punto de conexión de SCM de la Function App, como `https://<myfunctionapp>.scm.azurewebsites.net/`. |

![Configurar Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Opciones de implementación

| | |
|-|-|
| ![Opciones de implementación de Function App en Azure Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | Functions permite desarrollar código de funciones en la máquina local. Después, puede cargar el proyecto de Function App local en Azure. Además de la carga por FTP tradicional, Functions permite implementar Function App con soluciones populares de integración continua, como GitHub, Azure DevOps, Dropbox, Bitbucket y otras. Para más información, vea [Implementación continua para Azure Functions](functions-continuous-deployment.md). Para realizar cargas manuales con FTP o Git local, también debe [configurar las credenciales de implementación](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![CORS de Function App en Azure Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | Para evitar la ejecución de código malintencionado en los servicios, App Service bloquea las llamadas a las Function App desde orígenes externos. Functions admite el uso compartido de recursos entre orígenes (CORS) para que pueda definir una "lista blanca" de orígenes permitidos desde los que las funciones puedan aceptar solicitudes remotas.  |

![Configuración de CORS de Function App](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Autenticación

| | |
|-|-|
| ![Autenticación de Function App en Azure Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | Si las funciones usan un desencadenador HTTP, puede requerir que las llamadas se autentiquen primero. App Service admite la autenticación de Azure Active Directory y el inicio de sesión en proveedores locales, como Facebook, Microsoft y Twitter. Para más información sobre cómo configurar los proveedores de autenticación específicos, consulte [Autenticación y autorización en Azure App Service](../app-service/overview-authentication-authorization.md). |

![Configuración de la autenticación de una Function App](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>Definición de la API

| | |
|-|-|
| ![Definición de API de Function App mediante Swagger en Azure Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | Functions admite Swagger para permitir que los clientes consuman las funciones desencadenadas por HTTP de forma más fácil. Para más información sobre cómo crear definiciones de API con Swagger, visite [Hospedaje de una API RESTful con CORS en Azure App Service](../app-service/app-service-web-tutorial-rest-api.md). También puede usar Functions Proxies para definir una única superficie de API para varias funciones. Para más información, vea [Trabajo con Azure Functions Proxies](functions-proxies.md). |

![Configuración de la API de Function App](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Pasos siguientes

+ [Configuración de Azure App Service](../app-service/configure-common.md)
+ [Implementación continua para Azure Functions](functions-continuous-deployment.md)




---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 5ce0f18c1ec7a0fcb6465ab20e774976552687f1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133411"
---
## <a name="set-up-your-web-server-or-project"></a>Configuración de un proyecto o servidor web

> ¿Prefiere descargar este proyecto de ejemplo en su lugar? Realice cualquiera de las siguientes acciones:
> 
> - Para ejecutar el proyecto con un servidor web local, como Node.js, [descargue los archivos del proyecto](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip).
>
> - (Opcional) Parar ejecutar el proyecto con el servidor IIS [descargue el proyecto de Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).
>
> Luego, para configurar el ejemplo de código antes de ejecutarlo, vaya al [paso de configuración](#register-your-application).

## <a name="prerequisites"></a>Requisitos previos

* Para ejecutar este tutorial, necesita un servidor web local, como [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core) o la integración de IIS Express con [Visual Studio 2017](https://www.visualstudio.com/downloads/).

* Si usa Node.js para ejecutar el proyecto, instale un entorno de desarrollo integrado (IDE), como [Visual Studio Code](https://code.visualstudio.com/download), para editar los archivos del proyecto.

* Las instrucciones de esta guía se basan tanto en Node.js como en Visual Studio 2017, pero puede usar cualquier otro entorno de desarrollo o servidor web.

## <a name="create-your-project"></a>Creación del proyecto

> ### <a name="option-1-nodejs-or-other-web-servers"></a>Opción 1: Node.js u otros servidores web
> Asegúrese de haber instalado [Node.js](https://nodejs.org/en/download/) y haga lo siguiente:
> - Cree una carpeta para hospedar la aplicación.
>
> ### <a name="option-2-visual-studio"></a>Opción 2: Visual Studio
> Si usa Visual Studio y crea un proyecto, haga lo siguiente:
> 1. Abra Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto**.
> 1. En **Visual C#\Web**, seleccione **Aplicación web ASP.NET (.NET Framework)** .
> 1. Escriba un nombre para la aplicación y seleccione **Aceptar**.
> 1. En **Nueva aplicación web ASP.NET**, seleccione **Vacía**.

## <a name="create-the-spa-ui"></a>Creación de la interfaz de usuario de SPA
1. Cree un archivo *index.html* para JavaScript SPA. Si usa Visual Studio, seleccione el proyecto (la carpeta raíz del proyecto), haga clic con el botón derecho y seleccione **Agregar** > **Nuevo elemento** > **Página HTML** y asígnele el nombre *index.html*.

1. En el archivo *index.html*, agregue el código siguiente:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.js"></script>
   </head>
   <body>
       <h2>Welcome to MSAL.js Quickstart</h2><br/>
       <h4 id="WelcomeMessage"></h4>
       <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
       <pre id="json"></pre>
       <script>
           //JS code
       </script>
   </body>
   </html>
   ```

   > [!TIP]
   > Puede reemplazar la versión de MSAL.js del script anterior con la versión más reciente en la sección de [versiones de MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).

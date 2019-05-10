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
ms.openlocfilehash: 805b773795e7c8a96fe1b94abecd860475a4c7a1
ms.sourcegitcommit: abeefca6cd5ca01c3e0b281832212aceff08bf3e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "64993333"
---
## <a name="setting-up-your-web-server-or-project"></a>Configuración del servidor web o proyecto

> ¿Prefiere descargar este proyecto de ejemplo en su lugar?
> - [Descargue los archivos del proyecto](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) para ejecutarlos con un servidor web local (como Node)
>
> o
> - (Opcional) [Descargue el proyecto de Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip) para ejecutarlo con un servidor IIS.
>
> A continuación, vaya al [paso de configuración](#register-your-application) para configurar el código de ejemplo antes de ejecutarlo.

## <a name="prerequisites"></a>Requisitos previos
Se necesita un servidor web local como [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core) o la integración de IIS Express con [Visual Studio 2017](https://www.visualstudio.com/downloads/) para ejecutar este tutorial.

Si usa Node.js para ejecutar el proyecto, instale un IDE como [Visual Studio Code](https://code.visualstudio.com/download) para editar los archivos de proyecto.

Las instrucciones de esta guía están basadas tanto en Node.js como en Visual Studio 2017, pero puede usar cualquier otro entorno de desarrollo o servidor web.

## <a name="create-your-project"></a>Creación del proyecto

> ### <a name="option-1-node-other-web-servers"></a>Opción 1: Nodo u otros servidores web
> Asegúrese de que ha instalado [Node.js](https://nodejs.org/en/download/) y, a continuación, siga el paso mostrado a continuación:
> - Cree una carpeta para hospedar la aplicación.

<p><!-- -->

> ### <a name="option-2-visual-studio"></a>Opción 2: Visual Studio
> Si usa Visual Studio y crea un nuevo proyecto, siga estos pasos para crear una nueva solución de Visual Studio:
> 1.    En Visual Studio:  **Archivo > Nuevo > Proyecto**
> 2.    En **Visual C#\Web**, seleccione **Aplicación web ASP.NET (.NET Framework)**.
> 3.    Escriba el nombre de la aplicación y seleccione **Aceptar**.
> 4.    En **Nueva aplicación web ASP.NET**, seleccione **Vacía**.

## <a name="create-your-single-page-applications-ui"></a>Cree la interfaz de usuario de la aplicación de una sola página
1. Cree un archivo `index.html` para JavaScript SPA. Si usa Visual Studio, seleccione el proyecto (carpeta raíz del proyecto), haga clic con el botón derecho y seleccione: **Agregar > Nuevo elemento > Página HTML** y asígnele el nombre index.html.

2. Agregue el siguiente código a su página:
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

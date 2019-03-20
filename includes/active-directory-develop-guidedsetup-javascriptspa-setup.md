---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 703a8ee989a4655768e09440ed250ade6c000050
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58114156"
---
## <a name="setting-up-your-web-server-or-project"></a>Configuración del servidor web o proyecto

> ¿Prefiere descargar este proyecto de ejemplo en su lugar?
> - [Descargue los archivos del proyecto](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) para un servidor web local, como Node
>
> o
> - [Descargue el proyecto de Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)
>
> A continuación, vaya al [paso de configuración](#register-your-application) para configurar el código de ejemplo antes de ejecutarlo.

## <a name="prerequisites"></a>Requisitos previos
Se necesita un servidor web local como [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core) o la integración de IIS Express con [Visual Studio 2017](https://www.visualstudio.com/downloads/) para ejecutar este tutorial.

Las instrucciones de esta guía están basadas tanto en Node.js como en Visual Studio 2017, pero puede usar cualquier otro entorno de desarrollo o servidor web.

## <a name="create-your-project"></a>Creación del proyecto

> ### <a name="option-1-node-other-web-servers"></a>Opción 1: Nodo / otros servidores web
> Asegúrese de que ha instalado [Node.js](https://nodejs.org/en/download/) y, a continuación, siga el paso mostrado a continuación:
> - Cree una carpeta para hospedar la aplicación.

<p/><!-- -->

> ### <a name="option-2-visual-studio"></a>Opción 2: Visual Studio
> Si usa Visual Studio y crea un nuevo proyecto, siga estos pasos para crear una nueva solución de Visual Studio:
> 1.    En Visual Studio:  **Archivo > Nuevo > proyecto**
> 2.    En **Visual C#\Web**, seleccione **Aplicación web ASP.NET (.NET Framework)**.
> 3.    Escriba el nombre de la aplicación y seleccione **Aceptar**.
> 4.    En **Nueva aplicación web ASP.NET**, seleccione **Vacía**.


## <a name="create-your-single-page-applications-ui"></a>Cree la interfaz de usuario de la aplicación de una sola página
1. Cree un archivo `index.html` para JavaScript SPA. Si utiliza Visual Studio, seleccione el proyecto (carpeta raíz del proyecto), haga clic y seleccione: **Agregar > nuevo elemento > página HTML** y asígnele el nombre index.html.

2. Agregue el siguiente código a su página:
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.js"></script>
       <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
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

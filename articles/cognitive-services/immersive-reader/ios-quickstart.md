---
title: 'Inicio rápido: Creación de una aplicación iOS que inicia el Lector inmersivo (Swift)'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, creará una aplicación iOS desde cero y agregará la funcionalidad de Lector inmersivo.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 08/01/2019
ms.author: metan
ms.openlocfilehash: 64fd7508244f2123cc10ee96ec4f805050aedfaa
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69899529"
---
# <a name="quickstart-create-an-ios-app-that-launches-the-immersive-reader-swift"></a>Inicio rápido: Creación de una aplicación iOS que inicia el Lector inmersivo (Swift)

El [Lector inmersivo](https://www.onenote.com/learningtools) es una herramienta diseñada de forma inclusiva que implementa técnicas demostradas para mejorar la comprensión lectora.

En este inicio rápido, creará una aplicación iOS desde cero e integrará el Lector inmersivo mediante su SDK. Se puede encontrar un completo ejemplo funcional [aquí](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/samples/quickstart-swift).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* [Xcode](https://apps.apple.com/us/app/xcode/id497799835?mt=12)
* Un recurso de Lector inmersivo configurado para la autenticación de Azure Active Directory (Azure AD). Siga [estas instrucciones](./azure-active-directory-authentication.md) para realizar la configuración. Al configurar las propiedades del proyecto de ejemplo, necesitará algunos de los valores creados aquí. Guarde la salida de la sesión en un archivo de texto para futuras referencias.

## <a name="create-an-xcode-project"></a>Creación de un proyecto de Xcode

Cree un proyecto nuevo en Xcode.

![Nuevo proyecto](./media/ios/xcode-create-project.png)

Elija **Single View App** (Aplicación de vista única).

![Nueva aplicación de vista única](./media/ios/xcode-single-view-app.png)

## <a name="get-the-sdk-cocoapod"></a>Obtención del SDK CocoaPod
La manera más sencilla de usar el SDK de Lector inmersivo es a través de CocoaPods. Para instalarlo a través de CocoaPods:
1. [Instale CocoaPods](http://guides.cocoapods.org/using/getting-started.html): siga la guía de introducción para instalar CocoaPods.
2. Ejecute `pod init` en el directorio raíz del proyecto de Xcode para crear un podfile.
3.  Agregue `pod 'immersive-reader-sdk', :path => 'https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/immersive-reader-sdk'` para incorporar CocoaPod al podfile. El podfile debe tener un aspecto similar al siguiente, reemplazando el nombre del destino por quickstart-swift:
 ```ruby
  platform :ios, '9.0'

  target 'quickstart-swift' do
  use_frameworks!
  # Pods for quickstart-swift
  pod 'immersive-reader-sdk', :path => 'https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS/immersive-reader-sdk'
  end
```
4. En el terminal, en el directorio del proyecto de Xcode, ejecute el comando `pod install` para instalar el pod del SDK de Lector inmersivo.
5. Agregue `import immersive_reader_sdk` a todos los archivos que necesitan hacer referencia al SDK.
6. Asegúrese de abrir el proyecto abriendo el archivo `.xcworkspace` y no el archivo `.xcodeproj`.

## <a name="acquire-an-azure-ad-authentication-token"></a>Adquisición de un token de autenticación de Azure AD

Para esta parte, necesitará algunos valores del paso anterior de requisitos previos de configuración de la autenticación de Azure AD. Vuelva a consultar el archivo de texto que ha guardado de esa sesión.

````text
TenantId     => Azure subscription TenantId
ClientId     => Azure AD ApplicationId
ClientSecret => Azure AD Application Service Principal password
Subdomain    => Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')
````

En la carpeta principal del proyecto, que contiene el archivo ViewController.swift, cree un archivo de clase Swift denominado Constants.swift. Reemplace la clase por el código siguiente y agregue sus propios valores cuando proceda. Conserve este archivo como un archivo local que solo existe en el equipo y asegúrese de no confirmar este archivo en el control de código fuente, ya que contiene secretos que no deben hacerse públicos. Se recomienda no mantener secretos en la aplicación. En su lugar, se recomienda usar un servicio back-end para obtener el token; así los secretos se pueden mantener fuera de la aplicación y del dispositivo. El punto de conexión de la API de back-end debe estar protegido mediante algún tipo de autenticación (por ejemplo, [OAuth](https://oauth.net/2/)) para evitar que usuarios no autorizados obtengan tokens y los utilicen en el servicio Lector inmersivo y su facturación; este trabajo queda fuera del ámbito de este inicio rápido.

[!code-swift[Constants](~/ImmersiveReaderSdk/iOS/samples/quickstart-swift/quickstart-swift/Constants.swift)]

## <a name="set-up-the-app-to-run-without-a-storyboard"></a>Configuración de la aplicación para que se ejecute sin un guion gráfico

Abra AppDelegate.swift y reemplace el archivo por el siguiente código.

[!code-swift[AppDelegate](~/ImmersiveReaderSdk/iOS/samples/quickstart-swift/quickstart-swift/AppDelegate.swift)]

## <a name="create-the-launch-view-controller-and-add-sample-content"></a>Creación del lanzamiento del controlador de vista e incorporación de contenido de ejemplo

Cambie el nombre de ViewController.swift a LaunchViewController.swift y reemplace el archivo por el código siguiente.

[!code-swift[LaunchViewController](~/ImmersiveReaderSdk/iOS/samples/quickstart-swift/quickstart-swift/LaunchViewController.swift)]

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

Seleccione como destino un simulador o un dispositivo para establecer el esquema de archivo en Xcode.
![Esquema de archivo](./media/ios/xcode-archive-scheme.png)<br/>
![Selección del destino](./media/ios/xcode-select-target.png)

En Xcode, presione Ctrl+R o haga clic en el botón de reproducción para ejecutar el proyecto; la aplicación debe iniciarse en el simulador o el dispositivo especificados.

En la aplicación, debe ver lo siguiente:

![Aplicación de ejemplo](./media/ios/sample-app-ipad.png)

Al hacer clic en el botón "Lector inmersivo", verá que se inicia dicha herramienta con el contenido de la aplicación.

![Lector inmersivo](./media/ios/immersive-reader-ipad.png)

## <a name="next-steps"></a>Pasos siguientes

* Explore el [SDK del Lector inmersivo para iOS](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS) y la [Referencia del SDK del Lector inmersivo para iOS](./ios-reference.md).

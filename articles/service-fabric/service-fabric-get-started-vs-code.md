---
title: Introducción a Azure Service Fabric con VS Code | Microsoft Docs
description: En este artículo se ofrece una introducción a la creación de aplicaciones de Service Fabric mediante Visual Studio Code.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: f977a48338f784562ec84355aabb212e5a3dade4
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59543675"
---
# <a name="service-fabric-for-visual-studio-code"></a>Service Fabric para Visual Studio Code

La [extensión Service Fabric Reliable Services para VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) proporciona las herramientas necesarias para crear, compilar y depurar aplicaciones de Service Fabric en los sistemas operativos Windows, Linux y macOS.

En este artículo se proporciona información general sobre los requisitos y la configuración de la extensión, así como el uso de los distintos comandos que la extensión proporciona. 

> [!IMPORTANT]
> Las aplicaciones de Java para Service Fabric se pueden desarrollar en máquinas Windows, pero solo se pueden implementar en clústeres Linux de Azure. En Windows no se admite la depuración de aplicaciones de Java.

## <a name="prerequisites"></a>Requisitos previos

Los siguientes requisitos previos deben instalarse en todos los entornos.

* [Visual Studio Code](https://code.visualstudio.com/)
* [Node.js](https://nodejs.org/)
* [Git](https://git-scm.com/)
* [SDK de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)
* Generadores Yeoman: instale los generadores adecuados para su aplicación

   ```sh
   npm install -g yo
   npm install -g generator-azuresfjava
   npm install -g generator-azuresfcsharp
   npm install -g generator-azuresfcontainer
   npm install -g generator-azuresfguest
   ```

Los siguientes requisitos previos deben instalarse para programar con Java:

* [SDK de Java](https://aka.ms/azure-jdks) (versión 1.8)
* [Gradle](https://gradle.org/install/)
* [Depurador para la extensión VS Code de Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug) Necesario para depurar los servicios de Java. La depuración de los servicios de Java solo se admite en Linux. Para instalar, puede hacer clic en el icono Extensiones en la **Barra de actividades** de VS Code y buscar la extensión, o puede usar VS Code Marketplace.

Los siguientes requisitos previos deben instalarse para programar con .NET Core o C#.

* [.NET Core](https://www.microsoft.com/net/learn/get-started) (versión 2.0.0 o posteriores)
* [Extensión VS Code de C# para Visual Studio Code (con tecnología OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Necesario para depurar los servicios de C#. Para instalar, puede hacer clic en el icono Extensiones en la **Barra de actividades** de VS Code y buscar la extensión, o puede usar VS Code Marketplace.

## <a name="setup"></a>Configuración

1. Abra VS Code.
2. Haga clic en el icono Extensiones en la **Barra de actividad** en el lateral izquierdo de VS Code. Busque "Service Fabric". Haga clic en **Instalar** para instalar la extensión de Service Fabric Reliable Services.

## <a name="commands"></a>Comandos:
La extensión de Service Fabric Reliable Services para VS Code ofrece muchos comandos para ayudar a los desarrolladores a crear e implementar proyectos de Service Fabric. Para llamar a los comandos desde la **paleta de comandos**, presione `(Ctrl + Shift + p)`, escriba el nombre de comando en la barra de entrada y seleccione el comando deseado en la lista de mensajes. 

* Service Fabric: Crear la aplicación 
* Service Fabric: Publicar aplicación 
* Service Fabric: Implementación de aplicación 
* Service Fabric: Quitar la aplicación  
* Service Fabric: Crear aplicación 
* Service Fabric: Limpiar la aplicación 

### <a name="service-fabric-create-application"></a>Service Fabric: Crear la aplicación

El **de Service Fabric: Crear aplicación** comando crea una nueva aplicación de Service Fabric en el área de trabajo actual. Dependiendo de qué generadores Yeoman estén instalados en el equipo de desarrollo, puede crear varios tipos de aplicación de Service Fabric, incluidos proyectos de Java, C#, Container y Guest. 

1.  Seleccione el **de Service Fabric: Agregar servicio** comando
2.  Seleccione el tipo de la nueva aplicación de Service Fabric. 
3.  Escriba el nombre de la aplicación que desea crear.
3.  Seleccione el tipo de servicio que desea agregar a la aplicación de Service Fabric. 
4.  Siga las indicaciones para asignar un nombre al servicio. 
5.  La nueva aplicación de Service Fabric aparece en el área de trabajo.
6.  Abra la nueva carpeta de aplicación para que se convierta en la carpeta raíz del área de trabajo. Puede continuar la ejecución de los comandos desde aquí.

### <a name="service-fabric-add-service"></a>Service Fabric: Agregar servicio
El **de Service Fabric: Agregar servicio** comando agrega un nuevo servicio a una aplicación de Service Fabric existente. La aplicación a la que se agregará el servicio debe ser el directorio raíz del área de trabajo. 

1.  Seleccione el **de Service Fabric: Agregar servicio** comando.
2.  Seleccione el tipo de la aplicación de Service Fabric actual. 
3.  Seleccione el tipo de servicio que desea agregar a la aplicación de Service Fabric. 
4.  Siga las indicaciones para asignar un nombre al servicio. 
5.  El nuevo servicio aparece en el directorio del proyecto. 

### <a name="service-fabric-publish-application"></a>Service Fabric: Publicar aplicación
El **de Service Fabric: Publicar aplicación** comando implementa la aplicación de Service Fabric en un clúster remoto. El clúster de destino puede ser un clúster seguro o no seguro. Si no se establecen los parámetros en Cloud.json, la aplicación se implementa en el clúster local.

1.  La primera vez que se compila la aplicación, se genera un archivo Cloud.json en el directorio del proyecto.
2.  Escriba los valores para el clúster al que le gustaría conectarse en el archivo Cloud.json.
3.  Seleccione el **de Service Fabric: Publicar aplicación** comando.
4.  Consulte el clúster de destino con Service Fabric Explorer para confirmar que la aplicación se ha instalado. 

### <a name="service-fabric-deploy-application-localhost"></a>Service Fabric: Implementación de aplicación (Localhost)
El **de Service Fabric: Implementar aplicación** comando implementa la aplicación de Service Fabric en el clúster local. Asegúrese de que el clúster local se está ejecutando antes de usar el comando. 

1. Seleccione el **de Service Fabric: Implementar aplicación** comando
2. Ver el clúster local mediante Service Fabric Explorer (http:\//localhost:19080 / Explorer) para confirmar que se ha instalado la aplicación. Esto puede tardar algún tiempo, tenga paciencia.
3. También puede usar **Service Fabric: Publicar aplicación** comando sin parámetros establecidos en el archivo Cloud.json para implementar en un clúster local.

> [!NOTE]
> No se permite implementar aplicaciones de Java en el clúster local en equipos Windows.

### <a name="service-fabric-remove-application"></a>Service Fabric: Quitar la aplicación
El **de Service Fabric: Quitar aplicación** comando quita una aplicación de Service Fabric del clúster que se implementó previamente para usar la extensión de VS Code. 

1.  Seleccione el **de Service Fabric: Quitar aplicación** comando.
2.  Consulte el clúster con Service Fabric Explorer para confirmar que la aplicación se ha quitado. Esto puede tardar algún tiempo, tenga paciencia.

### <a name="service-fabric-build-application"></a>Service Fabric: Crear aplicación
El **de Service Fabric: Generar aplicación** comando puede crear entornos Java o C# aplicaciones de Service Fabric. 

1.  Asegúrese de que está en la carpeta raíz de la aplicación antes de ejecutar este comando. El comando identifica el tipo de aplicación (C# o Java) y la compila como corresponda.
2.  Seleccione el **de Service Fabric: Build Application**.
3.  La salida del proceso de compilación se envía al terminal integrado.

### <a name="service-fabric-clean-application"></a>Service Fabric: Limpiar la aplicación
El **de Service Fabric: Limpiar aplicación** comando elimina todos los archivos jar y las bibliotecas nativas generadas por la compilación. Solo es válido para aplicaciones de Java. 

1.  Asegúrese de que está en la carpeta raíz de la aplicación antes de ejecutar este comando. 
2.  Seleccione el **de Service Fabric: Limpiar aplicación** comando.
3.  La salida del proceso de limpieza se envía al terminal integrado.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre el [desarrollo y la depuración de aplicaciones de C# para Service Fabric con VS Code](./service-fabric-develop-csharp-applications-with-vs-code.md).
* Obtenga más información sobre el [desarrollo y la depuración de aplicaciones de Java para Service Fabric con VS Code](./service-fabric-develop-java-applications-with-vs-code.md).

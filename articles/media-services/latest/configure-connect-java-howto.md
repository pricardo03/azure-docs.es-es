---
title: 'Conectarse a la API de Azure Media Services v3: Java'
description: Obtenga información sobre cómo conectarse a la API de Media Services v3 con Java.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2019
ms.author: juliako
ms.openlocfilehash: b7ee54c852ce3332415b69ca6105b472dab0ab8a
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66480256"
---
# <a name="connect-to-media-services-v3-api---java"></a>Conectarse a la API de Media Services v3 - Java

Este artículo muestra cómo conectar con el SDK de Java de Azure Media Services v3 con el inicio de sesión entidad de servicio en el método.

En este artículo, se utiliza el código de Visual Studio para desarrollar la aplicación de ejemplo.

## <a name="prerequisites"></a>Requisitos previos

- Siga [escritura Java con Visual Studio Code](https://code.visualstudio.com/docs/java/java-tutorial) para instalar:

   - JDK
   - Apache Maven
   - Paquete de extensiones de Java
- Asegúrese de establecer `JAVA_HOME` y `PATH` variables de entorno.
- [Cree una cuenta de Media Services](create-account-cli-how-to.md). Asegúrese de recordar el nombre del grupo de recursos y el nombre de cuenta de Media Services.
- Siga los pasos descritos en la [acceder a las API](access-api-cli-how-to.md) tema. Registre el identificador de suscripción, identificador de la aplicación (Id. de cliente), la clave de autenticación (secreto) y el identificador del inquilino que necesita en un paso posterior.

Consulte también:

- [Java en Visual Studio Code](https://code.visualstudio.com/docs/languages/java)
- [Administración de proyectos de Java en VS Code](https://code.visualstudio.com/docs/java/java-project)

## <a name="create-a-maven-project"></a>Creación de un proyecto de Maven

Abra una herramienta de línea de comandos y `cd` en un directorio donde desea crear el proyecto.
    
```
mvn archetype:generate -DgroupId=com.azure.ams -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Al ejecutar el comando, el `pom.xml`, `App.java`, y otros archivos se crean. 

## <a name="add-dependencies"></a>Adición de dependencias

1. En Visual Studio Code, abra la carpeta donde está el proyecto
1. Busque y abra el `pom.xml`
1. Agregue las dependencias necesarias

    ```xml
   <dependency>
     <groupId>com.microsoft.azure.mediaservices.v2018_07_01</groupId>
     <artifactId>azure-mgmt-media</artifactId>
     <version>1.0.0-beta-3</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.rest</groupId>
     <artifactId>client-runtime</artifactId>
     <version>1.6.6</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-client-authentication</artifactId>
     <version>1.6.6</version>
   </dependency>
    ```

## <a name="connect-to-the-java-client"></a>Conectar con el cliente de Java

1. Abra el `App.java` archivo `src\main\java\com\azure\ams` y asegúrese de que el paquete se incluye en la parte superior:

    ```java
    package com.azure.ams;
    ```
1. En la instrucción del paquete, agregue estas instrucciones de importación:
   
   ```java
   import com.microsoft.azure.AzureEnvironment;
   import com.microsoft.azure.credentials.ApplicationTokenCredentials;
   import com.microsoft.azure.management.mediaservices.v2018_07_01.implementation.MediaManager;
   import com.microsoft.rest.LogLevel;
   ```
1. Para crear las credenciales de Active Directory que necesita para realizar solicitudes, agregue el siguiente código al método principal de la clase de aplicación y establezca los valores que obtuvo de [acceder a las API](access-api-cli-how-to.md):
   
   ```java
   final String clientId = "00000000-0000-0000-0000-000000000000";
   final String tenantId = "00000000-0000-0000-0000-000000000000";
   final String clientSecret = "00000000-0000-0000-0000-000000000000";
   final String subscriptionId = "00000000-0000-0000-0000-000000000000";

   try {
      ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(clientId, tenantId, clientSecret, AzureEnvironment.AZURE);
      credentials.withDefaultSubscriptionId(subscriptionId);

      MediaManager manager = MediaManager
              .configure()
              .withLogLevel(LogLevel.BODY_AND_HEADERS)
              .authenticate(credentials, credentials.defaultSubscriptionId());
      System.out.println("signed in");
   }
   catch (Exception e) {
      System.out.println("Exception encountered.");
      System.out.println(e.toString());
   }
   ```
1. Ejecute la aplicación.

## <a name="see-also"></a>Vea también

- [Conceptos de Azure Media Services](concepts-overview.md)
- [SDK de Java](https://aka.ms/ams-v3-java-sdk)
- [Referencia de Java](https://aka.ms/ams-v3-java-ref)
- [com.microsoft.azure.mediaservices.v2018_07_01:azure-mgmt-media](https://search.maven.org/artifact/com.microsoft.azure.mediaservices.v2018_07_01/azure-mgmt-media/1.0.0-beta/jar)

## <a name="next-steps"></a>Pasos siguientes

Ahora puede incluir `import com.microsoft.azure.management.mediaservices.v2018_07_01.*;` y comenzar a manipular las entidades.

---
title: Fuente de cómo usar una NuGet personalizada en espacios de desarrollo de Azure
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: johnsta
ms.author: johnsta
ms.date: 05/11/2018
ms.topic: conceptual
description: Use una fuente NuGet personalizada para acceder a paquetes NuGet y utilizarlos en Azure Dev Spaces.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, contenedores
manager: ghogen
ms.openlocfilehash: 1a000e378a9b8ecfb09d778fd6444e3f24b3df7b
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57772473"
---
#  <a name="use-a-custom-nuget-feed-in-an-azure-dev-space"></a>Uso de una fuente NuGet personalizada en Azure Dev Spaces

Una fuente NuGet proporciona una manera cómoda de incluir orígenes de paquetes en un proyecto. Azure Dev Spaces deberá poder acceder a esta fuente para que las dependencias se instalen correctamente en el contenedor de Docker.

## <a name="set-up-a-nuget-feed"></a>Configuración de una fuente NuGet

Para configurar una fuente NuGet:
1. Agregue una [referencia de paquete](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) en el archivo `*.csproj` bajo en nodo `PackageReference`.

   ```xml
   <ItemGroup>
       <!-- ... -->
       <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
       <!-- ... -->
   </ItemGroup>
   ```

2. Crear un archivo [NuGet.Config](https://docs.microsoft.com/nuget/reference/nuget-config-file) en la carpeta del proyecto.
     * Use la sección `packageSources` para hacer referencia a la ubicación de la fuente NuGet. Importante: La fuente NuGet debe ser de acceso público.
     * Use la sección `packageSourceCredentials` para configurar las credenciales de nombre de usuario y contraseña. 

   ```xml
   <packageSources>
       <add key="Contoso" value="https://contoso.com/packages/" />
   </packageSources>

   <packageSourceCredentials>
       <Contoso>
           <add key="Username" value="user@contoso.com" />
           <add key="ClearTextPassword" value="33f!!lloppa" />
       </Contoso>
   </packageSourceCredentials>
   ```

3. Si usa control de código fuente:
    - Haga referencia a `NuGet.Config` en el archivo `.gitignore` para no confirmar accidentalmente las credenciales en el repositorio de origen.
    - Abra el archivo `azds.yaml` del proyecto, busque la sección `build` e inserte el fragmento de código siguiente para asegurarse de que el archivo `NuGet.Config` se sincronizará con Azure de forma que se utilice durante el proceso de creación de la imagen de contenedor. (De forma predeterminada, Azure Dev Spaces no sincroniza los archivos que coinciden con las reglas `.gitignore` y `.dockerignore`).

        ```yaml
        build:
        useGitIgnore: true
        ignore:
        - “!NuGet.Config”
        ```


## <a name="next-steps"></a>Pasos siguientes

Una vez completados los pasos anteriores, la próxima vez que ejecute `azds up` (o presione `F5` en VSCode o Visual Studio), Azure Dev Spaces sincronizará el archivo `NuGet.Config` en Azure, que, a continuación, lo utilizará `dotnet restore` para instalar las dependencias de paquete en el contenedor.


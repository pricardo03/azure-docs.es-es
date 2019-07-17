---
title: Configuración de Azure Functions para los proveedores personalizados de Azure
description: En este tutorial se explica cómo crear una función de Azure y cómo configurarla para que funcione con proveedores personalizados de Azure.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: d7e4de43659db88bfd9aad40cc3b9f1753189bba
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799124"
---
# <a name="setup-azure-functions-for-azure-custom-providers"></a>Configuración de Azure Functions para los proveedores personalizados de Azure

Los proveedores personalizados le permiten personalizar los flujos de trabajo en Azure. Un proveedor personalizado es un contrato entre Azure y un `endpoint`. Este tutorial le guiará por proceso de configuración de una función de Azure para que funcione como un proveedor personalizado `endpoint`.

Este tutorial se divide en los pasos siguientes:

- Creación de la función de Azure
- Instalación de enlaces de tabla de Azure
- Actualización de los métodos HTTP de RESTful
- Adición de paquetes NuGet de Azure Resource Manager

Este tutorial se basará en los siguientes tutoriales:

- [Creación de la primera función de Azure con Azure Portal](../azure-functions/functions-create-first-azure-function.md)

## <a name="creating-the-azure-function"></a>Creación de la función de Azure

> [!NOTE]
> En este tutorial, vamos a crear un punto de conexión de servicio simple con una función de Azure, aunque el proveedor personalizado puede usar cualquier `endpoint` públicamente accesible. Azure Logic Apps, Azure API Management y Azure Web Apps son algunas excelentes alternativas.

Para iniciar este tutorial, debe seguir el tutorial donde se explica cómo [crear la primera función de Azure en Azure Portal](../azure-functions/functions-create-first-azure-function.md). El tutorial creará una función de webhook de .NET Core que puede modificarse en Azure Portal.

## <a name="install-azure-table-bindings"></a>Instalación de enlaces de tabla de Azure

Esta sección le guiará por unos pasos rápidos para instalar los enlaces de Azure Table Storage.

1. Vaya a la pestaña `Integrate` para buscar HttpTrigger.
2. Haga clic en `+ New Input`.
3. Seleccione `Azure Table Storage`.
4. Instale `Microsoft.Azure.WebJobs.Extensions.Storage` si no está instalado.
5. Actualice `Table parameter name` a "tableStorage" y `Table name` a "myCustomResources".
6. Guarde el parámetro de entrada actualizado.

![Información general del proveedor personalizado](./media/create-custom-providers/azure-functions-table-bindings.png)

## <a name="update-restful-http-methods"></a>Actualización de los métodos HTTP de RESTful

Esta sección le guiará por unos pasos rápidos para configurar la función de Azure con el fin de incluir los métodos de solicitud de RESTful del proveedor personalizado.

1. Vaya a la pestaña `Integrate` para buscar HttpTrigger.
2. Actualice los `Selected HTTP methods` en : GET, POST, DELETE y PUT.

![Información general del proveedor personalizado](./media/create-custom-providers/azure-functions-http-methods.png)

## <a name="modifying-the-csproj"></a>Modificación de csproj

> [!NOTE]
> Si csproj no está en el directorio, se puede agregar manualmente o aparecerá cuando la extensión `Microsoft.Azure.WebJobs.Extensions.Storage` esté instalada en la función.

Después, actualizaremos el archivo csproj para incluir las bibliotecas NuGet útiles que facilitarán el análisis de las solicitudes entrantes de los proveedores personalizados. Siga los pasos de [agregar extensiones desde el portal](../azure-functions/install-update-binding-extensions-manual.md) y actualice csproj para incluir las siguientes referencias del paquete:

```xml
<PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
<PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
<PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
```

Archivo csproj de ejemplo:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <WarningsAsErrors />
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
    <PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
    <PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
  </ItemGroup>
</Project>
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, vamos a configurar una función de Azure para que funcione como un proveedor personalizado de Azure `endpoint`. Vaya al siguiente artículo para aprender a crear un `endpoint` del proveedor personalizado de RESTful.

- [Tutorial: Creación de un punto de conexión de proveedor personalizado de RESTful](./tutorial-custom-providers-function-authoring.md)

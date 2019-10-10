---
title: Especificación de variables de entorno para servicios de Azure Service Fabric | Microsoft Docs
description: Se muestra cómo usar variables de entorno en aplicaciones de Service Fabric.
documentationcenter: .net
author: mikkelhegn
manager: markfuss
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: df9b199c24301016b9f9da8a8dec52129bbf94bd
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703538"
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Especificación de variables de entorno para servicios de Service Fabric

En este artículo se muestra cómo especificar variables de entorno para un servicio o un contenedor de Service Fabric.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Procedimiento para especificar variables de entorno para servicios

En este ejemplo, puede establecer una variable de entorno para un contenedor. En el artículo se da por supuesto que ya tiene un manifiesto de servicio y aplicación.

1. Abra el archivo ServiceManifest.xml.
2. En el elemento `CodePackage`, agregue un nuevo elemento `EnvironmentVariables` y un elemento `EnvironmentVariable` para cada variable de entorno.

    ```xml
    <CodePackage Name="MyCode" Version="CodeVersion1">
            ...
            <EnvironmentVariables>
                  <EnvironmentVariable Name="MyEnvVariable" Value="DefaultValue"/>
                  <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentVariables>
    </CodePackage>
    ```

   Estas variables de entorno se pueden invalidar en el manifiesto de aplicación.

3. Para invalidar las variables de entorno en el manifiesto de aplicación, use el elemento `EnvironmentOverrides`.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="specifying-environment-variables-dynamically-using-docker-compose"></a>Especificación dinámica de variables de entorno con Docker Compose

Service Fabric admite la posibilidad de [usar Docker Compose para la implementación](service-fabric-docker-compose.md#supported-compose-directives). Los archivos de Compose pueden obtener variables de entorno del shell. Este comportamiento se puede usar para sustituir dinámicamente los valores de entorno que se busquen:

```yml
environment:
  - "hostname:${hostname}"
```

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre algunos de los conceptos principales que se describen en este artículo, consulte los artículos de [Administración de aplicaciones para varios entornos](service-fabric-manage-multiple-environment-app-configuration.md).

Para obtener más información sobre otras funcionalidades de administración de aplicaciones disponibles en Visual Studio, vea [Administración de aplicaciones de Service Fabric en Visual Studio](service-fabric-manage-application-in-visual-studio.md).

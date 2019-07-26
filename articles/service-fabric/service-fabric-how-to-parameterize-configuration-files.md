---
title: Parametrización de los archivos de configuración en Azure Service Fabric | Microsoft Docs
description: Aprenda a parametrizar los archivos de configuración en Service Fabric.
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/09/2018
ms.author: mikhegn
ms.openlocfilehash: dad497978de7187177998524db3b2f2ee448c717
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464780"
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Cómo parametrizar los archivos de configuración en Service Fabric

En este artículo se muestra cómo parametrizar un archivo de configuración de Service Fabric.  Si aún no conoce los conceptos básicos de administración de aplicaciones para varios entornos, lea [Administración de aplicaciones en varios entornos](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="procedure-for-parameterizing-configuration-files"></a>Procedimiento para parametrizar los archivos de configuración

En este ejemplo, se reemplaza un valor de configuración mediante parámetros en la implementación de la aplicación.

1. Abra el archivo *\<MyService>\PackageRoot\Config\Settings.xml* en el proyecto de servicio.
1. Establezca un nombre del parámetro de configuración y un valor, por ejemplo, el tamaño de caché igual a 25; para ello, agregue el siguiente código XML:

   ```xml
    <Section Name="MyConfigSection">
      <Parameter Name="CacheSize" Value="25" />
    </Section>
   ```

1. Guarde y cierre el archivo.
1. Abra el archivo *\<MyApplication>\ApplicationPackageRoot\ApplicationManifest.xml*.
1. En el archivo ApplicationManifest.xml, declare un parámetro y el valor predeterminado en el elemento `Parameters`.  Se recomienda que el nombre del parámetro contenga el nombre del servicio (por ejemplo, "MiServicio").

   ```xml
    <Parameters>
      <Parameter Name="MyService_CacheSize" DefaultValue="80" />
    </Parameters>
   ```
1. En la sección `ServiceManifestImport` del archivo ApplicationManifest.xml, agregue un elemento `ConfigOverrides` y `ConfigOverride` que haga referencia al paquete de configuración, la sección y el parámetro.

   ```xml
    <ConfigOverrides>
      <ConfigOverride Name="Config">
          <Settings>
            <Section Name="MyConfigSection">
                <Parameter Name="CacheSize" Value="[MyService_CacheSize]" />
            </Section>
          </Settings>
      </ConfigOverride>
    </ConfigOverrides>
   ```

> [!NOTE]
> Cuando se agrega un elemento ConfigOverride, Service Fabric siempre elige los parámetros de la aplicación o el valor predeterminado especificado en el manifiesto de la aplicación.
>
>

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre otras funcionalidades de administración de aplicaciones disponibles en Visual Studio, vea [Administración de aplicaciones de Service Fabric en Visual Studio](service-fabric-manage-application-in-visual-studio.md).

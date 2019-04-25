---
title: Actualización de la configuración de un clúster independiente de Azure Service Fabric | Microsoft Docs
description: Obtenga información acerca de cómo actualizar la configuración que ejecuta un clúster de Service Fabric independiente.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: f99c1ebb64bf881bcd42f15e13bb81b96ccfa064
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60387135"
---
# <a name="upgrade-the-configuration-of-a-standalone-cluster"></a>Actualización de la configuración de un clúster independiente 

En cualquier sistema moderno, la capacidad de actualización es clave para el éxito a largo plazo del producto. Un clúster de Azure Service Fabric es un recurso que usted posee. En este artículo se describe cómo actualizar las opciones de configuración de un clúster de Service Fabric independiente.

## <a name="customize-cluster-settings-in-the-clusterconfigjson-file"></a>Personalización de la configuración del clúster en el archivo ClusterConfig.json
Los clústeres independientes se configuran mediante el archivo *ClusterConfig.json*. Para obtener más información acerca de las distintas opciones de configuración, vea [Opciones de configuración de un clúster de Windows independiente](service-fabric-cluster-manifest.md).

Puede agregar, actualizar o eliminar la configuración en la sección `fabricSettings` en la sección [Propiedades del clúster](./service-fabric-cluster-manifest.md#cluster-properties) de *ClusterConfig.json*. 

Por ejemplo, el siguiente código JSON agrega una nueva configuración *MaxDiskQuotaInMB* a la sección *Diagnósticos* de `fabricSettings`:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Cuando haya modificado la configuración del archivo ClusterConfig.json, [pruebe la configuración del clúster](#test-the-cluster-configuration) y, después, [actualice la configuración del clúster](#upgrade-the-cluster-configuration) para aplicar la configuración al clúster. 

## <a name="test-the-cluster-configuration"></a>Prueba de la configuración del clúster
Antes de iniciar la actualización de la configuración, puede probar el nuevo archivo JSON de configuración del clúster mediante la ejecución del siguiente script de PowerShell en el paquete independiente:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>
```

O bien, puede utilizar este script:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>
```

Algunas configuraciones no se pueden actualizar, como los puntos de conexión, el nombre del clúster, la dirección IP del nodo, etc. El nuevo archivo JSON de configuración del clúster se prueba en el antiguo y generará errores en la ventana de PowerShell si hay cualquier problema.

## <a name="upgrade-the-cluster-configuration"></a>Actualizar la configuración del clúster
Para actualizar la configuración del clúster, ejecute [Start-ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade). El dominio de actualización procesa la actualización de la configuración.

```powershell
Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

## <a name="upgrade-cluster-certificate-configuration"></a>Actualización de la configuración de un certificado de clúster
Se utiliza un certificado de clúster para la autenticación entre nodos de clúster. La sustitución del certificado debe realizarse con precaución adicional ya que en caso de error se bloqueará la comunicación entre los nodos del clúster.

Se admiten cuatro opciones:  

* Actualización del certificado único: La ruta de actualización es el certificado A (principal) -> certificado B (principal) -> certificado C (principal) ->...

* Actualización de dos certificados: La ruta de actualización es el certificado A (principal) -> certificado A (principal) y B (secundario) -> certificado B (principal) -> certificado B (principal) y C (secundario) -> certificado C (principal) ->...

* Actualización del tipo de certificado: Configuración de certificados basada en CommonName de configuración <> – basados en huella digital de certificado. Por ejemplo, Huella digital del certificado A (principal) y Huella digital B (secundaria) -> Certificado CommonName C.

* Actualización de huella digital del emisor de certificado: La ruta de actualización es Certificate CN = A, IssuerThumbprint = IT1 (principal) -> Certificate CN = A, IssuerThumbprint = IT1, IT2 (principal) -> Certificate CN = A, IssuerThumbprint = IT2 (principal).


## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre cómo personalizar la [configuración de clúster de Service Fabric](service-fabric-cluster-fabric-settings.md).
* Obtenga información sobre cómo [escalar o reducir horizontalmente el clúster](service-fabric-cluster-scale-up-down.md).
* Obtenga información sobre [actualizaciones de aplicaciones](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG

---
title: Ejemplo de implementación de script de la CLI de Azure Service Fabric (sfctl)
description: Implementación de una aplicación en un clúster de Azure Service Fabric mediante la CLI de Azure Service Fabric
services: service-fabric
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
origin.date: 04/16/2018
ms.date: 04/29/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: f3ab32101a46c1044954f2efe88ac05ab81af24a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622005"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Implementación de una aplicación en un clúster de Service Fabric

Este script de ejemplo copia un paquete de aplicación en un almacén de imágenes de clúster, registra el tipo de aplicación en el clúster y crea una instancia de la aplicación a partir del tipo de aplicación. En este momento también se crea cualquier servicio predeterminado.

Instale la [CLI de Service Fabric](../service-fabric-cli.md) si es necesario.

## <a name="sample-script"></a>Script de ejemplo

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth.cloudapp.chinacloudapi.cn:19080

# Upload the application files to the image store
# (note the last folder name, Debug in this example)
sfctl application upload \
    --path  C:\Code\svcfab-vs\svcfab-vs\pkg\Debug \
    --show-progress

# Register the application (manifest files) from the image store
# (Note the last folder from the previous command is used: Debug)
sfctl application provision \
    --application-type-build-path Debug \
    --timeout 500

# Create an instance of the registered application and 
# auto deploy any defined services
sfctl application create \
    --app-name fabric:/MyApp \
    --app-type MyAppType \
    --app-version 1.0.0

```

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Cuando termine, se puede usar el script [remove](cli-remove-application.md) para quitar la aplicación. El script remove elimina la instancia de aplicación, anula el registro del tipo de aplicación y elimina el paquete de aplicación del almacén de imágenes.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de la CLI de Service Fabric](../service-fabric-cli.md).

Puede encontrar ejemplos adicionales de la CLI de Service Fabric para Azure Service Fabric en los [ejemplos de la CLI de Service Fabric](../samples-cli.md).

<!--Update_Description: update meta properties, update link -->
---
title: 'Ejemplo de script de la CLI de Service Fabric: Actualización de una aplicación en un clúster'
description: 'Ejemplo de script de la CLI de Service Fabric: Actualización de una aplicación con una nueva versión Este ejemplo también actualiza una aplicación implementada con los nuevos bits.'
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: atsenthi
ms.custom: ''
ms.openlocfilehash: 72195a3a127e33ffa6118f77c4fa58ba5f60ee17
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68600092"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Incorporación de un certificado de aplicación a un clúster de Service Fabric

Este script de ejemplo carga una nueva versión de una aplicación existente y, después, actualiza una aplicación implementada con los nuevos bits.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-sh[main](../../../cli_scripts/service-fabric/upgrade-application/upgrade-application.sh "Upload and update an application on a Service Fabric cluster")]

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de la CLI de Service Fabric](../service-fabric-cli.md).

Puede encontrar ejemplos adicionales de la CLI de Service Fabric para Azure Service Fabric en los [ejemplos de la CLI de Service Fabric](../samples-cli.md).

---
title: Guía para desarrolladores de Azure Alemania | Microsoft Docs
description: En este artículo se comparan características y se ofrecen orientaciones sobre el desarrollo de aplicaciones para Azure Alemania.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: ralfwi
ms.openlocfilehash: 5b0816079cdccd8a75830764fc8b1ad8a4d1b95e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033730"
---
# <a name="azure-germany-developer-guide"></a>Guía para desarrolladores de Azure Alemania
El entorno de Azure Alemania es una instancia de Microsoft Azure independiente del resto de la red de Microsoft. Esta guía aborda las diferencias que los desarrolladores de aplicaciones y administradores deben conocer para interactuar y trabajar con regiones independientes de Azure.

## <a name="overview"></a>Información general
Microsoft proporciona varias herramientas para ayudar a los desarrolladores a crear e implementar aplicaciones en la nube para los servicios de Microsoft Azure global ("Azure global") y los servicios de Microsoft Azure Alemania. Azure Alemania aborda las necesidades de seguridad y cumplimiento de los clientes para seguir las normativas de privacidad de datos alemanas. Azure Alemania ofrece aislamiento físico y de red de las implementaciones de Azure global y proporciona un administrador de datos que actúa bajo la ley alemana.

Cuando los desarrolladores crean e implementan aplicaciones en Azure Alemania, en lugar de en Azure global, estos necesitan conocer las diferencias entre los dos conjuntos de servicios. Los ámbitos específicos que deben comprenderse tienen que ver con instalar y configurar su entorno de programación, configurar puntos de conexión, escribir aplicaciones e implementarlas como servicios en Azure Alemania.

La información de esta guía resume estas diferencias. Complementa la información que está disponible en el sitio [Azure Alemania](https://azure.microsoft.com/overview/clouds/germany/ "Azure Germany") y el [Centro de documentación de Azure](https://azure.microsoft.com/documentation/). 

La información oficial también podría estar disponible en otras ubicaciones, como:
* [Centro de confianza de Microsoft Azure](https://azure.microsoft.com/support/trust-center/ "Microsoft Azure Trust Center") 
* [Blog de Azure](https://azure.microsoft.com/blog/ "Azure blog")
* [Blog de Azure Alemania](https://blogs.msdn.microsoft.com/azuregermany/ "Azure Germany blog")

## <a name="guidance-for-developers"></a>Instrucciones para desarrolladores
La mayoría del contenido técnico actualmente disponible da por supuesto que las aplicaciones se desarrollan para Azure global y no para Azure Alemania. Por esta razón, es importante tener en cuenta dos diferencias clave en las aplicaciones que desarrolle para hospedar en Azure Alemania:

* Ciertos servicios y características que se encuentran en regiones específicas de Azure global pueden no estar disponibles en Azure Alemania.
* Las configuraciones de características en Azure Alemania pueden diferir de las de Azure global. Por lo tanto, es importante revisar el código de ejemplo, las configuraciones y los pasos para asegurarse de que genera y ejecuta dentro del entorno de Cloud Services de Azure Alemania.

En la actualidad, Centro de Alemania y Nordeste de Alemania son las regiones que están disponibles en Azure Alemania. Para conocer las regiones y los servicios disponibles, consulte [Productos disponibles por región](https://azure.microsoft.com/regions/services).


## <a name="endpoint-mapping"></a>Asignación de puntos de conexión
Para obtener más información sobre la asignación de los puntos de conexión de Azure global y Azure SQL Database a puntos de conexión específicos de Azure Alemania, consulte la tabla siguiente:

| NOMBRE | Punto de conexión de Azure Alemania |
| --- | --- |
| ActiveDirectoryServiceEndpointResourceId | https://management.core.cloudapi.de/ |
| GalleryUrl                               | https://gallery.cloudapi.de/ |
| ManagementPortalUrl                      | https://portal.microsoftazure.de/ |
| ServiceManagementUrl                     | https://management.core.cloudapi.de/ |
| PublishSettingsFileUrl                   | https://manage.microsoftazure.de/publishsettings/index |
| ResourceManagerUrl                       | https://management.microsoftazure.de/ |
| SqlDatabaseDnsSuffix                     | .database.cloudapi.de |
| StorageEndpointSuffix                    | core.cloudapi.de |
| ActiveDirectoryAuthority                 | https://login.microsoftonline.de/ |
| GraphUrl                                 | https://graph.cloudapi.de/ |
| TrafficManagerDnsSuffix                  | azuretrafficmanager.de |
| AzureKeyVaultDnsSuffix                   | vault.microsoftazure.de |
| AzureKeyVaultServiceEndpointResourceId   | https://vault.microsoftazure.de |
| Sufijo de Service Bus                       | ServiceBus.cloudapi.de |


## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de Azure Alemania, consulte los siguientes recursos:

* [Suscribirse a una prueba](https://azure.microsoft.com/free/germany/)
* [Adquisición de Azure Alemania](https://azure.microsoft.com/overview/clouds/germany/)
* Si ya tiene una cuenta de Azure Alemania, [inicie sesión](https://portal.microsoftazure.de/).
* [Introducción a Azure Alemania](./germany-welcome.md)
* [Blog de Azure Alemania](https://blogs.msdn.microsoft.com/azuregermany/)
* [Cumplimiento de Azure](https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings)


---
title: 'Preguntas frecuentes sobre los servicios de FHIR en Azure: Azure API for FHIR'
description: En este artículo se responden a las preguntas más frecuentes sobre Azure API for FHIR
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 50a610c3fb92b65b62e17a986ea1c0f70d0a0f1d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033776"
---
# <a name="frequently-asked-questions-about-azure-api-for-fhir"></a>Preguntas más frecuentes sobre Azure API for FHIR

## <a name="storage-location"></a>Ubicación de almacenamiento

**¿Están los datos detrás de las API de FHIR&reg; almacenadas en Azure?** Sí, los datos se almacenan en bases de datos administradas en Azure. Azure API for FHIR no proporciona el acceso directo al almacén de datos subyacente.

## <a name="identity-providers"></a>Proveedores de identidades

Actualmente se admite Microsoft Azure Active Directory como proveedor de identidades.

## <a name="supported-fhir-version"></a>Versión de FHIR admitida

Azure API for FHIR (PaaS): Se admite la versión 3.0.1

FHIR Server para Azure (OSS): Se admite la versión 4.0.0, la versión más reciente de la especificación FHIR, además de la versión 3.0.1.

Consulte las [características admitidas](fhir-features-supported.md) para obtener más información. Lea sobre los cambios entre las versiones en el [historial de versiones de HL7 FHIR](http://hl7.org/fhir/R4/history.html)

## <a name="oss-and-azure-api-for-fhir"></a>OSS y Azure API for FHIR

¿Cuál es la diferencia entre Microsoft FHIR Server de código abierto para Azure y Azure API for FHIR? Azure API for FHIR es una versión hospedada y administrada del software de código abierto Microsoft FHIR Server para Azure. En el servicio administrado, Microsoft proporciona todo el mantenimiento, las actualizaciones, etc. Al ejecutar el servidor OSS FHIR Server para Azure, tiene acceso directo a los servicios subyacentes, pero también es responsable del mantenimiento, actualización del servidor y de todo el trabajo de cumplimiento requerido si almacena datos de PHI.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha leído algunas de las preguntas más frecuentes sobre Azure API for FHIR. Lea acerca de las características de API admitidas en Microsoft FHIR Server para Azure.
 
>[!div class="nextstepaction"]
>[Características de FHIR admitidas](fhir-features-supported.md)
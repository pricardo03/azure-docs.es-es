---
title: Preguntas más frecuentes sobre la plataforma de Marketplace para el módulo de IoT Edge | Microsoft Docs
description: Preguntas más frecuentes sobre la plataforma de Marketplace para el módulo de IoT Edge.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 2c2e7729eb70a5dd37dc4df10605eec9429e1043
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48807463"
---
# <a name="iot-edge-module-marketplace-frequently-asked-questions"></a>Preguntas más frecuentes sobre la plataforma de Marketplace para el módulo de IoT Edge


## <a name="what-is-the-edge-module-marketplace"></a>¿Qué es la plataforma de Marketplace para el módulo de IoT Edge?


La plataforma de Marketplace para el módulo de IoT Edge es una lista de módulos de Edge *certificados* y compilados previamente que se pueden *descubrir* en Azure Marketplace.

![Módulos de IoT Edge](./media/cloud-partner-portal-iot-edge-module-faq/iot-edge-modules.png)

## <a name="where-will-edge-modules-be-visible"></a>¿Dónde se podrán ver los módulos de IoT Edge? 


En [Marketplace de Azure Portal](https://ms.portal.azure.com/) (experiencia autenticada), en la categoría de Internet de las cosas, etiquetados como "Módulo de IoT Edge".

Y en el [sitio web de Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/internet-of-things?page=1) (experiencia anónima), en la categoría de Internet de las cosas, etiquetados como "Módulo de IoT Edge".

## <a name="is-it-open-to-partners"></a>¿Está abierto a los asociados?


Todavía no. Actualmente, solo los módulos creados por Microsoft están publicados en la sección de IoT Edge de Marketplace. 

## <a name="why-should-partners-publish-their-iot-edge-modules"></a>¿Por qué deberían publicar los asociados sus módulos de IoT Edge?


-   Para aumentar la visibilidad de su producto agregando este canal al mercado y dando a conocer sus soluciones.

-   Para conseguir más clientes potenciales. Como parte de Azure Marketplace, puede obtener detalles acerca de quién está interesado en su solución.

-   Para ser uno de los primeros en aprovechar sus funcionalidades de monetización.

## <a name="what-is-the-onboarding-process"></a>¿En qué consiste el proceso de incorporación?


Aunque aún no está abierto, el proceso de incorporación se realizará a través de Azure Marketplace. Puede encontrar directrices detalladas en la [Guía de publicación de Azure Marketplace y AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide). Consulte el tipo de anuncio de transacción para Containers. 

En primer lugar, los asociados deben convertirse en anunciantes en Azure Marketplace. Posteriormente, podrán incorporar sus módulos de IoT Edge mediante [Cloud Partner Portal](./cloud-partner-portal-getting-started-with-the-cloud-partner-portal.md).

## <a name="are-there-any-monetization-capabilities"></a>¿Hay ninguna funcionalidad de monetización?


En este momento no hay ninguna lista para usarse. Nuestro primer objetivo es abrir un Marketplace con módulos de IoT Edge *gratis* o de la modalidad *Traiga su propia licencia*. Iremos agregando más funcionalidades de monetización como, por ejemplo, un modelo de facturación de consumo.

## <a name="what-is-bring-your-own-license-byol"></a>¿En qué consiste la opción Traiga su propia licencia (BYOL)?


La definición oficial que se encuentra en las [Directivas de participación de Microsoft Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) es:

- *Los clientes obtienen el derecho de acceso o uso de la oferta fuera de Azure Marketplace y no se les cobran tarifas de Azure Marketplace por su uso de la oferta en dicha plataforma.*

Depende de los asociados conceder licencias para su software y obtener ingresos por ello.

## <a name="can-partners-publish-a-freemium-module"></a>¿Pueden los asociados publicar un módulo "freemium"?


Sí, los módulos freemium son módulos disponibles de forma gratuita, aunque con algunas restricciones, y estos se considerarán como cualquier otra publicación.

## <a name="is-intellectual-property-protected"></a>¿Está protegida la propiedad intelectual?


Un módulo de Edge es un contenedor compatible con Docker. Depende de los asociados proteger la propiedad intelectual (IP) del contenido de los contenedores distribuidos.

## <a name="where-will-the-modules-be-hosted"></a>¿Donde se hospedarán los módulos?


Los módulos de IoT Edge se hospedarán en un registro de contenedor de propiedad de Microsoft que se podrá consultar de forma anónima, como Docker Hub.

## <a name="what-are-the-integration-plans-between-the-azure-marketplace-and-the-azure-iot-tools"></a>¿Cuáles son los planes de integración entre Azure Marketplace y las herramientas de Azure IoT?

Vamos a crear una integración aún más estrecha entre Azure Marketplace y las herramientas de Azure IoT. Son ejemplos de que lo que tenemos en mente es para habilitar la navegación en la plataforma de Marketplace para los módulos de IoT Edge directamente desde el portal de IoT Hub o directamente desde Visual Studio Code.

## <a name="which-operating-system-or-architecture-should-my-container-support"></a>¿Con qué sistema operativo o arquitectura debe ser compatible mi contenedor?

Los módulos de IoT Edge deben ser compatibles con el mismo sistema operativo o matriz de arquitectura que IoT Edge en producción. Esta lista se guarda en [Compatibilidad con Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support). Por ejemplo, un módulo debe ser compatible actualmente con Linux x64 y Linux ARM32.

## <a name="are-there-any-other-certification-constraints-to-be-aware-of"></a>¿Hay alguna otra restricción de certificación a tener en cuenta?

Todavía estamos trabajando en el conjunto exacto de restricciones de certificación. Los principios rectores incluyen:

-   Promover la calidad frente a la cantidad.

-   Contenedores específicos de IoT Edge (no aleatorios).

-   Calidad de producción.

-   Implementación en 1 clic (se proporciona al menos un conjunto de valores de configuración predeterminados).

## <a name="any-other-questions"></a>¿Alguna otra pregunta?


Póngase en contacto con el equipo de [incorporación a Azure IoT Edge](mailto:azureiotedgeonboarding@service.microsoft.com).

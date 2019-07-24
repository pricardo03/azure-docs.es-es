---
title: Alertas de seguridad en Azure Security Center | Microsoft Docs
description: En este tema se explica qué son las alertas de seguridad y los distintos tipos disponibles en Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 1b71e8ad-3bd8-4475-b735-79ca9963b823
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 91dd397095718b3b43e41767af422801fd50b7f1
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295693"
---
# <a name="security-alerts-in-azure-security-center"></a>Alertas de seguridad en Azure Security Center

En este artículo se presentan los distintos tipos de alertas de seguridad disponibles en Azure Security Center (ASC). Existe una amplia variedad de alertas para los numerosos y diversos tipos de recursos. ASC genera alertas para los recursos implementados en Azure, así como para los recursos implementados en entornos locales y entornos en la nube híbrida. 

## <a name="what-are-security-alerts"></a>¿Qué son las alertas de seguridad?

Las alertas son notificaciones que Security Center genera cuando detecta amenazas en los recursos. Asigna prioridades y enumera las alertas, junto con la información necesaria para que pueda investigar rápidamente el problema. Security Center también proporciona recomendaciones sobre el modo en que puede corregir un ataque.

## <a name="how-does-security-center-detect-threats"></a>¿Cómo detecta Security Center las amenazas?

Para detectar amenazas reales y reducir los falsos positivos, Security Center recopila, analiza e integra automáticamente los datos de registro de los recursos de Azure, la red y las soluciones de asociados conectados, como firewalls y soluciones de protección de puntos de conexión. Security Center analiza estos datos (a menudo, relacionando la información de diferentes orígenes) para identificar las amenazas.

ASC supervisa los recursos, ya estén implementados en Azure o en otros entornos locales y en la nube híbrida. Para más información sobre detección y respuesta a amenazas, consulte [Detección de las amenazas y respuesta por parte de Azure Security Center](security-center-detection-capabilities.md#asc-detects).

## <a name="security-alert-types"></a>Tipos de alertas de seguridad

Los temas siguientes le guiarán a través de las diferentes alertas de ASC según los tipos de recursos:

* [Alertas de servidores y máquinas virtuales de IaaS](security-center-alerts-iaas.md)
* [Alertas de procesos nativos](security-center-alerts-compute.md)
* [Alertas de servicios de datos](security-center-alerts-data-services.md)

Los temas siguientes explican cómo aprovecha Security Center los diferentes datos de telemetría que recopila de la integración con la infraestructura de Azure a fin de aplicar capas de protección adicionales para los recursos implementados en Azure:

* [Alertas de la capa de servicios](security-center-alerts-service-layer.md)
* [Integración con productos de seguridad de Azure](security-center-alerts-integration.md)

## <a name="what-are-alert-incidents"></a>¿Qué son los incidentes de alertas?

Un incidente de seguridad es una recopilación de alertas relacionadas, en lugar de una enumeración de alertas individuales. Security Center usa la fusión para poner en correlación diferentes alertas y señales de baja fidelidad en incidentes de seguridad.

Utilizando los incidentes, Security Center proporciona una vista única de una campaña de ataques y todas las alertas relacionadas. Esta vista le permite comprender rápidamente las acciones que ha realizado el atacante y qué recursos se han visto afectados. Para más información, consulte [Correlación de alertas inteligentes en la nube](security-center-alerts-cloud-smart.md).

## <a name="get-started-with-alerts"></a>Introducción a las alertas

Consulte los temas siguientes para ver más información sobre los recursos supervisados por ASC e instrucciones sobre cómo responder a las alertas presentadas por ASC.

* Para ver qué plataformas y características están protegidas por ASC, consulte [Características y plataformas compatibles con Azure Security Center](security-center-os-coverage.md).  
* Para comprender qué son los incidentes de seguridad y cómo responde ASC ante ellos, consulte [Control de incidentes de seguridad en Azure Security Center](security-center-incident.md). 
* Para aprender a administrar las alertas de seguridad, consulte [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md).
* Para información acerca de cómo validar que Security Center esté configurado correctamente y crear una alerta de prueba, consulte [Validación de alertas en Azure Security Center](security-center-alert-validation.md).  


## <a name="upgrade-to-standard-for-advanced-detections"></a>Actualización a la versión estándar para detecciones avanzadas

Para configurar las detecciones avanzadas, actualice a la versión estándar de Azure Security Center. 

1. En el menú Security Center, seleccione **Directiva de seguridad**.
2. Para las suscripciones que desea actualizar a la versión estándar, haga clic en **Editar configuración**. 
3. En la página Configuración, seleccione **Plan de tarifa**. 
   Hay una evaluación gratuita disponible para un mes. Para más información, consulte la [página de precios](https://azure.microsoft.com/pricing/details/security-center/). 

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido qué son las alertas de seguridad y los diferentes tipos de alertas disponibles en Security Center. Para obtener más información, consulte los temas siguientes:

* [Guía de planeamiento y operaciones de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)
* [Preguntas más frecuentes sobre Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-faq): Preguntas más frecuentes acerca del uso del servicio.


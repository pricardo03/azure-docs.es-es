---
title: Correlación de alertas inteligentes en la nube en Azure Security Center (incidentes) | Microsoft Docs
description: En este tema se explica cómo la fusión usa la correlación de alertas inteligentes en la nube para generar incidentes de seguridad en Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: e9d5a771-bfbe-458c-9a9b-a10ece895ec1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 7ba2cf14d9fac100f44a1ef23997b27ba062bee0
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295857"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Correlación de alertas inteligentes en la nube en Azure Security Center (incidentes)

Security Center analiza continuamente las cargas de trabajo de la nube híbrida y usa el análisis avanzado e inteligencia de amenazas para alertarle de cualquier actividad malintencionada.

A medida que crece la cobertura contra amenazas y aumenta la necesidad de detectar incluso el más leve indicio de riesgo, para los analistas de seguridad puede suponer un reto evaluar las diferentes alertas e identificar un ataque real. Security Center permite a los analistas gestionar más fácilmente las alertas y diagnosticar los ataques cuando se producen, al poner en correlación diferentes alertas y señales de baja fidelidad para convertirlas en incidentes de seguridad.

La fusión es la tecnología y el back-end analítico que permite generar los incidentes de Security Center, al poner en correlación diferentes alertas y señales contextuales entre sí. La fusión analiza las distintas señales notificadas en una suscripción entre los recursos y busca patrones dominantes que muestran indicios o una progresión de ataque con información contextual compartida que indica el procedimiento de respuesta unificado que debería adoptarse para cada caso.

El análisis de fusión combina el conocimiento de dominios de seguridad con inteligencia artificial para analizar las alertas y detectar nuevos patrones de ataque cuando se producen. 

Security Center utiliza la matriz de ataque MITRE para asociar alertas con su intención percibida, lo que ayuda a formalizar el conocimiento de dominios de seguridad. Además, mediante el uso de la información recopilada para cada paso de un ataque, Security Center puede descartar actividades que parecen ser pasos de un ataque, pero no lo son.  

Dado que a menudo se producen ataques entre varios inquilinos, Security Center puede combinar algoritmos de inteligencia artificial para analizar las secuencias de ataque que se notifican en cada suscripción para identificarlas como patrones de alerta dominantes en lugar de considerar que se trata de una asociación casual.

Durante la investigación de un incidente, los analistas a menudo necesitan más contexto para llegar a un veredicto sobre la naturaleza de la amenaza y cómo mitigarla. Por ejemplo, incluso cuando se detecta una anomalía de red, sin saber qué más sucede en la red o con respecto al recurso objetivo, es difícil determinar las medidas que se deben tomar a continuación. En estos casos, pueden ser de utilidad los artefactos, los eventos relacionados y la información que un incidente de seguridad puede incluir. La información adicional disponible de los incidentes de seguridad variará según el tipo de amenaza detectada y la configuración de su entorno. 

![Detalles de incidentes de seguridad](./media/security-center-alerts-cloud-smart/security-incident.png)

Para comprender mejor qué son los incidentes de seguridad, consulte [Control de incidentes de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-incident).


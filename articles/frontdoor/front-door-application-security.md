---
title: 'Azure Front Door Service: seguridad en el nivel de aplicación | Microsoft Docs'
description: Este artículo le ayudará a entender cómo habilitar Azure Front Door Service para proteger y asegurar sus servidores back-end de aplicación.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: e5714f60b7fdd790f3af8e31250c41038110fd08
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046728"
---
# <a name="application-layer-security-with-front-door"></a>Seguridad en el nivel de aplicación con Front Door
Azure Front Door Service proporciona funcionalidad de protección de aplicaciones web para proteger las aplicaciones web contra ataques de red y vulnerabilidades web habituales, tales como la inyección de código SQL o scripts entre sitios (XSS). La seguridad en el nivel de aplicación de Front Door está habilitada para servidores front-end http(s), está distribuida globalmente y tiene disponibilidad total para detener los ataques malintencionados en el perímetro de la red de Azure, muy lejos de los servidores back-end. Gracias a su mayor seguridad y la optimización de su rendimiento, Front Door ofrece experiencias web rápidas y segurar a los usuarios finales.

## <a name="application-protection"></a>Protección de las aplicaciones
La protección de las aplicaciones de Front Door se configura en cada entorno perimetral en todo el mundo, en consonancia con las aplicaciones, e impide automáticamente que el tráfico que no sea http(s) llegue a sus aplicaciones web. La arquitectura distribuida multiinquilino permite una protección global a escala sin sacrificar el rendimiento. Para las cargas de trabajo http(s), el servicio de protección de aplicaciones de Front Door ofrece un motor de reglas enriquecido para reglas personalizadas, un conjunto de reglas preconfiguradas frente a ataques comunes, y registros detallados de todas las solicitudes que coincidan con una regla. Se permiten acciones flexibles tales como permiso, bloqueo o solo registro.

## <a name="custom-access-control-rules"></a>Reglas de control de acceso personalizadas
- **Listas de direcciones IP permitidas y denegadas:** puede configurar reglas personalizadas para controlar el acceso a las aplicaciones web en función de una lista de direcciones IP de cliente. Se admiten IP v4 e IP v6
- **Control de acceso basado en la región geográfica:** puede configurar reglas personalizadas para controlar el acceso a las aplicaciones web en función del código de país del que procede una dirección IP de cliente.
- **Filtrado de los parámetros HTTP:** puede configurar reglas de acceso personalizada basadas en la coincidencia de los parámetros de la solicitud http(s), incluidos encabezados, direcciones URL y cadenas de consulta.

## <a name="azure-managed-rules"></a>Reglas administradas por Azure
- De forma predeterminada, hay habilitado un conjunto preconfigurado de reglas para las vulnerabilidades OWASP más habituales. En su versión preliminar, el conjunto de reglas incluye la comprobación de solicitudes sqli y xss. Se agregarán reglas adicionales. Puede elegir comenzar con la acción solo registro para confirmar si las reglas preconfiguradas funcionan según lo previsto para sus aplicaciones. 

## <a name="rate-limiting"></a>Limitación de frecuencia
- Es una regla de control de la frecuencia para limitar el tráfico anormalmente alto desde cualquier dirección IP de cliente.  Puede establecer un umbral para el número de solicitudes web que se permiten a una dirección IP de cliente en un minuto.

## <a name="centralized-protection-policy"></a>Directiva de protección centralizada
- Puede definir varias reglas de protección y agregarlas a una directiva por orden de prioridad. Las reglas personalizadas tienen mayor prioridad que el conjunto de reglas administrado a la hora de permitir excepciones. Hay una única directiva asociada a la aplicación web.  La misma directiva de protección de aplicaciones web se replica en todos los servidores perimetrales en todas las ubicaciones, para garantizar una directiva de seguridad coherente en todas las regiones.

## <a name="configuration"></a>Configuración
- Durante la versión preliminar, puede usar las API REST, PowerShell o la CLI para crear e implementar directivas y reglas de protección de aplicaciones de Front Door. Antes de que el servicio está disponible con carácter general, se admitirá el acceso al portal. 


## <a name="monitoring"></a>Supervisión
Front Door permite supervisar las aplicaciones web frente a ataques mediante métricas en tiempo real que se integran con Azure Monitor para realizar un seguimiento de las alertas y supervisar con facilidad las tendencias.

## <a name="pricing"></a>Precios
La seguridad en el nivel de aplicación de Front Door es gratuita durante la versión preliminar.


## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [crear una instancia de Front Door](quickstart-create-front-door.md).
- Obtenga información sobre [cómo funciona Front Door](front-door-routing-architecture.md).

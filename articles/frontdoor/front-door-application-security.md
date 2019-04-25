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
ms.openlocfilehash: c7b99548e2fe1ad0c1cab39953e28a97e7ebff4b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60193924"
---
# <a name="application-layer-security-with-front-door"></a>Seguridad en el nivel de aplicación con Front Door
Azure Front Door Service proporciona funcionalidad de protección de aplicaciones web para proteger las aplicaciones web contra ataques de red y vulnerabilidades web habituales, tales como la inyección de código SQL o scripts entre sitios (XSS). La seguridad en el nivel de aplicación de Front Door está habilitada para servidores front-end http(s), está distribuida globalmente y tiene disponibilidad total para detener los ataques malintencionados en el perímetro de la red de Azure, muy lejos de los servidores back-end. Gracias a su mayor seguridad y la optimización de su rendimiento, Front Door ofrece experiencias web rápidas y segurar a los usuarios finales.

## <a name="application-protection"></a>Protección de las aplicaciones
La protección de las aplicaciones de Front Door se configura en cada entorno perimetral en todo el mundo, en consonancia con las aplicaciones, e impide automáticamente que el tráfico que no sea http(s) llegue a sus aplicaciones web. Nuestra arquitectura multiinquilino distribuida permite aplicar una protección global a cualquier escala sin sacrificar el rendimiento. En el caso de las cargas de trabajo http(s), el servicio de protección de aplicaciones de Front Door dispone de un avanzado motor de reglas personalizadas, un conjunto de reglas preconfiguradas contra ataques comunes y registros detallados de todas las solicitudes que coinciden con una regla. Se pueden utilizar acciones flexibles, como Permitir, Bloquear o Registrar solo.

## <a name="custom-access-control-rules"></a>Reglas de control de acceso personalizadas
- **Direcciones IP permitidas de lista y la lista de bloques:** Puede configurar reglas personalizadas para controlar el acceso a sus aplicaciones web basado en lista de direcciones IP de cliente. Se admiten direcciones IP v4 e IP v6.
- **Control de acceso basado en geográfica:** Puede configurar reglas personalizadas para controlar el acceso a sus aplicaciones web en función de código de país que proviene de una dirección IP de cliente
- **Filtrado de los parámetros HTTP:** Puede configurar reglas de acceso personalizada basadas en la coincidencia de parámetros de solicitud HTTP incluyen encabezados de dirección URL y las cadenas de consulta

## <a name="azure-managed-rules"></a>Reglas administradas por Azure
- De forma predeterminada, hay habilitado un conjunto preconfigurado de reglas para las vulnerabilidades OWASP más habituales. En su versión preliminar, el conjunto de reglas incluye la comprobación de solicitudes sqli y xss. En el futuro, se agregarán otras reglas. Si lo desea, puede comenzar con la acción de solo registro para comprobar si las reglas preconfiguradas funcionan según lo previsto con sus aplicaciones. 

## <a name="rate-limiting"></a>Limitación de frecuencia
- Las reglas de control de frecuencia están diseñadas para limitar el tráfico anormalmente alto procedente de cualquier dirección IP de cliente.  Puede establecer un umbral en función del número de solicitudes web que se permiten en una dirección IP de cliente durante un minuto.

## <a name="centralized-protection-policy"></a>Directiva de protección centralizada
- Puede definir varias reglas de protección y agregarlas a una directiva por orden de prioridad. Las reglas personalizadas tienen una prioridad mayor que el conjunto de reglas administrado a la hora de permitir excepciones. Hay una única directiva asociada a la aplicación web.  La misma directiva de protección de aplicaciones web se replica en todos los servidores perimetrales de todas las ubicaciones, lo que garantiza que se aplica una directiva de seguridad coherente en todas las regiones.

## <a name="configuration"></a>Configuración
- Durante la versión preliminar, puede usar las API de REST, PowerShell o la CLI para crear e implementar las directivas y las reglas de protección de aplicaciones de Front Door. Se podrá acceder al portal antes de que el servicio esté disponible con carácter general. 


## <a name="monitoring"></a>Supervisión
Front Door permite supervisar las aplicaciones web frente a ataques utilizando métricas en tiempo real que se integran con Azure Monitor para realizar un seguimiento de las alertas y supervisar con facilidad las tendencias.

## <a name="pricing"></a>Precios
La seguridad en el nivel de aplicación de Front Door es gratis durante la versión preliminar.


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).

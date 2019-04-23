---
title: Configuración de directiva de firewall de aplicaciones web con Azure puerta de entrada
description: Obtenga información sobre firewall de aplicaciones web (WAF).
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: tyao;kumud
ms.openlocfilehash: 4c2f070e9b3c972f063008df8880b196ddb069cc
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59797470"
---
# <a name="policy-settings-for-web-application-firewall-with-azure-front-door"></a>Configuración de directiva de firewall de aplicaciones web con Azure puerta de entrada

Una directiva de Firewall de aplicaciones Web (WAF) permite controlar el acceso a sus aplicaciones web mediante un conjunto de reglas personalizadas y administradas. El nombre de la directiva de WAF debe ser único. Recibirá un error de validación si intenta utilizar un nombre existente. Hay varias opciones de nivel de directiva que se aplican a todas las reglas especificadas para esa directiva como se describe en este artículo.

## <a name="waf-state"></a>Estado de WAF

Una directiva de WAF para la puerta de entrada puede estar en uno de los dos estados siguientes:
- **Habilitada:** Cuando se habilita una directiva, WAF está inspeccionando activamente las solicitudes entrantes y realiza las acciones correspondientes según las definiciones de reglas
- **Deshabilitada:** : cuando una directiva está deshabilitada, inspección de WAF está en pausa. Las solicitudes entrantes omitirán WAF y se envían al back-end basado en ruta de acceso principal.

## <a name="waf-mode"></a>Modo de WAF

Directiva de WAF puede configurarse para ejecutarse en los dos modos siguientes:

- **Modo de detección** cuando se ejecuta en modo de detección, WAF no realizar cualquier acción distinta de supervisar y registrar la solicitud y las reglas de WAF coincidente en los registros de WAF. Activar el registro de diagnósticos para la puerta de entrada (al usar el portal, esto puede lograrse yendo a la **diagnósticos** sección en el portal de Azure).

- **Modo de prevención** cuando se configura para ejecutarse en modo de prevención, WAF toma la acción especificada, si una solicitud coincide con una regla. También se registran las solicitudes coincidentes en los registros de WAF.

## <a name="waf-response-for-blocked-requests"></a>Respuesta de WAF para las solicitudes bloqueadas

De forma predeterminada, cuando el WAF bloquea una solicitud debido a una regla coincidente, devuelve un código de 403 estado- **se bloquea la solicitud** mensaje. También se devuelve una cadena de referencia para el registro.

Puede definir un código de estado de respuesta personalizada y un mensaje de respuesta cuando se bloquea una solicitud con WAF. Se admiten los siguientes códigos de estado personalizado:

- 200 ACEPTAR
- 403    Forbidden
- 405 método no permitido
- 406 No aceptable
- 429 demasiadas solicitudes

Mensaje de respuesta y el código de estado de respuesta personalizada es una configuración de nivel de directiva. Una vez configurado, todas las solicitudes bloqueadas obtención el mismo estado de respuesta personalizada y el mensaje de respuesta.

## <a name="uri-for-redirect-action"></a>URI de acción de redirección

Es necesario definir un URI para redirigir las solicitudes como si el **REDIRIGIR** se selecciona la acción para cualquiera de las reglas contenidas en una directiva de WAF. Esta redirección URI debe ser un sitio válido de HTTP (S) y una vez configurado, todas las solicitudes se redirigirán las reglas de coincidencia con una acción de "REDIRIGIR" al sitio especificado.


## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre cómo definir WAF [respuestas personalizadas](waf-front-door-configure-custom-response-code.md)

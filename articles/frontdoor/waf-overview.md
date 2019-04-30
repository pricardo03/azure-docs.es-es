---
title: ¿Qué es firewall de aplicaciones web de Azure para el acceso de principal de Azure?  (versión preliminar)
description: Obtenga información sobre cómo Azure firewall de aplicaciones web para el servicio de puerta de Azure protege sus aplicaciones web frente a ataques malintencionados.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2019
ms.author: kumud;tyao
ms.openlocfilehash: 17cf6629aca6c73bc96e4cf0c172a2e87a7aafb8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61459357"
---
# <a name="what-is-azure-web-application-firewall-for-azure-front-door-preview"></a>¿Qué es firewall de aplicaciones web de Azure para el acceso de principal de Azure?  (versión preliminar)

Firewall de aplicaciones web de Azure (WAF) (versión preliminar) proporciona una protección centralizada para las aplicaciones web que se entregan globalmente mediante la puerta de Azure. Está diseñado y opera para defenderse de los servicios web frente a ataques y vulnerabilidades comunes y mantener el servicio de alta disponibilidad para los usuarios, además de ayudarle a cumplir los requisitos de cumplimiento.

> [!IMPORTANT]
> El firewall de aplicaciones web de Azure (WAF) para el acceso de principal de Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Las aplicaciones Web son cada vez más los objetivos de ataques malintencionados como denegación de ataques de scripts entre sitios, ataques de inyección SQL e inundaciones de servicio. Estos ataques malintencionados podrían provocar la pérdida de datos y la interrupción del servicio, suponen una amenaza considerable para los propietarios de aplicaciones web.

Impedir tales ataques en el código de aplicación puede ser un verdadero desafío y requerir tareas rigurosas de mantenimiento, aplicación de revisiones y supervisión en varias capas de la topología de aplicación. Un firewall de aplicaciones web centralizado facilita enormemente la administración de la seguridad y proporciona mayor protección a los administradores de la aplicación frente a amenazas o intrusiones. Además, una solución WAF puede reaccionar ante una amenaza de seguridad más rápida al aplicar revisiones a vulnerabilidades conocidas en una ubicación central, en lugar de proteger cada una de las aplicaciones web individuales.

WAF para la puerta delantera es una solución global y centralizada. Se implementa en las ubicaciones de borde de red de Azure en todo el mundo y todas las solicitudes entrantes para una aplicación web de WAF habilitado entregada por la puerta de entrada se inspeccionan en el perímetro de red. Esto permite WAF evitar ataques malintencionados cerca de los orígenes de ataque, antes de que entre la red virtual y ofrece una protección a escala global sin sacrificar el rendimiento. Una directiva de WAF se puede vincular fácilmente a cualquier perfil de la puerta delantera en su suscripción y reglas nuevas se pueden implementar en cuestión de minutos, que le permite responder rápidamente a los cambiantes patrones de amenazas.

![Firewall de aplicaciones web de Azure](./media/waf-overview/web-application-firewall-overview.png)

También se puede habilitar el WAF de Azure con Application Gateway. Para obtener más información, consulte [firewall de aplicaciones Web](../application-gateway/waf-overview.md).

## <a name="waf-policy-and-rules"></a>WAF directivas y reglas

Puede configurar una directiva de WAF y asociar la directiva a uno o más puerta delantera front-end para la protección. Una directiva de WAF consta de dos tipos de reglas de seguridad:
- reglas personalizadas que se crean por el cliente.
- conjuntos de reglas administrado que son una colección administrada de Azure configurado previamente el conjunto de reglas. Cuando ambos están presentes, reglas personalizadas se ejecutan antes de ejecutar las reglas en un conjunto de reglas administrado. Una regla está formada por una condición de coincidencia, una prioridad y una acción. Tipos de acción admitidos son: Permitir, bloque, el registro y la REDIRECCIÓN. Puede crear una directiva totalmente personalizada que cumpla los requisitos de protección de aplicaciones específicas mediante la combinación de reglas personalizadas y administradas.

Las reglas dentro de una directiva se ejecutan en un orden por prioridad donde la prioridad es un entero único que define el orden de ejecución de la regla. Valor entero más pequeño indica una prioridad más alta y los que se evalúan antes que las reglas con un valor entero superior. Una vez que se compara una regla, la acción correspondiente que se definió en la regla se aplica a la solicitud. Una vez que se procesa una coincidencia, no se procesan más reglas con prioridades inferiores.

Una aplicación web entregada por la puerta de entrada puede tener solo una directiva de WAF asociada a la vez. Sin embargo, puede tener una configuración de la puerta delantera sin las directivas de WAF asociadas con él. Si una directiva de WAF está presente, se replica a todos nuestras ubicaciones de los bordes para garantizar la coherencia de las directivas de seguridad en todo el mundo.

## <a name="waf-modes"></a>Modos de WAF

Directiva de WAF puede configurarse para ejecutarse en los dos modos siguientes:

- **Modo de detección:** Cuando se ejecuta en modo de detección, WAF no toma todas las demás acciones que no sean de monitores y registra la solicitud y las reglas de WAF coincidentes para los registros de WAF. Puede activar el registro de diagnósticos para la puerta de entrada (al usar el portal, esto puede lograrse yendo a la **diagnósticos** sección en el portal de Azure).

- **Modo de prevención:** Cuando se configura para ejecutarse en modo de prevención, WAF toma la acción especificada, si una solicitud coincide con una regla y si se encuentra una coincidencia, no se evalúa ninguna otra regla con prioridad más baja. También se registran las solicitudes coincidentes en los registros de WAF.

## <a name="waf-actions"></a>Acciones de WAF

Los clientes de WAF pueden optar por ejecutarlo desde una de las acciones cuando una solicitud coincide con las condiciones de una regla:
- **Permitir:**  Solicitud pasa a través del WAF y se reenvía al back-end. Ninguna otra reglas de prioridad inferior pueden bloquear esta solicitud.
- **Bloquear:** La solicitud está bloqueada y WAF envía una respuesta al cliente sin reenviar la solicitud al back-end.
- **Log:**  Se registra la solicitud en los registros de WAF y WAF continúa evaluar las reglas de prioridad inferior.
- **Redirigir:** WAF redirige la solicitud al URI especificado. El URI especificado es una configuración de nivel de directiva. Una vez configurado, todas las solicitudes que coinciden con el **redirigir** acción se enviará a ese URI.


## <a name="waf-rules"></a>Reglas de WAF

Una directiva de WAF puede constar de dos tipos de reglas de seguridad - reglas personalizadas, creadas por el cliente y los conjuntos de reglas administrado, administrados de Azure configurado previamente el conjunto de reglas.

### <a name="custom-authored-rules"></a>Reglas creadas por personalizadas
Puede configurar reglas personalizadas WAF como sigue:

- **Direcciones IP permitidas de lista y la lista de bloques:** Puede configurar reglas personalizadas para controlar el acceso a sus aplicaciones web basado en una lista de direcciones IP de cliente o intervalos de direcciones IP. Se admiten los tipos de direcciones IPv4 e IPv6. Esta lista puede configurarse para bloquear o permitir las solicitudes que la IP de origen coincide con una dirección IP en la lista.

- **Control de acceso basado en geográfica:** Puede configurar reglas personalizadas para controlar el acceso a sus aplicaciones web en función del código de país asociado con la dirección IP de un cliente.

- **Control de acceso basado en los parámetros HTTP:** Puede configurar reglas personalizadas basadas en la cadena que coincida con los parámetros de solicitud HTTP/HTTPS, como las cadenas de consulta, args POST, URI de solicitud, encabezado de solicitud y el cuerpo de solicitud.

- **Solicitar control de acceso basada en métodos:** Puede configurar reglas personalizadas basadas en el método de solicitud HTTP de la solicitud, como GET, PUT o HEAD.

- **Restricción del tamaño:** Puede configurar reglas personalizadas basadas en las longitudes de determinadas partes de una solicitud como cadena de consulta, Uri, o el cuerpo de la solicitud.

- **Las reglas de limitación de velocidad:** Las reglas de control de frecuencia están diseñadas para limitar el tráfico anormalmente alto procedente de cualquier dirección IP de cliente. Puede configurar un umbral del número de solicitudes web que permite desde una dirección IP de cliente durante una duración de un minuto. Esto es distinto de una regla personalizada basada en la lista Permitir o bloquear IP que permita todos o bloques de todos los de solicitud desde una dirección IP de cliente. Limitación de velocidad se puede combinar con otras condiciones de coincidencia como parámetros HTTP (S) para el control granular de velocidad de coincidencia.


### <a name="azure-managed-rule-sets"></a>Conjuntos de reglas administradas de Azure

Conjuntos de reglas administradas de Azure proporcionan una manera fácil de implementar la protección frente a un conjunto común de las amenazas de seguridad. Dado que estos conjuntos de reglas son administradas por Azure, se actualizan las reglas según sea necesario para protegerse frente a nuevas firmas de ataque. En la versión preliminar pública, administrados de Azure predeterminado conjunto de reglas incluye reglas con respecto a las siguientes categorías de amenaza:

- El scripting entre sitios
- Ataques de Java
- Inclusión de archivos local
- Ataques de inyección de PHP
- Ejecución de comando remoto
- Inclusión de archivo remoto
- Fijación de sesión
- Protección contra la inyección de código SQL

Se incrementará el número de versión de la regla predeterminada establece cuando se agregan nuevas firmas de ataque en el conjunto de reglas.
Conjunto de reglas predeterminado está habilitado de forma predeterminada en el modo de detección de las directivas de WAF. Puede deshabilitar o habilitar reglas individuales dentro de predeterminado conjunto de reglas para satisfacer las necesidades de su aplicación. También puede establecer acciones específicas (permitir, bloquear, REDIRECCIÓN/LOG) por regla. Acción predeterminada consiste en bloque. Además, pueden configurarse reglas personalizadas en la misma directiva de WAF si desea omitir cualquiera de las reglas previamente configuradas en el conjunto de reglas predeterminado.
Siempre se aplican las reglas personalizadas antes de que se evalúan las reglas en el conjunto de reglas predeterminado. Si una solicitud coincide con una regla personalizada, se aplica la acción de regla correspondiente, y la solicitud se bloquea o se pasa al back-end, sin la invocación de las reglas más personalizadas o las reglas en el conjunto de reglas predeterminado. Además, tiene la opción para quitar el conjunto de reglas predeterminado de las directivas de WAF.


## <a name="configuration"></a>Configuración
Durante la versión preliminar pública:
- Configuración e implementación de todos los tipos de reglas de WAF es compatible con las API de REST, plantillas de Azure Resource Manager y Azure PowerShell.
- Mediante el portal de Azure, puede configurar o ver solo administrados de Azure predeterminado conjunto de reglas.

## <a name="monitoring"></a>Supervisión

Supervisión de WAF en la puerta de entrada se integra con Azure Monitor para realizar un seguimiento de las alertas y supervisar fácilmente las tendencias del tráfico.

## <a name="pricing"></a>Precios

Durante la versión preliminar pública, cualquier uso de WAF para puerta delantera es gratuito y no se cobrará.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).


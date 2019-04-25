---
title: Novedades de Azure Application Gateway
description: Obtenga información acerca de las novedades de Azure Application Gateway, como, por ejemplo, las notas de la versión más recientes, los problemas conocidos, las correcciones de errores, las funcionalidades en desuso y los próximos cambios.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.date: 4/1/2019
ms.author: victorh
ms.openlocfilehash: f686c8ac53db2d128cf5bb20f252c547348e5ac7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58863107"
---
# <a name="whats-new-in-azure-application-gateway"></a>Novedades de Azure Application Gateway

Azure Application Gateway se actualiza de forma continuada. Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

- Versiones más recientes
- Problemas conocidos
- Corrección de errores
- Funciones obsoletas

## <a name="new-features"></a>Nuevas características

|Característica  |DESCRIPCIÓN  |Fecha de adición  |
|---------|---------|---------|
|CRUD y reescrituras de encabezado     |Ya puede reescribir encabezados HTTP. Vea [Tutorial: Crear una instancia de Application Gateway y reescribir los encabezados HTTP](tutorial-http-header-rewrite-powershell.md) para más información.|Diciembre de 2018|
|Configuración de WAF y lista de exclusión     |Hemos agregado más opciones para ayudarle a configurar el WAF y reducir los falsos positivos. Consulte [Listas de exclusión y límites de tamaño de solicitud del firewall de aplicaciones web](application-gateway-waf-configuration.md) para más información.|Diciembre de 2018|
|Escalado automático, redundancia de zona, versión preliminar del soporte técnico de VIP estática     |En la versión v2 de SKU hay muchas mejoras, como el escalado automático, un rendimiento mejorado, etc. Consulte [¿Qué es Azure Application Gateway?](overview.md#autoscaling-public-preview) para más información.|Septiembre de 2018|
|Purga de la conexión     |Purga de conexión le permite eliminar correctamente los miembros de los grupos de back-end. Para más información, consulte: [Purga de conexión](overview.md#connection-draining).|Septiembre de 2018|
|Páginas de error personalizadas     |Con páginas de errores personalizadas, puede crear una página de errores en el mismo formato que el resto de los sitios web. Para ello, consulte [Create Application Gateway custom error pages](custom-error.md) (Creación de páginas de errores personalizadas de Application Gateway).|Septiembre de 2018|
|Mejoras en las métricas     |Puede obtener una mejor visión del estado de Application Gateway con métricas mejoradas. Para habilitar las métricas en Application Gateway, consulte [Mantenimiento del back-end, registros de diagnóstico y métricas de Application Gateway](application-gateway-diagnostics.md).|Junio de 2018|

## <a name="known-issues"></a>Problemas conocidos

- [Problemas conocidos de la versión v2 de SKU](application-gateway-autoscaling-zone-redundant.md#known-issues-and-limitations)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Application Gateway, consulte [¿Qué es Azure Application Gateway?](overview.md)
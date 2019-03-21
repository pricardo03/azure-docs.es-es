---
title: Introducción al redireccionamiento para Azure Application Gateway
description: Aprenda sobre la funcionalidad de redirección de Azure Application Gateway
services: application-gateway
documentationcenter: na
author: amsriva
manager: jpconnock
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/19/2018
ms.author: amsriva
ms.openlocfilehash: 8e88e0e11b3ccab7cc2c68b2617df2d588680780
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58170061"
---
# <a name="application-gateway-redirect-overview"></a>Introducción a la redirección de Application Gateway

Puede usar la puerta de enlace de aplicación para redirigir el tráfico.  Tiene un mecanismo de redirección genérico que permite redirigir el tráfico recibido en un agente de escucha a otro agente de escucha o a un sitio externo. Esto simplifica la configuración de la aplicación, optimiza el uso de recursos y admite nuevos escenarios de redirección incluida la global y la basada en la ruta de acceso.

Es un escenario común de redirección para muchas aplicaciones web compatibles con HTTP automática al redireccionamiento de HTTPS para asegurarse de que todas las comunicaciones entre la aplicación y sus usuarios se realiza a través de una ruta de acceso cifrada. En el pasado, los clientes usaban técnicas como la creación de un grupo back-end dedicado cuya única finalidad era redirigir las solicitudes que recibía de HTTP a HTTPS. Con compatibilidad con la redirección de Application Gateway, puede hacerlo simplemente con agregar una nueva configuración de redirección a una regla de enrutamiento y especificar otro agente de escucha con el protocolo HTTPS como el agente de escucha de destino.

Se admiten los siguientes tipos de redirección:

- 301 Permanent Redirect
- 302 (encontrado)
- 303 ver otras
- Redirección temporal 307

La compatibilidad con la redirección de Application Gateway ofrece las siguientes funcionalidades:

-  **Redireccionamiento global**

   Redirecciona desde un agente de escucha a otro en la puerta de enlace. Permite la redirección de HTTP a HTTPS en un sitio.
- **Redireccionamiento basado en la ruta de acceso**

   Este tipo de redirección permite la redirección de HTTP a HTTPS solo en un área de un sitio específico, por ejemplo un área de carro de la compra que se indica mediante /carro/*.
- **Redirección a un sitio externo**

![redirección](./media/redirect-overview/redirect.png)

Con este cambio, los clientes tienen que crear un nuevo objeto de configuración de redireccionamiento, que especifique el agente de escucha de destino o el sitio externo al que se desea dirigir el redireccionamiento. El elemento de configuración también admite opciones para anexar la cadena de consulta y la ruta de acceso URI para la dirección URL redirigida. También puede elegir el tipo de redirección. Una vez creada esta configuración de redireccionamiento, se adjunta al agente de escucha de origen a través de una nueva regla. Cuando se usa una regla básica, la configuración de redirección se asocia a un agente de escucha de origen y es una redirección global. Cuando se utiliza una regla basada en rutas de acceso, la configuración de redireccionamiento se define en el mapa de rutas de acceso de dirección URL. Por lo tanto, solo se aplica al área específica de la ruta de acceso de un sitio.

### <a name="next-steps"></a>Pasos siguientes

[Configuración de la redirección de direcciones URL en una puerta de enlace de aplicaciones](tutorial-url-redirect-powershell.md)

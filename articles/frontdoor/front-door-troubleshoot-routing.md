---
title: 'Solución de problemas: Solucionar problemas de configuración de Azure Front Door Service | Microsoft Docs'
description: En este tutorial, aprenderá a solucionar por sí mismo algunos de los problemas comunes que pueden aparecer en Front Door.
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2018
ms.author: sharadag
ms.openlocfilehash: 7a261d65a7bd3eea150dd764c65b94ddd47466b3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58100316"
---
# <a name="troubleshooting-common-routing-issues"></a>Solución de problemas comunes de enrutamiento
En este artículo se describe cómo solucionar algunos de los problemas comunes de enrutamiento que pueden aparecer en la configuración de Azure Front Door Service. 

## <a name="hostname-not-routing-to-backend-and-returns-400-status-code"></a>El nombre de host no se enruta al back-end y devuelve el código de estado 400


### <a name="symptom"></a>Síntoma
- Ha creado una instancia de Front Door, pero una solicitud al host de front-end devuelve un código de estado HTTP 400.

  - Ha creado una asignación de DNS a partir de un dominio personalizado para el host de front-end que configuró. Sin embargo, al enviar una solicitud al nombre de host de dominio personalizado, se devuelve un código de estado HTTP 400 y no parece redirigirse a los back-ends que ha configurado.

### <a name="cause"></a>Causa
- Este síntoma puede ocurrir si no ha configurado una regla de enrutamiento para el dominio personalizado que agregó como host de front-end. Debe agregarse de manera explícita una regla de enrutamiento para ese host de front-end, incluso si ya se ha configurado una para el host de front-end en el subdominio de Front Door (*.azurefd.net) al que el dominio personalizado tiene una asignación de DNS.

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas
- Agregue una regla de enrutamiento desde el dominio personalizado para el grupo de back-end deseado.

## <a name="request-to-frontend-hostname-returns-404-status-code"></a>La solicitud al nombre de host de front-end devuelve el código de estado 404

### <a name="symptom"></a>Síntoma
- Ha creado una instancia de Front Door y configurado un host de front-end, un grupo de back-end con al menos un back-end y una regla de enrutamiento que conecta el host de front-end con el grupo de back-end. El contenido parece no estar disponible al enviar una solicitud al host de front-end configurado, ya que se devuelve un código de estado HTTP 404.

### <a name="cause"></a>Causa
Hay varias causas posibles para este síntoma:
 - El back-end no es un back-end orientado al público y no es visible para el servicio de Front Door.

- El back-end está mal configurado, lo que hace que el servicio de Front Door envíe la solicitud incorrecta (es decir, el back-end solo acepta HTTP, pero usted no ha desmarcado la opción de permitir HTTPS, por lo que Front Door está intentando reenviar solicitudes HTTPS).
- El back-end rechaza el encabezado de host que se ha reenviado con la solicitud al back-end.
- La configuración para el back-end aún no se ha implementado por completo.

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas
1. Tiempo de implementación
    - Asegúrese de haber esperado unos 10 minutos para que se implemente la configuración.

2. Compruebe la configuración de back-end
   - Navegue hasta el grupo de back-ends al que debería enrutarse la solicitud (depende de cómo haya configurado la regla de enrutamiento) y compruebe que el _tipo de host de back-end_ y el nombre de host de back-end sean correctos. Si el back-end es un host personalizado, asegúrese de haberlo escrito correctamente. 

   - Compruebe sus puertos HTTP y HTTPS. En la mayoría de los casos, 80 y 443 (respectivamente) son correctos y no será necesario realizar ningún cambio. Sin embargo, existe la posibilidad de que el back-end no esté configurado de este modo y esté escuchando en otro puerto.

     - Compruebe el _Encabezado host de back-end_ configurado para los back-ends a los que debería enrutarse el host de front-end. En la mayoría de los casos, este encabezado debería ser el mismo que el _Nombre de host de back-end_. Sin embargo, un valor incorrecto puede provocar distintos códigos de estado HTTP 4xx si el back-end espera algo diferente. Si escribe la dirección IP de su back-end, es posible que deba establecer el _Encabezado host de back-end_ en el nombre de host del backend.


3. Compruebe la configuración de la regla de enrutamiento
     - Vaya a la regla de enrutamiento que debe enrutar del nombre de host de front-end en cuestión a un grupo de back-end. Asegúrese de que los protocolos aceptados estén configurados correctamente o, si no, asegúrese de que el protocolo que usará Front Door al reenviar la solicitud esté configurado correctamente. Los _protocolos aceptados_ determinan qué solicitudes debe aceptar Front Door, y el _Protocolo de reenvío_ en la pestaña _Avanzadas_ determina qué protocolo debe usar Front Door para reenviar la solicitud al back-end.
          - Por ejemplo, si el back-end solo acepta solicitudes HTTP, las siguientes configuraciones serían válidas:
               - Los _protocolos aceptados_ son HTTP y HTTPS. El _protocolo de reenvío_ es HTTP. Confrontar solicitud no funcionará, ya que HTTPS es un protocolo permitido y, si llegó una solicitud como HTTPS, Front Door intentaría reenviarla mediante HTTPS.

               - Los _protocolos aceptados_ son HTTP. El _protocolo de reenvío_ es confrontar solicitud o HTTPS.

   - Haga clic en la pestaña _Avanzadas_ en la parte superior del panel de configuración de la regla de enrutamiento. La opción _Reescritura de dirección URL_ está deshabilitada de manera predeterminada, y solo debería usar este campo si quiere restringir el ámbito de los recursos hospedados en back-end que quiera que estén disponibles. Cuando esté deshabilitado, Front Door reenviará la misma ruta de acceso de solicitud que reciba. Es posible que este campo esté mal configurado y Front Door esté solicitando un recurso del back-end que no está disponible; por tanto, devuelve un código de estado HTTP 404.


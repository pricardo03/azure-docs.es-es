---
title: Habilitación de SSL de extremo a extremo en Azure Application Gateway
description: En este artículo se proporciona información general sobre la compatibilidad de Application Gateway con el protocolo SSL de extremo a extremo.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 10/23/2018
ms.author: amsriva
ms.openlocfilehash: fcb49f532d5dfcd340baf017bd55c69d4e81e0e6
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630689"
---
# <a name="overview-of-end-to-end-ssl-with-application-gateway"></a>Introducción a SSL de extremo a extremo con Application Gateway

Application Gateway es compatible con la terminación SSL en la puerta de enlace; después, el tráfico fluye normalmente sin cifrar a los servidores back-end. Esta característica permite a los servidores web liberarse de la costosa sobrecarga de cifrado y descifrado. Sin embargo, para algunos clientes, la comunicación sin cifrar en los servidores back-end no es una opción aceptable. Esta comunicación sin cifrar podría deberse a los requisitos de seguridad, a los requisitos de cumplimiento o a la posibilidad de que la aplicación solo acepte una conexión segura. Para tales aplicaciones, Application Gateway admite el cifrado SSL de un extremo a otro.

El protocolo SSL de un extremo a otro permite transmitir de forma segura información confidencial cifrada al back-end, al mismo tiempo que se siguen aprovechando las ventajas del equilibrio de carga de capa 7 que proporciona Application Gateway. Algunas de estas características son la afinidad de la sesión basada en las cookies, el enrutamiento basado en direcciones URL, la compatibilidad para el enrutamiento basado en sitios o la capacidad para insertar encabezados X-Forwarded-*.

Cuando se configura con el modo de comunicación de SSL de un extremo a otro, Application Gateway finaliza las sesiones SSL en la puerta de enlace y descifra el tráfico de usuario. Después, aplica las reglas configuradas para seleccionar una instancia de grupo de back-end adecuada en la que enrutar el tráfico. Posteriormente, Application Gateway inicia una nueva conexión SSL al servidor back-end y vuelve a cifrar los datos mediante el certificado de clave pública del servidor back-end antes de transmitir la solicitud al back-end. El protocolo SSL de un extremo a otro se habilita estableciendo la configuración de protocolo de **BackendHTTPSetting** en Https, que, luego, se aplica a un grupo de back-end. Cada servidor back-end del grupo de back-end con SSL de extremo a extremo habilitado debe configurarse con un certificado para permitir la comunicación segura.

![Escenario de SSL de un extremo a otro][1]

En este ejemplo, las solicitudes mediante TLS1.2 se enrutan a los servidores back-end en Pool1 con SSL de un extremo a otro.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>SSL de un extremo a otro e incorporación a listas blancas de certificados

Application Gateway solo se comunica con instancias de back-end conocidas, que tienen en la lista blanca su certificado con Application Gateway. Para habilitar la creación de listas blancas de certificados, debe cargar la clave pública de los certificados de servidor back-end en Application Gateway (no el certificado raíz). Solo se permiten conexiones en back-ends conocidos y en la lista blanca. Los back-ends restantes producen un error de puerta de enlace. Los certificados autofirmados son para fines de prueba que y no se recomiendan para cargas de trabajo de producción. Antes de poder usar estos certificados, deben estar en la lista blanca con Application Gateway, tal y como se describe en los pasos anteriores.

> [!NOTE]
> La configuración del certificado de autenticación no es necesaria para servicios de Azure de confianza, como Azure App Service.

## <a name="end-to-end-ssl-with-the-v2-sku"></a>SSL de extremo a extremo con la SKU v2

Los certificados de autenticación han dejado de usarse, y se han reemplazado por certificados raíz de confianza en la SKU v2 de Application Gateway. Funcionan de forma similar a los certificados de autenticación, aunque con algunas diferencias importantes:

- Los certificados firmados por autoridades de entidad de certificación conocidas cuyo CN coincida con el nombre de host de la configuración de back-end HTTP no requieren ningún paso adicional para que el SSL de extremo a extremo funcione. 

   Por ejemplo, si los certificados de back-end están emitidos por una entidad de certificación conocida y tienen un CN de contoso.com, y también se establece el campo de host de la configuración de HTTP de back-end en contoso.com, no se requiere ningún otro paso adicional. Puede establecer el protocolo de configuración HTTP en HTTPS y tanto el sondeo de estado como la ruta de acceso de los datos estarían habilitados para SSL. Si utiliza Azure App Service u otros servicios web de Azure como back-end, estos también serían de confianza de manera implícita y no se necesitarían pasos adicionales para el SSL de extremo a extremo.
- Si el certificado está autofirmado o lo han firmado intermediarios desconocidos, tendrá que definir un certificado raíz de confianza para habilitar SSL de extremo a extremo en la SKU v2. La instancia de Application Gateway se comunicará únicamente con back-ends cuyo certificado raíz del certificado del servidor coincida con alguno de la lista de certificados raíz de confianza en la configuración HTTP de back-end asociada con el grupo.
- Además de la coincidencia del certificado raíz, Application Gateway también valida si el valor de host especificado en la configuración de HTTP del back-end coincide con el del nombre común (CN) presentado por el certificado SSL del servidor de back-end. Cuando se intenta establecer una conexión SSL con el back-end, Application Gateway establece la extensión de indicación de nombre de servidor (SNI) en el host especificado en la configuración de HTTP de back-end.
- Si se elige la opción de **elegir el nombre de host a partir de la dirección de back-end** en lugar del campo de host en la configuración de HTTP de back-end, el encabezado SNI siempre se establece en el nombre de dominio completo del grupo del back-end y el CN del certificado SSL del servidor de back-end debe coincidir con su nombre de dominio completo. Los miembros del grupo de back-end con direcciones IP no se admiten en este escenario.
- El certificado raíz es un certificado raíz codificado en base64 a partir de los certificados de servidor de back-end.

## <a name="next-steps"></a>Pasos siguientes

Después aprender acerca de SSL de un extremo a otro, consulte [Configuración de SSL de un extremo a otro con Application Gateway mediante PowerShell](application-gateway-end-to-end-ssl-powershell.md) para crear una puerta de enlace de aplicaciones utilizando SSL de un extremo a otro.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png

---
title: Habilitación de SSL de extremo a extremo en Azure Application Gateway
description: En este artículo se proporciona información general sobre la compatibilidad de Application Gateway con el protocolo SSL de extremo a extremo.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 3/19/2019
ms.author: victorh
ms.openlocfilehash: ee901fdcae9717cc6d03d7653bcaacc0c32518e0
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66254316"
---
# <a name="overview-of-ssl-termination-and-end-to-end-ssl-with-application-gateway"></a>Información general de la terminación SSL y SSL de extremo a extremo con Application Gateway

Capa de Sockets seguros (SSL) es la tecnología de seguridad estándar para establecer un vínculo cifrado entre un servidor web y un explorador. Este vínculo garantiza que todos los datos se pasan entre el servidor web y los exploradores siguen siendo privados y cifrados. Application gateway admite ambos terminación SSL en la puerta de enlace, así como el cifrado SSL de extremo a extremo.

## <a name="ssl-termination"></a>Terminación de SSL

Application Gateway es compatible con la terminación SSL en la puerta de enlace; después, el tráfico fluye normalmente sin cifrar a los servidores back-end. Hay una serie de ventajas de la terminación SSL en application gateway:

- **Rendimiento mejorado** – el mayor rendimiento al realizar el descifrado SSL es el protocolo de enlace. Para mejorar el rendimiento, el servidor realiza el descifrado almacena en caché los identificadores de sesión SSL y administra los vales de sesión TLS. Si esto se realiza en la puerta de enlace de aplicaciones, todas las solicitudes del mismo cliente pueden usar los valores almacenados en caché. Si se realiza en los servidores back-end, cada vez que las solicitudes del cliente van a un servidor diferente que el cliente debe re‑authenticate. El uso de vales TLS puede ayudar a mitigar este problema, pero no son compatibles con todos los clientes y puede ser difíciles de configurar y administrar.
- **Mejor utilización de los servidores back-end** : procesamiento de SSL/TLS es en gran medida la CPU y se está volviendo más intensivo a medida que aumentan los tamaños de clave. Quitar este trabajo de los servidores back-end les permite centrarse en lo que son más eficaces en entrega de contenido.
- **El enrutamiento inteligente** – descifrando el tráfico, la puerta de enlace de aplicaciones tiene acceso al contenido de solicitud, como encabezados, URI y así sucesivamente y puede usar estos datos para enrutar las solicitudes.
- **Administración de certificados** – certificados solo deben ser adquirió e instaló en la puerta de enlace de la aplicación y no todos los servidores back-end. Esto ahorra tiempo y dinero.

Para configurar la terminación SSL, se requiere un certificado SSL que se agregarán a la escucha para habilitar la puerta de enlace de aplicaciones derivar una clave simétrica según la especificación del protocolo SSL. A continuación, se usa la clave simétrica para cifrar y descifrar el tráfico enviado a la puerta de enlace. El certificado SSL debe estar en formato de intercambio de información Personal (PFX). Este formato de archivo permite la exportación de la clave privada, lo que es necesario para que la puerta de enlace de aplicaciones pueda realizar el cifrado y descifrado del tráfico.

> [!NOTE] 
>
> Puerta de enlace de aplicación no proporciona ninguna capacidad para crear un nuevo certificado o enviar una solicitud de certificado a una entidad de certificación.

Para que la conexión SSL para que funcione, deberá asegurarse de que el certificado SSL cumple las condiciones siguientes:

- Que la fecha y hora actual está dentro de la "Válido desde" y "Válido para" intervalo de fechas en el certificado.
- El "nombre común" del certificado coincide con el encabezado de host en la solicitud. Por ejemplo, si el cliente realiza una solicitud a `https://www.contoso.com/`, el certificado común debe ser `www.contoso.com`.

### <a name="certificates-supported-for-ssl-termination"></a>Certificados que se admiten para la terminación SSL

Application gateway admite los siguientes tipos de certificados:

- Certificado de CA (entidad de certificación): Un certificado de CA es un certificado digital emitido por una entidad de certificación (CA)
- Certificado de validación Extendida (validación extendida): Un certificado de validación Extendida es un directrices de certificado estándar del sector. Esto a la barra del explorador localizador verde y publicar el nombre de la compañía también.
- Certificado comodín: Este certificado es compatible con cualquier número de subdominios en función de *. site.com, donde el subdominio reemplazaría el *. Sin embargo, no, es compatible con site.com, por lo que en caso de que los usuarios tienen acceso a su sitio Web sin necesidad de escribir el interlineado "www", el certificado comodín no explicará.
- Los certificados autofirmados: Los exploradores de cliente no confían en estos certificados y avisará al usuario que el certificado del servicio virtual no forma parte de una cadena de confianza. Los certificados autofirmados son buenos para pruebas o en entornos donde los administradores controlan a los clientes y pueden omitir con seguridad las alertas de seguridad del explorador. Las cargas de trabajo de producción nunca deben usar certificados autofirmados.

Para obtener más información, consulte [configure la terminación SSL con application gateway](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal).

### <a name="size-of-the-certificate"></a>Tamaño del certificado
Compruebe el [límites de Application Gateway](https://docs.microsoft.com/azure/azure-subscription-service-limits#application-gateway-limits) sección para saber la SSL máxima tamaño admitido de certificado.

## <a name="end-to-end-ssl-encryption"></a>El cifrado SSL de extremo a otro

Pueden que algunos clientes no desean comunicación sin cifrar en los servidores back-end. Esto podría deberse a los requisitos de seguridad y cumplimiento normativo o bien a la posibilidad de que la aplicación solo acepte una conexión segura. Para tales aplicaciones, Application Gateway admite el cifrado SSL de un extremo a otro.

El protocolo SSL de un extremo a otro permite transmitir de forma segura información confidencial cifrada al back-end, al mismo tiempo que se siguen aprovechando las ventajas del equilibrio de carga de capa 7 que proporciona Application Gateway. Algunas de estas características son la afinidad de la sesión basada en las cookies, el enrutamiento basado en direcciones URL, la compatibilidad para el enrutamiento basado en sitios o la capacidad para insertar encabezados X-Forwarded-*.

Cuando se configura con el modo de comunicación de SSL de un extremo a otro, Application Gateway finaliza las sesiones SSL en la puerta de enlace y descifra el tráfico de usuario. Después, aplica las reglas configuradas para seleccionar una instancia de grupo de back-end adecuada en la que enrutar el tráfico. Posteriormente, Application Gateway inicia una nueva conexión SSL al servidor back-end y vuelve a cifrar los datos mediante el certificado de clave pública del servidor back-end antes de transmitir la solicitud al back-end. Cualquier respuesta del servidor web pasa por el mismo proceso en su regreso al usuario final. SSL de extremo a otro se habilita estableciendo la configuración del protocolo [configuración HTTP de back-end](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) a HTTPS, que, a continuación, se aplica a un grupo de back-end.

La directiva SSL se aplica al tráfico de front-end y back-end. En el front-end, Application Gateway actúa como el servidor y aplica la directiva. En el back-end, Application Gateway actúa como cliente y envía la información de protocolo o cifrado como la preferencia durante el protocolo de enlace SSL.

Application gateway solo comunica con las instancias de back-end que tienen ya sea en la lista blanca su certificado con la puerta de enlace de aplicaciones o cuyos certificados están firmados por entidades de CA conocidas que el CN del certificado coincide con el nombre de host en el HTTP configuración de back-end. Estos incluyen los servicios de Azure de confianza, como Azure App service web apps y Azure API Management.

Si los certificados de los miembros del grupo de back-end no están firmados por las autoridades de entidad de certificación conocidas, cada instancia en el grupo de back-end con SSL de extremo a extremo habilitado debe configurarse con un certificado para permitir una comunicación segura. Al agregar el certificado, se garantiza que la puerta de enlace de aplicaciones solo se comunique con instancias back-end conocidas. Esto protege aún más la comunicación de un extremo a otro.

> [!NOTE] 
>
> Configuración del certificado de autenticación no es necesario para servicios de Azure de confianza, como Azure App service web apps y Azure API Management.

> [!NOTE] 
>
> El certificado agregado al **configuración HTTP de back-end** para autenticar el back-end de los servidores pueden ser el mismo que el certificado agregado a la **escucha** para la terminación de SSL en application gateway o diferentes para seguridad mejorada.

![Escenario de SSL de un extremo a otro][1]

En este ejemplo, las solicitudes mediante TLS1.2 se enrutan a los servidores back-end en Pool1 con SSL de un extremo a otro.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>SSL de un extremo a otro e incorporación a listas blancas de certificados

Application Gateway solo se comunica con instancias de back-end conocidas, que tienen en la lista blanca su certificado con Application Gateway. Para habilitar la creación de listas blancas de certificados, debe cargar la clave pública de los certificados de servidor back-end en Application Gateway (no el certificado raíz). Solo se permiten conexiones en back-ends conocidos y en la lista blanca. Los back-ends restantes producen un error de puerta de enlace. Los certificados autofirmados son para fines de prueba que y no se recomiendan para cargas de trabajo de producción. Antes de poder usar estos certificados, deben estar en la lista blanca con Application Gateway, tal y como se describe en los pasos anteriores.

> [!NOTE]
> La configuración del certificado de autenticación no es necesaria para servicios de Azure de confianza, como Azure App Service.

## <a name="end-to-end-ssl-with-the-v2-sku"></a>SSL de extremo a extremo con la SKU v2

Los certificados de autenticación han dejado de usarse, y se han reemplazado por certificados raíz de confianza en la SKU v2 de Application Gateway. Funcionan de forma similar a los certificados de autenticación, aunque con algunas diferencias importantes:

- Los certificados firmados por autoridades de entidad de certificación conocidas cuyo CN coincida con el nombre de host de la configuración de back-end HTTP no requieren ningún paso adicional para que el SSL de extremo a extremo funcione. 

   Por ejemplo, si los certificados de back-end están emitidos por una entidad de certificación conocida y tienen un CN de contoso.com, y también se establece el campo de host de la configuración de HTTP de back-end en contoso.com, no se requiere ningún otro paso adicional. Puede establecer el protocolo de configuración HTTP en HTTPS y tanto el sondeo de estado como la ruta de acceso de los datos estarían habilitados para SSL. Si usa Azure App Service u otros servicios web Azure como su back-end, estos también son implícitamente la confianza y realizar ningún paso adicional es necesario para SSL de extremo a extremo.
- Si el certificado está autofirmado o lo han firmado intermediarios desconocidos, tendrá que definir un certificado raíz de confianza para habilitar SSL de extremo a extremo en la SKU v2. La instancia de Application Gateway se comunicará únicamente con back-ends cuyo certificado raíz del certificado del servidor coincida con alguno de la lista de certificados raíz de confianza en la configuración HTTP de back-end asociada con el grupo.
- Además de la coincidencia del certificado raíz, Application Gateway también valida si el valor de host especificado en la configuración de HTTP del back-end coincide con el del nombre común (CN) presentado por el certificado SSL del servidor de back-end. Cuando se intenta establecer una conexión SSL con el back-end, Application Gateway establece la extensión de indicación de nombre de servidor (SNI) en el host especificado en la configuración de HTTP de back-end.
- Si se elige la opción de **elegir el nombre de host a partir de la dirección de back-end** en lugar del campo de host en la configuración de HTTP de back-end, el encabezado SNI siempre se establece en el nombre de dominio completo del grupo del back-end y el CN del certificado SSL del servidor de back-end debe coincidir con su nombre de dominio completo. Los miembros del grupo de back-end con direcciones IP no se admiten en este escenario.
- El certificado raíz es un certificado raíz codificado en base64 a partir de los certificados de servidor de back-end.

## <a name="next-steps"></a>Pasos siguientes

Después aprender acerca de SSL de un extremo a otro, consulte [Configuración de SSL de un extremo a otro con Application Gateway mediante PowerShell](application-gateway-end-to-end-ssl-powershell.md) para crear una puerta de enlace de aplicaciones utilizando SSL de un extremo a otro.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png

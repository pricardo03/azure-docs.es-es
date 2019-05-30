---
title: Comprender y solucionar problemas de la CORS de Proxy de aplicación de Azure AD
description: Proporciona una descripción de la CORS en Azure AD Application Proxy y cómo identificar y resolver los problemas de la CORS.
services: active-directory
author: jeevanbisht
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: 2b6adcf4231aa44a4f28d277e963efa16de8af81
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399347"
---
# <a name="understand-and-solve-azure-active-directory-application-proxy-cors-issues"></a>Comprender y solucionar problemas de Azure Active Directory Application Proxy CORS

[Los recursos entre orígenes (CORS) de uso compartido](http://www.w3.org/TR/cors/) a veces puede presentar desafíos para las aplicaciones y API que publique a través de Azure Active Directory Application Proxy. Este artículo describen problemas de la CORS de Proxy de aplicación de Azure AD y sus soluciones.

Seguridad del explorador normalmente impide que una página web realice solicitudes AJAX a otro dominio. Esta restricción se denomina *"directiva de mismo origen"* e impide que un sitio malintencionado lea los datos confidenciales desde otro sitio. Sin embargo, a veces es posible que desee permitir que otros sitios de llamada a la API web. CORS es una norma de W3C que permite a un servidor ser menos exigentes con la directiva de mismo origen y permitir que algunas solicitudes entre orígenes y rechaza otras.

## <a name="understand-and-identify-cors-issues"></a>Comprender e identificar problemas de la CORS

Dos direcciones URL tienen el mismo origen si tienen esquemas idénticos, los hosts y los puertos ([6454 RFC](https://tools.ietf.org/html/rfc6454)), como:

-   http:\//contoso.com/foo.html
-   http:\//contoso.com/bar.html

Las siguientes direcciones URL tengan diferentes orígenes que el anterior dos:

-   http:\//contoso.net - dominio diferente
-   http:\//contoso.com:9000/foo.html - puerto diferente
-   https:\//contoso.com/foo.html - esquema diferente
-   http:\//www.contoso.com/foo.html - subdominio distinto

Directiva de mismo origen impide que las aplicaciones acceso a recursos de otros orígenes, a menos que utilicen los encabezados de control de acceso correctas. Si los encabezados de CORS falta o es incorrecta, podrá realizar solicitudes entre orígenes. 

Puede identificar problemas de la CORS mediante las herramientas de depuración del explorador:

1. Inicie el explorador y vaya a la aplicación web.
1. Presione **F12** para que aparezca la consola de depuración.
1. Intente reproducir la transacción y revise el mensaje de consola. Una infracción de la CORS genera un error acerca del origen de la consola.

En la siguiente captura de pantalla, seleccionar la **Pruébelo** botón hizo un mensaje de error CORS que https:\//corswebclient-contoso.msappproxy.net no se encontró en el encabezado de Access-Control-Allow-Origin.

![Problema CORS](./media/application-proxy-understand-cors-issues/image3.png)

## <a name="cors-challenges-with-application-proxy"></a>Desafíos de la CORS con el Proxy de aplicación

El ejemplo siguiente ilustra el típico escenario CORS de Proxy de aplicación de Azure AD. Los hosts de servidor interno un **CORSWebService** controlador web API y un **CORSWebClient** que llama a **CORSWebService**. Hay una solicitud AJAX desde **CORSWebClient** a **CORSWebService**.

![Solicitud del mismo origen local](./media/application-proxy-understand-cors-issues/image1.png)

La aplicación CORSWebClient funciona cuando se hospeda de forma local, pero el produce un error al cargar o errores cuando se publica a través de Azure AD Application Proxy. Si opta por publicar las aplicaciones CORSWebClient y CORSWebService por separado como aplicaciones diferentes a través del Proxy de aplicación, las dos aplicaciones se hospedan en dominios diferentes. Una solicitud AJAX desde CORSWebClient a CORSWebService es una solicitud entre orígenes y se produce un error.

![La solicitud CORS de Proxy de aplicación](./media/application-proxy-understand-cors-issues/image2.png)

## <a name="solutions-for-application-proxy-cors-issues"></a>Soluciones para problemas de la CORS de Proxy de aplicación

Puede resolver el problema anterior de la CORS en cualquiera de las diversas maneras.

### <a name="option-1-set-up-a-custom-domain"></a>Opción 1: Configurar un dominio personalizado

Usar un Proxy de aplicación de Azure AD [dominio personalizado](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) para publicar desde el mismo origen, sin tener que realizar cambios en los orígenes de la aplicación, el código o los encabezados. 

### <a name="option-2-publish-the-parent-directory"></a>Opción 2: Publicar en el directorio principal

Publique el directorio principal de ambas aplicaciones. Esta solución funciona especialmente bien si tiene solo dos aplicaciones en el servidor web. En lugar de publicar cada aplicación por separado, puede publicar el directorio principal común, lo que resulta en el mismo origen.

Los ejemplos siguientes muestra la página de Azure AD Application Proxy para la aplicación CORSWebClient del portal.  Cuando el **dirección URL interna** está establecido en *contoso.com/CORSWebClient*, la aplicación no puede realizar solicitudes correctas realizadas al *contoso.com/CORSWebService* directorio, ya que son entre orígenes. 

![Publicar aplicación de forma individual](./media/application-proxy-understand-cors-issues/image4.png)

En su lugar, establezca el **dirección URL interna** para publicar el directorio principal, que incluye el *CORSWebClient* y *CORSWebService* directorios:

![Publicar el directorio principal](./media/application-proxy-understand-cors-issues/image5.png)

Las direcciones URL de aplicación resultante resuelvan con eficacia el problema CORS:

- https:\//corswebclient-contoso.msappproxy.net/CORSWebService
- https:\//corswebclient-contoso.msappproxy.net/CORSWebClient

### <a name="option-3-update-http-headers"></a>Opción 3: Actualizar los encabezados HTTP

Agregar un encabezado de respuesta HTTP personalizado en el servicio web para que coincida con la solicitud de origen. Para sitios Web ejecutándose en Internet Information Services (IIS), use el Administrador de IIS para modificar el encabezado:

![Agregar encabezado de respuesta personalizado en el Administrador de IIS](./media/application-proxy-understand-cors-issues/image6.png)

Esta modificación no requiere ningún cambio de código. Puede comprobarlo en los seguimientos de Fiddler:

**Registrar la adición de encabezado**\
HTTP/1.1 200 OK\
Cache-Control: no-cache\
Pragma: no-cache\
Tipo de contenido: texto/sin formato; charset = utf-8\
Expira:-1\
Variar: Encoding\ Aceptar
Servidor:  Microsoft-IIS/8.5 Microsoft-HTTPAPI/2.0\
**Acceso Access-Control-Allow-Origin: https://corswebclient-contoso.msappproxy.net** \
X-AspNet-Version: 4.0.30319\
X-Powered-By: ASP.NET\
Longitud del contenido: 17

### <a name="option-4-modify-the-app"></a>Opción 4: Modificar la aplicación

Puede cambiar la aplicación para admitir la CORS agregando el encabezado de Access-Control-Allow-Origin, con los valores adecuados. La manera de agregar el encabezado depende del lenguaje de código de la aplicación. Cambiar el código es la opción menos recomendada, porque requiere más esfuerzo.

### <a name="option-5-extend-the-lifetime-of-the-access-token"></a>Opción 5: Ampliar la vigencia del token de acceso

No se puede resolver, por ejemplo, cuando la aplicación se redirige a algunos problemas de la CORS *login.microsoftonline.com* para realizar la autenticación y expira el token de acceso. La CORS llamar, a continuación, se produce un error. Una solución alternativa para este escenario es ampliar la duración del token de acceso, para evitar que expire durante una sesión de usuario. Para obtener más información acerca de cómo hacerlo, consulte [vigencia de tokens configurables de Azure AD](../develop/active-directory-configurable-token-lifetimes.md).

## <a name="see-also"></a>Vea también
- [Tutorial: Agregar una aplicación de forma local para el acceso remoto a través del Proxy de aplicación en Azure Active Directory](application-proxy-add-on-premises-application.md) 
- [Planear una implementación de Azure AD Application Proxy](application-proxy-deployment-plan.md) 
- [Acceso remoto a aplicaciones locales a través de Azure Active Directory Application Proxy](application-proxy.md) 

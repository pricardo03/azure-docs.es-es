---
title: Depurar aplicaciones de Proxy de aplicación - Azure Active Directory | Microsoft Docs
description: Depurar problemas con aplicaciones de Azure Active Directory (Azure AD) Application Proxy.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: japere
ms.openlocfilehash: d0a12bde119e9dae3f950603fac4bce060bb5f91
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66172271"
---
# <a name="debug-application-proxy-application-issues"></a>Depurar problemas de aplicaciones de Proxy de aplicación 

En este artículo le ayudará a solucionar problemas con aplicaciones de Azure Active Directory (Azure AD) Application Proxy. Si usa el servicio de Proxy de aplicación para el acceso remoto a una aplicación web de un entorno local, pero tiene problemas para conectarse a la aplicación, use este diagrama de flujo para depurar problemas de aplicaciones. 

## <a name="before-you-begin"></a>Antes de empezar

Al solucionar problemas de Proxy de aplicación, es recomendable que comience con los conectores. En primer lugar, siga el flujo de solución de problemas en [depurar el conector de Proxy de aplicación emite](application-proxy-debug-connectors.md) para asegurarse de que los conectores del Proxy de aplicación están configurados correctamente. Si sigue teniendo problemas, vuelva a este artículo para solucionar problemas de la aplicación.  

Para obtener más información sobre el Proxy de aplicación, consulte:

- [Acceso remoto a aplicaciones locales a través del Proxy de aplicación](application-proxy.md)
- [Conectores del Proxy de aplicación](application-proxy-connectors.md)
- [Instalar y registrar un conector](application-proxy-add-on-premises-application.md)
- [Solución de problemas y mensajes de error de Proxy de aplicación](application-proxy-troubleshoot.md)

## <a name="flowchart-for-application-issues"></a>Diagrama de flujo para problemas de aplicaciones

Este diagrama de flujo le guiará a través de los pasos para depurar algunos de los problemas más comunes con la conexión a la aplicación. Para obtener más información acerca de cada paso, consulte la tabla que sigue el diagrama de flujo.

![Diagrama de flujo que muestra los pasos para depurar una aplicación](media/application-proxy-debug-apps/application-proxy-apps-debugging-flowchart.png)

|  | . | DESCRIPCIÓN | 
|---------|---------|---------|
|1 | Abra un explorador, obtener acceso a la aplicación y escriba sus credenciales | Pruebe a usar sus credenciales para iniciar la sesión en la aplicación y busque los errores relacionados con el usuario, como [no se puede tener acceso a esta aplicación corporativa](application-proxy-sign-in-bad-gateway-timeout-error.md). |
|2 | Comprobar asignación del usuario en la aplicación | Asegúrese de que su cuenta de usuario tiene permiso para acceder a la aplicación desde dentro de la red corporativa y, a continuación, pruebe a iniciar sesión la aplicación siguiendo los pasos descritos en [probar la aplicación](application-proxy-add-on-premises-application.md#test-the-application). Si persisten los problemas de inicio de sesión, vea [cómo solucionar errores de inicio de sesión](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-troubleshoot-sign-in-errors).  |
|3 | Abra un explorador e intenta acceder a la aplicación | Si se produce un error inmediatamente, compruebe que el Proxy de aplicación está correctamente configurado. Para obtener más información acerca de los mensajes de error específico, consulte [problemas de solución de problemas de Proxy de aplicación y mensajes de error](application-proxy-troubleshoot.md).  |
|4 | Compruebe la configuración de dominio personalizado o solucionar el error | Si la página no se muestra, asegúrese de que el dominio personalizado está configurado correctamente revisando [trabajar con dominios personalizados](application-proxy-configure-custom-domain.md).<br></br>Si no se carga la página y aparece un mensaje de error, solucione el error consultando [problemas de solución de problemas de Proxy de aplicación y mensajes de error](application-proxy-troubleshoot.md). <br></br>Si tarda más de 20 segundos para que aparezca un mensaje de error, podría haber un problema de conectividad. Vaya a la [los conectores del Proxy de aplicación depurar](application-proxy-debug-connectors.md) artículo de solución de problemas.  |
|5 | Si los problemas persisten, vaya a la depuración de conector | Podría haber un problema de conectividad entre el proxy y el conector o entre el conector y el back-end. Vaya a la [los conectores del Proxy de aplicación depurar](application-proxy-debug-connectors.md) artículo de solución de problemas. |
|6 | Publicar todos los recursos, compruebe las herramientas para desarrolladores del explorador y reparación de vínculos | Asegúrese de que la ruta de acceso de publicación incluye todas las imágenes necesarias, scripts y hojas de estilos para la aplicación. Para obtener más información, consulte [agregar una aplicación local a Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). <br></br>Usar herramientas de desarrollo (herramientas de F12 de Internet Explorer o Microsoft Edge) y la comprobación del explorador para publicar problemas, como se describe en [página de la aplicación no se muestra correctamente](application-proxy-page-appearance-broken-problem.md). <br></br>Revise las opciones para resolver los vínculos rotos en [no funcionan los vínculos en la página](application-proxy-page-links-broken-problem.md). |
|7 | Comprobación de latencia de red | Si la página se cargue lentamente, obtenga información sobre cómo minimizar la latencia de red en [consideraciones para reducir la latencia](application-proxy-network-topology.md#considerations-for-reducing-latency). | 
|8 | Consulte la Ayuda de solución de problemas adicional | Si los problemas persisten, buscar artículos de solución de problemas adicionales en el [documentación de solución de problemas de Proxy de aplicación](application-proxy-page-appearance-broken-problem.md). |

## <a name="next-steps"></a>Pasos siguientes


* [Publicación de aplicaciones en redes independientes y ubicaciones mediante grupos de conectores](application-proxy-connector-groups.md)
* [Trabajo con servidores proxy locales existentes](application-proxy-configure-connectors-with-proxy-servers.md)
* [Solución de problemas y mensajes de error de Proxy de aplicación](application-proxy-troubleshoot.md)
* [Cómo instalar de forma silenciosa el conector de Proxy de aplicación de Azure AD](application-proxy-register-connector-powershell.md)

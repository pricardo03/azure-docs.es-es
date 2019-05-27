---
title: Depurar los conectores del Proxy de aplicación - Azure Active Directory | Microsoft Docs
description: Depurar problemas con los conectores del Proxy de aplicación de Azure Active Directory (Azure AD).
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
ms.openlocfilehash: c3088ae777fe1a64be218105d36fdb9e01d7b798
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66172241"
---
# <a name="debug-application-proxy-connector-issues"></a>Depurar los problemas del conector de Proxy de aplicación 

En este artículo le ayudará a solucionar problemas con los conectores del Proxy de aplicación de Azure Active Directory (Azure AD). Si usa el servicio de Proxy de aplicación para el acceso remoto a una aplicación web de un entorno local, pero tiene problemas para conectarse a la aplicación, use este diagrama de flujo para depurar problemas del conector. 

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se da por supuesto que ha instalado el conector del Proxy de aplicación y está teniendo un problema. Al solucionar problemas de Proxy de aplicación, es recomendable que comience con este flujo de solución de problemas para determinar si los conectores del Proxy de aplicación están configurados correctamente. Si sigue teniendo problemas para conectarse a la aplicación, siga el flujo de solución de problemas en [problemas de aplicaciones de Proxy de aplicación depurar](application-proxy-debug-apps.md).  


Para obtener más información sobre cómo Proxy de aplicación y usar sus conectores, consulte:

- [Acceso remoto a aplicaciones locales a través del Proxy de aplicación](application-proxy.md)
- [Conectores del Proxy de aplicación](application-proxy-connectors.md)
- [Instalar y registrar un conector](application-proxy-add-on-premises-application.md)
- [Solución de problemas y mensajes de error de Proxy de aplicación](application-proxy-troubleshoot.md)

## <a name="flowchart-for-connector-issues"></a>Diagrama de flujo para los problemas del conector

Este diagrama de flujo le guiará a través de los pasos para depurar algunos de los problemas más comunes de conector. Para obtener más información acerca de cada paso, consulte la tabla que sigue el diagrama de flujo.

![Diagrama de flujo que muestra los pasos para depurar un conector](media/application-proxy-debug-connectors/application-proxy-connector-debugging-flowchart.png)

|  | . | DESCRIPCIÓN | 
|---------|---------|---------|
|1 | Busque el grupo de conectores asignado a la aplicación | Es probable que tenga un conector instalado en varios servidores, en cuyo caso los conectores deben ser [asignados a grupos de conectores](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups). Para más información acerca de los grupos de conectores, consulte [Publicación de aplicaciones en redes independientes y ubicaciones mediante grupos de conectores](application-proxy-connector-groups.md). |
|2 | Instalar el conector y asignar un grupo | Si no tiene instalado un conector, consulte [instalar y registrar un conector](application-proxy-add-on-premises-application.md#install-and-register-a-connector).<br></br>Si el conector no está asignado a un grupo, consulte [asignar el conector a un grupo](application-proxy-connector-groups.md#create-connector-groups).<br></br>Si la aplicación no está asignada a un grupo de conectores, consulte [asignar la aplicación a un grupo de conectores](application-proxy-connector-groups.md#assign-applications-to-your-connector-groups).|
|3 | Ejecutar una prueba de puerto en el servidor de conector | En el servidor de conector, ejecute una prueba de puerto mediante el uso de [telnet](https://docs.microsoft.com/windows-server/administration/windows-commands/telnet) o herramienta de otras pruebas de puerto para comprobar si los puertos 80 y 443 estén abiertos.|
|4 | Configurar los puertos y dominios | [Asegúrese de que están configurados correctamente los puertos y dominios](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment) para que el conector funcione correctamente, hay determinados puertos que deben estar abiertos y direcciones URL que debe tener acceso a su servidor. |
|5 | Comprobar si un proxy de back-end está en uso | Compruebe si los conectores son usa servidores proxy de back-end o la omisión de ellos. Para obtener más información, consulte [solucionar problemas del proxy del conector y problemas de conectividad del servicio](application-proxy-configure-connectors-with-proxy-servers.md#troubleshoot-connector-proxy-problems-and-service-connectivity-issues). |
|6 | Actualizar el conector y actualizador para usar al proxy de back-end | Si un proxy de back-end está en uso, desea asegurarse de que el conector está usando al mismo proxy. Para obtener más información acerca de la solución de problemas y configuración de conectores para trabajar con servidores proxy, consulte [trabajar con los existentes en el entorno local los servidores proxy](application-proxy-configure-connectors-with-proxy-servers.md). |
|7 | Carga la dirección URL interna de la aplicación en el servidor de conector | En el servidor de conector, cargar la dirección URL interna de la aplicación. |
|8 | Compruebe la conectividad de red interna | Hay un problema de conectividad en la red interna que no puede diagnosticar este flujo de depuración. La aplicación debe ser accesible internamente para los conectores para que funcione. Puede habilitar y ver los registros de eventos de conector, como se describe en [los conectores del Proxy de aplicación](application-proxy-connectors.md#under-the-hood). |
|9 | Aumentar el valor de tiempo de espera en el back-end | En el **configuración adicional** para la aplicación, cambie la **el tiempo de espera de back-end de aplicación** si se establece en **largo**. Consulte [agregar una aplicación local a Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad). |
|10 | Si los problemas persisten, orientadas a problemas específicos de flujo, aplicación de revisión y depuración de flujos de inicio de sesión único | Use la [problemas de aplicaciones de Proxy de aplicación depurar](application-proxy-debug-apps.md) flujo de solución de problemas. |

## <a name="next-steps"></a>Pasos siguientes


* [Publicación de aplicaciones en redes independientes y ubicaciones mediante grupos de conectores](application-proxy-connector-groups.md)
* [Trabajo con servidores proxy locales existentes](application-proxy-configure-connectors-with-proxy-servers.md)
* [Solución de problemas y mensajes de error de Proxy de aplicación](application-proxy-troubleshoot.md)
* [Cómo instalar de forma silenciosa el conector de Proxy de aplicación de Azure AD](application-proxy-register-connector-powershell.md)

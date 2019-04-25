---
title: ¿Qué es el agente Azure AD Connect Admin - Azure AD Connect | Microsoft Docs
description: Describe las herramientas utilizadas para sincronizar y supervisar el entorno local con Azure AD.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/03/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e33143626e136523b4af086e841b92e9ad30fa86
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60295027"
---
# <a name="what-is-the-azure-ad-connect-admin-agent"></a>¿Qué es el agente administrador de Azure AD Connect? 
El agente Azure AD Connect administración es un nuevo componente de Azure Active Directory Connect que se instala en un servidor de Azure Active Directory Connect. Sirve para recopilar datos específicos de su entorno de Active Directory que ayuda a Microsoft ingeniero para solucionar problemas al abrir una incidencia de soporte técnico.

Cuando se instala, el agente de Azure AD Connect administración espera solicitudes específicas para datos de Azure Active Directory, obtiene los datos solicitados desde el entorno de sincronización y lo envía a Azure Active Directory, donde se presentará a Microsoft la compatibilidad ingeniero.

No se almacena la información que el agente Azure AD Connect administración recupera desde el entorno de cualquier manera, solo se muestra para el ingeniero de soporte técnico de Microsoft para ayudarles a investigar y solucionar problemas de Azure Active Directory Connect caso de soporte relacionada que abrió

## <a name="how-is-the-azure-ad-connect-admin-agent-installed-on-the-azure-ad-connect-server"></a>¿Cómo se instala el agente Azure AD Connect administrador en el servidor de Azure AD Connect? 
Después de instalar el agente de administración, verá los siguientes dos programas nuevo en la lista de "Agregar o quitar programas" en el Panel de Control del servidor: 

![Agente de administración](media/whatis-aadc-admin-agent/adminagent1.png)

No quite ni cancelar instalar los programas, ya que son una parte fundamental de la instalación de Azure AD Connect.

## <a name="what-data-in-my-sync-service-is-shown-to-the-microsoft-service-engineer"></a>¿En mi servicio de sincronización de los datos que se muestran al ingeniero de servicio de Microsoft?
Al abrir un caso de soporte técnico, el ingeniero de soporte técnico de Microsoft puede ver, para un usuario determinado, los datos pertinentes en Active Directory, el espacio del conector de Active Directory en el servidor de Azure Active Directory Connect, el espacio del conector de Azure Active Directory en Azure Servidor de Active Directory Connect y el metaverso en el servidor de Azure Active Directory Connect.

El ingeniero de soporte técnico de Microsoft no se puede cambiar los datos en el sistema y no puede ver las contraseñas.

## <a name="what-if-i-dont-want-the-microsoft-support-engineer-to-access-my-data"></a>¿Qué ocurre si quiero que el ingeniero de soporte técnico de Microsoft para obtener acceso a Mis datos? 
 
Si no desea que el ingeniero de servicios de Microsoft para obtener acceso a los datos de una llamada de soporte técnico puede deshabilitar esto modificando el archivo de configuración de servicio, como se describe a continuación: 

1.  Abra **C:\Program Files\Microsoft Azure AD Connect administración Agent\AzureADConnectAdministrationAgentService.exe.config** en el Bloc de notas.
2.  Deshabilitar **UserDataEnabled** tal como se muestra a continuación. Si **UserDataEnabled** configuración existe y está establecida en true y, después, establézcalo en false. Si la configuración no existe, agregue la configuración tal como se muestra a continuación.    

    ```xml
    <appSettings>
      <add key="TraceFilename" value="ADAdministrationAgent.log" />
      <add key="UserDataEnabled" value="false" />
    </appSettings>
    ```

3.  Guarde el archivo de configuración.
4.  Reinicie el servicio de agente de Azure AD Connect administración tal como se muestra a continuación

![Agente de administración](media/whatis-aadc-admin-agent/adminagent2.png)

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
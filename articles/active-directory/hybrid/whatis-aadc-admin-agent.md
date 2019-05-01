---
title: ¿Qué es el agente Azure AD Connect Admin - Azure AD Connect | Microsoft Docs
description: Describe las herramientas utilizadas para sincronizar y supervisar el entorno local con Azure AD.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36ab3fff4294b4cda3d1554ef2761d3f4acaca35
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64687250"
---
# <a name="what-is-the-azure-ad-connect-admin-agent"></a>¿Qué es el agente administrador de Azure AD Connect? 
El agente Azure AD Connect administración es un nuevo componente de Azure Active Directory Connect que se puede instalar en un servidor de Azure Active Directory Connect. Sirve para recopilar datos específicos de su entorno de Active Directory que ayuda a Microsoft ingeniero para solucionar problemas al abrir una incidencia de soporte técnico. 

>[!NOTE]
>El agente de administración no está instalado y habilitado de forma predeterminada.  Debe instalar al agente con el fin de recopilar datos para ayudar en casos de soporte técnico.

Cuando se instala, el agente de Azure AD Connect administración espera solicitudes específicas para obtiene los datos solicitados desde el entorno de sincronización de datos de Azure Active Directory y lo envía a Azure Active Directory, donde se presentará a Microsoft la compatibilidad ingeniero. 

No se almacena la información que el agente Azure AD Connect administración recupera desde el entorno de cualquier manera, solo se muestra para el ingeniero de soporte técnico de Microsoft para ayudarles a investigar y solucionar problemas de Azure Active Directory Connect caso de soporte relacionada que abra a administración de agente de Azure AD Connect no está instalado en el servidor de Connect de Azure AD de forma predeterminada. 

## <a name="install-the-azure-ad-connect-administration-agent-on-the-azure-ad-connect-server"></a>Instale al agente Azure AD Connect administración en el servidor de Azure AD Connect 
Los archivos binarios de agente de Azure AD Connect administración se colocan en el servidor de AAD Connect. Para instalar al agente, haga lo siguiente: 



1. Abra powershell en modo de administrador 
2. Desplácese al directorio donde está ubicado el cd "C:\Program Files\Microsoft Azure Connect\SetupFiles activo del directorio" la aplicación 
3. Ejecute la aplicación AADConnectAdminAgentSetup.exe 
 
Cuando se le solicite, escriba sus credenciales de administrador global de Azure AD. 

>[!NOTE]
>Hay un problema conocido que se le pedirá las credenciales varias veces. Este problema se solucionará en la próxima versión.

Después de instalar el agente, verá los siguientes dos programas nuevo en la lista de "Agregar o quitar programas" en el Panel de Control del servidor: 

![Agente de administración](media/whatis-aadc-admin-agent/adminagent1.png)

## <a name="what-data-in-my-sync-service-is-shown-to-the-microsoft-service-engineer"></a>¿En mi servicio de sincronización de los datos que se muestran al ingeniero de servicio de Microsoft? 
Al abrir una incidencia de soporte técnico el ingeniero de soporte técnico de Microsoft puede ver, para un usuario determinado, los datos pertinentes en Active Directory, el espacio del conector de Active Directory en el servidor de Azure Active Directory Connect, el espacio del conector de Azure Active Directory en Azure Servidor de Active Directory Connect y el metaverso en el servidor de Azure Active Directory Connect. 

El ingeniero de soporte técnico de Microsoft no se puede cambiar los datos en el sistema y no puede ver las contraseñas. 

## <a name="what-if-i-dont-want-the-microsoft-support-engineer-to-access-my-data"></a>¿Qué ocurre si quiero que el ingeniero de soporte técnico de Microsoft para obtener acceso a Mis datos? 
Una vez que el agente está instalado, si no desea que el ingeniero de servicios de Microsoft para obtener acceso a los datos de una llamada de soporte técnico, puede deshabilitar la funcionalidad modificando el archivo de configuración de servicio, como se describe a continuación: 

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

---
title: Referencia de configuración del acceso condicional de Azure Active Directory | Microsoft Docs
description: Obtenga una visión general de la configuración admitida en una directiva de acceso condicional de Azure Active Directory.
services: active-directory.
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/30/2019
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: 6c340cde488ee37d2454468356f1ceee03f74844
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302111"
---
# <a name="azure-active-directory-conditional-access-settings-reference"></a>Referencia de configuración del acceso condicional de Azure Active Directory

Puede usar el [acceso condicional de Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md) para controlar el modo en que los usuarios autorizados pueden acceder a sus recursos.   

En este artículo se proporciona información de soporte técnico para las siguientes opciones de configuración de una directiva de acceso condicional: 

- Asignaciones de aplicaciones de nube

- Condición de plataforma de dispositivos 

- Condición de aplicaciones cliente

- Requisito de aplicación cliente aprobado


Si no es la información que está buscando, deje un comentario al final de este artículo.

## <a name="cloud-apps-assignments"></a>Asignaciones de aplicaciones de nube

Con las directivas de acceso condicional, puede controlar cómo los usuarios acceden a las [aplicaciones en la nube](conditions.md#cloud-apps). Al configurar una directiva de acceso condicional, debe seleccionar al menos una aplicación en la nube. 

![Seleccione las aplicaciones de nube de la directiva](./media/technical-reference/09.png)


### <a name="microsoft-cloud-applications"></a>Aplicaciones de nube de Microsoft

Puede asignar una directiva de acceso condicional a las siguientes aplicaciones de nube de Microsoft:

- Azure Information Protection: [Más información](/azure/information-protection/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)

- Azure RemoteApp

- Azure SQL Database: [más información ](https://docs.microsoft.com/azure/sql-database/sql-database-conditional-access)

- Microsoft Dynamics 365

- Yammer de Microsoft Office 365

- Microsoft Office 365 Exchange Online

- Microsoft Office 365 SharePoint Online (incluye OneDrive para la Empresa y Project Online)

- Microsoft Power BI 

- Azure DevOps

- Equipos de Microsoft


### <a name="other-applications"></a>Otras aplicaciones 

Además de las aplicaciones de nube de Microsoft, puede asignar una directiva de acceso condicional a los siguientes tipos de aplicaciones de nube:

- Aplicaciones conectadas a Azure AD

- Aplicaciones de software como servicio (SaaS) federadas y preintegradas

- Aplicaciones que utilizan contraseña de inicio de sesión único (SSO)

- Aplicaciones de línea de negocio

- Aplicaciones que usan el proxy de aplicación de Azure AD


## <a name="device-platform-condition"></a>Condición de plataforma de dispositivos

En una directiva de acceso condicional, puede configurar la condición de la plataforma de dispositivos para asociar la directiva al sistema operativo que se está ejecutando en un cliente. El acceso condicional de Azure AD admite las siguientes plataformas de dispositivo:

- Android

- iOS

- Windows Phone

-  Windows

- macOS


![Asociar la directiva de acceso al sistema operativo del cliente](./media/technical-reference/41.png)





## <a name="client-apps-condition"></a>Condición de aplicaciones cliente 

En la directiva de acceso condicional, puede configurar la condición de las [aplicaciones cliente](conditions.md#client-apps) para asociar la directiva con la aplicación cliente que ha iniciado un intento de acceso. Si establece la condición de aplicaciones cliente, podrá conceder o bloquear el acceso cuando se realiza un intento de acceso desde estos tipos de aplicaciones cliente:

- Browser
- Aplicaciones de escritorio y aplicaciones móviles

![Control de acceso de las aplicaciones cliente](./media/technical-reference/03.png)

### <a name="supported-browsers"></a>Exploradores compatibles 

En la directiva de acceso condicional, puede seleccionar **Exploradores** como aplicación cliente.

![Controlar el acceso de los exploradores admitidos](./media/technical-reference/05.png)

Esta configuración funciona con todos los exploradores. Sin embargo, para satisfacer una directiva de dispositivo, como un requisito de dispositivo compatible, se admiten los sistemas operativos y exploradores siguientes:


| SO                     | Exploradores                            | Soporte técnico     |
| :--                    | :--                                 | :-:         |
| Windows 10             | Internet Explorer, Microsoft Edge, Chrome     | ![Comprobar][1] |
| Windows 8 / 8.1        | Internet Explorer, Chrome           | ![Comprobar][1] |
| Windows 7              | Internet Explorer, Chrome           | ![Comprobar][1] |
| iOS                    | Safari, Intune Managed Browser      | ![Comprobar][1] |
| Android                | Safari, Intune Managed Browser      | ![Comprobar][1] |
| Windows Phone          | Internet Explorer, Microsoft Edge             | ![Comprobar][1] |
| Windows Server 2016    | Internet Explorer, Microsoft Edge             | ![Comprobar][1] |
| Windows Server 2016    | Chrome                              | Próximamente |
| Windows Server 2012 R2 | Internet Explorer, Chrome           | ![Comprobar][1] |
| Windows Server 2008 R2 | Internet Explorer, Chrome           | ![Comprobar][1] |
| macOS                  | Chrome, Safari                      | ![Comprobar][1] |



#### <a name="chrome-support"></a>Compatibilidad con Chrome

Para la compatibilidad con Chrome en **Windows 10 Creators Update (versión 1703)** o posterior, instale [esta extensión](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

Para implementar automáticamente esta extensión en los exploradores de Chrome, cree la siguiente clave del Registro:

|    |    |
|--- | ---|
|Ruta de acceso | HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome\ExtensionInstallForcelist |
|NOMBRE | 1 |
|Type | REG_SZ (String) |
|Datos | ppnbnpeolgkicgegkbkbjmhlideopiji; https://clients2.google.com/service/update2/crx

Para la compatibilidad con Chrome en **Windows 8.1 y 7**, cree la siguiente clave del Registro:

|    |    |
|--- | ---|
|Ruta de acceso | HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\AutoSelectCertificateForUrls |
|NOMBRE | 1 |
|Type | REG_SZ (String) |
|Datos | {"pattern":"https://device.login.microsoftonline.com","filter":{"ISSUER":{"CN":"MS-Organization-Access"}}}|

Estos exploradores admiten la autenticación de dispositivo, lo que permite identificar y validar el dispositivo con respecto a una directiva. Se produce un error en la comprobación del dispositivo si el explorador se ejecuta en modo privado. 


### <a name="supported-mobile-applications-and-desktop-clients"></a>Clientes de escritorio y aplicaciones móviles compatibles

En la directiva de acceso condicional, puede seleccionar **Aplicaciones móviles y aplicaciones de escritorio** como aplicación cliente.


![Controlar el acceso de aplicaciones móviles o clientes de escritorio compatibles](./media/technical-reference/06.png)


Esta configuración afecta a los intentos de acceso realizados desde las siguientes aplicaciones móviles y aplicaciones de escritorio: 


|Aplicaciones cliente|Servicio de destino|Plataforma|
|---|---|---|
|Azure RemoteApp|Servicio Azure Remote App|Windows 10, Windows 8.1, Windows 7, iOS, Android y Mac OS X|
|Aplicación de Dynamics CRM|Dynamics CRM|Windows 10, Windows 8.1, iOS y Android|
|Aplicación Correo/Calendario/Contactos, Outlook 2016, Outlook 2013 |Office 365 Exchange Online|Windows 10|
|Directiva de MFA y de ubicación para las aplicaciones. No se admiten las directivas basadas en dispositivos. |Cualquier servicio de aplicaciones de Mis aplicaciones|Android e iOS|
|Microsoft Teams Services: controla todos los servicios que admiten Microsoft Teams y todas sus aplicaciones cliente: escritorio de Windows, iOS, Android, WP y cliente web|Equipos de Microsoft|Windows 10, Windows 8.1, Windows 7, iOS, Android y macOS |
|Aplicaciones de Office 2016, Office 2013, cliente de sincronización de OneDrive (consulte las [notas](https://support.office.com/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e))|Office 365 SharePoint Online|Windows 8.1, Windows 7, Windows 7|
|Aplicaciones de Office 2016, aplicaciones de Universal Office, Office 2013, cliente de sincronización de OneDrive (consulte las [notas](https://support.office.com/article/Azure-Active-Directory-conditional-access-with-the-OneDrive-sync-client-on-Windows-028d73d7-4b86-4ee0-8fb7-9a209434b04e)), está previsto que en el futuro haya compatibilidad con grupos de Office y con la aplicación SharePoint|Office 365 SharePoint Online|Windows 10|
|Office 2016 para macOS (solo Word, Excel, PowerPoint y OneNote). Soporte técnico de OneDrive para la Empresa previsto para el futuro|Office 365 SharePoint Online|Mac OS X|
|Aplicaciones móviles de Office|Office 365 SharePoint Online|Android, iOS|
|Aplicación de Yammer para Office|Yammer para Office 365|Windows 10, iOS y Android|
|Outlook 2016 (Office para macOS)|Office 365 Exchange Online|Mac OS X|
|Outlook 2016, Outlook 2013, Skype Empresarial|Office 365 Exchange Online|Windows 8.1, Windows 7, Windows 7|
|Aplicación móvil de Outlook|Office 365 Exchange Online|Android, iOS|
|Aplicación de PowerBI|Servicio de PowerBI|Windows 10, Windows 8.1, Windows 7, Android e iOS|
|Skype Empresarial|Office 365 Exchange Online|Android, IOS |
|Aplicación de Azure DevOps|Azure DevOps|Windows 10, Windows 8.1, Windows 7, iOS y Android|


## <a name="support-for-legacy-authentication"></a>Compatibilidad con la autenticación heredada

Si selecciona **Otros clientes**, puede especificar una condición que afecta a las aplicaciones que usan la autenticación básica con protocolos de correo electrónico como IMAP, MAPI, POP, SMTP y aplicaciones de Office más antiguas que no usan la autenticación moderna.  

![Otros clientes](./media/technical-reference/11.png)

Para más información, consulte [Aplicaciones cliente](conditions.md#client-apps).

## <a name="approved-client-app-requirement"></a>Requisito de aplicación cliente aprobado 

En la directiva de acceso condicional, puede requerir que un intento de acceso a las aplicaciones en la nube seleccionadas se realice desde una aplicación cliente aprobada. 

![Control de acceso de las aplicaciones cliente aprobadas](./media/technical-reference/21.png)

Esta configuración se aplica a las aplicaciones cliente siguientes:


- Explorador administrado de Microsoft Intune
- Microsoft Power BI
- Microsoft Invoicing
- Microsoft Launcher
- Microsoft Azure Information Protection
- Microsoft Excel
- Microsoft Kaizala 
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype Empresarial
- Microsoft StaffHub
- Equipos de Microsoft
- Microsoft Visio
- Microsoft Word
- Microsoft To-Do
- Microsoft Stream
- Microsoft Edge



**Comentarios:**

- Las aplicaciones cliente aprobadas admiten la característica de administración de aplicaciones móviles de Intune.

- Requisito de la opción **Solicitar aplicación cliente aprobada**:

    - Solo admite iOS y Android como [condición de plataformas de dispositivo](#device-platform-condition).


## <a name="next-steps"></a>Pasos siguientes

- Para ver una introducción al acceso condicional, consulte [Acceso condicional en Azure Active Directory](../active-directory-conditional-access-azure-portal.md).
- Si está listo para configurar las directivas de acceso condicional de su entorno, consulte [Procedimientos recomendados para el acceso condicional en Azure Active Directory](best-practices.md).



<!--Image references-->
[1]: ./media/technical-reference/01.png



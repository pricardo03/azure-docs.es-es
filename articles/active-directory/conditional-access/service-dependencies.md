---
title: ¿Cuáles son las dependencias del servicio de acceso condicional de Azure Active Directory? | Microsoft Docs
description: Obtenga información sobre cómo se usan las condiciones en el acceso condicional de Azure Active Directory para desencadenar una directiva.
services: active-directory
keywords: acceso condicional a aplicaciones, acceso condicional con Azure AD, acceso seguro a recursos de empresa, directivas de acceso condicional
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/18/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: f727fc7133ebc9ee124e63253e8a266862b0d908
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522037"
---
# <a name="what-are-service-dependencies-in-azure-active-directory-conditional-access"></a>¿Cuáles son las dependencias del servicio de acceso condicional de Azure Active Directory? 


Con las directivas de acceso condicional, puede especificar los requisitos de acceso a sitios Web y servicios. Por ejemplo, los requisitos de acceso pueden incluir exigir autenticación multifactor (MFA) o [dispositivos administrados](require-managed-devices.md). 


Cuando tiene acceso a un sitio o servicio directamente, es fácil normalmente evaluar el impacto de una directiva relacionada. Por ejemplo, si tiene una directiva que requiere MFA para SharePoint Online configurados, MFA se aplica para cada inicio de sesión en el portal web de SharePoint. Sin embargo, no siempre resulta sencillo para evaluar el impacto de una directiva porque no hay aplicaciones en la nube con dependencias en otras aplicaciones de nube. Por ejemplo, Microsoft Teams aprovecha SharePoint online. Por lo tanto, cuando tiene acceso a Microsoft Teams en nuestro escenario actual, también está sujetas a la directiva de MFA de SharePoint.   


## <a name="policy-enforcement"></a>Aplicación de directivas 

Si tiene una dependencia de servicio configurada, la directiva puede aplicarse mediante la aplicación de tiempo de compilación o en tiempo de ejecución. 

**Aplicación de directivas con enlace anticipado** significa que un usuario debe cumplir la directiva de servicios dependientes antes de acceder a la aplicación que realiza la llamada. Por ejemplo, un usuario debe cumplir la directiva de SharePoint antes de iniciar sesión en MS Teams. 

**Aplicación de directivas en tiempo de ejecución** se produce después el usuario inicie sesión en la aplicación que realiza la llamada. Cumplimiento se aplaza hasta al llamar a las solicitudes de aplicación, un token para el servicio de nivel inferior. Algunos ejemplos son MS Teams acceso a Planner y Office.com acceso a SharePoint. 

El diagrama siguiente muestra las dependencias del servicio MS Teams. Flechas sólidas indican cumplimiento enlazado la flecha discontinua para Planner Indica el cumplimiento en tiempo de ejecución. 



![Dependencias de los servicios MS Teams](./media/service-dependencies/01.png)



  

Como práctica recomendada, debe establecer directivas comunes en aplicaciones relacionadas y servicios siempre que sea posible. Tener una postura de seguridad coherente proporciona la mejor experiencia de usuario. Por ejemplo, establecer una directiva común a través de Exchange Online, SharePoint Online, MS Teams y Skype para la empresa reduce considerablemente los mensajes inesperados que pueden surgir de diferentes directivas que se va a aplicar a los servicios de nivel inferiores. 

La tabla siguiente se enumeran las dependencias del servicio adicional, donde deben cumplir las aplicaciones cliente  

| Aplicaciones cliente         | Servicio de bajada                          | Cumplimiento |
| :--                 | :--                                         | ---         | 
| Azure Data Lake     | Administración de Microsoft Azure (portal y API) | Enlace anticipado |
| Microsoft Classroom | Exchange                                    | Enlace anticipado |
|                     | SharePoint                                  | Enlace anticipado  |
| Equipos de Microsoft     | Exchange                                    | Enlace anticipado |
|                     | Planificador de MS                                  | En tiempo de ejecución  |
|                     | SharePoint                                  | Enlace anticipado |
|                     | Skype Empresarial Online                   | Enlace anticipado |
| Portal de Office       | Exchange                                    | En tiempo de ejecución  |
|                     | SharePoint                                  | En tiempo de ejecución  |
| Grupos de Outlook      | Exchange                                    | Enlace anticipado |
|                     | SharePoint                                  | Enlace anticipado |
| PowerApps           | Administración de Microsoft Azure (portal y API) | Enlace anticipado |
|                     | Microsoft Azure Active Directory              | Enlace anticipado |
| proyecto             | Dynamics CRM                                | Enlace anticipado |
| Skype Empresarial  | Exchange                                    | Enlace anticipado |
| Visual Studio       | Administración de Microsoft Azure (portal y API) | Enlace anticipado |



## <a name="next-steps"></a>Pasos siguientes

Para aprender cómo implementar el acceso condicional en un entorno, consulte [Plan de la implementación del acceso condicional a Azure Active Directory](plan-conditional-access.md).

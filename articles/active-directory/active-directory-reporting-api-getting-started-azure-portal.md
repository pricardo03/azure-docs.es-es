---
title: Introducción a la API de generación de informes de Azure AD | Microsoft Docs
description: Introducción a la API de generación informes de Azure Active Directory
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: df0b672a07575a0d26fff89c90008043d02818dd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589107"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Introducción a la API de generación informes de Azure Active Directory

Azure Active Directory proporciona una variedad de [informes](active-directory-reporting-azure-portal.md). Los datos de estos informes pueden ser muy útiles para las aplicaciones, como sistemas SIEM, auditorías y herramientas de inteligencia empresarial. 

Mediante la API de generación de informes de Azure AD, puede acceder mediante programación a los datos a través de un conjunto de API de REST. Estas API pueden llamarse desde una variedad de lenguajes de programación y herramientas.

En este artículo se proporciona un plan para acceder a los datos de informes mediante la API relacionada.

Si tiene problemas, consulte [Obtención de soporte técnico para Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).


## <a name="prerequisites"></a>requisitos previos

Para acceder a la API de generación de informes, incluso si planea el acceso a la API mediante un script, necesita:

1. Asignar roles (Lector de seguridad Reader, Administrador de seguridad y Administrador global)
2. Registro de una aplicación
3. Concesión de permisos
4. Recopilación de configuraciones


 
Para obtener instrucciones detalladas, consulte [Requisitos previos para acceder a la API de generación de informes de Azure AD](active-directory-reporting-api-prerequisites-azure-portal.md).


## <a name="recommendation"></a>Recomendación 

Si planea recuperar datos de informes sin la intervención del usuario, debería considerar el uso de la API de generación de informes de Azure AD con certificados.

Para instrucciones detalladas, consulte [Obtención de datos mediante Reporting API de Azure Active Directory con certificados](active-directory-reporting-api-with-certificates.md).


## <a name="explore"></a>Exploración

Obtenga una primera impresión de las API de generación de informes:
   
   - [Uso de los ejemplos de la API de auditoría](active-directory-reporting-api-audit-samples.md) 
 
   - [Uso de los ejemplos de la API de generación de informes de actividad de inicio de sesión](active-directory-reporting-api-sign-in-activity-samples.md)


## <a name="customize"></a>Personalizar  

Cree su propia solución: 
   
   - [Uso de la referencia de la API de auditoría](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 

   - [Uso de la referencia de la API de generación de informes de actividad de inicio de sesión](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)




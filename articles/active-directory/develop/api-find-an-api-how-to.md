---
title: Búsqueda de una API específica necesaria para una aplicación desarrollada internamente | Microsoft Docs
description: Configuración de los permisos necesarios para acceder a una API determinada en una aplicación desarrollada internamente con Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 8d0b9f219d7a0bc61e3d12acfaae6015963401f9
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44723878"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Búsqueda de una API específica necesaria para una aplicación desarrollada internamente

El acceso a API requiere configurar los ámbitos y roles de acceso. Si desea exponer su API web de aplicaciones de recursos a aplicaciones cliente, debe configurar los roles y ámbitos de acceso de la API. Si desea que una aplicación cliente pueda acceder a una API web, debe configurar los permisos de acceso a la API en el registro de la aplicación.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configuración de una aplicación de recursos para exponer las API web

Al exponer la API web, la API se muestra en la lista **Seleccionar una API** al agregar permisos al registro de una aplicación. Para agregar ámbitos de acceso, siga los pasos descritos en [Agregar ámbitos de acceso a la aplicación de recursos](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Configuración de una aplicación cliente para acceder a las API web

Al agregar permisos al registro de la aplicación, también puede **agregar acceso a API** a las API web expuestas. Para acceder a API web, siga los pasos descritos en [Para agregar credenciales o permisos para acceder a las API web](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Pasos siguientes

-   [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Descripción del manifiesto de aplicación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)



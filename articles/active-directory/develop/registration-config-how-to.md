---
title: Selección de los permisos para una determinada API | Microsoft Docs
description: Aprenda a buscar los puntos de conexión de autenticación de una aplicación personalizada que esté desarrollando o registrando con Azure AD.
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
ms.openlocfilehash: f6c2540d8f0bb49491a428b085d20067a36d970a
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44723924"
---
# <a name="how-to-select-permissions-for-a-given-api"></a>Selección de los permisos para una determinada API

Puede buscar los puntos de conexión de autenticación para su aplicación en [Azure Portal](https://portal.azure.com).

-   Acceda a [Azure Portal](https://portal.azure.com).

-   En el panel de navegación izquierdo, haga clic en **Azure Active Directory**.

-   Haga clic en **Registros de aplicaciones** y elija **Puntos de conexión**.

-   Se abrirá la página **Puntos de conexión** con una lista de todos los puntos de conexión de autenticación del inquilino.

-   Utilice el punto de conexión que corresponda al protocolo de autenticación que esté utilizando, junto con el identificador de aplicación, para diseñar la solicitud de autenticación específica de la aplicación.

## <a name="next-steps"></a>Pasos siguientes
[Guía del desarrollador de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide#authentication-and-authorization-protocols)

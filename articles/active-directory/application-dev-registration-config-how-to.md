---
title: Selección de los permisos para una determinada API | Microsoft Docs
description: Aprenda a buscar los puntos de conexión de autenticación de una aplicación personalizada que esté desarrollando o registrando con Azure AD.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: dcb3a8f735b72b2408e28d2f0dc61b2c634baf96
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366640"
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

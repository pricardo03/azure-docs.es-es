---
title: Problema al crear una aplicación de proxy de aplicación | Microsoft Docs
description: Cómo solucionar los problemas que surgen al crear aplicaciones de proxy de aplicación en el portal de administración de Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7aad73331d06828a4bfe909dbb3b98350b1d0d6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60293602"
---
# <a name="problem-creating-an-application-proxy-application"></a>Problema al crear una aplicación de proxy de aplicación 

A continuación, se muestran algunos de los problemas habituales a los que se enfrentan los usuarios cuando crean una aplicación de proxy de aplicación.

## <a name="recommended-documents"></a>Documentos recomendados 

Para más información sobre la creación de una aplicación de proxy de aplicación mediante el portal de administración, consulte [Publicación de aplicaciones mediante el proxy de aplicación de Azure AD](application-proxy-add-on-premises-application.md).

Si está siguiendo los pasos de ese documento y obtiene un error al crear la aplicación, consulte los detalles del error para obtener información y sugerencias para corregir la aplicación. La mayoría de los mensajes de error incluyen una sugerencia de corrección. 

## <a name="specific-things-to-check"></a>Aspectos específicos que comprobar

Para evitar errores habituales, compruebe que:

-   Sea un administrador con permiso para crear una aplicación de proxy de aplicación;

-   La dirección URL interna sea única;

-   La dirección URL externa sea única;

-   Las direcciones URL empiecen por http o https y terminen en "/";

-   La dirección URL debe ser un nombre de dominio y no una dirección IP.

El mensaje de error debería aparecer en la esquina superior derecha cuando cree la aplicación. También puede seleccionar el icono de notificación para ver los mensajes de error.

   ![Mensaje de notificación](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Pasos siguientes
[Habilitación del proxy de aplicación en Azure Portal](application-proxy-add-on-premises-application.md)

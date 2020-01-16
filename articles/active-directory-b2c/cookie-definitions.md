---
title: 'Definiciones de cookie: Azure Active Directory B2C | Microsoft Docs'
description: Proporciona definiciones para las cookies utilizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 66de4559ed006735f53ff993cce29370428b9998
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75930888"
---
# <a name="cookies-definitions-for-azure-active-directory-b2c"></a>Definiciones de cookie para Azure Active Directory B2C

En la tabla siguiente se enumeran las cookies utilizadas en Azure Active Directory B2C.

| Nombre | Domain | Expiration | Propósito |
| ----------- | ------ | -------------------------- | --------- |
| x-ms-cpim-admin | main.b2cadmin.ext.azure.com | Fin de [sesión del explorador](session-behavior.md) | Contiene datos de pertenencia de usuario entre inquilinos. Los inquilinos de los que es miembro un usuario y el nivel de pertenencia (administrador o usuario). |
| x-ms-cpim-slice | login.microsoftonline.com, b2clogin.com, dominio con marca | Fin de [sesión del explorador](session-behavior.md) | Se utiliza para enrutar las solicitudes a la instancia de producción adecuada. |
| x-ms-cpim-trans | login.microsoftonline.com, b2clogin.com, dominio con marca | Fin de [sesión del explorador](session-behavior.md) | Se utiliza para realizar un seguimiento de las transacciones (número de solicitudes de autenticación a Azure AD B2C) y la transacción actual. |
| x-ms-cpim-sso:{Id} | login.microsoftonline.com, b2clogin.com, dominio con marca | Fin de [sesión del explorador](session-behavior.md) | Se utiliza para mantener la sesión de SSO. |
| x-ms-cpim-cache:{id}_n | login.microsoftonline.com, b2clogin.com, dominio con marca | Fin de [sesión del explorador](session-behavior.md), autenticación correcta | Se utiliza para mantener el estado de la solicitud. |
| x-ms-cpim-csrf | login.microsoftonline.com, b2clogin.com, dominio con marca | Fin de [sesión del explorador](session-behavior.md) | Token de falsificación de solicitud entre sitios usado para la protección frente a falsificación de solicitud entre sitios. |
| x-ms-cpim-dc | login.microsoftonline.com, b2clogin.com, dominio con marca | Fin de [sesión del explorador](session-behavior.md) | Se utiliza para el enrutamiento de red de Azure AD B2C. |
| x-ms-cpim-ctx | login.microsoftonline.com, b2clogin.com, dominio con marca | Fin de [sesión del explorador](session-behavior.md) | Context |
| x-ms-cpim-rp | login.microsoftonline.com, b2clogin.com, dominio con marca | Fin de [sesión del explorador](session-behavior.md) | Se utiliza para almacenar datos de pertenencia en el inquilino del proveedor de recursos. |
| x-ms-cpim-rc | login.microsoftonline.com, b2clogin.com, dominio con marca | Fin de [sesión del explorador](session-behavior.md) | Se utiliza para almacenar la cookie de retransmisión. |

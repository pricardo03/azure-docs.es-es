---
title: Deshabilitación de la comprobación de correos electrónicos durante la suscripción de consumidores en Azure Active Directory B2C | Microsoft Docs
description: En este tema se muestra cómo deshabilitar la comprobación de correos electrónicos durante la suscripción de consumidores en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 2/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e36dd19aa020b8cb2a623cda904cf7fa8a0b26da
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004607"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Deshabilitación de la comprobación del correo electrónico durante la suscripción de consumidores en Azure Active Directory B2C 
Cuando se habilita, Azure Active Directory (Azure AD) B2B proporciona al consumidor la posibilidad de iniciar sesión en las aplicaciones ofreciendo una dirección de correo electrónico y creando una cuenta local. Azure AD B2C garantiza direcciones de correo electrónico válidas exigiendo que los consumidores puedan comprobarlas durante el proceso de suscripción. También impide que un proceso automatizado malintencionado genere cuentas falsas para aplicaciones.

Algunos desarrolladores de aplicaciones prefieren pasar por alto la comprobación de correos electrónicos durante el proceso de suscripción y, en su lugar, son los consumidores los que tienen que comprobar la dirección de correo electrónico más tarde. Para ello, Azure AD B2C puede configurarse para deshabilitar la comprobación de correo electrónico. Si lo hace, crea un proceso de inicio de sesión más eficaz y ofrece a los desarrolladores la flexibilidad para diferenciar los consumidores que han comprobado su dirección de correo electrónico de los que no lo ha hecho.

De forma predeterminada, las directivas de inicio de sesión tienen activada la comprobación de correos electrónicos. Siga estos pasos para desactivarla:

1. Haga clic en **Sign-up policies** (Directivas de registro) o **Sign-up or sign-in policies** (Directivas de inicio de sesión o registro) dependiendo de lo que haya configurado para el inicio de sesión.
2. Haga clic en la directiva (por ejemplo, "B2C_1_SiUp") para abrirla. 
3. Haga clic en **Editar** en la parte superior de la hoja.
4. Haga clic en **Page UI Customization** (Personalización de la IU de la página).
5. Haga clic en **Página de suscripción de cuenta local**.
6. Haga clic en **Dirección de correo electrónico** en la columna **Nombre** bajo la sección **Atributos de suscripción**.
7. Establezca la opción **Requerir comprobación** en **No**.
8. Haga clic en **Aceptar** en la parte inferior hasta llegar a la hoja **Editar directiva**.
9. Haga clic en **Guardar** en la parte superior de la hoja. ¡Y ya está!

> [!NOTE]
> Deshabilitar la comprobación de correos electrónicos en el proceso de registro puede provocar que se reciban correos no deseados. Si deshabilita el valor predeterminado, se recomienda agregar su propio sistema de comprobación.
> 
>
---
title: Deshabilitación de la comprobación de correos electrónicos durante la suscripción de consumidores en Azure Active Directory B2C | Microsoft Docs
description: En este tema se muestra cómo deshabilitar la comprobación de correos electrónicos durante la suscripción de consumidores en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: e139f40e9724840f3dad4dc9f0b4d5317a75ebd4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60317282"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Deshabilitación de la comprobación del correo electrónico durante la suscripción de consumidores en Azure Active Directory B2C 
Cuando se habilita, Azure Active Directory (Azure AD) B2B proporciona al consumidor la posibilidad de iniciar sesión en las aplicaciones ofreciendo una dirección de correo electrónico y creando una cuenta local. Azure AD B2C garantiza direcciones de correo electrónico válidas exigiendo que los consumidores puedan comprobarlas durante el proceso de suscripción. También impide que un proceso automatizado malintencionado genere cuentas falsas para aplicaciones.

Algunos desarrolladores de aplicaciones prefieren pasar por alto la comprobación de correos electrónicos durante el proceso de suscripción y, en su lugar, son los consumidores los que tienen que comprobar la dirección de correo electrónico más tarde. Para ello, Azure AD B2C puede configurarse para deshabilitar la comprobación de correo electrónico. Si lo hace, crea un proceso de inicio de sesión más eficaz y ofrece a los desarrolladores la flexibilidad para diferenciar los consumidores que han comprobado su dirección de correo electrónico de los que no lo ha hecho.

De forma predeterminada, los flujos de usuario de inicio de sesión tienen activada la comprobación de correos electrónicos. Siga estos pasos para desactivarla:

1. Haga clic en **Flujos de usuario**.
2. Haga clic en el flujo de usuario (por ejemplo, "B2C_1_SiUp") para abrirlo. 
3. Haga clic en **Diseños de página**.
4. Haga clic en **Página de suscripción de cuenta local**.
5. Haga clic en **Dirección de correo electrónico** en la columna **Nombre** bajo la sección **Atributos de usuario**.
6. En **Requiere verificación**, seleccione **No**.
7. Haga clic en **Guardar** en la parte superior de la hoja. ¡Y ya está!

> [!NOTE]
> Deshabilitar la comprobación de correos electrónicos en el proceso de registro puede provocar que se reciban correos no deseados. Si deshabilita el valor predeterminado, se recomienda agregar su propio sistema de comprobación.
> 
>

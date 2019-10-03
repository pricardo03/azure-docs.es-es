---
title: Deshabilitar la comprobación del correo electrónico durante la suscripción de consumidores en Azure Active Directory B2C
description: Aprenda a deshabilitar la comprobación del correo electrónico durante la suscripción de consumidores en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 9a7adc04991dd4a472bdaf1aa47aacaf6cdeb190
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256926"
---
# <a name="disable-email-verification-during-customer-sign-up-in-azure-active-directory-b2c"></a>Deshabilitar la comprobación del correo electrónico durante la suscripción de consumidores en Azure Active Directory B2C

De forma predeterminada, Azure Active Directory B2C (Azure AD B2C) comprueba la dirección de correo electrónico del cliente para las cuentas locales (cuentas de usuarios que se registran con la dirección de correo electrónico o el nombre de usuario). Azure AD B2C garantiza direcciones de correo electrónico válidas exigiendo que los consumidores puedan comprobarlas durante el proceso de suscripción. También impide que actores malintencionados usen procesos automatizados para generar cuentas fraudulentas en las aplicaciones.

Algunos desarrolladores de aplicaciones prefieren pasar por alto la comprobación de correos electrónicos durante el proceso de suscripción y, en su lugar, son los consumidores los que tienen que comprobar la dirección de correo electrónico más tarde. Para ello, Azure AD B2C puede configurarse para deshabilitar la comprobación de correo electrónico. Si se hace esto, se crea un proceso de inicio de sesión más eficaz y ofrece a los desarrolladores la flexibilidad para diferenciar aquellos consumidores que han comprobado su dirección de correo electrónico de los que no lo ha hecho.

Siga estos pasos para deshabilitar la comprobación de correo electrónico:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com)
1. Use el filtro **Directorio y suscripción** del menú superior para seleccionar el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Flujos de usuario**.
1. Seleccione el flujo de usuario para el que desea deshabilitar la comprobación de correo electrónico. Por ejemplo, *B2C_1_signinsignup*.
1. Seleccione **Diseños de página**.
1. Seleccione **Página de suscripción de cuenta local**.
1. En **Atributos de usuario**, seleccione **Dirección de correo electrónico**.
1. En el menú desplegable **REQUIERE COMPROBACIÓN**, seleccione **No**.
1. Seleccione **Guardar**. La comprobación de correo electrónico se deshabilitará en este flujo de usuario.

> [!WARNING]
> Deshabilitar la comprobación de correos electrónicos en el proceso de registro puede provocar que se reciban correos no deseados. Si deshabilita la comprobación de correo electrónico predeterminada que proporciona Azure AD B2C, es recomendable implementar un sistema de comprobación de reemplazo.

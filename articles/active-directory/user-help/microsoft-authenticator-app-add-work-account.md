---
title: Adición de una cuenta profesional o educativa a la aplicación Microsoft Authenticator de Azure Active Directory | Microsoft Docs
description: Cómo agregar su cuenta profesional o educativa a la aplicación Microsoft Authenticator para la comprobación en dos fases.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: lizross
ms.reviewer: olhaun
ms.openlocfilehash: 6d92306b7588eeb74a51b379c139e63555b32ba9
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55471545"
---
# <a name="add-your-work-or-school-account"></a>Adición de una cuenta profesional o educativa
Si su organización usa la verificación en dos pasos, puede configurar su cuenta profesional o educativa para usar la aplicación Microsoft Authenticator como uno de los métodos de verificación.

>[!Important]
>Antes de agregar su cuenta, debe descargar e instalar la aplicación Microsoft Authenticator. Si no lo ha hecho aún, siga los pasos descritos en el artículo para [descargar e instalar la aplicación](microsoft-authenticator-app-how-to.md).

## <a name="add-your-work-or-school-account"></a>Adición de una cuenta profesional o educativa

1. En el equipo, vaya a la página [Comprobación de seguridad adicional](https://aka.ms/mfasetup).

    >[!Note]
    >Si no ve la página **Comprobación de seguridad adicional**, es posible que el administrador haya activado la experiencia de información de seguridad (versión preliminar). Si es así, debe seguir las instrucciones de la sección [Configuración de la información de seguridad para usar una aplicación autenticadora](security-info-setup-auth-app.md). Si no es así, deberá ponerse en contacto con el departamento de soporte técnico de su organización para obtener ayuda. Para más información sobre la información de seguridad, consulte [Manage your security info](security-info-manage-settings.md) (Administrar la información de seguridad).

2. Active la casilla situada junto a **Aplicación Authenticator** y luego seleccione **Configurar**.

    Aparece la página **Configurar aplicación móvil**.
    
    ![Pantalla que proporciona el código QR](./media/microsoft-authenticator-app-how-to/auth-app-barcode.png)

3. Abra la aplicación Microsoft Authenticator, seleccione **Agregar cuenta** en el icono **Personalizar y controlar** en la esquina superior derecha y, a continuación, seleccione **Cuenta profesional o educativa**.

4. Use la cámara del dispositivo para detectar el código QR desde la pantalla **Configurar aplicación móvil** del equipo y, después, elija **Listo**.

    >[!Note]
    >Si la cámara no puede capturar el código QR, puede agregar manualmente la información de su cuenta a la aplicación Microsoft Authenticator para realizar la verificación en dos fases. Para obtener más información y saber cómo hacerlo, consulte [Manually add your account](microsoft-authenticator-app-add-account-manual.md) (Adición manual de la cuenta).

5. Revise la pantalla **Cuentas** de la aplicación en el dispositivo, para asegurarse de que la cuenta es correcta y de que hay un código de verificación de seis dígitos asociado. Para mayor seguridad, el código de verificación cambia cada 30 segundos para impedir que se pueda usar el mismo código varias veces.

    ![Pantalla Cuentas](./media/microsoft-authenticator-app-how-to/auth-app-accounts.png)

## <a name="next-steps"></a>Pasos siguientes

- Después de agregar las cuentas a la aplicación, puede iniciar sesión con la aplicación Authenticator en su dispositivo. Para más información, consulte cómo [iniciar sesión con la aplicación](microsoft-authenticator-app-phone-signin-faq.md).

- Para dispositivos con iOS, también puede hacer una copia de seguridad en la nube de las credenciales de su cuenta y de la configuración relacionada de la aplicación, como el orden de las cuentas. Para más información, consulte [Copia de seguridad y recuperación con la aplicación Microsoft Authenticator](microsoft-authenticator-app-backup-and-recovery.md).

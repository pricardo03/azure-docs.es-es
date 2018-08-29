---
title: Registre su dispositivo personal en la red de su organización - Azure Active Directory | Microsoft Docs
description: Aprenda a registrar su dispositivo personal en la red de su organización para que pueda tener acceso a los recursos protegidos de su organización.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: lizross
ms.reviewer: jairoc
ms.openlocfilehash: 7126a47bd90168c7d86fe9fcc05fab0a60955063
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180689"
---
# <a name="register-your-personal-device-on-your-organizations-network"></a>Registre su dispositivo personal en la red de su organización
Registre su dispositivo personal, que suele ser un teléfono o una tablet, en la red de su organización. Una vez registrado su dispositivo, podrá tener acceso a los recursos restringidos de su organización.

>[!Note]
>En este artículo se usa un dispositivo Windows con fines de demostración, pero también puede registrar dispositivos que ejecuten iOS, Android o macOS.

## <a name="what-happens-when-you-register-your-device"></a>Qué ocurre al registrar su dispositivo
Durante el registro de su dispositivo en la red de su organización, se producirán las acciones siguientes:

- Windows registra su dispositivo en la red de su organización.

- Opcionalmente, en función de las opciones de su organización, puede que se le pida configurar una verificación en dos pasos a través de [Multi-Factor Authentication](multi-factor-authentication-end-user-first-time.md) o [información de seguridad](user-help-security-info-overview.md).

- Opcionalmente, en función de las opciones de su organización, puede que se le inscriba automáticamente en la administración de dispositivos móviles, como Microsoft Intune. Para obtener más información sobre cómo inscribirse en Microsoft Intune, consulte [Enroll your device in Intune](https://docs.microsoft.com/intune-user-help/enroll-your-device-in-intune-all) (Inscribir su dispositivo en Intune).

- Seguirá el proceso de inicio de sesión, haciendo uso del nombre de usuario y la contraseña de su cuenta Microsoft personal.

## <a name="to-register-your-windows-device"></a>Para registrar su dispositivo Windows

Siga estos pasos para registrar su dispositivo personal en su red.

1. Abra **Configuración** y, a continuación, seleccione **Cuentas**.

    ![Cuentas en la pantalla Configuración](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Seleccione **Correo electrónico y cuentas** y, a continuación, seleccione **Join a Microsoft account** (Unirse a una cuenta Microsoft).

    ![Vínculos Correo electrónico y cuentas y Add a Microsoft account (Agregar una cuenta Microsoft)](./media/user-help-register-device-on-network/register-device-email-and-accounts.png)

3. En la pantalla **Add your Microsoft account** (Agregar su cuenta Microsoft), escriba su dirección de correo electrónico de su cuenta Microsoft personal.

    ![Pantalla Add your Microsoft account (Agregar su cuenta Microsoft), con correo electrónico](./media/user-help-register-device-on-network/register-device-add-accounts.png)

4. En la pantalla **Escribir contraseña**, escriba la contraseña de su cuenta Microsoft personal y, a continuación, seleccione **Iniciar sesión**.

    ![Pantalla Escribir contraseña](./media/user-help-register-device-on-network/register-device-enter-password.png)

5. Complete el resto del proceso de registro, incluidas la aprobación de su solicitud de verificación de identidad (si usa la verificación en dos pasos) y la configuración de Windows Hello (si es necesario).

## <a name="to-make-sure-youre-registered"></a>Para asegurarse de que se ha registrado
Puede asegurarse de que se ha registrado fijándose en su configuración.

1. Abra **Configuración** y, a continuación, seleccione **Cuentas**.

    ![Cuentas en la pantalla Configuración](./media/user-help-register-device-on-network/register-device-settings-accounts.png)

2. Seleccione **Correo electrónico y cuentas** y asegúrese de que ve su cuenta Microsoft personal.

    ![Pantalla Obtener acceso a trabajo o escuela con cuenta Contoso conectada](./media/user-help-register-device-on-network/register-device-verify-account.png)

## <a name="next-steps"></a>Pasos siguientes
Después de registrar su dispositivo personal en la red de su organización, debe poder tener acceso a la mayoría de sus recursos.

- Si su organización quiere que se una a su dispositivo de trabajo, consulte [Una su dispositivo de trabajo a la red de su organización](user-help-join-device-on-network.md).




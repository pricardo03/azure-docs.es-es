---
title: 'Inicio rápido: Autoservicio de restablecimiento de contraseña de Azure AD'
description: En este inicio rápido aprenderá a configurar rápidamente el autoservicio de restablecimiento de contraseña de Azure AD para que los usuarios puedan restablecer sus propias contraseñas y así reducir la carga de trabajo del departamento de TI.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: quickstart
ms.date: 12/10/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd178f52665c77f03a48d87a9e73c9019390bb21
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76154863"
---
# <a name="quickstart-configure-azure-active-directory-self-service-password-reset"></a>Inicio rápido: Configuración del autoservicio de restablecimiento de contraseña de Azure Active Directory

En este inicio rápido, se configura el autoservicio de restablecimiento de contraseña (SSPR) de Azure Active Directory (AD) para permitir a los usuarios restablecer sus contraseñas o desbloquear sus cuentas. Con SSPR, los usuarios pueden restablecer sus propias credenciales sin ayuda del administrador o del departamento de soporte técnico. Esta capacidad permite a los usuarios recuperar el acceso a su cuenta sin tener que esperar más asistencia.

> [!IMPORTANT]
> Este inicio rápido muestra a los administradores cómo habilitar el autoservicio de restablecimiento de contraseña. Los usuarios finales registrados para el restablecimiento de contraseña de autoservicio que necesiten volver a su cuenta deben ir a https://aka.ms/sspr.
>
> Si el equipo de TI no ha habilitado la capacidad para restablecer su propia contraseña, póngase en contacto con el departamento de soporte técnico para obtener ayuda adicional.

## <a name="prerequisites"></a>Prerequisites

* Un inquilino de Azure AD activo con al menos una licencia de prueba habilitada.
    * Si es preciso, [cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Una cuenta con privilegios de administrador global.
* Un usuario de prueba que no sea administrador con una contraseña que conozca, como *testuser*.
    * Si necesita crear un usuario, consulte [Inicio rápido: Incorporación de nuevos usuarios a Azure Active Directory](../add-users-azure-active-directory.md).
* Un grupo piloto con el que probar que el usuario de prueba no administrador es miembro, como *SSPR-Test-Group*.
    * Si necesita crear un grupo, consulte [Creación de un grupo y adición de miembros en Azure Active Directory](../active-directory-groups-create-azure-portal.md).

## <a name="enable-self-service-password-reset"></a>Habilitar el autoservicio de restablecimiento de contraseña

[Vea este proceso en vídeo en YouTube](https://youtu.be/Pa0eyqjEjvQ).

1. En el menú de Azure Portal o en la página **principal**, seleccione **Azure Active Directory** y, después, elija **Restablecimiento de contraseña**.

1. En la página **Propiedades**, en la opción de **Se habilitó el restablecimiento de contraseña del autoservicio.** , elija **Seleccionados**.
1. Elija **Seleccionar grupo** y, después, seleccione el grupo piloto que ha creado como parte de la sección de requisitos previos de este artículo, como *SSPR-Test-Group*. Cuando esté preparado, seleccione **Guardar**.
1. En la página **Métodos de autenticación**, elija lo siguiente y, después, elija **Guardar**:
    * Número de métodos requeridos para el restablecimiento: **1**
    * Métodos disponibles para los usuarios:
        * **Código de aplicación móvil**
        * **Correo electrónico**

    > [!div class="mx-imgBorder"]
    > ![Elección de métodos de autenticación para SSPR][Authentication]

4. En la página **Registro**, elija lo siguiente y, después, elija **Guardar**:
   * Exigir a los usuarios que se registren al iniciar sesión: **Sí**
   * Establecer el número de días que pasan hasta que se pide a los usuarios que vuelvan a confirmar su información de autenticación: **365**

## <a name="test-self-service-password-reset"></a>Autoservicio de restablecimiento de contraseña de prueba

Ahora, pruebe la configuración de SSPR con un usuario de prueba que forme parte del grupo seleccionado en la sección anterior, como *testuser*. Dado que Microsoft impone requisitos de autenticación estrictos para las cuentas de administrador de Azure, realizar la prueba con una cuenta de administrador puede cambiar el resultado. Para más información acerca de la directiva de contraseñas de administrador, consulte nuestro artículo sobre la [directiva de contraseñas](concept-sspr-policy.md).

1. Abra una nueva ventana del explorador en modo de incógnito o InPrivate, y vaya a [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup).
2. Inicie sesión con un usuario de prueba que no sea administrador, como*testuser* y registre el teléfono de autenticación.
3. Una vez que haya terminado, seleccione el botón marcado como **Parece correcto** y cierre la ventana del explorador.
4. Abra una nueva ventana del explorador en modo de incógnito o InPrivate, y vaya a [https://aka.ms/sspr](https://aka.ms/sspr).
5. Escriba el identificador de usuario de los usuarios de prueba sin privilegios de administrador, como *testuser*, los caracteres del CAPTCHA y seleccione **Siguiente**.
6. Siga los pasos de comprobación para restablecer la contraseña.

## <a name="clean-up-resources"></a>Limpieza de recursos

Para deshabilitar el autoservicio de restablecimiento de contraseña, busque **Azure Active Directory** en Azure Portal y selecciónelo. Seleccione **Restablecimiento de contraseña** y, después, elija **Ninguno** en **Se habilitó el restablecimiento de contraseña del autoservicio**. Cuando esté preparado, seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a configurar el autoservicio de restablecimiento de contraseña para los usuarios que solo están en la nube. Para averiguar cómo completar una implementación más detallada, continúe con nuestra guía de implementación.

> [!div class="nextstepaction"]
> [Implementación del autoservicio de restablecimiento de contraseña](howto-sspr-deployment.md)

[Authentication]: ./media/quickstart-sspr/sspr-authentication-methods.png "Métodos de autenticación de Azure AD disponibles y cantidad requerida"

<!-- INTERNAL LINKS -->
[register-sspr]: ../user-help/active-directory-passwords-reset-register.md
[reset-password]: ../user-help/active-directory-passwords-update-your-own-password.md

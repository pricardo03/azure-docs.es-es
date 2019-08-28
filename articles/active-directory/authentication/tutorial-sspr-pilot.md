---
title: Habilitación de un entorno piloto de SSPR de Azure AD
description: En este tutorial, habilitará el autoservicio de restablecimiento de contraseñas en Azure AD para un grupo piloto de usuarios
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 08/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 685c928a20e9bfb47365b5ffa769f1a223d2c563
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69561506"
---
# <a name="tutorial-complete-an-azure-ad-self-service-password-reset-pilot-roll-out"></a>Tutorial: Realización de una implementación piloto del autoservicio de restablecimiento de contraseñas en Azure AD

En este tutorial, habilitará una implementación piloto del autoservicio de restablecimiento de contraseñas (SSPR) de Azure AD en la organización y la probará con una cuenta sin privilegios de administrador.

Es importante que cualquier prueba del autoservicio de restablecimiento de contraseñas se realice con cuentas que no tengan privilegios de administrador. Microsoft administra la directiva de restablecimiento de contraseñas para las cuentas de administrador y requiere el uso de métodos de autenticación más seguros. Esta directiva no permite el uso de preguntas y respuestas de seguridad, y requiere estos dos métodos para el restablecimiento.

> [!div class="checklist"]
> * Habilitar el autoservicio de restablecimiento de contraseña
> * Prueba de SSPR como un usuario

## <a name="prerequisites"></a>Requisitos previos

* Configurar una cuenta de administrador global

## <a name="enable-self-service-password-reset"></a>Habilitar el autoservicio de restablecimiento de contraseña

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador global.
1. Vaya a **Azure Active Directory** y seleccione **Password Reset** (Restablecimiento de contraseñas).
1. Comience con un grupo piloto habilitando la contraseña de autoservicio para un subconjunto de usuarios de la organización.
   * En la página **Propiedades**, debajo de la opción **Se habilitó el restablecimiento de contraseña del autoservicio**, elija **Seleccionado** y escoja un grupo piloto.
      * Solo los miembros del grupo de Azure AD específico que elija podrán usar la funcionalidad SSPR. Se recomienda definir un grupo de usuarios y usarlo al implementar la funcionalidad para una prueba de concepto. Aquí se admite el anidamiento de grupos de seguridad.
      * Asegúrese de que los usuarios del grupo que escogió tengan las licencias adecuadas.
   * Haga clic en **Guardar**
1. En la página **Métodos de autenticación**
   * Establezca el valor de **Número de métodos requeridos para el restablecimiento** en **1**.
   * Elija qué **Métodos disponibles para los usuarios** desea permitir en la organización. En este tutorial, seleccione las casillas para habilitar **Correo electrónico**, **Teléfono móvil**, **Teléfono del trabajo**, **Notificación de la aplicación móvil** y  **Código de aplicación móvil**.
   * Haga clic en **Guardar**
1. En la página **Registro**
   * Seleccione **Sí** en **Exigir a los usuarios que se registren al iniciar sesión**.
   * Establezca el valor de **Número de días que pasan hasta que se pide a los usuarios que vuelvan a confirmar su información de autenticación** en **180**.
   * Haga clic en **Guardar**
1. En la página **Notificaciones**
   * Establezca la opción **Notificar a los usuarios el restablecimiento de contraseña** en **Sí**.
   * Establezca **Notificar a todos los administradores cuando otros administradores restablezcan su contraseña** en **Sí**.
1. En la página **Personalización**
   * Microsoft recomienda que establezca la opción **Personalizar el vínculo del departamento de soporte técnico** en **Sí** y proporcione la dirección de correo electrónico o la dirección URL de la página web donde los usuarios puedan obtener ayuda adicional de su organización en el campo **Custom helpdesk email or URL** (Dirección URL o correo electrónico del departamento de soporte técnico de personalizados).
   * En este tutorial, dejaremos la opción **Personalizar el vínculo del departamento de soporte técnico** establecida en **No**.

El autoservicio de restablecimiento de contraseñas está ahora configurado para los usuarios en la nube del grupo piloto.

## <a name="test-sspr-as-a-user"></a>Prueba de SSPR como un usuario

Pruebe el autoservicio de restablecimiento de contraseñas con un usuario de prueba sin privilegios de administrador que sea miembro del grupo piloto. **Si usa una cuenta que tenga roles de administrador asignados, los métodos de autenticación y su número pueden ser diferentes de los que haya seleccionado, ya que Microsoft administra la directiva del administrador.**

1. Abra una nueva ventana del explorador en modo de incógnito o InPrivate.
1. Con un usuario de prueba, regístrese en el autoservicio de restablecimiento de contraseñas mediante el portal de registro que se encuentra en [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup).
1. Con el mismo usuario de prueba, explore el portal de autoservicio de restablecimiento de contraseñas [https://aka.ms/sspr](https://aka.ms/sspr) e intente restablecer la contraseña utilizando la información proporcionada en el paso anterior.
1. Debería poder restablecer correctamente la contraseña.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si decide que ya no desea utilizar la funcionalidad que se ha configurado como parte de este tutorial, realice el siguiente cambio.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Vaya a **Azure Active Directory** y seleccione **Password Reset** (Restablecimiento de contraseñas).
1. En la página **Propiedades**, bajo la opción **Se habilitó el restablecimiento de contraseña del autoservicio**, elija **Ninguno**.
1. Haga clic en **Guardar**

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha habilitado el autoservicio de restablecimiento de contraseñas en Azure AD. Continúe con el siguiente tutorial para ver cómo se puede integrar una infraestructura de Active Directory Domain Services local en la experiencia de autoservicio de restablecimiento de contraseñas.

> [!div class="nextstepaction"]
> [Habilitación de la escritura diferida local de SSPR](tutorial-enable-writeback.md)

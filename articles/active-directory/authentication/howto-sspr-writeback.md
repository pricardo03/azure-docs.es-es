---
title: Cómo configurar la escritura diferida de contraseñas para el autoservicio de restablecimiento de contraseña de Azure AD
description: Use Azure AD y Azure AD Connect para realizar la escritura diferida de contraseñas en un directorio local.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: e613ff742096077fe1765d4b855b6c7d409cc228
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158954"
---
# <a name="how-to-configure-password-writeback"></a>Cómo configurar la escritura diferida de contraseñas

Se recomienda que use la característica de actualización automática de [Azure AD Connect](./../connect/active-directory-aadconnect-get-started-express.md) si quiere utilizar la escritura diferida de contraseñas.

En los pasos siguientes se da por supuesto que ya ha configurado Azure AD Connect en su entorno mediante la configuración [Rápida](./../connect/active-directory-aadconnect-get-started-express.md) o [Personalizada](./../connect/active-directory-aadconnect-get-started-custom.md).

1. Para configurar y habilitar la escritura diferida de contraseñas, inicie sesión en su servidor de Azure AD Connect e inicie el asistente de configuración de **Azure AD Connect**.
2. En la página **principal**, seleccione **Configurar**.
3. En la página **Tareas adicionales**, seleccione **Personalizar las opciones de sincronización** y haga clic en **Siguiente**.
4. En la página **Conectar con Azure AD**, escriba una credencial de administrador global y elija **Siguiente**.
5. En las páginas de filtrado **Conectar directorios** y **Dominio/Unidad organizativa**, seleccione **Siguiente**.
6. En la página **Características opcionales**, active la casilla junto a **Escritura diferida de contraseñas** y haga clic en **Siguiente**.
   ![Habilitar la escritura diferida de contraseñas en Azure AD Connect][Writeback]
7. En la página **Listo para configurar**, haga clic en **Configurar** y espere a que se complete el proceso.
8. Cuando finalice la configuración, seleccione **Salir**.

Para tareas de solución de problemas comunes relacionadas con la escritura diferida de contraseñas, consulte la sección [Solución de problemas con la escritura diferida de contraseñas](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) en nuestro artículo de solución de problemas.

## <a name="active-directory-permissions"></a>Permisos de Active Directory

La cuenta especificada en la utilidad de Azure AD Connect debe tener establecidos los siguientes elementos si quiere estar en el ámbito de SSPR:

* **Restablecimiento de contraseñas** 
* Haga clic en **Cambiar contraseña**. 
* **Permisos de escritura** en `lockoutTime`
* **Permisos de escritura** en `pwdLastSet`
* **Permisos extendidos** en:
   * El objeto raíz de *cada dominio* de ese bosque.
   * Las unidades organizativas (OU) de usuario que quiera que estén en el ámbito de SSPR.

Si no está seguro de cuál es la cuenta a la que se hace referencia en la descripción, abra la interfaz de usuario de la configuración de Azure Active Directory Connect y seleccione la opción **Ver la configuración actual**. La cuenta a la que necesita agregar permisos se enumera en **Directorios sincronizados**.

Si establece estos permisos, la cuenta de servicio de agente de administración de cada bosque podrá administrar las contraseñas en nombre de las cuentas de usuario de dicho bosque. 

> [!IMPORTANT]
> Si no asigna estos permisos, aunque la escritura diferida parezca estar configurada correctamente, los usuarios encontrarán errores al intentar administrar sus contraseñas locales desde la nube.
>

> [!NOTE]
> Estos permisos pueden tardar una hora (o más) en replicarse en todos los objetos del directorio.
>

Para configurar los permisos adecuados para que se realice la escritura diferida de contraseñas, complete los pasos siguientes:

1. Abra "Usuarios y equipos de Active Directory" con una cuenta que tenga los permisos de administración de dominio adecuados.
2. En el menú **Ver**, asegúrese de que la opción **Características avanzadas** está activada.
3. En el panel izquierdo, haga clic con el botón derecho en el objeto que representa la raíz del dominio y elija **Propiedades** > **Seguridad** > **Avanzado**.
4. En la pestaña **Permisos**, haga clic en **Agregar**.
5. Elija la cuenta a la que se van a aplicar los permisos (en el programa de instalación de Azure AD Connect).
6. En la lista desplegable **Se aplica a**, seleccione **Descendent User objects** (Objetos de usuario descendiente).
7. En **Permisos**, active las casillas para los siguientes elementos:
    * **Restablecimiento de contraseñas**
    * Haga clic en **Cambiar contraseña**.
    * **Escribir lockoutTime**
    * **Escribir pwdLastSet**
8. Haga clic en **Aplicar o Aceptar** para aplicar los cambios y salir de los cuadros de diálogo abiertos.

## <a name="next-steps"></a>Pasos siguientes

[¿Qué es la escritura diferida de contraseñas?](concept-sspr-writeback.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Habilitar la escritura diferida de contraseñas en Azure AD Connect"

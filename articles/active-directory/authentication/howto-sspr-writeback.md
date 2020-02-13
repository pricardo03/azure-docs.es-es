---
title: 'Configuración de la escritura diferida de contraseñas para SSPR: Azure Active Directory'
description: Use Azure AD y Azure AD Connect para realizar la escritura diferida de contraseñas en un directorio local.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1fa447312ad6a1f92eaed1164020cb6ee95606e
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77161603"
---
# <a name="how-to-configure-password-writeback"></a>Procedimiento: Configurar la escritura diferida de contraseñas

En los pasos siguientes se da por supuesto que ya ha configurado Azure AD Connect en su entorno mediante la configuración [Rápida](../hybrid/how-to-connect-install-express.md) o [Personalizada](../hybrid/how-to-connect-install-custom.md).

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

> [!WARNING]
> La escritura diferida de contraseñas dejará de funcionar para los clientes que usan versiones de Azure AD Connect 1.0.8641.0 y anteriores cuando el [servicio de Azure Access Control (ACS) se retire el 7 de noviembre de 2018](../azuread-dev/active-directory-acs-migration.md). Las versiones de Azure AD Connect 1.0.8641.0 y anteriores ya no permitirán la escritura diferida de contraseñas a partir de ese momento porque dependen de ACS para esa funcionalidad.
>
> Para evitar una interrupción en el servicio y actualizar desde una versión anterior de Azure AD Connect a una versión más reciente, consulte el artículo [Azure AD Connect: actualización de una versión anterior a la versión más reciente](../hybrid/how-to-upgrade-previous-version.md)
>

## <a name="licensing-requirements-for-password-writeback"></a>Requisitos de licencia para la escritura diferida de contraseñas

**El restablecimiento de contraseñas de autoservicio/cambio/desbloqueo con escritura diferida local es una característica premium de Azure AD**. Para más información sobre licencias, consulte la [página de precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

Para poder usar la escritura diferida de contraseñas, debe tener una de las siguientes licencias asignadas en el inquilino:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 o A3
* Enterprise Mobility + Security E5 o A5
* Microsoft 365 E3 o A3
* Microsoft 365 E5 o A5
* Microsoft 365 F1
* Microsoft 365 Empresa

> [!WARNING]
> Los planes de licencias de Office 365 independientes *no admiten "Self-Service Password Reset/Change/Unlock with on-premises writeback"* (Autoservicio de restablecimiento/modificación/desbloqueo de contraseñas con escritura en diferido local) y requieren que tenga uno de los planes anteriores para que sirva esta funcionalidad.
>

## <a name="active-directory-permissions-and-on-premises-password-complexity-policies"></a>Permisos de Active Directory y directivas de complejidad de contraseñas locales 

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
6. En la lista desplegable **Se aplica a**, seleccione **Descendent User objects** (Objetos del usuario descendientes).
7. En **Permisos**, active las casillas para las siguientes opciones:
    * Haga clic en **Cambiar contraseña**.
    * **Restablecimiento de contraseñas**
8. En **Propiedades**, active las casillas de las siguientes opciones:
    * **Escribir lockoutTime**
    * **Escribir pwdLastSet**
9. Haga clic en **Aplicar o Aceptar** para aplicar los cambios y salir de los cuadros de diálogo abiertos.

Puesto que el origen de autoridad es local, las directivas de complejidad de contraseñas se aplican desde el mismo origen de datos conectado. Asegúrese de que ha cambiado las directivas de grupo existentes para "Duración mínima de la contraseña". La directiva de grupo no se debe establecer en 1, lo que significa que la contraseña debe tener al menos un día antes de poder actualizarse. Debe asegurarse de que está establecida en 0. Esta configuración se puede encontrar en `gpmc.msc`, en **Configuración del equipo > Directivas > Configuración de Windows > Configuración de seguridad > Directivas de cuenta**. Ejecute `gpupdate /force` para asegurarse de que el cambio surta efecto. 

## <a name="next-steps"></a>Pasos siguientes

[¿Qué es la escritura diferida de contraseñas?](concept-sspr-writeback.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Habilitar la escritura diferida de contraseñas en Azure AD Connect"

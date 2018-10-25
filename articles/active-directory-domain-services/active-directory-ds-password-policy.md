---
title: 'Azure Active Directory Domain Services: directiva de contraseña | Microsoft Docs'
description: Descripción de las directivas de contraseña en dominios administrados.
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 1a14637e-b3d0-4fd9-ba7a-576b8df62ff2
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: maheshu
ms.openlocfilehash: 66db13ea2b40d32b03d57e7d039863315312d734
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043114"
---
# <a name="password-and-account-lockout-policies-on-managed-domains"></a>Directivas de bloqueo de cuenta y contraseña en dominios administrados
En este artículo se describen las directivas de contraseña predeterminadas en un dominio administrado. También se describe cómo puede configurar estas directivas.

## <a name="fine-grained-password-policies-fgpp"></a>Directivas de contraseña específicas (FGPP)
Use directivas de contraseña específicas para indicar muchas directivas de contraseña dentro de un único dominio. FGPP le permite aplicar diferentes restricciones para directivas de bloqueo de cuenta y contraseña a diferentes conjuntos de usuarios en un dominio. Por ejemplo, puede aplicar la configuración de contraseña estricta a cuentas con privilegios.

Con FGPP puede configurar las siguientes opciones de contraseña:
* Longitud mínima de contraseña
* Historial de contraseñas
* Las contraseñas deben satisfacer los requisitos de complejidad
* Antigüedad mínima de contraseña
* Antigüedad máxima de contraseña
* Directiva de bloqueo de cuentas
    * Duración de bloqueo de cuenta
    * Número de intentos de inicio de sesión con error permitidos
    * Restablecer el número de intentos de inicio de sesión con error después de


## <a name="default-fine-grained-password-policy-settings-on-a-managed-domain"></a>Configuración predeterminada de la directiva de contraseña específica en un dominio administrado
En la siguiente captura de pantalla se ilustra la directiva de contraseña específica configurada en un dominio administrado de Azure AD Domain Services.

![Directiva de contraseña específica predeterminada](./media/how-to/default-fgpp.png)

> [!NOTE]
> No se puede modificar ni eliminar la directiva de contraseña específica integrada predeterminada. Los miembros del grupo "Administradores de controladores de dominio de AAD" pueden crear una FGPP personalizada y configurarla para invalidar (tener preferencia sobre) la FGPP integrada predeterminada.
>
>

## <a name="password-policy-settings"></a>Configuración de directiva de contraseña
En un dominio administrado, las siguientes directivas de contraseña están configuradas de forma predeterminada:
* Longitud mínima de contraseña (caracteres): 7
* Antigüedad máxima de contraseña (duración): 90 días
* Las contraseñas deben satisfacer los requisitos de complejidad

### <a name="account-lockout-settings"></a>Configuración de bloqueo de cuenta
En un dominio administrado, las siguientes directivas de bloqueo de cuenta están configuradas de forma predeterminada:
* Duración del bloqueo de cuenta: 30
* Número de intentos de inicio de sesión con error permitidos: 5
* Restablecer el número de intentos de inicio de sesión con error después de: 30 minutos

De manera eficaz, las cuentas de usuario se bloquean durante 30 minutos si se usan cinco contraseñas no válidas en dos minutos. Las cuentas se desbloquean automáticamente pasados 30 minutos.


## <a name="create-a-custom-fine-grained-password-policy-fgpp-on-a-managed-domain"></a>Creación de una directiva de contraseña específica (FGPP) personalizada en un dominio administrado
Puede crear una directiva FGPP personalizada y aplicarla a grupos específicos del dominio administrado. Esta configuración invalida eficazmente la directiva FGPP predeterminada configurada para el dominio administrado.

> [!TIP]
> Solo los miembros del grupo **"Administradores de controladores de dominio de AAD"** tienen los permisos para crear directivas de contraseña específicas personalizadas.
>
>

Además, también puede crear directivas de contraseña específicas personalizadas y aplicarlas a cualquier unidad organizativa personalizada que cree en el dominio administrado.

Puede configurar una directiva FGPP personalizada por los siguientes motivos:
* Para establecer una directiva de bloqueo de cuenta diferente
* Para configurar un valor de duración de contraseña predeterminado para el dominio administrado

Para crear una directiva FGPP personalizada en el dominio administrado:
1. Inicie sesión en la máquina virtual Windows que use para administrar el dominio administrado. Si no tiene uno, siga las instrucciones para [administrar un dominio administrado](active-directory-ds-admin-guide-administer-domain.md).
2. Inicie el **Centro de administración de Active Directory** en la máquina virtual.
3. Haga clic en el nombre de dominio (por ejemplo, "contoso100.com").
4. Haga doble clic en **System** (Sistema) para abrir el contenedor del sistema.
5. Haga doble clic en **Password Settings Container** (Contenedor de configuraciones de contraseña).
6. Verá la directiva FGPP integrada predeterminada del dominio administrado llamada **AADDSSTFPSO**. No se puede modificar esta directiva FGPP integrada. Sin embargo, puede crear una directiva FGPP personalizada para invalidar la predeterminada.
7. En el panel **Tasks** (Tareas) de la derecha, haga clic en **New** (Nuevo) y seleccione **Password Settings** (Configuración de contraseña).
8. En el cuadro de diálogo **Create Password Settings** (Crear configuración de contraseña), especifique la configuración de contraseña personalizada que se aplicará como parte de la directiva FGPP personalizada. Recuerde establecer la prioridad de forma adecuada para invalidar la directiva FGPP predeterminada.

  ![Creación de formularios personalizados](./media/how-to/custom-fgpp.png)

  > [!TIP]
  > **No olvide desactivar la opción de protección frente a eliminación accidental.** Si esta opción está seleccionada, la directiva FGPP no se puede guardar.
  >
  >

9. En **Directly Applies To** (Se aplica directamente a), haga clic en el botón **Add** (Agregar). En el cuadro de diálogo **Select Users or Groups** (Seleccionar usuarios y grupos) haga clic en el botón **Locations** (Ubicaciones).

  ![Seleccionar Usuarios y grupos](./media/how-to/fgpp-applies-to.png)

10. En el cuadro de diálogo **Locations** (Ubicaciones), expanda el nombre de dominio y haga clic en **AADDC Users** (Usuarios de AADDC). Ahora puede seleccionar un grupo de la unidad organizativa de usuarios integrada al que aplicar la directiva FGPP.

  ![Seleccione la unidad organizativa al que pertenece ese grupo](./media/how-to/fgpp-container.png)

11. Escriba el nombre del grupo y haga clic en el botón **Check Names** (Comprobar nombres) para validar que existe el grupo.

  ![Selección del grupo al que aplicar la directiva FGPP](./media/how-to/fgpp-apply-group.png)

12. El nombre del grupo se muestra en la sección **Directly Applies To** (Se aplica directamente a). Haga clic en el botón **OK** (Aceptar) para guardar estos cambios.

  ![FGPP aplicada](./media/how-to/fgpp-applied.png)

> [!TIP]
> **Para aplicar directivas de contraseña personalizadas a cuentas de usuario de una unidad organizativa personalizada:** las directivas de contraseñas específicas solo pueden aplicarse a grupos. Para configurar una directiva de contraseña personalizada solo para los usuarios de una unidad organizativa personalizada, cree un grupo que incluya a los usuarios de esa unidad organizativa.
>
>

## <a name="next-steps"></a>Pasos siguientes
* [Más información sobre las directivas de contraseña específicas de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770394)
* [Configuración de directivas de contraseña específicas mediante el Centro de administración de AD](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#fine_grained_pswd_policy_mgmt)

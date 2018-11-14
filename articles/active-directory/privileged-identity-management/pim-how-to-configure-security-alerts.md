---
title: Configuración de alertas de seguridad para roles de directorio de Azure AD en PIM | Microsoft Docs
description: Aprenda a configurar alertas de seguridad para los roles de directorios de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 11/01/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: e7204c223681b9a33c439b0d9fc653167422384a
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51011704"
---
# <a name="configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Configuración de alertas de seguridad para roles de directorio de Azure AD en PIM

Azure AD Privileged Identity Management (PIM) genera alertas cuando existen actividades sospechosas o no seguras en su entorno. Cuando se desencadena una alerta, se muestra en el panel de PIM. Seleccione la alerta para ver un informe en el que se enumeren los usuarios o roles que activaron la alerta.

![Alertas de seguridad de PIM: captura de pantalla](./media/pim-how-to-configure-security-alerts/pim-directory-alerts.png)

## <a name="security-alerts"></a>Alertas de seguridad

En esta sección se enumeran todas las alertas de seguridad de roles de directorio, y cómo resolverlas y evitarlas. La gravedad tiene el significado siguiente:

* **Alta**: requiere acción inmediata debido a la infracción de una directiva.
* **Media**: no requiere acción inmediata, pero indica una posible infracción de una directiva.
* **Baja**: no requiere acción inmediata pero sugiere un cambio de directiva preferible.

### <a name="roles-are-being-assigned-outside-of-pim"></a>Los roles se asignan fuera de PIM

| | |
| --- | --- |
| **Gravedad** | Alto |
| **¿Por qué se recibe esta alerta?** | Las asignaciones de roles con privilegios realizadas fuera de PIM no se supervisan correctamente y pueden indicar un ataque activo. |
| **Solución** | Revise los usuarios de la lista y quíteles los roles con privilegios asignados fuera de PIM. |
| **Prevención** | Investigue dónde se están asignando a los usuarios roles con privilegios fuera de PIM y prohíba las asignaciones futuras a partir de ahí. |
| **Acción de mitigación en el portal** | Quítele a la cuenta su rol con privilegios. |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Posibles cuentas obsoletas en un rol con privilegios

| | |
| --- | --- |
| **Gravedad** | Mediano |
| **¿Por qué se recibe esta alerta?** | Las cuentas que no han cambiado su contraseña recientemente podrían ser cuentas de servicio o compartidas en las que no se realiza mantenimiento. Estas cuentas en roles con privilegios son vulnerables a los atacantes. |
| **Solución** | Revise las cuentas de la lista. Si ya no necesitan acceso, quíteles sus roles con privilegios. |
| **Prevención** | Asegúrese de que las cuentas que se comparten tengan contraseñas seguras que rotan cuando se produce un cambio en los usuarios que conocen la contraseña. </br>Revise con regularidad las cuentas con roles con privilegios mediante revisiones de acceso y quite las asignaciones de roles que ya no sean necesarias. |
| **Acción de mitigación en el portal** | Quítele a la cuenta su rol con privilegios. |

### <a name="users-arent-using-their-privileged-roles"></a>Los usuarios no están usando sus roles con privilegios

| | |
| --- | --- |
| **Gravedad** | Bajo |
| **¿Por qué se recibe esta alerta?** | El hecho de que los usuarios tengan asignados roles con privilegios que no necesitan aumenta la probabilidad de un ataque. También es más fácil para los atacantes permanecer desapercibidos en cuentas que no se usan activamente. |
| **Solución** | Revise los usuarios de la lista y quíteles los roles con privilegios que no necesiten. |
| **Prevención** | Asigne roles con privilegios solo a los usuarios que tengan una justificación comercial. </br>Programe revisiones de acceso periódicas para comprobar que los usuarios todavía necesitan el acceso. |
| **Acción de mitigación en el portal** | Quítele a la cuenta su rol con privilegios. |
| **Desencadenador** | Se desencadena si un usuario pasa un cierto tiempo sin activar un rol. |
| **Número de días** | Este valor especifica el número de días, de 0 a 100, que un usuario puede pasar sin activar un rol.|

### <a name="there-are-too-many-global-administrators"></a>Hay demasiados administradores globales

| | |
| --- | --- |
| **Gravedad** | Bajo |
| **¿Por qué se recibe esta alerta?** | Administrador global es el rol con privilegios más elevados. Si un administrador global se ve comprometido, el atacante obtiene acceso a todos sus permisos, lo que pone en peligro todo el sistema. |
| **Solución** | Revise los usuarios de la lista y quite todos los que no necesiten para nada el rol de administrador global. </br>Asigne a estos usuarios roles con privilegios menores. |
| **Prevención** | Asigne a los usuarios el rol con privilegios mínimos que necesiten. |
| **Acción de mitigación en el portal** | Quítele a la cuenta su rol con privilegios. |
| **Desencadenador** | Se desencadena si se cumplen dos criterios diferentes, y puede configurar ambos. En primer lugar, debe alcanzar un determinado umbral de administradores globales. En segundo lugar, un porcentaje determinado de las asignaciones de roles totales deben ser administradores globales. Si solo se cumple uno de estos criterios, la alerta no aparece. |
| **Número mínimo de administradores globales** | Esta configuración especifica el número de administradores globales, de 2 a 100, que se considera una cantidad poco segura. |
| **Porcentaje de Administradores globales** | Esta configuración especifica el porcentaje mínimo de administradores que son administradores globales, de 0 % a 100 %, que no es seguro en su entorno. |

### <a name="roles-are-being-activated-too-frequently"></a>Se activan roles con demasiada frecuencia

| | |
| --- | --- |
| **Gravedad** | Bajo |
| **¿Por qué se recibe esta alerta?** | Varias activaciones del mismo rol con privilegios por el mismo usuario es un signo de un ataque. |
| **Solución** | Revise los usuarios de la lista y asegúrese de que la [duración de la activación](pim-how-to-change-default-settings.md) de su rol con privilegios se establezca con tiempo suficiente para realizar sus tareas. |
| **Prevención** | Asegúrese de que la [duración de la activación](pim-how-to-change-default-settings.md) de roles con privilegios se establezca con el tiempo suficiente para que los usuarios realicen sus tareas.</br>[Exija MFA](pim-how-to-change-default-settings.md) para roles con privilegios que tengan cuentas compartidas por varios administradores. |
| **Acción de mitigación en el portal** | N/D |
| **Desencadenador** | Se desencadena si un usuario activa el mismo rol con privilegios varias veces dentro de un período especificado. Puede configurar el período de tiempo y el número de activaciones. |
| **Período de tiempo de renovación de activaciones** | Esta opción especifica el período de tiempo, en días, horas, minutos y segundos, que quiere usar para realizar el seguimiento de renovaciones sospechosas. |
| **Number of activation renewals** (Número de renovaciones de activación) | Esta opción especifica el número de activaciones, de 2 a 100, que merece la pena tener en cuenta, en el período de tiempo que eligió. Puede cambiar este valor moviendo el control deslizante o escribiendo un número en el cuadro de texto. |

### <a name="roles-dont-require-mfa-for-activation"></a>Los roles no requieren MFA para la activación

| | |
| --- | --- |
| **Gravedad** | Bajo |
| **¿Por qué se recibe esta alerta?** | Sin MFA, los usuarios en peligro pueden activar roles con privilegios. |
| **Solución** | Revise la lista de roles y [exija MFA](pim-how-to-change-default-settings.md) para cada rol. |
| **Prevención** | [Exija MFA](pim-how-to-change-default-settings.md) para cada rol.  |
| **Acción de mitigación en el portal** | Permite que se exija MFA para la activación del rol con privilegios. |

## <a name="configure-security-alert-settings"></a>Configuración de alertas de seguridad

Puede personalizar algunas de las alertas de seguridad de PIM para que funcionen con su entorno sus objetivos de entorno y seguridad. Siga estos pasos para abrir la configuración de alertas de seguridad:

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de Azure AD**.

1. Haga clic en **Configuración** y luego en **Alertas**.

    ![Navegación a la configuración de alertas de seguridad](./media/pim-how-to-configure-security-alerts/settings-alerts.png)

1. Haga clic en el nombre de una alerta para configurar el valor de esa alerta.

    ![Configuración de alertas de seguridad](./media/pim-how-to-configure-security-alerts/security-alert-settings.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de roles de directorio de Azure AD en PIM](pim-how-to-change-default-settings.md)

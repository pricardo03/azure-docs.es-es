---
title: Creación de un grupo dinámico y comprobación del estado en Azure Active Directory | Microsoft Docs
description: Cómo crear una regla de pertenencia a grupo en el portal de Azure, comprobar el estado.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f828ff83e6b9c60eb08edef7f47e88185fb5aef8
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58199688"
---
# <a name="create-a-dynamic-group-and-check-status"></a>Creación de un grupo dinámico y comprobación de su estado

En Azure Active Directory (Azure AD), puede usar reglas para determinar la pertenencia a grupos en función de las propiedades de usuario o dispositivo. Este artículo explica cómo configurar una regla para un grupo dinámico en el portal de Azure.
Se admite la pertenencia dinámica para grupos de seguridad o grupos de Office 365. Cuando se aplica una regla de pertenencia a grupo, se evalúan los atributos de usuario y dispositivo coincidencias con la regla de pertenencia. Cuando se cambia un atributo para un usuario o dispositivo, se procesan todas las reglas de grupo dinámico de la organización para los cambios de pertenencia. Los usuarios y dispositivos se agregan o quitan si cumplen las condiciones para un grupo.

Para obtener ejemplos de sintaxis, las propiedades admitidas, operadores y valores para una regla de pertenencia, vea [reglas de pertenencia dinámica para grupos en Azure Active Directory](groups-dynamic-membership.md).

## <a name="to-create-a-group-membership-rule"></a>Para crear una regla de pertenencia a grupo, siga estos pasos:

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com) con una cuenta que se encuentra en el administrador Global, Administrador de Intune o rol de usuario administrador en el inquilino.
2. Seleccione **Grupos**.
3. Seleccione **Todos los grupos** y, luego, **Nuevo grupo**.

   ![Seleccione el comando Agregar nuevo grupo](./media/groups-create-rule/new-group-creation.png)

4. En el **grupo** , escriba un nombre y una descripción para el nuevo grupo. Seleccione un **tipo de pertenencia** para los usuarios o dispositivos y a continuación, seleccione **agregar una consulta dinámica**. Puede usar el generador de reglas para crear una regla sencilla, o [escribir usted mismo una regla de pertenencia](groups-dynamic-membership.md).

   ![Agregar regla de pertenencia para un grupo dinámico](./media/groups-create-rule/add-dynamic-group-rule.png)

5. Para ver las propiedades de extensión personalizada disponibles para su consulta de pertenencia
   1. Seleccione **obtener propiedades de extensión personalizada**
   2. Escriba el identificador de aplicación y, a continuación, seleccione **actualizar propiedades**. 
6. Después de crear la regla, seleccione **Agregar consulta** en la parte superior de la hoja.
7. Seleccione **Crear** on the **Grupo** para crear el grupo.

Si la regla que escribió no es válida, obtener una explicación de por qué no se pudo procesar la regla se muestra en la esquina superior derecha del portal. Léala con cuidado para saber cómo corregir la regla.

## <a name="turn-on-or-off-welcome-email"></a>Activar o desactivar el correo electrónico de bienvenida

Cuando se crea un nuevo grupo de Office 365, se envía a una notificación bienvenida a los usuarios que se agregan al grupo. Más adelante, si cambian los atributos de un usuario o dispositivo, se procesan todas las reglas de grupo dinámico de la organización para los cambios de pertenencia. A continuación, los usuarios que se agregan también reciben la notificación de bienvenida. Puede desactivar este comportamiento en [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/Set-UnifiedGroup?view=exchange-ps). 

## <a name="check-processing-status-for-a-rule"></a>Comprobación del estado de procesamiento de una regla

Puede ver el estado de procesamiento de la pertenencia y la última fecha actualizada en la página **Información general** del grupo dinámico.
  
  ![visualización del estado del grupo dinámico](./media/groups-create-rule/group-status.png)

Los mensajes de estado siguientes se pueden mostrar para el estado de **procesamiento de la pertenencia**:

* **Evaluando**:  se ha recibido el cambio de grupo y se están evaluando las actualizaciones.
* **Procesando**: las actualizaciones se están procesando.
* **Actualización completada**: se ha completado el procesamiento y se han realizado todas las actualizaciones aplicables.
* **Error de procesamiento**:  No se pudo completar el procesamiento debido a un error al evaluar la regla de pertenencia.
* **Actualización en pausa**: el administrador han pausado las actualizaciones de la regla de pertenencia dinámica. MembershipRuleProcessingState se establece en "Paused".

Los mensajes de estado siguientes se pueden mostrar para el estado **Última actualización de la pertenencia**:

* &lt;**Fecha y hora**&gt;: la última vez que se actualizó la pertenencia.
* **En curso**: las actualizaciones están actualmente en curso.
* **Desconocido**: No se puede recuperar la hora de última actualización. El grupo podría ser nuevo.

Si se produce un error al procesar la regla de pertenencia para un grupo específico, se muestra una alerta en la parte superior de la **página de información general** del grupo. Si no se puede procesar ninguna actualización de pertenencia dinámica pendiente para todos los grupos del inquilino durante más de 24 horas, se muestra una alerta en la parte superior de **Todos los grupos**.

![alertas de mensaje de error de procesamiento](./media/groups-create-rule/processing-error.png)

En estos artículos se proporciona información adicional sobre los grupos en Azure Active Directory.

* [Consulta de los grupos existentes](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Crear un nuevo grupo y agregar miembros](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Administrar la configuración de un grupo](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Administrar la pertenencia a grupos](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Administrar reglas dinámicas de los usuarios de un grupo](groups-dynamic-membership.md)

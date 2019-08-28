---
title: 'Actualización y administración de una regla de grupo dinámico y solución problemas de pertenencia: Azure Active Directory | Microsoft Docs'
description: Cómo crear reglas de pertenencia a grupos en Azure Portal y comprobar el estado.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 08/12/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce3bce5e2656efea0a4fc3d7aa6be46f1e6926ec
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69656927"
---
# <a name="update-a-dynamic-group-to-manage-membership-in-azure-active-directory"></a>Actualización de un grupo dinámico para administrar la pertenencia en Azure Active Directory

En Azure Active Directory (Azure AD), puede usar reglas para determinar la pertenencia a grupos según las propiedades de usuario o dispositivo. En este artículo se explica cómo configurar una regla para un grupo dinámico en Azure Portal.
La pertenencia dinámica se admite para grupos de seguridad o grupos de Office 365. Cuando se aplica una regla de pertenencia a grupos, se evalúan los atributos de usuario y dispositivo para ver si coinciden con la regla de pertenencia. Cuando cambian los atributos de un usuario o dispositivo, se procesan todos los cambios de pertenencia de las reglas de grupo dinámico de la organización. Los usuarios y dispositivos se agregan o quitan si cumplen las condiciones de un grupo.

Para ver ejemplos de sintaxis, propiedades admitidas, operadores y valores de una regla de pertenencia, consulte [Reglas de pertenencia dinámica a grupos de Azure Active Directory](groups-dynamic-membership.md).

## <a name="to-update-a-group-membership-rule"></a>Para actualizar una regla de pertenencia a grupo

1. Inicie sesión en el [Centro de administración de Azure AD](https://aad.portal.azure.com) con una cuenta de administrador global, administrador de Intune o administrador de usuarios en el inquilino.
1. Seleccione **Grupos** > **Todos los grupos**.
1. Seleccione un grupo para abrir su perfil.
1. En la página de perfil del grupo, seleccione **Reglas de pertenencia dinámica**. El generador de reglas admite hasta cinco expresiones. Para agregar seis o más expresiones, debe usar el cuadro de texto.

   ![Adición de una regla de pertenencia a un grupo dinámico](./media/groups-update-rule/update-dynamic-group-rule.png)

1. Para ver las propiedades de extensión personalizadas disponibles para su regla de pertenencia, siga estos pasos:
   1. Seleccione **Obtener las propiedades de extensión personalizadas**.
   2. Escriba el identificador de aplicación y, luego, seleccione **Actualizar propiedades**.
1. Después de actualizar la regla, seleccione **Guardar**.

Si la regla que escribió no es válida, se muestra una explicación de por qué no se pudo procesar la regla en la notificación de Azure del portal. Léala con cuidado para saber cómo corregir la regla.

## <a name="check-processing-status-for-a-rule"></a>Comprobación del estado de procesamiento de una regla

Puede ver el estado de procesamiento de la pertenencia y la última fecha actualizada en la página **Información general** del grupo dinámico.
  
  ![visualización del estado del grupo dinámico](./media/groups-create-rule/group-status.png)

Los mensajes de estado siguientes se pueden mostrar para el estado de **procesamiento de la pertenencia**:

* **Evaluando**:  se ha recibido el cambio de grupo y se están evaluando las actualizaciones.
* **Procesando**: las actualizaciones se están procesando.
* **Actualización completada**: se ha completado el procesamiento y se han realizado todas las actualizaciones aplicables.
* **Error de procesamiento**:  no se pudo completar el procesamiento debido a un error al evaluar la regla de pertenencia.
* **Actualización en pausa**: el administrador han pausado las actualizaciones de la regla de pertenencia dinámica. MembershipRuleProcessingState se establece en "Paused".

Los mensajes de estado siguientes se pueden mostrar para el estado **Última actualización de la pertenencia**:

* **Fecha y hora**: la última vez que se actualizó la pertenencia.
* **En curso**: las actualizaciones están actualmente en curso.
* **Desconocido**: no se puede recuperar la hora de la última actualización. El grupo podría ser nuevo.

Si se produce un error al procesar la regla de pertenencia para un grupo específico, se muestra una alerta en la parte superior de la **página de información general** del grupo. Si no se puede procesar ninguna actualización de pertenencia dinámica pendiente para todos los grupos del inquilino durante más de 24 horas, se muestra una alerta en la parte superior de **Todos los grupos**.

![alertas de mensajes de error de procesamiento](./media/groups-create-rule/processing-error.png)

## <a name="next-steps"></a>Pasos siguientes

En estos artículos se proporciona información adicional sobre cómo trabajar con los grupos dinámicos en Azure AD.

* Para obtener una referencia completa de la estructura de las reglas dinámicas, consulte [Sintaxis de las reglas de pertenencia dinámica](groups-dynamic-membership.md).
* [Creación de un grupo de pertenencia estático e incorporación de miembros](../fundamentals/active-directory-groups-create-azure-portal.md).

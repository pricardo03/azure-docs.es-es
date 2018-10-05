---
title: Creación de un grupo dinámico y comprobación del estado en Azure Active Directory | Microsoft Docs
description: Cómo crear reglas de pertenencia a un grupo en Azure Portal y comprobar el estado.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/20/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: e418316a74ccf27ec730261957a8b6c64de5d063
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47040608"
---
# <a name="create-a-dynamic-group-and-check-status"></a>Creación de un grupo dinámico y comprobación de su estado

En Azure Active Directory (Azure AD), puede crear grupos mediante la aplicación de una regla para determinar la pertenencia según las propiedades de usuario o dispositivo. Cuando los atributos de un usuario o dispositivo cambian, Azure AD evalúa todas las reglas de grupos dinámicos del inquilino de Azure AD y realiza adiciones o eliminaciones. Si un usuario o un dispositivo cumplen una regla de un grupo, se agregan como miembros y, cuando ya no la cumplen, se eliminan.

En este artículo se describe cómo configurar una regla en Azure Portal para la pertenencia dinámica en grupos de seguridad o grupos de Office 365. Para ver ejemplos de sintaxis de reglas y una lista completa de las propiedades, operadores y valores admitidos para una regla de pertenencia, consulte [Reglas de pertenencia a grupos dinámicos en Azure Active Directory](groups-dynamic-membership.md).

## <a name="to-create-a-group-membership-rule"></a>Para crear una regla de pertenencia a grupo, siga estos pasos:

1. Inicie sesión en el [Centro de administración de Azure AD](https://aad.portal.azure.com) con una cuenta que sea administrador global, administrador del servicio Intune o administrador de cuentas de usuario en el inquilino.
2. Seleccione **Grupos**.
3. Seleccione **Todos los grupos** y, luego, **Nuevo grupo**.

   ![Add new group](./media/groups-create-rule/new-group-creation.png)

4. En la hoja **Grupo** , escriba un nombre y una descripción para el nuevo grupo. En **Tipo de pertenencia**, seleccione **Usuario dinámico** o **Dispositivo dinámico**, dependiendo de si quiere crear una regla para usuarios o dispositivos y, a continuación, seleccione **Agregar una consulta dinámica**. Puede usar el generador de reglas para crear una regla sencilla o escribir usted mismo una regla de pertenencia. Este artículo contiene más información sobre los atributos de usuario y dispositivo disponibles, así como ejemplos de reglas de pertenencia.

   ![Adición de una regla de pertenencia dinámica](./media/groups-create-rule/add-dynamic-group-rule.png)

5. Después de crear la regla, seleccione **Agregar consulta** en la parte superior de la hoja.
6. Seleccione **Crear** on the **Grupo** para crear el grupo.

> [!TIP]
> Se produce un error al crear el grupo si la regla que escribió no tiene el formato correcto o no es válida. Se muestra una notificación en la esquina superior derecha del portal, con una explicación de por qué no se pudo procesar la regla. Léala con cuidado para saber cómo debe ajustar la regla para que sea válida.

## <a name="check-processing-status-for-a-membership-rule"></a>Comprobación del estado de procesamiento de una regla de pertenencia

Puede ver el estado de procesamiento de la pertenencia y la última fecha actualizada en la página **Información general** del grupo dinámico.
  
  ![visualización del estado de grupo dinámico](./media/groups-create-rule/group-status.png)

Los mensajes de estado siguientes se pueden mostrar para el estado de **procesamiento de la pertenencia**:

* **Evaluando**: se ha recibido el cambio de grupo y las actualizaciones se están evaluando.
* **Procesando**: las actualizaciones se están procesando.
* **Actualización completada**: se ha completado el procesamiento y se han realizado todas las actualizaciones aplicables.
* **Error de procesamiento**: se detectó un error al evaluar la regla de pertenencia y no se pudo completar el procesamiento.
* **Actualización en pausa**: el administrador han pausado las actualizaciones de la regla de pertenencia dinámica. MembershipRuleProcessingState se establece en "Paused".

Los mensajes de estado siguientes se pueden mostrar para el estado **Última actualización de la pertenencia**:

* &lt;**Fecha y hora**&gt;: la última vez que se actualizó la pertenencia.
* **En curso**: las actualizaciones están actualmente en curso.
* **Desconocido**: no se puede recuperar la hora de la última actualización. Puede deberse a que el grupo sea recién creado.

Si se produce un error al procesar la regla de pertenencia para un grupo específico, se muestra una alerta en la parte superior de la **página de información general** del grupo. Si no se puede procesar ninguna actualización de pertenencia dinámica pendiente para todos los grupos del inquilino durante más de 24 horas, se muestra una alerta en la parte superior de **Todos los grupos**.

![mensaje de error de procesamiento](./media/groups-create-rule/processing-error.png)

En estos artículos se proporciona información adicional sobre los grupos en Azure Active Directory.

* [Ver los grupos existentes](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Crear un nuevo grupo y agregar miembros](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Administrar la configuración de un grupo](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Administrar la pertenencia a grupos](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Administrar reglas dinámicas de los usuarios de un grupo](groups-dynamic-membership.md)

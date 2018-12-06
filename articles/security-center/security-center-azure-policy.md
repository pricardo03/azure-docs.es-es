---
title: Directivas de seguridad de Azure Security Center configuradas como parte de Azure Policy y visualizadas en Security Center | Microsoft Docs
description: Con este documento aprenderá a configurar las directivas en Azure Policy o visualizarlas en Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/25/2018
ms.author: rkarlin
ms.openlocfilehash: 330b66e64484417e50f39c35cf90a6fd62b1e888
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334676"
---
# <a name="view-security-policies"></a>Visualización de directivas de seguridad

En este artículo se explica cómo configurar directivas de seguridad y cómo visualizarlas en Security Center. Azure Security Center asigna automáticamente sus [directivas de seguridad integradas](security-center-policy-definitions.md) en cada suscripción que se incorpora. Puede configurarlas en [Azure Policy](../azure-policy/azure-policy-introduction.md), que también le permite establecer directivas en grupos de administración y entre varias suscripciones.

Para obtener instrucciones sobre cómo establecer directivas con PowerShell, vea [Inicio rápido: Creación de una asignación de directiva para identificar recursos no compatibles con el módulo Azure RM PowerShell](../azure-policy/assign-policy-definition-ps.md).



## <a name="what-are-security-policies"></a>¿Qué son las directivas de seguridad?
Una directiva de seguridad define la configuración deseada de las cargas de trabajo. Además, ayuda a garantizar el cumplimiento de los requisitos de seguridad normativos o de la empresa. En Azure Policy, puede definir directivas para las suscripciones de Azure y adaptarlas al tipo de carga de trabajo o a la confidencialidad de los datos. Por ejemplo, es posible que las aplicaciones que usan datos regulados, como la información de identificación personal, requieran un mayor nivel de seguridad que otras cargas de trabajo. Para establecer una directiva en varias suscripciones o grupos de administración, debe configurarla en [Azure Policy](../azure-policy/azure-policy-introduction.md).



Las directivas de seguridad generan las recomendaciones de seguridad que se muestran en Azure Security Center. Puede supervisar si se cumplen para identificar posibles puntos vulnerables y mitigar amenazas. Consulte la lista de [directivas de seguridad integradas](security-center-policy-definitions.md) para más información sobre cómo determinar la opción más adecuada en su caso.

### <a name="management-groups"></a>Grupos de administración
Si su organización tiene varias suscripciones, puede que necesite una manera de administrar el acceso, las directivas y el cumplimiento de esas suscripciones de forma eficaz. Los grupos de administración de Azure proporcionan un nivel de ámbito por encima de las suscripciones. Las suscripciones se organizan en contenedores llamados "grupos de administración" y aplican sus condiciones de gobierno a los grupos de administración. Todas las suscripciones dentro de un grupo de administración heredan automáticamente las directivas que se aplican al grupo de administración. Cada directorio tiene un grupo de administración de nivel superior único denominado "raíz". Este grupo de administración raíz está integrado en la jerarquía de manera que contiene todos los grupos de administración y suscripciones. Este grupo de administración raíz permite que las directivas globales y las asignaciones de control de acceso basado en rol (RBAC) se apliquen en el nivel de directorio. Para configurar grupos de administración para usarlos con Azure Security Center, siga las instrucciones que se describen en [Visibilidad de todos los inquilinos en Azure Security Center](security-center-management-groups.md).

> [!NOTE]
> Es importante que comprenda la jerarquía de grupos de administración y suscripciones. Para más información sobre los grupos de administración, la administración raíz y el acceso a grupos de administración, consulte [Organización de recursos con grupos de administración de Azure](../governance/management-groups/index.md#root-management-group-for-each-directory).
>

## <a name="how-security-policies-work"></a>¿Cómo funcionan las directivas de seguridad?
Security Center crea automáticamente una directiva de seguridad predeterminada para cada una de las suscripciones de Azure. Puede modificar las directivas de Azure Policy para realizar lo siguiente:
- Crear nuevas definiciones de directiva.
- Asignar directivas entre grupos de administración y suscripciones, que pueden representar una organización entera o una unidad de negocio dentro de la organización.
- Supervisar el cumplimiento de las directivas.

Para más información sobre Azure Policy, consulte [Creación y administración de directivas para aplicar el cumplimiento](../azure-policy/create-manage-policy.md).

Una directiva de Azure consta de los siguientes componentes:

- Una **directiva** es una regla.
- Una **iniciativa** es una colección de directivas.
- Una **asignación** es la aplicación de una iniciativa o una directiva para un ámbito concreto (grupo de administración, suscripción o grupo de recursos).

Un recurso se evalúa con las directivas que están asignadas a él y recibe una proporción de cumplimiento en función del número de directivas con las que guarda conformidad el recurso.

## <a name="who-can-edit-security-policies"></a>¿Quién puede editar directivas de seguridad?
Security Center usa el control de acceso basado en rol (RBAC), que proporciona roles integrados que se pueden asignar a usuarios, grupos y servicios en Azure. Cuando un usuario abre Security Center, solo ve la información relacionada con los recursos a los que tiene acceso. Esto significa que a los usuarios se les asigna el rol de propietario, colaborador o lector para la suscripción o el grupo de recursos a los que pertenece un recurso. Además de estos roles, hay dos roles específicos de Security Center:

- Lector de seguridad: el usuario tiene derecho a visualizar el contenido de Security Center (recomendaciones, alertas, directivas y estados) pero no puede realizar cambios.
- Administrador de seguridad: tiene los mismos derechos que el lector de seguridad, pero también puede actualizar la directiva de seguridad o descartar recomendaciones y alertas.

## <a name="edit-security-policies"></a>Edición de directivas de seguridad
Puede editar la directiva de seguridad predeterminada de cada una de las suscripciones y grupos de administración de Azure en [Azure Policy](../governance/policy/tutorials/create-and-manage.md). Para modificar una directiva de seguridad, debe ser propietario, colaborador o administrador de seguridad de la suscripción o del grupo de administración que la contiene.

Para obtener instrucciones sobre cómo editar una directiva de seguridad en Azure Policy, consulte [Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md).

## <a name="view-security-policies"></a>Visualización de directivas de seguridad

Para ver las directivas de seguridad de Security Center:

1. En el panel de **Security Center**, seleccione **Directiva de seguridad**.

    ![El panel Administración de directivas](./media/security-center-policies/security-center-policy-mgt.png)

  En la pantalla **Administración de directivas**, puede ver el número de grupos de administración, suscripciones y áreas de trabajo, así como la estructura del grupo de administración.

  > [!NOTE]
  > - El panel de Security Center podría mostrar un número de suscripciones en **Cobertura de la suscripción** superior al que se muestra en **Administración de directivas**. En la cobertura de suscripción se muestra el número de suscripciones Estándar, Gratis y "no cubiertas". Las suscripciones "no cubiertas" no tienen habilitado Security Center y no se muestran en **Administración de directivas**.
  >

  En las columnas de la tabla se muestra:

 - **Policy initiative assignment** (Asignación de iniciativa de directiva): iniciativas y [directivas integradas](security-center-policy-definitions.md) de Security Center que se asignan a una suscripción o un grupo de administración.
 - **Compliance** (Cumplimiento): puntuación general de cumplimiento para un grupo de administración, una suscripción o un área de trabajo. La puntuación es la media ponderada de las asignaciones. La media ponderada tiene en cuenta el número de directivas en una única asignación y el número de recursos al que se aplica la asignación.

 Por ejemplo, si la suscripción tiene dos máquinas virtuales y tiene asignada una iniciativa con cinco directivas, entonces usted tiene 10 valoraciones en la suscripción. Si una de las máquinas virtuales no cumple las dos directivas, la puntuación general de cumplimiento de la asignación de la suscripción es del 80 %.

 - **Coverage** (Cobertura): identifica el plan de tarifa, Gratis o Estándar, en el que se ejecuta el grupo de administración, la suscripción o el área de trabajo.  Para obtener más información sobre los planes de tarifa de Security Center, vea [Precios](security-center-pricing.md).
 - **Settings** (Configuración): las suscripciones tienen el vínculo **Edit settings** (Editar configuración). Seleccionar **Edit settings** (Editar configuración) le permite actualizar su [configuración de Security Center](security-center-policies-overview.md) para cada grupo de administración o suscripción.

2. Seleccione el grupo de administración o la suscripción cuyas directivas desea ver.

  - La pantalla **Directiva de seguridad** refleja la acción realizada por las directivas asignadas en el grupo de administración o la suscripción que ha seleccionado.
  - En la parte superior, use los vínculos proporcionados para abrir cada **asignación** de directiva que se aplique al grupo de administración o la suscripción. Puede usar los vínculos para obtener acceso a la asignación y editar o deshabilitar la directiva. Por ejemplo, si ve que una asignación de directiva determinada está denegando la protección de puntos de conexión, puede usar el vínculo para acceder a la directiva y editarla o deshabilitarla.
  - En la lista de directivas, puede ver la aplicación efectiva de la directiva en su suscripción o grupo de administración. Esto significa que se tiene en cuenta la configuración de cada directiva que se aplica al ámbito y se proporciona el resultado acumulado de la acción que realiza la directiva. Por ejemplo, si en una asignación la directiva está deshabilitada, pero en otra está definida en AuditIfNotExist, el efecto acumulado aplica AuditIfNotExist. El efecto más activo siempre tiene prioridad.
  - Las directivas pueden tener los siguientes efectos: Append, Audit, AuditIfNotExists, Deny, DeployIfNotExists y Disabled. Para más información sobre cómo se aplican los efectos, consulte [Descripción de los efectos de Policy](../governance/policy/concepts/effects.md).

   ![pantalla de la directiva](./media/security-center-policies/policy-screen.png)

> [!NOTE]
> - Cuando vea directivas asignadas, puede ver varias asignaciones y cómo se configura cada asignación individualmente.

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a configurar directivas de seguridad en Security Center. Para más información sobre Security Center, consulte los siguientes artículos:

* [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md): aprenda a planear y comprender las consideraciones de diseño sobre Azure Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): aprenda a administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
* [Visibilidad de todos los inquilinos en Azure Security Center](security-center-management-groups.md): aprenda a configurar grupos de administración para Azure Security Center.
* [Preguntas frecuentes acerca de Azure Security Center](security-center-faq.md): obtenga respuestas a las preguntas más frecuentes sobre cómo usar el servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/): encuentre entradas de blog sobre el cumplimiento y la seguridad de Azure.

Para más información acerca de Azure Policy, consulte [¿Qué es Azure Policy?](../azure-policy/azure-policy-introduction.md)

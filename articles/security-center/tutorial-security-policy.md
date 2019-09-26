---
title: Trabajo con directivas de seguridad | Microsoft Docs
description: En este artículo se describe cómo trabajar con directivas de seguridad en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2019
ms.author: memildin
ms.openlocfilehash: 11e1c837e416a5d2105f494c43823298edfd4355
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71200497"
---
# <a name="working-with-security-policies"></a>Uso de directivas de seguridad

En este artículo se explica cómo configurar directivas de seguridad y cómo visualizarlas en Security Center. Azure Security Center asigna automáticamente sus [directivas de seguridad integradas](security-center-policy-definitions.md) en cada suscripción que se incorpora. Puede configurarlas en [Azure Policy](../governance/policy/overview.md), que también le permite establecer directivas en grupos de administración y entre varias suscripciones.

Para obtener instrucciones sobre cómo establecer directivas con PowerShell, consulte [Inicio rápido: Creación de una asignación de directiva para identificar recursos no compatibles mediante el módulo de Azure PowerShell](../governance/policy/assign-policy-powershell.md).

>[!NOTE]
> Security Center inició su integración con Azure Policy. Los clientes existentes se migrarán automáticamente a la nueva iniciativa integrada en Azure Policy, en lugar de las directivas de seguridad anteriores de Security Center. Este cambio no afectará a los recursos ni al entorno, excepto la presencia de la nueva iniciativa en Azure Policy.

## <a name="what-are-security-policies"></a>¿Qué son las directivas de seguridad?
Una directiva de seguridad define la configuración deseada de las cargas de trabajo. Además, ayuda a garantizar el cumplimiento de los requisitos de seguridad normativos o de la empresa. En Azure Policy, puede definir directivas para las suscripciones de Azure y adaptarlas al tipo de carga de trabajo o a la confidencialidad de los datos. Por ejemplo, es posible que las aplicaciones que usan datos regulados, como datos personales o de clientes, requieran un mayor nivel de seguridad que otras cargas de trabajo. Para establecer una directiva en varias suscripciones o grupos de administración, debe configurarla en [Azure Policy](../governance/policy/overview.md).

Las directivas de seguridad generan las recomendaciones de seguridad que se muestran en Azure Security Center. Puede supervisar si se cumplen para identificar posibles puntos vulnerables y mitigar amenazas. Consulte la lista de [directivas de seguridad integradas](security-center-policy-definitions.md) para más información sobre cómo determinar la opción más adecuada en su caso.

Al habilitar Security Center, la directiva de seguridad integrada en Security Center se refleja en Azure Policy como una iniciativa integrada en la categoría de Security Center. La iniciativa integrada se asigna automáticamente a todas las suscripciones registradas de Security Center (niveles Gratis o Estándar). La iniciativa integrada contiene solo las directivas de auditoría.


### <a name="management-groups"></a>Grupos de administración
Si su organización tiene varias suscripciones, puede que necesite una manera de administrar el acceso, las directivas y el cumplimiento de esas suscripciones de forma eficaz. Los grupos de administración de Azure proporcionan un nivel de ámbito por encima de las suscripciones. Las suscripciones se organizan en contenedores llamados "grupos de administración" y aplican sus condiciones de gobernanza a los grupos de administración. Todas las suscripciones dentro de un grupo de administración heredan automáticamente las directivas que se aplican al grupo de administración. Cada directorio tiene un grupo de administración de nivel superior único denominado "raíz". Este grupo de administración raíz está integrado en la jerarquía de manera que contiene todos los grupos de administración y suscripciones. Este grupo de administración raíz permite que las directivas globales y las asignaciones de control de acceso basado en rol (RBAC) se apliquen en el nivel de directorio. Para configurar grupos de administración para usarlos con Azure Security Center, siga las instrucciones que se describen en [Visibilidad de todos los inquilinos en Azure Security Center](security-center-management-groups.md).

> [!NOTE]
> Es importante que comprenda la jerarquía de grupos de administración y suscripciones. Para más información sobre los grupos de administración, la administración raíz y el acceso a grupos de administración, consulte [Organización de recursos con grupos de administración de Azure](../governance/management-groups/index.md#root-management-group-for-each-directory).
>

## <a name="how-security-policies-work"></a>¿Cómo funcionan las directivas de seguridad?
Security Center crea automáticamente una directiva de seguridad predeterminada para cada una de las suscripciones de Azure. Puede modificar las directivas de Azure Policy para realizar lo siguiente:
- Crear nuevas definiciones de directiva.
- Asignar directivas entre grupos de administración y suscripciones, que pueden representar una organización entera o una unidad de negocio dentro de la organización.
- Supervisar el cumplimiento de las directivas.

Para más información sobre Azure Policy, consulte [Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md).

Una directiva de Azure consta de los siguientes componentes:

- Una **directiva** es una regla.
- Una **iniciativa** es una colección de directivas.
- Una **asignación** es la aplicación de una iniciativa o una directiva para un ámbito concreto (grupo de administración, suscripción o grupo de recursos).

## <a name="view-security-policies"></a>Visualización de directivas de seguridad

Para ver las directivas de seguridad de Security Center:

1. En el panel de **Security Center**, seleccione **Directiva de seguridad**.

    ![El panel Administración de directivas](./media/security-center-policies/security-center-policy-mgt.png)

   En la pantalla **Administración de directivas**, puede ver el número de grupos de administración, suscripciones y áreas de trabajo, así como la estructura del grupo de administración.

   > [!NOTE]
   > El panel de Security Center podría mostrar un número de suscripciones en **Cobertura de la suscripción** superior al que se muestra en **Administración de directivas**. En la cobertura de suscripción se muestra el número de suscripciones Estándar, Gratis y "no cubiertas". Las suscripciones "no cubiertas" no tienen habilitado Security Center y no se muestran en **Administración de directivas**.
   >

2. Seleccione el grupo de administración o la suscripción cuyas directivas desea ver.

   - La pantalla **Directiva de seguridad** refleja la acción realizada por las directivas asignadas en el grupo de administración o la suscripción que ha seleccionado.
   - En la parte superior, use los vínculos proporcionados para abrir cada **asignación** de directiva que se aplique al grupo de administración o la suscripción. Puede usar los vínculos para obtener acceso a la asignación y editar o deshabilitar la directiva. Por ejemplo, si ve que una asignación de directiva determinada está denegando la protección de puntos de conexión, puede usar el vínculo para acceder a la directiva y editarla o deshabilitarla.
   - En la lista de directivas, puede ver la aplicación efectiva de la directiva en su suscripción o grupo de administración. Esto significa que se tiene en cuenta la configuración de cada directiva que se aplica al ámbito y se proporciona el resultado acumulado de la acción que realiza la directiva. Por ejemplo, si en una asignación la directiva está deshabilitada, pero en otra está definida en AuditIfNotExist, el efecto acumulado aplica AuditIfNotExist. El efecto más activo siempre tiene prioridad.
   - El efecto de las directivas puede ser: Append, Audit, AuditIfNotExists, Deny, DeployIfNotExists, Disabled. Para más información sobre cómo se aplican los efectos, consulte [Descripción de los efectos de Policy](../governance/policy/concepts/effects.md).

   ![pantalla de la directiva](./media/security-center-policies/policy-screen.png)

> [!NOTE]
> Cuando vea directivas asignadas, puede ver varias asignaciones y cómo se configura cada asignación individualmente.

## <a name="edit-security-policies"></a>Edición de directivas de seguridad
Puede editar la directiva de seguridad predeterminada de cada una de las suscripciones y grupos de administración de Azure en [Azure Policy](../governance/policy/tutorials/create-and-manage.md). Para modificar una directiva de seguridad, debe ser propietario o administrador de seguridad de la suscripción o del grupo de administración que la contiene.

Para obtener instrucciones sobre cómo editar una directiva de seguridad en Azure Policy, consulte [Creación y administración de directivas para aplicar el cumplimiento](../governance/policy/tutorials/create-and-manage.md).

Puede editar directivas de seguridad mediante el portal de Azure Policy, la API REST o con Windows PowerShell. En el siguiente ejemplo se proporcionan instrucciones para la edición mediante la API REST.


## <a name="disable-security-policies"></a>Deshabilitar las directivas de seguridad
Si la directiva de seguridad predeterminada genera una recomendación que no es pertinente para su entorno, puede deshabilitar la definición de directiva que envía la recomendación para detenerla.
Para más información sobre las recomendaciones, consulte [Administración de las recomendaciones de seguridad](security-center-recommendations.md).

1. En Security Center, en **DIRECTIVA Y CUMPLIMIENTO**, seleccione **Directiva de seguridad**.

   ![administración de directivas](./media/tutorial-security-policy/policy-management.png)

2. Haga clic en el grupo de administración o de suscripción para el que quiere deshabilitar la recomendación.

   > [!Note]
   > Recuerde que un grupo de administración aplica sus directivas a sus suscripciones. Por lo tanto, si deshabilita la directiva de una suscripción y la suscripción pertenece a un grupo de administración que usa la misma directiva, seguirá recibiendo las recomendaciones de directivas. La directiva se seguirá aplicando desde el nivel de administración y las recomendaciones se seguirán generando.

1. Haga clic en la directiva asignada.

   ![deshabilitar la directiva](./media/tutorial-security-policy/security-policy.png)

1. En la sección **PARÁMETROS**, busque la directiva que invoca la recomendación que quiere deshabilitar y, en la lista desplegable, seleccione **Deshabilitado**

   ![deshabilitar la directiva](./media/tutorial-security-policy/disable-policy.png)
1. Haga clic en **Save**(Guardar).
   > [!Note]
   > Los cambios de deshabilitación de directiva pueden tardar hasta 12 horas en surtir efecto.


### <a name="configure-a-security-policy-using-the-rest-api"></a>Configuración de una directiva de seguridad mediante la API REST

Como parte de la integración nativa con Azure Policy, Azure Security Center le permite aprovechar la API REST de Azure Policy para crear asignaciones de directivas. Las siguientes instrucciones le guiarán en la creación de asignaciones de directivas, así como en la personalización de asignaciones existentes. 

Conceptos importantes de Azure Policy: 

- Una **definición de directiva** es una regla. 

- Una **iniciativa** es una colección de definiciones de directivas (reglas). 

- Una **asignación** es una aplicación de una iniciativa o una directiva para un ámbito concreto (grupo de administración o suscripción, entre otros). 

Security Center tiene una iniciativa incorporada que incluye todas las directivas de seguridad. Para evaluar las directivas de Security Center sobre los recursos de Azure, debe crear una asignación en el grupo de administración o en la suscripción que desee evaluar.

De forma predeterminada, lºa iniciativa integrada tiene todas las directivas de Security Center habilitadas. Puede elegir deshabilitar ciertas directivas de la iniciativa integrada; por ejemplo, puede aplicar todas las directivas de Security Center excepto **firewall de aplicación web**, cambiando el valor del parámetro de efecto de la directiva a **Deshabilitado**. 

### <a name="api-examples"></a>Ejemplos de API

En los siguientes ejemplos, reemplace estas variables:

- **{scope}** escriba el nombre del grupo de administración o de suscripción al que está aplicando la directiva.
- **{policyAssignmentName}** escriba el [nombre de la asignación de directiva pertinente](#policy-names).
- **{name}** escriba su nombre o el nombre del administrador que ha aprobado el cambio de directiva.

En este ejemplo se muestra cómo asignar la iniciativa integrada de Security Center a una suscripción o grupo de administración.
 
 ```
    PUT  
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

    Request Body (JSON) 

    { 

      "properties":{ 

    "displayName":"Enable Monitoring in Azure Security Center", 

    "metadata":{ 

    "assignedBy":"{Name}" 

    }, 

    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

    "parameters":{}, 

    } 

    } 
 ```

En este ejemplo se muestra cómo asignar la iniciativa integrada de Security Center a una suscripción, con las siguientes directivas deshabilitadas: 

- Actualizaciones del sistema ("systemUpdatesMonitoringEffect") 

- Configuraciones de seguridad ("systemConfigurationsMonitoringEffect") 

- Protección de extremos ("endpointProtectionMonitoringEffect") 

 ```
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
    
    Request Body (JSON) 
    
    { 
    
      "properties":{ 
    
    "displayName":"Enable Monitoring in Azure Security Center", 
    
    "metadata":{ 
    
    "assignedBy":"{Name}" 
    
    }, 
    
    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 
    
    "parameters":{ 
    
    "systemUpdatesMonitoringEffect":{"value":"Disabled"}, 
    
    "systemConfigurationsMonitoringEffect":{"value":"Disabled"}, 
    
    "endpointProtectionMonitoringEffect":{"value":"Disabled"}, 
    
    }, 
    
     } 
    
    } 
 ```
En este ejemplo se muestra cómo quitar una asignación:
 ```
    DELETE   
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
 ```

### Referencia de los nombres de directiva <a name="policy-names"></a>

|Nombre de directiva de Security Center|El nombre de la directiva aparece en Azure Policy |Nombre de parámetro del efecto de directiva|
|----|----|----|
|Cifrado de SQL |Supervisión de la base de datos SQL sin cifrar en Azure Security Center |sqlEncryptionMonitoringEffect| 
|Auditoría de SQL |Supervisión de la base de datos SQL no auditada en Azure Security Center |sqlAuditingMonitoringEffect|
|Actualizaciones del sistema |Supervisión de las actualizaciones del sistema que faltan en Azure Security Center |systemUpdatesMonitoringEffect|
|Cifrado de almacenamiento |Auditoría de la falta del cifrado de blob de las cuentas de almacenamiento |storageEncryptionMonitoringEffect|
|Acceso de red JIT |Supervisión del posible acceso de red Just-In-Time (JIT) en Azure Security Center |jitNetworkAccessMonitoringEffect |
|Controles de aplicación adaptables |Supervisión de una posible inclusión de aplicaciones en la lista de permitidos en Azure Security Center |adaptiveApplicationControlsMonitoringEffect|
|Grupos de seguridad de red |Supervisión del acceso de red permisivo en Azure Security Center |networkSecurityGroupsMonitoringEffect| 
|Configuraciones de seguridad |Supervisión de los puntos vulnerables del sistema operativo en Azure Security Center |systemConfigurationsMonitoringEffect| 
|Endpoint Protection |Supervisión de la falta de Endpoint Protection en Azure Security Center |endpointProtectionMonitoringEffect |
|Cifrado de discos |Supervisión de discos de máquinas virtuales sin cifrar en Azure Security Center |diskEncryptionMonitoringEffect|
|Evaluación de vulnerabilidades |Supervisión de los puntos vulnerables de máquinas virtuales en Azure Security Center |vulnerabilityAssessmentMonitoringEffect|
|Firewall de aplicaciones web |Supervisión de la aplicación web desprotegida en Azure Security Center |webApplicationFirewallMonitoringEffect |
|Firewall de próxima generación |Supervisión de puntos de conexión de red desprotegidos en Azure Security Center| |


### <a name="who-can-edit-security-policies"></a>¿Quién puede editar directivas de seguridad?
Security Center usa el control de acceso basado en rol (RBAC), que proporciona roles integrados que se pueden asignar a usuarios, grupos y servicios en Azure. Cuando un usuario abre Security Center, solo ve la información relacionada con los recursos a los que tiene acceso. Esto significa que a los usuarios se les asigna el rol de propietario, colaborador o lector para la suscripción o el grupo de recursos a los que pertenece un recurso. Además de estos roles, hay dos roles específicos de Security Center:

- Lector de seguridad: el usuario tiene derecho a visualizar el contenido de Security Center (recomendaciones, alertas, directivas y estados) pero no puede realizar cambios.
- Administrador de seguridad: tiene los mismos derechos que el lector de seguridad, pero también puede actualizar la directiva de seguridad o descartar recomendaciones y alertas.



## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido cómo editar las directivas de seguridad en Azure Policy. Para más información sobre Security Center, consulte los siguientes artículos:

* [Guía de planeamiento y operaciones de Azure Security Center](security-center-planning-and-operations-guide.md): Aprenda a planificar y entender las consideraciones de diseño sobre Azure Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): Aprenda a supervisar el estado de los recursos de Azure.
* [Administración y respuesta a alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): Aprenda a administrar y responder a las alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): Aprenda cómo supervisar el estado de mantenimiento de las soluciones de sus asociados.
* [Obtención de visibilidad de todos los inquilinos en Azure Security Center](security-center-management-groups.md): Aprenda a configurar grupos de administración para Azure Security Center.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): Obtenga respuestas para las preguntas más frecuentes acerca del uso del servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/): Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.

Para más información acerca de Azure Policy, consulte [¿Qué es Azure Policy?](../governance/policy/overview.md)

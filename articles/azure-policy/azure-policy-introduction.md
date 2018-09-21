---
title: Introducción a Azure Policy
description: Azure Policy es un servicio de Azure que se usa para crear, asignar y administrar las definiciones de directivas en el entorno de Azure.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 07/31/2018
ms.topic: overview
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 381263a39716ad1271c853ec4b7453a5a922b43e
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2018
ms.locfileid: "45633714"
---
# <a name="what-is-azure-policy"></a>¿Qué es Azure Policy?

IT Governance garantiza que cualquier organización puede alcanzar sus objetivos si usa TI de forma eficaz. Para ello crea claridad entre los proyectos de TI y los objetivos empresariales.

¿Su empresa experimenta muchos problemas de TI que parecen no resolverse nunca?
Un buen gobierno de TI implica planear las iniciativas y establecer prioridades a nivel estratégico que faciliten la administración y eviten problemas. Es aquí donde Azure Policy entra en juego.

Azure Policy es un servicio de Azure que se usa para crear, asignar y administrar directivas. Dichas directivas aplican distintas reglas y efectos a los recursos, con el fin de que estos sigan siendo compatibles con los estándares corporativos y los acuerdos de nivel de servicio. Para realizar estas funciones, Azure Policy ejecuta evaluaciones de los recursos y explora cuáles no son compatibles con las directivas que se han creado. Por ejemplo, puede haber una directiva que permita solo un tamaño de SKU concreto de las máquinas virtuales de un entorno. Una vez que se haya implementado esta directiva, se evaluará al crear y actualizar los recursos, así como en los recursos ya existentes. Más adelante se analizará más detalladamente cómo crear e implementar directivas con Azure Policy.

> [!IMPORTANT]
> La evaluación de cumplimiento de Azure Policy se ofrece ahora para todas las asignaciones independientemente del nivel de precios. Si las asignaciones no muestran los datos de cumplimiento, asegúrese de que la suscripción está registrada con el proveedor de recursos Microsoft.PolicyInsights.

## <a name="how-is-it-different-from-rbac"></a>¿En qué se diferencia de RBAC?

Hay algunas diferencias importantes entre directiva y control de acceso basado en roles (RBAC). RBAC se centra en las acciones del usuario en ámbitos diferentes. Por ejemplo, puede que se le agregue al rol de colaborador para un grupo de recursos en el ámbito deseado. El rol permite realizar cambios en ese grupo de recursos.
La directiva se centra en las propiedades de los recursos durante la implementación y para todos los recursos existentes. Por ejemplo, a través de directivas, puede controlar los tipos de recursos que se pueden aprovisionar. O puede restringir las ubicaciones en las que se pueden aprovisionar los recursos. A diferencia de RBAC, la directiva es un sistema que permite de manera predeterminada y niega explícitamente.

### <a name="rbac-permissions-in-azure-policy"></a>Permisos de RBAC en Azure Policy

Azure Policy tiene permisos representados como operaciones en dos proveedores de recursos diferente:

- [Microsoft.Authorization](../role-based-access-control/resource-provider-operations.md#microsoftauthorization)
- [Microsoft.PolicyInsight](../role-based-access-control/resource-provider-operations.md#microsoftpolicyinsights)

Varios de los roles integrados tienen distintos niveles de permisos en los recursos de Azure Policy, como **administrador de seguridad**, que puede administrar asignaciones de directivas y definiciones de directivas, pero no puede ver información de cumplimiento y **lector** , que puede leer detalles relacionados con las asignaciones y definiciones de directivas, pero que no puede realizar cambios ni ver información acerca del cumplimiento. Mientras que el rol **propietario** tiene todos los derechos completos, **colaborador** no tiene ningún permiso en Azure Policy. Para conceder permiso para ver los detalles de cumplimiento de una directiva, cree un [rol personalizado](../role-based-access-control/custom-roles.md).

## <a name="policy-definition"></a>Definición de directiva

El proceso de creación e implementación de una directiva en Azure Policy comienza con la creación de una definición de directiva. Cada definición de directiva tiene condiciones que regulan su aplicación. Además, tiene un efecto complementario que se produce cuando se cumplen las condiciones.

En Azure Portal, se ofrecen algunas directivas integradas que están disponibles de manera predeterminada. Por ejemplo: 

- **Requerir SQL Server 12.0**: esta definición de directiva tiene condiciones o reglas para garantizar que todos los servidores SQL usen la versión 12.0. Su efecto es denegar a todos los servidores que no cumplen con estos criterios.
- **SUK de cuenta de almacenamiento permitida**: esta definición de directiva tiene un conjunto de condiciones o reglas que determinan si una cuenta de almacenamiento que se está implementado se encuentra dentro de un conjunto de tamaños de SKU. Su efecto es denegar todas las cuentas de almacenamiento que no cumplen el conjunto de tamaños de SKU definidos.
- **Tipo de recurso permitido**: esta definición de directiva tiene un conjunto de condiciones o reglas para especificar los tipos de recursos que puede implementar la organización. Su efecto es denegar a todos los recursos que no forman parte de esta lista definida.
- **Ubicaciones permitidas**: esta directiva permite restringir las ubicaciones que la organización puede especificar al implementar los recursos. Su efecto se utiliza para exigir los requisitos de cumplimiento de replicación geográfica.
- **SKU de máquina virtual permitidas**: esta directiva permite especificar un conjunto de SKU de máquina virtual que la organización puede implementar.
- **Aplicar etiqueta y su valor predeterminado**: esta directiva aplica una etiqueta obligatoria y su valor predeterminado cuando el usuario no la especifica.
- **Forzar etiqueta y su valor**: esta directiva fuerza una etiqueta necesaria y su valor en un recurso.
- **Tipos de recursos no permitidos**: esta directiva permite especificar los tipos de recursos que la organización no puede implementar.

Para implementar estas definiciones de directiva (tanto las definiciones integradas como las personalizadas), será preciso asignarlas. Puede asignar cualquiera de estas directivas a través de Azure Portal, PowerShell o la CLI de Azure.

Tenga en cuenta que se realiza una reevaluación de una directiva aproximadamente cada hora, lo que significa que si realiza cambios en la definición de la directiva después de implementarla (creación de una asignación de directiva), se volverá a realizar una evaluación de los recursos antes de una hora.

Para obtener más información sobre las estructuras de las definiciones de directiva, consulte [Estructura de definición de directiva](policy-definition.md).

## <a name="policy-assignment"></a>Asignación de directiva

Una asignación de directiva es una definición de directiva que se asignó para que ocurra dentro de un ámbito específico. Este ámbito puede ir desde un [grupo de administración](../azure-resource-manager/management-groups-overview.md) a un grupo de recursos. El término *ámbito* hace referencia a todos los grupos de recursos, suscripciones o grupos de administración a los que se asigna la definición de directiva. Todos los recursos secundarios heredan las asignaciones de directivas. Esto significa que si una directiva se aplica a un grupo de recursos, es aplica también a todos los recursos de dicho grupo. Sin embargo, puede excluir un subámbito de la asignación de directiva.

Por ejemplo, en el ámbito de la suscripción, puede asignar una directiva que impida la creación de recursos de red. Sin embargo, excluye un grupo de recursos dentro de la suscripción que está diseñado para la infraestructura de red. Concede acceso a este grupo de recursos de red a los usuarios de confianza con la creación de recursos de red.

En otro ejemplo, es posible que desee asignar una directiva de lista de permitidos de tipos de recursos al nivel de grupo de administración. Y después asigne una directiva más permisiva (para permitir más tipos de recursos) en un grupo de administración secundario o incluso directamente en las suscripciones. Sin embargo, este ejemplo podría no funcionar porque la directiva es un sistema de denegación explícito. En su lugar, debe excluir el grupo de administración secundario o la suscripción de la asignación de directivas a nivel de grupo de administración. Después, asigne la directiva más permisiva en el grupo de administración secundario o en el nivel de suscripción. Para resumir, si alguna directiva tiene como resultado la denegación de un recurso, la única manera de permitir el recurso es modificar la directiva de denegación.

Para más información sobre la configuración de las definiciones y asignaciones de directiva, vea [Creación de una asignación de directiva para identificar recursos no compatibles en el entorno de Azure](assign-policy-definition.md).

## <a name="policy-parameters"></a>Parámetros de directiva

Los parámetros de directiva permiten simplificar la administración de directivas reduciendo el número de definiciones de directiva que debe crear. Cuando crea una definición de directiva, puede definir parámetros para que sea más genérica. Después, puede volver a usar esa definición de directiva para diferentes escenarios. Para ello, transmita distintos valores al asignar la definición de directiva. Por ejemplo, especifique un conjunto de ubicaciones para una suscripción.

Los parámetros se definen o crean cuando se crea una definición de directiva. Cuando se define un parámetro, recibe un nombre y, de manera opcional, un valor. Por ejemplo, podría definir un parámetro para una directiva denominada *location*. Después, puede asignarle valores diferentes, como *EastUS* o *WestUS*, al asignar una directiva.

Para más información sobre los parámetros de directiva, consulte [Información general sobre las directivas de recursos: parámetros](policy-definition.md#parameters).

## <a name="initiative-definition"></a>Definición de iniciativa

Una definición de iniciativa es una colección de definiciones de directiva personalizadas para alcanzar un único objetivo general. Las definiciones de iniciativa simplifican la administración y asignación de las definiciones de directiva. Tal simplificación se realiza mediante la agrupación de un conjunto de directivas como un solo elemento. Por ejemplo, podría crear una iniciativa titulada **Habilitar la supervisión en Azure Security Center**, con el objetivo de supervisar todas las recomendaciones de seguridad disponibles en Azure Security Center.

En esta iniciativa, tendría definiciones de directiva como las siguientes:

- **Supervisar SQL Database sin cifrar en Security Center**, para supervisar servidores y bases de datos SQL sin cifrar.
- **Supervisar las vulnerabilidades del SO en Security Center**, para supervisar los servidores que no cumplen con la base de referencia configurada.
- **Supervisar Endpoint Protection omitido en Security Center**, para supervisar los servidores que no tienen instalado un agente de protección de los puntos de conexión.

## <a name="initiative-assignment"></a>Asignación de iniciativa

Al igual que una asignación de directiva, una asignación de iniciativa es una definición de iniciativa que se asigna a un ámbito específico. Las asignaciones de iniciativa disminuyen la necesidad de crear varias definiciones de iniciativa para cada ámbito. Este ámbito también puede ir desde un grupo de administración a un grupo de recursos.

Del ejemplo anterior, la iniciativa **Habilitar la supervisión en Azure Security Center** se puede asignar a distintos ámbitos. Por ejemplo, se puede asignar una asignación a **subscriptionA**.
Otra se puede asignar a **subscriptionB**.

## <a name="initiative-parameters"></a>Parámetros de iniciativa

Al igual que los parámetros de directiva, los parámetros de iniciativa permiten simplificar la administración de iniciativas mediante la reducción de la redundancia. Los parámetros de iniciativa son esencialmente la lista de los parámetros que las definiciones de directiva usan dentro de la iniciativa.

Por ejemplo, imagine un escenario en el que tiene la definición de una iniciativa (**initiativeC**), con las definiciones de directiva **policyA** y **policyB**, y que cada una de ellas espera un tipo diferente de parámetro:

| Directiva | Nombre del parámetro |tipo del parámetro  |Nota: |
|---|---|---|---|
| policyA | allowedLocations | array  |Este parámetro espera una lista de cadenas para un valor, porque el tipo de parámetro está definido como una matriz |
| policyB | allowedSingleLocation |string |Este parámetro espera una palabra para un valor, porque el tipo de parámetro se definió como una cadena |

En este escenario, tiene tres opciones en el momento de definir los parámetros de iniciativa para **initiativeC**:

- Use los parámetros de las definiciones de directiva dentro de esta iniciativa: en este ejemplo, *allowedLocations* y *allowedSingleLocation* se convierten en parámetros de iniciativa para **initiativeC**.
- Proporcione valores a los parámetros de las definiciones de directiva dentro de esta definición de iniciativa. En este ejemplo, puede proporcionar una lista de ubicaciones al **parámetro de policyA: allowedLocations** y al **parámetro de policyB: allowedSingleLocation**. También puede proporcionar valores cuando asigne esta iniciativa.
- Proporcione una lista de las opciones de *value* que se puede usar cuando se asigna esta iniciativa. Cuando asigna esta iniciativa, los parámetros inherentes de las definiciones de directiva dentro de la iniciativa solo pueden tener valores provenientes de esta lista.

Por ejemplo, podría crear una lista de opciones de valores en una definición de iniciativa que contenga *EastUS*, *WestUS*, *CentralUS* y *WestEurope*. En su caso, no se puede proporcionar un valor diferente, como *Southeast Asia*, durante la asignación de la iniciativa, porque no forma parte de la lista.

## <a name="maximum-count-of-policy-objects"></a>Número máximo de objetos de directiva

[!INCLUDE [policy-limits](../../includes/azure-policy-limits.md)]

## <a name="recommendations-for-managing-policies"></a>Recomendaciones para la administración de directivas

Cuando se crean y administran asignaciones y definiciones de directiva, se recomienda seguir ciertos punteros y tener en cuenta algunas sugerencias:

- Si crea definiciones de directiva en el entorno, se recomienda comenzar con un efecto de auditoría, en contraposición con un efecto de denegación, para llevar un seguimiento del impacto que la definición de directiva tiene en los recursos del entorno. Si ya tiene scripts implementados para escalar verticalmente las aplicaciones de manera automática, establecer un efecto de denegación puede impedir las tareas de automatización que ya tiene implementadas.
- Es importante considerar las jerarquías organizativas cuando se crean definiciones y asignaciones. Se recomienda crear definiciones en un nivel más alto, por ejemplo, en el nivel del grupo de administración o de suscripción, y asignarlas en el próximo nivel secundario. Por ejemplo, si crea una definición de directiva en el nivel de grupo de administración, una asignación de directiva de esa definición se puede limitar a un nivel de suscripción dentro de dicho grupo de administración.
- Se recomienda usar siempre las definiciones de iniciativa en lugar de las definiciones de directiva, incluso si solo tiene considerada una directiva. Por ejemplo, si tiene una definición de directiva, *policyDefA*, y la crea bajo la definición de iniciativa *initiativeDefC*, si decide crear otra definición de directiva más tarde para *policyDefB* con objetivos similares a los de *policyDefA*, agréguela en *initiativeDefC* para llevar un mejor seguimiento de ellos.
- Tenga en cuenta que, una vez que crea una asignación de iniciativa desde una definición de iniciativa, cualquier definición de directiva nueva que se agregue a la definición de iniciativa se implementa automáticamente en las asignaciones de iniciativa bajo esa definición de iniciativa.
- Una vez que se desencadena una asignación de una iniciativa, se desencadenarán también todas las directivas de dicha iniciativa. Aunque, si tiene que ejecutar una directiva a título individual, es mejor no incluirla en una iniciativa.

## <a name="video-overview"></a>Introducción en vídeo

La siguiente introducción de Azure Policy es de la compilación 2018. Para descargar diapositivas o el vídeo, visite [Govern your Azure environment through Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) (Gobierno de un entorno de Azure mediante Azure Policy) en Channel 9.

> [!VIDEO https://www.youtube.com/embed/dxMaYF2GB7o]

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene información general acerca de Azure Policy y algunos de los conceptos clave, se recomienda seguir estos pasos:

- [Asignación de una definición de directiva](assign-policy-definition.md)
- [Asignación de una definición de directiva con la CLI de Azure](assign-policy-definition-cli.md)
- [Asignación de una definición de directiva con PowerShell](assign-policy-definition-ps.md)
- En [Organización de los recursos con grupos de administración de Azure](../azure-resource-manager/management-groups-overview.md), obtendrá información sobre lo que es un grupo de administración.
- Vea [Govern your Azure environment through Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) (Gobierno de un entorno de Azure mediante Azure Policy) en Channel 9
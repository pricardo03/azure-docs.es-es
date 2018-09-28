---
title: Introducción a Azure Blueprint
description: Azure Blueprint es un servicio de Azure que se usa para crear, definir e implementar artefactos en el entorno de Azure.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: overview
ms.service: blueprints
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: c7ffbe86407bc776870890e5b4151556f572832e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957523"
---
# <a name="what-is-azure-blueprints"></a>¿Qué es Azure Blueprint?

Del mismo modo que un plano técnico permite a un ingeniero o a un arquitecto bosquejar los parámetros de diseño de un proyecto, Azure Blueprint permite a los arquitectos de la nube y de TI central definir un conjunto repetible de recursos de Azure que implementa y cumple los estándares de la organización, sus requisitos y sus patrones. Azure Blueprint permite a los equipos de desarrollo aprovisionar y crear rápidamente nuevos entornos sabiendo que se crean cumpliendo los estándares organizativos y que contienen un conjunto de componentes integrados, como las redes, para acelerar el desarrollo y la entrega.

Los planos técnicos son una manera declarativa de organizar la implementación de varias plantillas de recursos y de otros artefactos, como son:

- Asignaciones de roles
- Asignaciones de directiva
- Plantillas del Administrador de recursos de Azure
- Grupos de recursos

## <a name="how-it-is-different-from-resource-manager-templates"></a>En qué difiere de las plantillas de Resource Manager

Los planos técnicos están diseñados para ayudar con la _configuración del entorno_, que a menudo se compone de un conjunto de grupos de recursos, directivas y asignaciones de roles, además de las implementaciones de plantillas de Resource Manager. Un plano es un paquete para reunir cada uno de estos tipos de _artefacto_ y permite componer y crear la versión de ese paquete, incluso mediante una canalización CI/CD. En última instancia, cada uno se asigna a una suscripción en una única operación que se puede auditar y seguir.

Casi todo lo que se puede querer incluir para la implementación en un plano técnico se puede conseguir con una plantilla de Resource Manager. Sin embargo, una plantilla de Resource Manager es un documento que no existe de forma nativa en Azure, cada una se almacena localmente o en el control de origen. La plantilla se usa para las implementaciones de uno o varios recursos de Azure, pero, una vez implementados los recursos, la conexión y la relación con la plantilla utilizada se pierden.

Con Azure Blueprint, la relación entre la definición del plano técnico (lo _que debe ser implementado_) y su asignación (lo _que se implementó_) permanece. Esta conexión permite mejorar el seguimiento y la auditoría de las implementaciones y la capacidad de actualizar de una vez varias suscripciones que se rigen por el mismo plano técnico, entre otras ventajas.

No hay que elegir entre una plantilla de Resource Manager y un plano técnico. Cada instancia del plano técnico puede tener o no _artefactos_ de una plantilla de Resource Manager. Esto significa que se pueden aprovechar los trabajos anteriores para desarrollar y mantener una biblioteca de plantillas de Resource Manager en Azure Blueprint.

## <a name="how-it-is-different-from-azure-policy"></a>¿En qué difiere de Azure Policy?

Un plano técnico es un paquete o contenedor para crear conjuntos muy específicos de estándares, patrones y requisitos relacionados con la implementación de servicios en la nube de Azure, la seguridad y el diseño que se puedan reutilizar para garantizar la coherencia y el cumplimiento normativo.

Una [directiva](../policy/overview.md) es un sistema de denegación explícita y permisos predeterminados que se centra en las propiedades de recursos durante su implementación y para los recursos ya existentes. Respalda a la administración de TI al asegurarse de que los recursos dentro de una suscripción cumplan los requisitos y estándares.

La inclusión de una directiva en un plano técnico permite no solo la creación del diseño o patrón correctos durante la asignación del plano técnico, sino que garantiza que solo los cambios aprobados o previstos se puedan efectuar en el entorno para garantizar el cumplimiento continuo con la intención del plano técnico.

Una directiva puede incluirse como uno de los muchos _artefactos_ en una definición de plano técnico. Los planos técnicos también permiten usar parámetros con las directivas y las iniciativas.

## <a name="blueprint-definition"></a>Definición del plano técnico

Un plano técnico se compone de _artefactos_. Los planos técnicos actualmente admiten los siguientes recursos como artefactos:

|Recurso  | Opciones de la jerarquía| DESCRIPCIÓN  |
|---------|---------|---------|
|Grupos de recursos     | Subscription | Cree un nuevo grupo de recursos para que lo usen otros artefactos incluidos en el plano técnico.  Estos grupos de recursos de marcador de posición permiten organizar los recursos exactamente como desee que se estructuren y proporciona un limitador de ámbito para los artefactos de asignación de roles y directivas, así como plantillas de Azure Resource Manager.         |
|Plantilla del Administrador de recursos de Azure      | Grupo de recursos | Estas plantillas se pueden usar para crear entornos complejos, como una granja de servidores SharePoint, Azure Automation State Configuration o un área de trabajo de Log Analytics. |
|Asignación de directiva     | Suscripción, grupo de recursos | Permite la asignación de una directiva o iniciativa para la suscripción o el grupo de administración al que el plano técnico se asigna. La directiva o iniciativa debe estar dentro del ámbito del plano técnico (en el grupo de administración del plano técnico o por debajo). Si la directiva o iniciativa tiene parámetros, estos se pueden asignar en la creación del plano técnico o durante su asignación.       |
|Asignación de roles   | Suscripción, grupo de recursos | Agregue un grupo o usuario existente a un rol integrado para asegurarse de que las personas adecuadas siempre tienen derechos de acceso a los recursos. Las asignaciones de roles se pueden definir para toda la suscripción o anidarse para un grupo de recursos específico incluido en el plano técnico. |

### <a name="blueprints-and-management-groups"></a>Planos técnicos y grupos de administración

Al crear una definición de plano técnico, definirá dónde se guarda. Actualmente, los planos técnicos solo se pueden guardar en un [grupo de administración](../management-groups/overview.md) al que tenga acceso de **colaborador**. El plano técnico estará entonces disponible para asignar a cualquier elemento secundario (suscripción o grupo de administración) de ese grupo de administración.

> [!IMPORTANT]
> Si no tiene acceso a ningún grupo de administración o a ningún grupo de administración configurado, al cargar la lista de definiciones de planos técnicos, se mostrará que ninguno está disponible y, al hacer clic en **Ámbito**, se abrirá una ventana con una advertencia acerca de cómo recuperar los grupos de administración. Para resolver este problema, asegúrese de que una suscripción a la que tenga el acceso adecuado forme parte de un [grupo de administración](../management-groups/overview.md).

### <a name="blueprint-parameters"></a>Parámetros de plano técnico

Los planos técnicos pueden pasar parámetros a una iniciativa o directiva, o a una plantilla de Azure Resource Manager.
Cuando algún _artefacto_ se agrega a un plano técnico, el autor es capaz de decidir si proporcionar un valor definido para cada asignación de plano técnico o permitir que cada asignación del plano técnico proporcione un valor en la asignación. Esta flexibilidad permite definir un valor predeterminado para todos los usos del plano técnico o que esa decisión se tome en el momento de la asignación.

> [!NOTE]
> Un plano técnico puede tener sus propios parámetros, pero actualmente solo se pueden crear si el plano técnico se genera desde la API REST en lugar de mediante el Portal.

Para más información, consulte el artículo sobre los [parámetros de un plano técnico](./concepts/parameters.md).

### <a name="blueprint-publishing"></a>Publicación de planos técnicos

Cuando se crea por primera vez un plano técnico, se considera que está en modo **borrador**. Cuando está preparado para asignarse, se debe **publicar**. La publicación requiere la definición de una cadena de **versión** (letras, números y guiones con una longitud máxima de 20 caracteres) junto con **notas de cambio** opcionales.
La **versión** la diferencia de los cambios futuros del mismo plano técnico y permite la asignación de cada versión. Esto también significa que a la misma suscripción se le pueden asignar **versiones** diferentes del mismo plano técnico. Cuando se realizan cambios adicionales en el proyecto, la **versión** **publicada** sigue existiendo, además de los **cambios sin publicar**. Una vez completados los cambios, el plano técnico actualizado se **publica** con una **versión** nueva y única, y ahora también se puede asignar.

## <a name="blueprint-assignment"></a>Asignación de plano técnico

Cada **versión** **publicada** de un plano técnico se puede asignar a una suscripción existente. En el portal, el plano técnico tendrá de forma predeterminada la **versión** **publicada** más recientemente. Si hay parámetros de artefacto (o parámetros de plano técnico), se definirán durante el proceso de asignación.

## <a name="permissions-in-azure-blueprints"></a>Permisos de Azure Blueprint

Para utilizar planos técnicos, debe ser autorizado mediante [control de acceso basado en rol](../../role-based-access-control/overview.md) (RBAC). Para crear planos técnicos, su cuenta necesita los siguientes permisos:

- `Microsoft.Blueprint/blueprints/write`: para crear una definición de plano técnico
- `Microsoft.Blueprint/blueprints/artifacts/write`: para crear artefactos en una definición de plano técnico
- `Microsoft.Blueprint/blueprints/versions/write`: para publicar un plano técnico

Para eliminar planos técnicos, su cuenta necesita los siguientes permisos:

- `Microsoft.Blueprint/blueprints/delete`
- `Microsoft.Blueprint/blueprints/artifacts/delete`
- `Microsoft.Blueprint/blueprints/versions/delete`

> [!NOTE]
> Como las definiciones de los planos técnicos se crean en un grupo de administración, los permisos de definición se deben conceder o heredar en un ámbito de un grupo de administración.

Para asignar o cancelar la asignación de un plano técnico, la cuenta necesita los siguientes permisos:

- `Microsoft.Blueprint/blueprintAssignments/write`: para asignar un plano técnico
- `Microsoft.Blueprint/blueprintAssignments/delete`: para cancelar la asignación de un plano técnico

> [!NOTE]
> Como las asignaciones de planos técnicos se crean en una suscripción, los permisos de asignación y de cancelación de la asignación deben concederse o heredarse en el ámbito de una suscripción.

Estos permisos se incluyen en el rol **Propietario** y, a excepción de los permisos de asignación de planos técnicos, también se incluyen en los roles **Colaborador**. Si estos roles integrados no satisfacen sus necesidades de seguridad, considere la posibilidad de crear un [rol personalizado](../../role-based-access-control/custom-roles.md).

> [!NOTE]
> La entidad de servicio para Azure Blueprint requiere el rol **Propietario** en la suscripción asignada con el fin de habilitar la implementación. Si usa el portal, este rol se le concede automáticamente y se revoca para la implementación. Si usa la API REST, este rol se debe conceder manualmente, pero se sigue revocando de forma automática una vez finalizada la implementación.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de un plano técnico: portal](create-blueprint-portal.md)
- [Creación de un plano técnico: API REST](create-blueprint-rest-api.md)
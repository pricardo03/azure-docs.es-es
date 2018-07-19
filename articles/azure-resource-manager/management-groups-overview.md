---
title: Organización de los recursos con grupos de administración de Azure | Microsoft Docs
description: Obtenga información sobre los grupos de administración y cómo utilizarlos.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/09/2018
ms.author: rithorn
ms.openlocfilehash: c8152a6c12c776806d9a17c5e434d825d6c91165
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38466650"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organización de los recursos con grupos de administración de Azure

Si su organización tiene varias suscripciones, puede que necesite una manera de administrar el acceso, las directivas y el cumplimiento de esas suscripciones de forma eficaz. Los grupos de administración de Azure proporcionan un nivel de ámbito por encima de las suscripciones. Las suscripciones se organizan en contenedores llamados "grupos de administración" y aplican sus condiciones de gobierno a los grupos de administración. Todas las suscripciones dentro de un grupo de administración heredan automáticamente las condiciones que se aplican al grupo de administración. Los grupos de administración proporcionan capacidad de administración de nivel empresarial a gran escala con independencia del tipo de suscripciones que tenga.

La característica de grupo de administración está disponible en versión preliminar pública. Para empezar a usar los grupos de administración, inicie sesión en [Azure Portal](https://portal.azure.com) y busque **Grupos de administración** en la sección **Todos los servicios**.

A modo de ejemplo, puede aplicar directivas a un grupo de administración que limite las regiones disponibles para la creación de máquinas virtuales. Esta directiva se aplicaría a todos los grupos de administración, las suscripciones y los recursos de ese grupo de administración, al permitir únicamente que se creen máquinas virtuales en esa región.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Jerarquía de los grupos de administración y las suscripciones

Puede compilar una estructura flexible de grupos de administración y suscripciones para organizar los recursos en una jerarquía para una administración unificada de las directivas y el acceso.
El siguiente diagrama muestra un ejemplo de jerarquía que consta de grupos de administración y suscripciones organizados por departamentos.

![árbol](media/management-groups/MG_overview.png)

Al crear una jerarquía agrupada por departamentos, es posible asignar roles de [control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/overview.md) que *hereden* en los departamentos de ese grupo de administración. Gracias a los grupos de administración, se reduce la carga de trabajo y el riesgo de errores, ya que el rol solo se asigna una vez.

### <a name="important-facts-about-management-groups"></a>Hechos importantes acerca de los grupos de administración

- Se admiten 10 000 grupos de administración en un único directorio.
- Un árbol de grupo de administración puede admitir hasta seis niveles de profundidad.
  - Este límite no incluye el nivel raíz o de suscripción.
- Cada grupo de administración y suscripción admite solo un elemento primario.
- Cada grupo de administración puede tener varios elementos secundarios.
- Todas las suscripciones y grupos de administración están dentro de una única jerarquía en cada directorio. Consulte [Hechos importantes sobre el grupo de administración raíz](#important-facts-about-the-root-management-group) para conoce las excepciones durante la versión preliminar.

### <a name="preview-subscription-visibility-limitation"></a>Limitación de visibilidad de la suscripción de la versión preliminar

Actualmente, hay una limitación en la versión preliminar por la que no es posible ver las suscripciones para las que ha heredado el acceso. El acceso se hereda con la suscripción, pero Azure Resource Manager aún no puede asignar el acceso de herencia.  

El uso de la API REST para obtener información sobre la suscripción devuelve detalles siempre y cuando tenga acceso, pero en Azure Portal y Azure Powershell no se muestran las suscripciones.

Se está trabajando en este elemento y se resolverá antes de que los grupos de administración se anuncien con "Disponibilidad general".  

### <a name="cloud-solution-provider-csp-limitation-during-preview"></a>Limitación del Proveedor de soluciones en la nube (CSP) durante la versión preliminar

Existe una limitación actual para los asociados del Proveedor de soluciones en la nube (CSP), y es que no es posible crear ni administrar los grupos de administración de sus clientes dentro del directorio de estos.  
Se está trabajando en este elemento y se resolverá antes de que los grupos de administración se anuncien con "Disponibilidad general".

## <a name="root-management-group-for-each-directory"></a>Un grupo de administración raíz para cada directorio

Cada directorio tiene un grupo de administración de nivel superior único denominado "raíz". Este grupo de administración raíz está integrado en la jerarquía de manera que contiene todos los grupos de administración y suscripciones. Este grupo de administración raíz permite que las directivas globales y las asignaciones de control de acceso basado en rol se apliquen en el nivel de directorio. Los [administradores de directorio necesitan elevar sus privilegios](../role-based-access-control/elevate-access-global-admin.md) para ser inicialmente el propietario del grupo raíz. Una vez que el administrador es el propietario del grupo, puede asignar cualquier control de acceso basado en rol a otros usuarios o grupos del directorio para administrar la jerarquía.  

### <a name="important-facts-about-the-root-management-group"></a>Hechos importantes acerca de los grupos de administración raíz

- El nombre y el identificador del grupo de administración raíz se proporcionan de forma predeterminada. El nombre para mostrar se puede actualizar en cualquier momento para mostrarse diferente en Azure Portal.
  - El nombre será "Grupo raíz de inquilino".
  - El identificador será el de Azure Active Directory.
- El grupo de administración raíz no se puede mover ni eliminar, a diferencia de los demás.  
- Todas las suscripciones y los grupos de administración se incluyen en el grupo de administración raíz único del directorio.
  - Todos los recursos del directorio pertenecen al grupo de administración raíz para la administración global.
  - Las suscripciones nuevas pertenecen de manera predeterminada y automática al grupo de administración raíz cuando se crean.
- Todos los clientes de Azure pueden ver el grupo de administración raíz, pero no todos los clientes tienen acceso para administrar ese grupo de administración raíz.
  - Todos los usuarios con acceso a una suscripción pueden ver el contexto de dónde está esa suscripción en la jerarquía.  
  - A ninguno se le concede acceso de forma predeterminada al grupo de administración raíz. Los administradores globales de directorio son los únicos usuarios que pueden elevarse ellos mismos para obtener acceso.  Una vez que tengan acceso, los administradores de directorio pueden asignar cualquier rol RBAC a otros usuarios para la administración.  

>[!NOTE]
>Si su directorio empezó usando el servicio de administración de grupos antes del 25 de junio de 2018, el directorio podría no estar configurado con todas las suscripciones en la jerarquía. El equipo del grupo de administración actualiza con carácter retroactivo cada directorio que comenzó con grupos de administración en la versión preliminar pública antes de esa fecha en julio de 2018. Todas las suscripciones en los directorios serán elementos secundarios en el grupo de administración raíz anterior.  
>
>Si tiene preguntas sobre este proceso retroactivo, póngase en contacto con: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>Instalación inicial de los grupos de administración

Cuando algún usuario comienza usando grupos de administración, se produce un proceso de configuración inicial. El primer paso es que el grupo de administración raíz se crea en el directorio. Una vez creado este grupo, todas las suscripciones existentes que existen en el directorio se convierten en elementos secundarios del grupo de administración raíz.  El motivo de este proceso es asegurarse de que hay solo una jerarquía de grupos de administración en un directorio.  La jerarquía única dentro del directorio permite a los clientes administrativos aplicar directivas y un acceso global que otros clientes dentro del directorio no puedan omitir. Nada que se haya asignado en la raíz se aplicará a todos los grupos de administración, suscripciones, grupos de recursos y recursos dentro del directorio al tener una jerarquía dentro del directorio.  

> [!IMPORTANT]
> Todas las asignaciones de acceso de los usuarios o de directivas en el grupo de administración raíz **se aplican a todos los recursos dentro del directorio**. Por este motivo, todos los clientes deben evaluar la necesidad de tener los elementos definidos en este ámbito.  Las asignaciones de directivas y de acceso de usuario deberían ser obligatorias solo en este ámbito.  
  
## <a name="management-group-access"></a>Acceso al grupo de administración

Los grupos de administración de Azure admiten el [control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/overview.md) para todos los accesos a recursos y las definiciones de roles. Estos permisos se heredan en los recursos secundarios que existen en la jerarquía. Cualquier rol de RBAC integrado puede asignarse a un grupo de administración que heredará la jerarquía para los recursos.  Por ejemplo, el colaborador de máquina virtual del rol de RBAC puede asignarse a un grupo de administración. Este rol no tiene ninguna acción en el grupo de administración, pero se heredará en todas las máquinas virtuales de ese grupo de administración.  

El gráfico siguiente muestra la lista de roles y las acciones admitidas en los grupos de administración.

| Nombre de rol de RBAC             | Crear | Cambiar nombre | Move | Eliminar | Asignar acceso | Asignar directiva | Lectura  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Propietario                       | X      | X      | X    | X      | X             |               | X     |
|Colaborador                 | X      | X      | X    | X      |               |               | X     |
|Lector                      |        |        |      |        |               |               | X     |
|Colaborador de directivas de recursos |        |        |      |        |               | X             |       |
|Administrador de acceso de usuario   |        |        |      |        | X             |               |       |

### <a name="custom-rbac-role-definition-and-assignment"></a>Asignación y definición de roles de RBAC personalizados

Los roles de RBAC personalizados no se admiten en los grupos de administración en este momento.  Consulte el [foro de comentarios sobre los grupos de administración](https://aka.ms/mgfeedback) para ver el estado de este elemento.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los grupos de administración, consulte:

- [Creación de grupos de administración para organizar los recursos de Azure](management-groups-create.md)
- [Cambio, eliminación y administración de los grupos de administración](management-groups-manage.md)
- [Instalación del módulo Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Revisión de las especificaciones de la API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Instalación de la extensión de la CLI de Azure](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)

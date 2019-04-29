---
title: Administrar grupos de Azure Resource Manager mediante el portal de Azure | Microsoft Docs
description: Usar el portal de Azure para administrar los grupos de Azure Resource Manager.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: jgao
ms.openlocfilehash: cb1eb5ac27c53f4c0d48fe3644febc62f848486d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60551317"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-the-azure-portal"></a>Administrar grupos de recursos de Azure Resource Manager mediante el portal de Azure

Aprenda a usar el [portal Azure](https://portal.azure.com) con [Azure Resource Manager](resource-group-overview.md) para administrar los grupos de recursos de Azure. Para administrar los recursos de Azure, consulte [los recursos de administración de Azure mediante el portal de Azure](./manage-resources-portal.md).

Otros artículos acerca de cómo administrar grupos de recursos:

- [Administrar grupos de recursos de Azure mediante la CLI de Azure](./manage-resources-cli.md)
- [Administrar grupos de recursos de Azure mediante Azure PowerShell](./manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-resource-group"></a>¿Qué es un grupo de recursos?

Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. El grupo de recursos puede incluir todos los recursos de la solución o solo aquellos que se desean administrar como grupo. Para decidir cómo asignar los recursos a los grupos de recursos, tenga en cuenta lo que más conviene a su organización. Por lo general, se recomienda agregar recursos que compartan el mismo ciclo de vida al mismo grupo de recursos para que los pueda implementar, actualizar y eliminar con facilidad como un grupo.

Los grupos de recursos almacenan metadatos acerca de los recursos. Por consiguiente, al especificar la ubicación del grupo de recursos, se especifica el lugar en que se almacenan dichos metadatos. Por motivos de compatibilidad, es posible que sea preciso asegurarse de que los datos se almacenan en una región concreta.

Los grupos de recursos almacenan metadatos acerca de los recursos. Al especificar una ubicación para el grupo de recursos, especificar dónde se almacenan dichos metadatos.

## <a name="create-resource-groups"></a>Crear grupos de recursos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **grupos de recursos**

    ![agregar grupo de recursos](./media/manage-resource-groups-portal/manage-resource-groups-add-group.png)
3. Seleccione **Agregar**.
4. Escriba los siguientes valores:

   - **Suscripción**: Seleccione su suscripción a Azure. 
   - **Grupo de recursos**: Escriba un nuevo nombre de grupo de recursos. 
   - **Región**: Seleccione una ubicación de Azure, como **EE. UU**.

     ![Creación de un grupo de recursos](./media/manage-resource-groups-portal/manage-resource-groups-create-group.png)
5. Seleccione **revisar y crear**
6. Seleccione **Crear**. Tarda unos segundos para crear un grupo de recursos.
7. Seleccione **actualizar** en el menú superior para actualizar la lista de grupos de recursos y, a continuación, seleccione el grupo de recursos recién creado para abrirlo. O bien seleccione **notificación**(el icono de campana) en la parte superior y, a continuación, seleccione **vaya al grupo de recursos** para abrir el grupo de recursos recién creado

    ![Vaya al grupo de recursos](./media/manage-resource-groups-portal/manage-resource-groups-add-group-go-to-resource-group.png)

## <a name="list-resource-groups"></a>Enumeración de grupos de recursos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Para obtener una lista de los grupos de recursos, seleccione **grupos de recursos**

    ![buscar grupos de recursos](./media/manage-resource-groups-portal/manage-resource-groups-list-groups.png)

3. Para personalizar la información mostrada para los grupos de recursos, seleccione **Editar columnas**. Captura de pantalla siguiente muestra las columnas de la suma que se pudo agregar a la pantalla:

## <a name="open-resource-groups"></a>Grupos de recursos abiertos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Grupos de recursos**.
3. Seleccione el grupo de recursos que desea abrir.

## <a name="delete-resource-groups"></a>Eliminar grupos de recursos

1. Abra el grupo de recursos que desea eliminar.  Consulte [abrir grupos de recursos](#open-resource-groups).
2. Seleccione **Eliminar grupo de recursos**.

    ![Eliminar grupo de recursos de azure](./media/manage-resource-groups-portal/delete-group.png)

Para obtener más información acerca de cómo Azure Resource Manager ordena la eliminación de recursos, consulte [eliminación del grupo de recursos de Azure Resource Manager](./resource-group-delete.md).

## <a name="deploy-resources-to-a-resource-group"></a>Implementar recursos en un grupo de recursos

Después de haber creado una plantilla de Resource Manager, puede usar el portal de Azure para implementar los recursos de Azure. Para crear una plantilla, consulte [inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md). Para implementar una plantilla mediante el portal, consulte [implementar recursos con plantillas de Resource Manager y Azure portal](resource-group-template-deploy-portal.md).

## <a name="move-to-another-resource-group-or-subscription"></a>Mover a otro grupo de recursos o suscripción

Puede mover los recursos en el grupo a otro grupo de recursos. Para obtener más información, consulte [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](resource-group-move-resources.md).

## <a name="lock-resource-groups"></a>Grupos de recursos de bloqueo

El bloqueo impide que otros usuarios de su organización eliminen o modifiquen los recursos críticos, como la suscripción de Azure, el grupo de recursos o recurso accidentalmente. 

1. Abra el grupo de recursos que desea eliminar.  Consulte [abrir grupos de recursos](#open-resource-groups).
2. En el panel izquierdo, seleccione **bloquea**.
3. Para agregar un bloqueo al grupo de recursos, seleccione **agregar**.
4. Escriba **nombre del bloqueo**, **tipo de bloqueo**, y **notas**. Incluyen los tipos de bloqueo **de sólo lectura**, y **eliminar**.

    ![grupo de recursos de bloqueo](./media/manage-resource-groups-portal/manage-resource-groups-add-lock.png)

Para obtener más información, consulte [bloqueo de recursos para impedir cambios inesperados](./resource-group-lock-resources.md).

## <a name="tag-resource-groups"></a>Grupos de recursos de etiqueta

Puede aplicar etiquetas a los recursos y grupos de recursos para organizar de manera lógica los recursos. Para obtener información, consulte [mediante etiquetas para organizar los recursos de Azure](./resource-group-using-tags.md#portal).

## <a name="export-resource-groups-to-templates"></a>Exportar grupos de recursos en plantillas

Después de configurar correctamente el grupo de recursos, puede ver la plantilla de Resource Manager para el grupo de recursos. Exportar la plantilla ofrece dos ventajas:

- Automatice las implementaciones futuras de la solución porque la plantilla contiene toda la infraestructura completa.
- Obtenga información sobre la sintaxis de la plantilla mediante la búsqueda en JavaScript Object Notation (JSON) que representa la solución.

Hay dos maneras de exportar una plantilla:

- Puede exportar la plantilla real que se utiliza para la implementación. La plantilla exportada incluye todos los parámetros y variables exactamente como aparecían en la plantilla original. Este enfoque es útil cuando se implementan recursos a través del portal y se desea ver la plantilla para crearlos. Esta plantilla es fácil de usar. 
- Puede exportar una plantilla generada que representa el estado actual del grupo de recursos. La plantilla exportada no se basa en ninguna plantilla que se haya utilizado para la implementación. Al contrario, crea una plantilla que es una "instantánea" o "copia de seguridad" del grupo de recursos. La plantilla exportada tiene muchos valores codificados de forma rígida y es probable que no tenga tantos parámetros como normalmente se definirían. Use esta opción para volver a implementar recursos en el mismo grupo de recursos. Para utilizar esta plantilla para otro grupo de recursos, es posible que tenga que modificarla de forma significativa.

### <a name="export-templates-from-deployment-history"></a>Exportar plantillas desde el historial de implementación

Este método exporta las plantillas para algunas implementaciones. Si han cambiado los recursos desde el portal o a agregar o quitar recursos en varias implementaciones, consulte [exportar plantillas de grupos de recursos](#export-templates-from-resource-groups).

1. Abra el grupo de recursos que desea exportar.  Consulte [abrir grupos de recursos](#open-resource-groups).
2. En el panel izquierdo, seleccione **Implementaciones** o seleccione el vínculo situado bajo **Implementaciones**.  En la captura de pantalla siguiente se muestra **Succeeded 4** porque había cuatro implementaciones separadas con cuatro nombres de implementación diferentes. Es posible que vea **1 correcto**.

    ![plantillas de exportación del grupo de recursos de Azure](./media/manage-resource-groups-portal/manage-resource-groups-export-templates-deployment-history.png)

3. Seleccione una de las implementaciones de la lista.
4. En el panel izquierdo, seleccione **plantilla**. Resource Manager recupera los seis archivos siguientes:

   - **Plantilla** : la plantilla que define la infraestructura de la solución. Cuando creó la cuenta de almacenamiento por medio del portal, Resource Manager usó una plantilla para implementarla y la guardó para futura referencia.
   - **Parámetros**: un archivo de parámetros que puede usar para pasar valores durante la implementación. Contiene los valores que proporcionó en la primera implementación. Todos estos valores se pueden cambiar al volver a implementar la plantilla.
   - **CLI**: archivo de script de la CLI de Azure que se puede usar para implementar la plantilla.
   - **PowerShell** : un archivo de script de Azure PowerShell que puede usar para implementar la plantilla.
   - **.NET** : una clase .NET que puede utilizar para implementar la plantilla.
   - **Ruby** : una clase Ruby que puede utilizar para implementar la plantilla.

     De forma predeterminada, el portal muestra la plantilla.

5. Seleccione **descargar** para exportar una plantilla en el equipo local.

    ![plantillas de exportación del grupo de recursos de Azure](./media/manage-resource-groups-portal/manage-resource-groups-export-templates-deployment-history-download.png)

<a name="export-templates-from-resource-groups"></a>
### <a name="export-templates-from-resource-groups"></a>Exportación de plantillas de grupos de recursos

Si ha cambiado los recursos desde el portal, o agregar o quitar recursos en varias implementaciones, la recuperación de una plantilla desde el historial de implementaciones no refleja el estado actual del grupo de recursos. En esta sección se muestra cómo exportar una plantilla que refleja el estado actual del grupo de recursos. Se ha diseñado como una instantánea del grupo de recursos, el cual puede usar para volver a implementar en el mismo grupo de recursos. Para usar la plantilla exportada para otras soluciones, debe modificarla de forma significativa.

1. Abra el grupo de recursos que desea exportar.  Consulte [abrir grupos de recursos](#open-resource-groups).
2. En el panel izquierdo, seleccione **Exportar plantilla**. Resource Manager recupera los seis archivos siguientes:

   - **Plantilla** : la plantilla que define la infraestructura de la solución. Cuando creó la cuenta de almacenamiento por medio del portal, Resource Manager usó una plantilla para implementarla y la guardó para futura referencia.
   - **Parámetros**: un archivo de parámetros que puede usar para pasar valores durante la implementación. Contiene los valores que proporcionó en la primera implementación. Todos estos valores se pueden cambiar al volver a implementar la plantilla.
   - **CLI**: archivo de script de la CLI de Azure que se puede usar para implementar la plantilla.
   - **PowerShell** : un archivo de script de Azure PowerShell que puede usar para implementar la plantilla.
   - **.NET** : una clase .NET que puede utilizar para implementar la plantilla.
   - **Ruby** : una clase Ruby que puede utilizar para implementar la plantilla.

     De forma predeterminada, el portal muestra la plantilla.
3. Seleccione **descargar** para exportar una plantilla en el equipo local.

Algunas plantillas exportadas necesitan algunas modificaciones para que se puedan usar. Para obtener información sobre cómo desarrollar las plantillas, consulte el [tutoriales paso a paso](/azure/azure-resource-manager/).

### <a name="export-template-before-deploying"></a>Exportar plantilla antes de implementar

Puede usar el portal para definir un recurso.  Antes de implementar el recurso, puede ver y exportar una plantilla. Para ver las instrucciones, consulte [Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md).

### <a name="fix-export-issues"></a>Solución de problemas de exportación

No todos los tipos de recursos admiten la función de exportación de plantilla. Solo se producirán problemas de exportación si esta se realiza desde un grupo de recursos y no desde el historial de implementación. Si su última implementación representa con precisión el estado actual del grupo de recursos, debe exportar la plantilla desde el historial de implementación en lugar de desde el grupo de recursos. Realice la exportación desde un grupo de recursos únicamente si ha realizado cambios en el grupo de recursos que no están definidos en una única plantilla.

Para evitar los problemas de exportación, agregue manualmente los recursos que faltan a la plantilla. El mensaje de error incluye los tipos de recursos que no se pueden exportar. Busque el tipo de recurso en [Referencia de plantilla](/azure/templates/). Por ejemplo, para agregar manualmente una puerta de enlace de red virtual, consulte [Microsoft.Network/virtualNetworkGateways template reference](/azure/templates/microsoft.network/virtualnetworkgateways) (Referencia de plantilla Microsoft.Network/virtualNetworkGateways). La referencia a la plantilla proporciona el código JSON necesario para agregar el recurso a la plantilla.

Después de obtener el formato JSON del recurso, debe obtener los valores del recurso. Puede ver los valores del recurso utilizando la operación GET en la API REST del tipo de recurso. Por ejemplo, para obtener los valores de la puerta de enlace de red virtual, consulte [Virtual Network Gateways - Get](/rest/api/network-gateway/virtualnetworkgateways/get) (Puertas de enlace de red virtual: Get).

## <a name="manage-access-to-resource-groups"></a>Administrar el acceso a los grupos de recursos

El [control de acceso basado en rol (RBAC)](../role-based-access-control/overview.md) es la forma en que se administra el acceso a los recursos en Azure. Para saber más, vea [Administración del acceso mediante RBAC y Azure Portal](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre Azure Resource Manager, consulte [Introducción a Azure Resource Manager](./resource-group-overview.md).
- Para obtener información sobre la sintaxis de la plantilla de Resource Manager, consulte [entender la estructura y sintaxis de plantillas de Azure Resource Manager](./resource-group-authoring-templates.md).
- Para obtener información sobre cómo desarrollar las plantillas, consulte el [tutoriales paso a paso](/azure/azure-resource-manager/).
- Para ver los esquemas de plantilla de Azure Resource Manager, consulte [referencia de plantilla](/azure/templates/).
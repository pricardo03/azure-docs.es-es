---
title: Administrar recursos de Azure mediante el portal de Azure | Microsoft Docs
description: Use el portal de Azure y Azure Resource Manager para administrar los recursos.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 20bf38b87ce29f8506a5611ecd25cf38f6d4ed61
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60550757"
---
# <a name="manage-azure-resources-by-using-the-azure-portal"></a>Administrar recursos de Azure mediante el portal de Azure

Aprenda a usar el [portal Azure](https://portal.azure.com) con [Azure Resource Manager](resource-group-overview.md) para administrar los recursos de Azure. Para administrar grupos de recursos, consulte [grupos de recursos de administración de Azure mediante el portal de Azure](./manage-resource-groups-portal.md).

Otros artículos sobre la administración de recursos:

- [Administrar recursos de Azure mediante la CLI de Azure](./manage-resources-cli.md)
- [Administrar recursos de Azure mediante Azure PowerShell](./manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="deploy-resources-to-a-resource-group"></a>Implementar recursos en un grupo de recursos

Después de haber creado una plantilla de Resource Manager, puede usar el portal de Azure para implementar los recursos de Azure. Para crear una plantilla, consulte [inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md). Para implementar una plantilla mediante el portal, consulte [implementar recursos con plantillas de Resource Manager y Azure portal](resource-group-template-deploy-portal.md).

## <a name="open-resources"></a>Recursos abiertos

Recursos de Azure se organizan por servicios de Azure y grupos de recursos. Los procedimientos siguientes se muestra cómo abrir una cuenta de almacenamiento denominada **mystorage0207**. La máquina virtual reside en un grupo de recursos denominado **mystorage0207rg**.

Para abrir un recurso de tipo de servicio:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En el panel izquierdo, seleccione el servicio de Azure. En este caso, **cuentas de almacenamiento**.  Si no ve el servicio que aparece, seleccione **todos los servicios**y, a continuación, seleccione el tipo de servicio.

    ![Abra el recurso de azure en el portal](./media/manage-resources-portal/manage-azure-resources-portal-open-service.png)

3. Seleccione el recurso que desea abrir.

    ![Abra el recurso de azure en el portal](./media/manage-resources-portal/manage-azure-resources-portal-open-resource.png)

    Una cuenta de almacenamiento tiene el siguiente aspecto:

    ![Abra el recurso de azure en el portal](./media/manage-resources-portal/manage-azure-resources-portal-open-resource-storage.png)

Para abrir un recurso por grupo de recursos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En el panel izquierdo, seleccione **grupos de recursos** para enumerar los recursos dentro del grupo.
3. Seleccione el recurso que desea abrir. 

## <a name="manage-resources"></a>Administración de recursos

Cuando vea un recurso en el portal, verá las opciones de administración de ese recurso concreto.

![administrar recursos de Azure](./media/manage-resources-portal/manage-azure-resources-portal-manage-resource.png)

La captura de pantalla muestra las opciones de administración para una máquina virtual de Azure. Puede realizar operaciones como iniciar, reiniciar y detener una máquina virtual.

## <a name="delete-resources"></a>Eliminar recursos

1. Abra el recurso en el portal. Para conocer los pasos, consulte [abrir recursos](#open-resources).
2. Seleccione **Eliminar**. Captura de pantalla siguiente muestra las opciones de administración para una máquina virtual.

    ![eliminar recursos de azure](./media/manage-resources-portal/manage-azure-resources-portal-delete-resource.png)
3. Escriba el nombre del recurso para confirmar la eliminación y, a continuación, seleccione **eliminar**.

Para obtener más información acerca de cómo Azure Resource Manager ordena la eliminación de recursos, consulte [eliminación del grupo de recursos de Azure Resource Manager](./resource-group-delete.md).

## <a name="move-resources"></a>Traslado de recursos

1. Abra el recurso en el portal. Para conocer los pasos, consulte [abrir recursos](#open-resources).
2. Seleccione **Mover**. Captura de pantalla siguiente muestra las opciones de administración para una cuenta de almacenamiento.

    ![mover recursos de azure](./media/manage-resources-portal/manage-azure-resources-portal-move-resource.png)
3. Seleccione **mover a otro grupo de recursos** o **Moeve a otra suscripción** según sus necesidades.

Para obtener más información, consulte [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](resource-group-move-resources.md).

## <a name="lock-resources"></a>Bloqueo de recursos

El bloqueo impide que otros usuarios de su organización eliminen o modifiquen los recursos críticos, como la suscripción de Azure, el grupo de recursos o recurso accidentalmente. 

1. Abra el recurso en el portal. Para conocer los pasos, consulte [abrir recursos](#open-resources).
2. Seleccione **bloquea**. Captura de pantalla siguiente muestra las opciones de administración para una cuenta de almacenamiento.

    ![recurso de bloqueo de azure](./media/manage-resources-portal/manage-azure-resources-portal-lock-resource.png)
3. Seleccione **agregar**y, a continuación, especifique las propiedades de bloqueo.

Para obtener más información, consulte [Bloqueo de recursos con el Administrador de recursos de Azure](resource-group-lock-resources.md).

## <a name="tag-resources"></a>Etiquetado de recursos

El etiquetado ayuda a organizar lógicamente de su grupo de recursos. 

1. Abra el recurso en el portal. Para conocer los pasos, consulte [abrir recursos](#open-resources).
2. Seleccione **Etiquetas**. Captura de pantalla siguiente muestra las opciones de administración para una cuenta de almacenamiento.

    ![recursos de la etiqueta de azure](./media/manage-resources-portal/manage-azure-resources-portal-tag-resource.png)
3. Especificar las propiedades de etiqueta y, a continuación, seleccione **guardar**.

Para obtener información, consulte [mediante etiquetas para organizar los recursos de Azure](./resource-group-using-tags.md#portal).

## <a name="monitor-resources"></a>Supervisión de recursos

Al abrir un recurso, el portal presenta predeterminada gráficos y tablas para la supervisión de ese tipo de recurso. Captura de pantalla siguiente muestra los gráficos para una máquina virtual:

![recursos de azure Monitor](./media/manage-resources-portal/manage-azure-resources-portal-monitor-resource.png)

Puede seleccionar el icono de anclaje en la esquina superior derecha de los gráficos para anclar el gráfico al panel. Para aprender a trabajar con paneles, vea el vídeo [Creación y uso compartido de paneles en Azure Portal](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-access-to-resources"></a>Administrar el acceso a los recursos

El [control de acceso basado en rol (RBAC)](../role-based-access-control/overview.md) es la forma en que se administra el acceso a los recursos en Azure. Para saber más, vea [Administración del acceso mediante RBAC y Azure Portal](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre Azure Resource Manager, consulte [Introducción a Azure Resource Manager](./resource-group-overview.md).
- Para obtener información sobre la sintaxis de la plantilla de Resource Manager, consulte [entender la estructura y sintaxis de plantillas de Azure Resource Manager](./resource-group-authoring-templates.md).
- Para obtener información sobre cómo desarrollar las plantillas, consulte el [tutoriales paso a paso](/azure/azure-resource-manager/).
- Para ver los esquemas de plantilla de Azure Resource Manager, consulte [referencia de plantilla](/azure/templates/).
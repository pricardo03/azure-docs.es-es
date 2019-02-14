---
title: Iniciar una revisión de acceso para los roles de recurso de Azure en PIM | Microsoft Docs
description: Aprenda a iniciar una revisión de acceso para los roles de recurso de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f4ebf9ec08915a557f897a743cac1160d7e6823
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173443"
---
# <a name="start-an-access-review-for-azure-resource-roles-in-pim"></a>Iniciar una revisión de acceso para los roles de recurso de Azure en PIM
Las asignaciones de roles se convierten en "obsoletas" cuando los usuarios tienen acceso con privilegios que ya no necesitan. Para reducir el riesgo asociado a estas asignaciones de roles obsoletos, los administradores de roles con privilegios deben revisar regularmente los roles. En este documento se describen los pasos para iniciar una revisión de acceso en Privileged Identity Management (PIM) para Azure Resources.

Desde la página principal de la aplicación de PIM, vaya a:

* **Revisiones de acceso** > **Agregar**

![Agregar revisiones de acceso](media/azure-pim-resource-rbac/rbac-access-review-home.png)

Al seleccionar el botón **Agregar**, aparece la hoja **Create an access review** (Crear una revisión de acceso). En esta hoja, configure la revisión con un nombre y un límite de tiempo, elija el rol que desea revisar y decida quién lleva a cabo la revisión.

![Creación de una revisión de acceso](media/azure-pim-resource-rbac/rbac-create-access-review.png)

### <a name="configure-the-review"></a>Configuración de la revisión
Para crear una revisión de acceso, en primer lugar asígnele un nombre y luego establezca una fecha de inicio y de finalización.

![Configuración de una revisión: captura de pantalla](media/azure-pim-resource-rbac/rbac-access-review-setting-1.png)

Asegúrese de que la longitud de la revisión sea lo suficientemente larga para que los usuarios la completen. Si finalizan antes de la fecha de finalización, siempre pueden detener la revisión pronto.

### <a name="choose-a-role-to-review"></a>Elija un rol para su revisión
Cada revisión se centra solo en un rol. A menos que iniciara la revisión de acceso desde una hoja de rol específica, debe elegir un rol ahora.

1. Vaya a **Pertenencia a rol de revisión**
   
    ![Revisión de pertenencia al rol: captura de pantalla](media/azure-pim-resource-rbac/rbac-access-review-setting-2.png)
2. Elija un rol de la lista.

### <a name="decide-who-will-perform-the-review"></a>Decida quién llevará a cabo la revisión
Hay tres opciones para realizar una revisión. Puede asignar la revisión a otra persona para que la complete, puede hacerla usted mismo cada usuario puede revisar su propio acceso.

1. Elija una de las opciones:
   
   * **Usuarios seleccionados**: use esta opción cuando no sepa quién necesita acceso. Con esta opción, puede asignar la revisión a un propietario de recursos o al administrador de grupos.
   * **Asignado (autoasignado)** : use esta opción para que los usuarios revisen sus propias asignaciones de roles.
   
2. Vaya a **Seleccionar revisores**.
   
    ![Selección de revisores: captura de pantalla](media/azure-pim-resource-rbac/rbac-access-review-setting-3.png)

### <a name="start-the-review"></a>Inicio de la revisión
Por último, puede requerir que los usuarios especifiquen un motivo para la aprobación del acceso. Si lo desea, agregue una descripción de la revisión. Luego, seleccione **Iniciar**.

Asegúrese de que permite que los usuarios sepan que les espera una revisión de acceso y muéstreles [cómo realizar una revisión de acceso](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Administración de la revisión de acceso
En el panel de PIM para recursos de Azure, puede hacer un seguimiento del progreso de las revisiones a medida que los revisores las completan. No se cambia ningún derecho de acceso en el directorio hasta que [la revisión haya finalizado](pim-resource-roles-complete-access-review.md).

Hasta que termine el período de revisión, puede recordar a los usuarios completar su revisión o detener la revisión antes desde la sección de revisiones de acceso.

## <a name="next-steps"></a>Pasos siguientes

- [Completar una revisión de acceso para los roles de recurso de Azure en PIM](pim-resource-roles-complete-access-review.md)
- [Realización de una revisión de acceso para los roles de recurso de Azure en PIM](pim-resource-roles-perform-access-review.md)
- [Iniciar una revisión de acceso para los roles de directorio de Azure AD en PIM](pim-how-to-start-security-review.md)

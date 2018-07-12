---
title: Inicio de una revisión del acceso | Microsoft Docs
description: Aprenda a crear una revisión de acceso para identidades con privilegios con la aplicación Privileged Identity Management de Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 3ead2fe01c932c76a83e989d8908de4c9bfa541b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38506425"
---
# <a name="how-to-start-an-access-review-in-azure-ad-privileged-identity-management"></a>Inicio de una revisión de acceso en Privileged Identity Management de Azure AD
Las asignaciones de roles se convierten en "obsoletas" cuando los usuarios tienen acceso con privilegios que ya no necesitan. A fin de reducir el riesgo asociado a estas asignaciones de roles obsoletos, los administradores de roles con privilegios o los administradores globales deben revisar regularmente los accesos para así poder supervisar los roles que se han concedido a los usuarios. En este documento se describen los pasos para iniciar una revisión de acceso en Azure AD Privileged Identity Management (PIM).

## <a name="start-an-access-review"></a>Inicio de una revisión de acceso
> [!NOTE]
> Si no ha agregado la aplicación PIM al panel de Azure Portal, consulte los pasos que se indican en [Introducción a Privileged Identity Management de Azure AD](pim-getting-started.md)
> 
> 

En la página principal de la aplicación PIM, hay tres formas de comenzar una revisión de acceso:

* **Revisiones de acceso** > **Agregar**
* Botón **Roles** > **Revisar**
* Seleccionar el rol específico que se va a revisar en la lista de roles > Botón **Revisar**

Al hacer clic en el botón **Revisar**, aparece la hoja **Iniciar revisión de acceso**. En esta hoja, va a configurar la revisión con un nombre y un límite de tiempo, elija un rol para revisar y decida quién llevará a cabo la revisión.

![Inicio de una revisión de acceso: captura de pantalla](./media/pim-how-to-start-security-review/PIM_start_review.png)

### <a name="configure-the-review"></a>Configuración de la revisión
Para crear una revisión de acceso, necesitará nombrarla y establecer una fecha de inicio y finalización.

![Configuración de una revisión: captura de pantalla](./media/pim-how-to-start-security-review/PIM_review_configure.png)

Asegúrese de que la longitud de la revisión sea lo suficientemente larga para que los usuarios la completen. Si finaliza antes de la fecha de finalización, siempre puede detener pronto la revisión.

### <a name="choose-a-role-to-review"></a>Elija un rol para su revisión
Cada revisión se centra solo en un rol. A menos que iniciara la revisión de acceso desde una hoja de rol específica, deberá elegir un rol ahora.

1. Vaya a **Revisar la pertenencia al rol**
   
    ![Revisión de pertenencia al rol: captura de pantalla](./media/pim-how-to-start-security-review/PIM_review_role.png)
2. Elija un rol de la lista.

### <a name="decide-who-will-perform-the-review"></a>Decida quién llevará a cabo la revisión
Hay tres opciones para realizar una revisión. Puede asignar la revisión a otra persona para que la complete, puede hacerlo usted mismo o hacer que cada usuario revise su propio acceso.

1. Vaya a **Seleccionar revisores**
   
    ![Selección de revisores: captura de pantalla](./media/pim-how-to-start-security-review/PIM_review_reviewers.png)
2. Elija una de las opciones:
   
   * **Seleccionar revisor**: utilice esta opción si no sabe quién requiere acceso. Con esta opción, puede asignar la revisión a un propietario de recursos o al administrador de grupos.
   * **Yo**: resulta útil si desea obtener una vista previa de cómo funcionan las revisiones de acceso o desea revisar en nombre de personas que no pueden hacerlo.
   * **Revisión de los propios miembros**: utilice esta opción para que los usuarios revisen ellos mismos sus propias asignaciones de roles.

### <a name="start-the-review"></a>Inicio de la revisión
Por último, tiene la opción de requerir que los usuarios proporcionen un motivo si aprueban su acceso. Agregue una descripción de la revisión si lo desea y seleccione **Iniciar**.

Asegúrese de que permiten a los usuarios saber que hay una revisión de acceso esperando para ellos y [cómo realizar una revisión de acceso](pim-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Administración de la revisión de acceso
Puede seguir el progreso de las revisiones a medida que los revisores las realizan en el panel de PIM de Azure AD, en la sección de revisiones de acceso. Los derechos de acceso no se cambian en el directorio hasta que [la revisión finaliza](pim-how-to-complete-review.md).

Hasta que termine el período de revisión, puede recordar a los usuarios completar su revisión o detener la revisión antes desde la sección de revisiones de acceso.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="pim-table-of-contents"></a>Tabla de contenido de PIM
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]

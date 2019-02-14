---
title: Seguimiento del uso de un laboratorio en Azure Lab Services | Microsoft Docs
description: En este tutorial, realizará el seguimiento del uso de un laboratorio como su creador o propietario.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: e2831191905da1b9e0ad55131be9eaa7aa13950e
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55894367"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Tutorial: Seguimiento del uso de un laboratorio en Azure Lab Services
Este tutorial muestra cómo un creador o propietario de un laboratorio puede realizar un seguimiento de su uso.

En este tutorial realizará lo siguiente:

> [!div class="checklist"]
> * Ver los usuarios registrados con el laboratorio
> * Visualización del uso de las máquinas virtuales en el laboratorio
> * Administración de máquinas virtuales de estudiantes 


## <a name="view-users-registered-with-the-lab"></a>Ver los usuarios registrados en el laboratorio

1. Vaya al [sitio web de Azure Lab Services](https://labs.azure.com). 
2. Seleccione **Iniciar sesión** y escriba las credenciales. Azure Lab Services es compatible con cuentas profesionales y cuentas Microsoft.
3. En la página **My labs** (Mis laboratorios), seleccione el laboratorio de cuyo uso desea realizar un seguimiento. 
4. Seleccione **Users** (Usuarios) en el menú izquierdo o el icono **Usuarios**. Verá los alumnos que se han registrado en el laboratorio. Seleccione **Registration link** (Vínculo de registro), copie el vínculo y envíelo a cualquier nuevo alumno que no se haya registrado aún en el laboratorio. 

    ![Usuarios registrados](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Visualización del uso de las máquinas virtuales en el laboratorio 

1. Seleccione **Máquinas virtuales** en el menú de la izquierda. 
2. Confirme que ve el estado de las máquinas virtuales y el número de horas que han estado en ejecución. El tiempo que el propietario de un laboratorio emplea en una máquina virtual de un alumno no cuenta para el tiempo de uso que se muestra en la última columna. 

    ![Uso de máquinas virtuales](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Administración de máquinas virtuales de estudiantes 
Cuando mantenga el mouse sobre una fila en la lista de máquinas virtuales, verá los controles para realizar las siguientes tareas (como se muestra en la imagen de la sección anterior): 

- Conexión a una máquina virtual
- Inicio de una máquina virtual
- Detención de una máquina virtual
- Eliminación de una máquina virtual


![Controles de máquinas virtuales](../media/tutorial-track-usage/vm-controls.png)

También puede usar los botones de barra de herramientas para iniciar, detener o eliminar una máquina virtual. 



## <a name="next-steps"></a>Pasos siguientes
Para más información sobre los laboratorios educativos, consulte los artículos enumerados en [Guías de procedimientos](how-to-manage-lab-accounts.md).

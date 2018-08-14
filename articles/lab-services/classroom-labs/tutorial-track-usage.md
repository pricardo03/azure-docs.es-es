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
ms.date: 07/23/2018
ms.author: spelluru
ms.openlocfilehash: 710d157dcf4c6d060e59bcfbb69455e2ddc91bdd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39450137"
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
4. Seleccione la pestaña **Users** (Usuarios). Verá los alumnos que se han registrado en el laboratorio. Seleccione **Registration link** (Vínculo de registro), copie el vínculo y envíelo a cualquier nuevo alumno que no se haya registrado aún en el laboratorio. 

    ![Usuarios registrados](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Visualización del uso de las máquinas virtuales en el laboratorio 

1. Seleccione **Máquinas virtuales** en el menú de la izquierda. 
2. Confirme que ve el estado de las máquinas virtuales y el número de horas que han estado en ejecución. El tiempo que emplea en una máquina virtual de un alumno no se cuenta con respecto al tiempo de uso que se muestra en la última columna. 

    ![Uso de máquinas virtuales](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Administración de máquinas virtuales de estudiantes 
A medida que mantenga el mouse sobre una fila en la lista de máquinas virtuales, verá los controles para realizar las tareas siguientes: 

- Conexión a una máquina virtual
- Inicio de una máquina virtual
- Detención de una máquina virtual
- Eliminación de una máquina virtual

![Controles de máquinas virtuales](../media/tutorial-track-usage/vm-controls.png) 



## <a name="next-steps"></a>Pasos siguientes
En este tutorial, aprendió a encontrar los usuarios que se han registrado en el laboratorio, a supervisar el uso de máquinas virtuales en el laboratorio y a administrarlas.

Para más información acerca de los laboratorios educativos, consulte los temas enumerados en [Guías de procedimientos](how-to-manage-lab-accounts.md).

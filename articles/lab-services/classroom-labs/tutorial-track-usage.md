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
ms.date: 10/18/2019
ms.author: spelluru
ms.openlocfilehash: 842392ab425628a1c82a39e25a65066064747211
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2019
ms.locfileid: "72675736"
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
En esta página, puede iniciar, detener o restablecer las máquinas virtuales de los alumnos mediante la lista desplegable de la columna **Estado** o los botones de la barra de herramientas. 

![Controles de máquinas virtuales](../media/tutorial-track-usage/vm-controls.png)

También puede usar los botones de barra de herramientas para iniciar, detener o eliminar una máquina virtual. 


## <a name="next-steps"></a>Pasos siguientes
Para más información sobre los laboratorios educativos, consulte los artículos enumerados en [Guías de procedimientos](how-to-manage-lab-accounts.md).

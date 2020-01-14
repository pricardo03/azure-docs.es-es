---
title: Cómo agregar un usuario como propietario de un laboratorio en Azure Lab Services
description: En este artículo se muestra cómo un administrador puede agregar un usuario como propietario de un laboratorio.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2019
ms.author: spelluru
ms.openlocfilehash: 9e007ee6c95843b7e8ef9d7a701237034811489e
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475254"
---
# <a name="how-to-add-a-user-as-an-owner-of-a-classroom-lab-in-azure-lab-services"></a>Cómo agregar un usuario como propietario de un laboratorio de clase en Azure Lab Services
En este artículo se muestra cómo agregar un usuario como propietario de un laboratorio en Azure Lab Services.

## <a name="add-user-to-the-reader-role-for-the-lab-account"></a>Adición de un usuario al rol de lector de la cuenta de laboratorio
1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el menú de la izquierda. Busque **Lab Services** y selecciónelo.
3. Seleccione la **cuenta de laboratorio** en la lista. 
2. En la **página Cuenta de laboratorio**, seleccione **Control de acceso (IAM)** en el menú izquierdo. 
2. En la página **Control de acceso (IAM)** , seleccione **Agregar** en la barra de herramientas y, a continuación, **Agregar asignación de roles**.

    ![Asignación de roles para la cuenta de laboratorio ](../media/how-to-add-user-lab-owner/lab-account-access-control-page.png)
3. En la página **Agregar una asignación de roles**, siga estos pasos: 
    1. Seleccione **Lector** para el **rol**. 
    2. Seleccione el usuario. 
    3. Seleccione **Guardar**. 

        ![Adición de un usuario al rol de lector de la cuenta de laboratorio ](../media/how-to-add-user-lab-owner/reader-lab-account.png)

## <a name="add-user-to-the-owner-role-for-the-lab"></a>Adición de un usuario al rol de propietario del laboratorio

1. En la página **Cuenta de laboratorio**, seleccione **Todos los laboratorios** en el menú de la izquierda.
2. Seleccione el **laboratorio** al que quiere agregar el usuario como propietario. 
    
    ![Selección del laboratorio ](../media/how-to-add-user-lab-owner/select-lab.png)    
3. En la página **Laboratorio**, seleccione **Control de acceso (IAM)** en el menú de la izquierda.
4. En la página **Control de acceso (IAM)** , seleccione **Agregar** en la barra de herramientas y, a continuación, **Agregar asignación de roles**.
5. En la página **Agregar una asignación de roles**, siga estos pasos: 
    1. Seleccione **Propietario** para el **rol**. 
    2. Seleccione el usuario. 
    3. Seleccione **Guardar**. 

## <a name="next-steps"></a>Pasos siguientes
Confirme que el usuario ve el laboratorio al iniciar sesión en el [portal de Lab Services](https://labs.azure.com).
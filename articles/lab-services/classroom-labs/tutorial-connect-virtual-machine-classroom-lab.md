---
title: Acceso a un laboratorio educativo en Azure Lab Services | Microsoft Docs
description: En este tutorial, va a acceder a máquinas virtuales en un laboratorio educativo configurado por un profesor.
services: devtest-lab, lab-services, virtual-machines
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
ms.openlocfilehash: 1328835714086dcec71b0e9dd4d1916794f557a6
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54390182"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Tutorial: Acceso a un laboratorio de clase en Azure Lab Services
En este tutorial, se conectará como alumno a una máquina virtual de un laboratorio educativo. 

En este tutorial realizará lo siguiente:

> [!div class="checklist"]
> * Usar un vínculo de registro 
> * Conexión a la máquina virtual

## <a name="use-the-registration-link"></a>Uso del vínculo de registro

1. Vaya a la **dirección URL de registro** que recibió del profesor o educador. 
2. Inicie sesión en el servicio con su cuenta organizativa para completar el registro. 
3. Una vez registrado, confirme que ve las máquinas virtuales de los laboratorios a los que tiene acceso. 
2. Espere hasta que la máquina virtual esté lista y, a continuación, **iníciela**. Este proceso tarda algún tiempo.  

    ![Inicio de la máquina virtual](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)

## <a name="connect-to-the-virtual-machine"></a>Conexión a la máquina virtual

1. Seleccione **Conectar** en el icono de la máquina virtual del laboratorio al que quiere acceder. 

    ![Conexión con una máquina virtual](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Guarde el archivo RDP en el disco duro y ábralo (suponiendo que sea una máquina virtual Windows).
3. Utilice el **nombre de usuario** y la **contraseña** que obtiene de su profesor o educador para iniciar sesión en la máquina. 

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha accedido a un laboratorio educativo de mediante el vínculo de registro que le proporcionó su profesor o instructor.

Como propietario del laboratorio, desea ver quién está registrado en él y hacer un seguimiento del uso de las máquinas virtuales. Avance al siguiente tutorial para aprender a realizar un seguimiento del uso del laboratorio:

> [!div class="nextstepaction"]
> [Realizar un seguimiento del uso de un laboratorio](tutorial-track-usage.md) 
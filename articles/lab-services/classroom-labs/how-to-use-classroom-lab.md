---
title: Acceso a un laboratorio de clase en Azure Lab Services | Microsoft Docs
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
ms.date: 11/15/2018
ms.author: spelluru
ms.openlocfilehash: b5a55a35dc4baacf8248adad3133220214050eeb
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706694"
---
# <a name="how-to-access-a-classroom-lab-in-azure-lab-services"></a>Acceso a un laboratorio de clase en Azure Lab Services
En este artículo se describe cómo acceder a un laboratorio de clase, conectarse a la máquina virtual en el laboratorio y detener la máquina virtual. 

## <a name="register-to-a-lab"></a>Registrarse en un laboratorio
1. Vaya a la **dirección URL de registro** que recibió del profesor o educador. 
2. Inicie sesión en el servicio con su cuenta organizativa para completar el registro. 
3. Una vez registrado, confirme que ve las máquinas virtuales de los laboratorios a los que tiene acceso. 
2. Espere hasta que la máquina virtual esté lista y, a continuación, **iníciela**. Este proceso tarda algún tiempo.  

    ![Inicio de la máquina virtual](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)


## <a name="view-all-the-classroom-labs"></a>Visualización de todos los laboratorios de clase
Después de registrarse en los laboratorios, puede ver todos los laboratorios educativos siguiendo estos pasos: 

1. Vaya a [https://labs.azure.com](https://labs.azure.com). 
2. Inicie sesión en el servicio mediante la cuenta de usuario que usó para registrarse en el laboratorio. 
3. Confirme que puede ver todos los laboratorios a los que tiene acceso. 

    ![Visualización de todos los laboratorios](../media/how-to-use-classroom-lab/all-labs.png)

## <a name="connect-to-the-virtual-machine-in-a-classroom-lab"></a>Conexión a la máquina virtual en un laboratorio de clase

1. Inicie la máquina virtual si aún no se ha iniciado y seleccione **Iniciar**.
2. Seleccione **Conectar** en el icono que representa la máquina virtual del laboratorio al que quiere acceder. 

    ![Visualización de todos los laboratorios](../media/how-to-use-classroom-lab/connect-button.png)
3. Guarde el archivo RDP (para máquinas virtuales Windows) en el disco duro y ábralo. 
4. Utilice el **nombre de usuario** y la **contraseña** que obtiene de su profesor o educador para iniciar sesión en la máquina. 

## <a name="stop-the-virtual-machine-in-a-classroom-lab"></a>Detención de la máquina virtual en un laboratorio de clase

Para detener la máquina virtual, seleccione **Detener** en el icono. Cuando se detiene la máquina virtual, el botón **Iniciar** en el icono se habilita. 

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como propietario del laboratorio, configurar y controlar el uso de un laboratorio](how-to-configure-student-usage.md)
 
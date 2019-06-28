---
title: Habilitación de entornos con varias máquinas virtuales en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo crear un entorno con varias máquinas virtuales dentro de una máquina virtual de plantilla en un laboratorio educativo de Azure Lab Services.
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
ms.date: 03/18/2019
ms.author: spelluru
ms.openlocfilehash: 6faf32232c42f863bff52fdfb3c0714aee8e9b88
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60702441"
---
# <a name="create-an-environment-with-multiple-vms-inside-a-template-vm-of-a-classroom-lab"></a>Creación de un entorno con varias máquinas virtuales en una máquina virtual de plantilla de un laboratorio educativo
Actualmente, Azure Lab Services le permite configurar una máquina virtual de una plantilla en un laboratorio y realizar una copia única disponible para cada uno de los usuarios. Pero si es usted un profesor de una materia de informática que enseña cómo configurar firewalls o servidores, puede que necesite proporcionar a cada uno de los alumnos un entorno en el que varias máquinas virtuales puedan comunicarse entre sí a través de una red.

La virtualización anidada le permite crear un entorno de varias máquinas virtuales dentro de una máquina virtual de plantilla de un laboratorio. La publicación de la plantilla proporcionará a cada usuario del laboratorio una máquina virtual configurada con varias máquinas virtuales dentro.

## <a name="what-is-nested-virtualization"></a>¿Qué es la virtualización anidada?
La virtualización anidada le permite crear máquinas virtuales dentro de una máquina virtual. La virtualización anidada se realiza a través de Hyper-V y solo está disponible en las máquinas virtuales de Windows.

Para más información sobre la virtualización anidada, vea los siguientes artículos:

- [Virtualización anidada en Azure](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Habilitación de la virtualización anidada en una máquina virtual de Azure](../../virtual-machines/windows/nested-virtualization.md)

## <a name="use-nested-virtualization-in-azure-lab-services"></a>Uso de la virtualización anidada en Azure Lab Services
Los pasos importantes son:

1. Crear una máquina de plantilla de **gran** tamaño de **Windows** para el laboratorio. 
2. Conectarla y [habilitar la virtualización anidada](../../virtual-machines/windows/nested-virtualization.md).


El procedimiento siguiente proporciona los pasos detallados: 

1. Cree una cuenta de laboratorio si todavía no tiene una. Para obtener instrucciones, consulte: [Tutorial: Configuración de una cuenta de laboratorio con Azure Lab Services](tutorial-setup-lab-account.md).
2. Vaya al [sitio web de Azure Lab Services](https://labs.azure.com). 
3. Seleccione **Iniciar sesión** y escriba las credenciales. Azure Lab Services es compatible con cuentas profesionales y cuentas Microsoft. 
4. En la ventana **Nuevo laboratorio**, lleve a cabo las siguientes acciones: 
    1. Especifique un **nombre** para el laboratorio. 
    2. Especifique el máximo **número de máquinas virtuales** del laboratorio. Puede aumentar o reducir el número de máquinas virtuales después de crear el laboratorio o en un laboratorio existente. Para más información, consulte [Actualización del número de máquinas virtuales en un laboratorio](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab).
    6. Seleccione **Guardar**.

        ![Creación de un laboratorio educativo](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. En la página **Seleccionar especificaciones de máquina virtual**, realice los pasos siguientes:
    1. Seleccione **Grande** para el tamaño de máquinas virtuales (VM) que se creará en el laboratorio. Actualmente, el tamaño grande es el único compatible con la virtualización anidada.
    2. Elija una imagen de máquina virtual que sea una **imagen de Windows**. La virtualización anidada solo está disponible en equipos de Windows. 
    3. Seleccione **Next** (Siguiente).

        ![Especificaciones de máquina virtual](../media/how-to-enable-multi-vm-environment/large-windows-vm.png)    
5. En la página **Establecer credenciales**, especifique las credenciales predeterminadas de todas las máquinas virtuales del laboratorio. 
    1. Especifique el **nombre del usuario** para todas las máquinas virtuales del laboratorio.
    2. Especifique la **contraseña** del usuario. 

        > [!IMPORTANT]
        > Tome nota de ambos. No se volverán a mostrar.
    3. Seleccione **Crear**. 

        ![Establecer credenciales](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. En la página **Configurar plantilla**, puede ver el estado del proceso de creación del laboratorio. La creación de la plantilla en el laboratorio tarda un máximo de 20 minutos. 

    ![Configurar plantilla](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Una vez completada la configuración de la plantilla, verá la siguiente página: 

    ![Página Configurar plantilla una vez terminada](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. En la página **Configurar plantilla**, seleccione **Conectar** para conectarse a la máquina virtual de plantilla a fin de configurar la virtualización anidada. También puede configurarla más adelante, después de completar los pasos descritos en este asistente. 
9. Dentro de la máquina virtual de plantilla, configure la virtualización anidada y una red virtual con varias máquinas virtuales. Para obtener instrucciones paso a paso detalladas, consulte [Habilitación de la virtualización anidada en una máquina virtual de Azure](../../virtual-machines/windows/nested-virtualization.md). Este es un resumen rápido de los pasos: 
    1. Habilite la característica de Hyper-V en la máquina virtual de plantilla.
    2. Configure una red virtual interna con conectividad a Internet para las máquinas virtuales anidadas.
    3. Cree máquinas virtuales a través del Administrador de Hyper-V.
    4. Asigne una dirección IP a las máquinas virtuales.
10. Seleccione **Siguiente** en la página de plantilla. 
11. En la página **Publicar la plantilla**, realice las acciones siguientes. 
    1. Para publicar inmediatamente la plantilla, seleccione **Publicar**.  

        > [!WARNING]
        > Una vez que publique, no se puede cancelar la publicación. 
    2. Para publicar más adelante, seleccione **Guardar para más adelante**. Puede publicar la plantilla de máquina virtual una vez finalizado el asistente. Para más información sobre cómo configurar y publicar una vez finalizado el asistente, consulte la sección [Publicación de la plantilla](how-to-create-manage-template.md#publish-the-template-vm) del artículo [Administración de laboratorios educativos](how-to-manage-classroom-labs.md).

        ![Publicar plantilla](../media/how-to-enable-multi-vm-environment/publish-template-page.png)
11. Puede ver el **progreso de la publicación** de la plantilla. Este proceso puede tardar hasta una hora. 

    ![Publicar plantilla: progreso](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Podrá ver la página siguiente cuando la plantilla se haya publicado correctamente. Seleccione **Listo**.

    ![Publicar plantilla: correctamente](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Verá el **panel** del laboratorio. 
    
    ![Panel del laboratorio de clase](../media/how-to-enable-multi-vm-environment/dashboard.png)


## <a name="next-steps"></a>Pasos siguientes

Ahora, cada usuario obtiene una sola máquina virtual que incluye un entorno de varias máquinas virtuales. Para obtener información sobre cómo agregar usuarios al laboratorio y enviarles un vínculo de registro, consulte el artículo siguiente: [Incorporación de usuarios al laboratorio](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
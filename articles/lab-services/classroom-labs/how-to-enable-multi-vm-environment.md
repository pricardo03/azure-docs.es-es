---
title: Habilite entornos de varias máquinas virtuales en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo crear un entorno con varias máquinas virtuales dentro de una máquina virtual de plantilla en un laboratorio de Azure Lab Services.
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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60702441"
---
# <a name="create-an-environment-with-multiple-vms-inside-a-template-vm-of-a-classroom-lab"></a>Crear un entorno con varias máquinas virtuales dentro de una máquina virtual de un laboratorio de clase de plantilla
Actualmente Azure Lab Services le permite configurar la máquina virtual de una plantilla en un laboratorio y realizar una copia única disponible para cada uno de los usuarios. Pero si es un profesor que enseña una clase de TI acerca de cómo configurar los firewalls o servidores, puede que necesite proporcionar a cada uno de los estudiantes un entorno en el que varias máquinas virtuales pueden comunicarse entre sí a través de una red.

Virtualización anidada le permite crear un entorno de varias máquinas virtuales dentro de la máquina virtual de un laboratorio plantilla. Publicar la plantilla proporciona a cada usuario en el laboratorio con una máquina virtual configurado con varias máquinas virtuales dentro de él.

## <a name="what-is-nested-virtualization"></a>¿Qué es la virtualización anidada?
Virtualización anidada le permite crear máquinas virtuales dentro de una máquina virtual. Virtualización anidada se realiza a través de Hyper-V y solo está disponible en las máquinas virtuales de Windows.

Para obtener más información acerca de la virtualización anidada, consulte los artículos siguientes:

- [Virtualización anidada en Azure](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Cómo habilitar la virtualización anidada en una máquina virtual de Azure](../../virtual-machines/windows/nested-virtualization.md)

## <a name="use-nested-virtualization-in-azure-lab-services"></a>Usar la virtualización anidada en Azure Lab Services
Los pasos importantes son:

1. Crear un **grandes** tamaño **Windows** máquina de la plantilla para el laboratorio. 
2. Conectarse a él y [habilitar virtualización anidada](../../virtual-machines/windows/nested-virtualization.md).


El siguiente procedimiento proporcionan los pasos detallados: 

1. Si aún no tiene uno, cree una cuenta de laboratorio. Para obtener instrucciones, consulte [Tutorial: Configurar una cuenta de laboratorio con Azure Lab Services](tutorial-setup-lab-account.md).
2. Vaya al [sitio web de Azure Lab Services](https://labs.azure.com). 
3. Seleccione **Iniciar sesión** y escriba las credenciales. Azure Lab Services es compatible con cuentas profesionales y cuentas Microsoft. 
4. En la ventana **Nuevo laboratorio**, lleve a cabo las siguientes acciones: 
    1. Especifique un **nombre** para el laboratorio. 
    2. Especifique el máximo **número de máquinas virtuales** del laboratorio. Puede aumentar o reducir el número de máquinas virtuales después de crear el laboratorio o en un laboratorio existente. Para más información, consulte [Actualización del número de máquinas virtuales en un laboratorio](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab).
    6. Seleccione **Guardar**.

        ![Creación de un laboratorio educativo](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. En la página **Seleccionar especificaciones de máquina virtual**, realice los pasos siguientes:
    1. Seleccione **grande** para el tamaño de máquinas virtuales (VM) que se creará en el laboratorio. Actualmente, solo el tamaño grande es compatible con la virtualización anidada.
    2. Elija una imagen de máquina virtual que sea un **imagen de Windows**. Virtualización anidada solo está disponible en equipos de Windows. 
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
8. En el **configurar plantilla** página, seleccione **Connect** para conectarse a la plantilla de máquina virtual para configurar la virtualización anidada. También puede configurar más adelante después de completar los pasos descritos en este asistente. 
9. Dentro de la máquina virtual de plantilla, configure la virtualización anidada y configurar una red virtual con varias máquinas virtuales. Para obtener instrucciones paso a paso detalladas, consulte [cómo habilitar virtualización anidada en una máquina virtual de Azure](../../virtual-machines/windows/nested-virtualization.md). Este es un resumen rápido de los pasos: 
    1. Habilitar la característica de Hyper-V en la máquina virtual de plantilla.
    2. Configurar una red virtual interna con conectividad a internet para las máquinas virtuales anidadas
    3. Creación de máquinas virtuales a través del Administrador de Hyper-V
    4. Asignar una dirección IP a las máquinas virtuales
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

Ahora, cada usuario obtiene una sola máquina virtual que incluye un entorno de varias máquinas virtuales dentro de él. Para obtener información sobre cómo agregar usuarios a la práctica y enviar el vínculo de registro a ellos, consulte el artículo siguiente: [Agregar usuarios al laboratorio](tutorial-setup-classroom-lab.md#add-users-to-the-lab).
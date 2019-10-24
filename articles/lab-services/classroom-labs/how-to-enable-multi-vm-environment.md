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
ms.date: 10/13/2019
ms.author: spelluru
ms.openlocfilehash: 9a86ba803f899e78b2ba9640e6cc317966969e64
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72332325"
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
1. Vaya al [sitio web de Azure Lab Services](https://labs.azure.com). Tenga en cuenta que Internet Explorer 11 aún no se admite. 
2. Seleccione **Iniciar sesión** y escriba las credenciales. Azure Lab Services es compatible con cuentas profesionales y cuentas Microsoft. 
3. Seleccione **New lab** (Nuevo laboratorio). 
    
    ![Creación de un laboratorio educativo](../media/tutorial-setup-classroom-lab/new-lab-button.png)
4. En la ventana **Nuevo laboratorio**, lleve a cabo las siguientes acciones: 
    1. Especifique un **nombre** para el laboratorio. 
    2. Seleccione **Large (nested virtualization)** (Grande [virtualización anidada]) o **Medium (nested virtualization)** (Medio [virtualización anidada]) como **tamaño de la máquina virtual**.
    6. Seleccione la **imagen** de Windows que desee usar. La virtualización anidada solo está disponible en equipos de Windows. 
    4. Después, seleccione **Siguiente**. 

        ![Creación de un laboratorio educativo](../media/how-to-enable-multi-vm-environment/new-lab-window.png)
    1. En la página **Virtual machine credentials** (Credenciales de la máquina virtual), especifique las credenciales predeterminadas de todas las máquinas virtuales del laboratorio. Especifique los valores de **Name** (Nombre) y **Password** (Contraseña) para el usuario y seleccione **Next** (Siguiente).  

        ![Nueva ventana de laboratorio](../media/tutorial-setup-classroom-lab/virtual-machine-credentials.png)

        > [!IMPORTANT]
        > Tome nota de ambos. No se volverán a mostrar.
    3. En la página **Lab policies** (Directivas del laboratorio), escriba el número de horas asignadas para cada usuario (**cuota para cada usuario**) fuera del tiempo programado para el laboratorio y seleccione **Finish** (Terminar). 

        ![Cuota para cada usuario](../media/tutorial-setup-classroom-lab/quota-for-each-user.png)
5. Debería ver la siguiente pantalla, que muestra el estado de la creación de la máquina virtual de plantilla. La creación de la plantilla en el laboratorio tarda un máximo de 20 minutos. 

    ![Estado de la creación de la plantilla de máquina virtual](../media/tutorial-setup-classroom-lab/create-template-vm-progress.png)
1. En la página **Template** (Plantilla), seleccione **Customize template** (Personalizar plantilla) en la barra de herramientas. 

    ![Botón Customize template (Personalizar plantilla)](../media/how-to-create-manage-template/customize-template-button.png)
2. En el cuadro de diálogo **Customize template** (Personalizar plantilla), seleccione **Continue** (Continuar). Una vez iniciada la plantilla y realizados los cambios, dejará de tener la misma configuración que las máquinas virtuales publicaron la última vez para los usuarios. Los cambios en la plantilla no se reflejarán en las máquinas virtuales existentes de los usuarios hasta que realice otra publicación.

    ![Cuadro de diálogo Customize (Personalizar)](../media/how-to-create-manage-template/customize-template-dialog.png)
1. Seleccione el botón **Connect to template** (Conectarse a la plantilla) en la barra de herramientas para conectarse a la máquina virtual de plantilla y configurar la virtualización anidada; para ello, siga las instrucciones. Si es una máquina Windows, verá una opción para descargar el archivo RDP. 

    ![Conexión a la plantilla de máquina virtual](../media/how-to-create-manage-template/connect-template-vm.png) 
9. Dentro de la máquina virtual de plantilla, configure la virtualización anidada y una red virtual con varias máquinas virtuales. Para obtener instrucciones paso a paso detalladas, consulte [Habilitación de la virtualización anidada en una máquina virtual de Azure](../../virtual-machines/windows/nested-virtualization.md). Este es un resumen rápido de los pasos: 
    1. Habilite la característica de Hyper-V en la máquina virtual de plantilla.
    2. Configure una red virtual interna con conectividad a Internet para las máquinas virtuales anidadas.
    3. Cree máquinas virtuales a través del Administrador de Hyper-V.
    4. Asigne una dirección IP a las máquinas virtuales.
10. En la página **Template** (Plantilla), seleccione **Publish** (Publicar) en la barra de herramientas. 

    ![Botón de publicación de plantilla](../media/tutorial-setup-classroom-lab/template-page-publish-button.png)

    > [!WARNING]
    > Una vez que publique, no se puede cancelar la publicación. 
8. En la página **Publish template** (Publicar plantilla), escriba el número de máquinas virtuales que desea crear en el laboratorio y seleccione **Publish** (Publicar). 

    ![Publicación de plantilla: número de máquinas virtuales](../media/tutorial-setup-classroom-lab/publish-template-number-vms.png)
11. Puede ver el **estado de la publicación** de la plantilla en la página. Este proceso puede tardar hasta una hora. 

    ![Publicar plantilla: progreso](../media/tutorial-setup-classroom-lab/publish-template-progress.png)


## <a name="next-steps"></a>Pasos siguientes

Ahora, cada usuario obtiene una sola máquina virtual que incluye un entorno de varias máquinas virtuales. Para obtener información sobre cómo agregar usuarios al laboratorio y enviarles un vínculo de registro, consulte el artículo siguiente: [Incorporación de usuarios al laboratorio](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
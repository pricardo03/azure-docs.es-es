---
title: Habilitación de la virtualización anidada en una plantilla de máquina virtual en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo crear una plantilla de máquina virtual con varias máquinas virtuales dentro.  En otras palabras, habilite la virtualización anidada en una plantilla de máquina virtual en Azure Lab Services.
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
ms.date: 10/04/2019
ms.author: spelluru
ms.openlocfilehash: 64097a5b3b62bcd5a84f4472a844bb95cf24cd6f
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555087"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services"></a>Habilitación de la virtualización anidada en una plantilla de máquina virtual en Azure Lab Services

Actualmente, Azure Lab Services le permite configurar una máquina virtual de una plantilla en un laboratorio y realizar una copia única disponible para cada uno de los usuarios. Si usted es un profesor que hace clases de redes, seguridad o TI, puede que necesite proporcionar a cada uno de sus alumnos un entorno en el que varias máquinas virtuales puedan comunicarse entre sí a través de una red.

La virtualización anidada le permite crear un entorno de varias máquinas virtuales dentro de una máquina virtual de plantilla de un laboratorio. La publicación de la plantilla proporcionará a cada usuario del laboratorio una máquina virtual configurada con varias máquinas virtuales dentro.  En este artículo se explica cómo configurar la virtualización anidada en una máquina de pantilla en Azure Lab Services.

## <a name="what-is-nested-virtualization"></a>¿Qué es la virtualización anidada?

La virtualización anidada le permite crear máquinas virtuales dentro de una máquina virtual. La virtualización anidada se realiza a través de Hyper-V y solo está disponible en las máquinas virtuales de Windows.

Para más información sobre la virtualización anidada, vea los siguientes artículos:

- [Virtualización anidada en Azure](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Habilitación de la virtualización anidada en una máquina virtual de Azure](../../virtual-machines/windows/nested-virtualization.md)

## <a name="considerations"></a>Consideraciones

Antes de configurar un laboratorio con virtualización anidada, estas son algunas cosas que debe tener en cuenta.

- Al crear un laboratorio, seleccione los tamaños **Medio (virtualización anidada)** o **Grande (virtualización anidada)** como tamaño de la máquina virtual. Estos tamaños de máquina virtual admiten la virtualización anidada.
- Elija un tamaño que proporcione un buen rendimiento para las máquinas virtuales de host y de cliente.  Recuerde que, al usar la virtualización, el tamaño que elija debe ser adecuado no solo para una máquina, sino también para el host y las máquinas cliente que deban ejecutarse simultáneamente.
- Las máquinas virtuales de cliente no tendrán acceso a los recursos de Azure como, por ejemplo, a los servidores DNS de la red virtual de Azure.
- La máquina virtual del host requiere configuración para permitir que la máquina cliente tenga conectividad a Internet.
- Las máquinas virtuales de cliente tienen licencia como máquinas independientes. Consulte [Licencias de Microsoft](https://www.microsoft.com/licensing/default) para más información sobre las licencias de los productos y sistemas operativos de Microsoft. Compruebe los contratos de licencia de cualquier otro software que se esté usando antes de configurar la plantilla de máquina virtual.

## <a name="enable-nested-virtualization-on-a-template-vm"></a>Habilitación de la virtualización anidada en una máquina virtual de plantilla

En este artículo se supone que ha creado una cuenta de laboratorio y un laboratorio.  Para más información sobre la creación de una nueva cuenta de laboratorio, consulte el [tutorial para configurar una cuenta de laboratorio](tutorial-setup-lab-account.md). Para más información sobre cómo crear un laboratorio, consulte el [tutorial Configuración de un laboratorio educativo](tutorial-setup-classroom-lab.md).

>[!IMPORTANT]
>Seleccione **Grande (virtualización anidada)** o **Medio (virtualización anidada)** como tamaño de la máquina virtual al crear el laboratorio.  De lo contrario, la virtualización anidada no funcionará.  

Para conectarse a la máquina de plantilla, consulte [Creación y administración de plantillas educativas en Azure Lab Services](how-to-create-manage-template.md). 

Los pasos de esta sección se centran en la configuración de la virtualización anidada para Windows Server 2016 o Windows Server 2019. Usará un script para configurar la plantilla de máquina virtual con Hyper-V.  Los pasos siguientes lo guiarán en el proceso de cómo usar los [scripts de Hyper-V para Lab Services](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV).

1. Si usa Internet Explorer, es posible que tenga que agregar `https://github.com` a la lista de sitios de confianza.
    1. Abra Internet Explorer.
    1. Seleccione el icono de engranaje y elija **Opciones de Internet**.  
    1. Cuando aparezca el cuadro de diálogo **Opciones de Internet**, seleccione **Seguridad**, **Sitios de confianza** y haga clic en el botón **Sitios**.
    1. Cuando aparezca el cuadro de diálogo **Sitios de confianza**, agregue `https://github.com` a la lista de sitios web de confianza y seleccione **Cerrar**.

        ![Sitios de confianza](../media/how-to-enable-nested-virtualization-template-vm/trusted-sites-dialog.png)
1. Descargue los archivos del repositorio de Git tal y como se indica en los pasos siguientes.
    1. Vaya a [https://github.com/Azure/azure-devtestlab/](https://github.com/Azure/azure-devtestlab/).
    1. Haga clic en el botón **Clonar o descargar**.
    1. Haga clic en **Descargar archivo ZIP**.
    1. Extraer el archivo ZIP

    >[!TIP]
    >También puede clonar el repositorio de Git en [https://github.com/Azure/azure-devtestlab.git](https://github.com/Azure/azure-devtestlab.git).

1. Inicie **PowerShell** en modo de **administrador**.
1. En la ventana de PowerShell, vaya hasta la carpeta con el script descargado. Si va desde la carpeta superior de los archivos del repositorio, el script se encuentra en `azure-devtestlab\samples\ClassroomLabs\Scripts\HyperV\`.
1. Es posible que tenga que cambiar la directiva de ejecución para ejecutar correctamente el script. Ejecute el siguiente comando:

    ```powershell
    Set-ExecutionPolicy bypass -force
    ```

1. Ejecute el script:

    ```powershell
    .\SetupForNestedVirtualization.ps1
    ```

    > [!NOTE]
    > El script puede requerir que se reinicie la máquina. Siga las instrucciones del script y vuelva a ejecutarlo hasta que aparezca el mensaje **Script completed** (Script completo) en la salida.
1. No olvide restablecer la directiva de ejecución. Ejecute el siguiente comando:

    ```powershell
    Set-ExecutionPolicy default -force
    ```

## <a name="conclusion"></a>Conclusión

Ahora la plantilla de máquina virtual está lista para crear máquinas virtuales de Hyper-V. Consulte [Crear una máquina virtual en Hyper-V](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v) para obtener instrucciones sobre cómo crear máquinas virtuales de Hyper-V. Consulte también [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/) para comprobar los sistemas operativos y el software disponibles.  

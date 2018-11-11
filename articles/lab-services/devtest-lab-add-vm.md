---
title: Incorporación de una máquina virtual a un laboratorio de Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo agregar una máquina virtual a un laboratorio de Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: spelluru
ms.openlocfilehash: e86568da7f3d607c90e42e09a61ced9993c4d744
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51254050"
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Incorporación de una máquina virtual a un laboratorio de Azure DevTest Labs
Si ya ha [creado su primera máquina virtual](devtest-lab-create-first-vm.md), es probable que lo hiciera desde una [imagen de Marketplace](devtest-lab-configure-marketplace-images.md) precargada. Ahora, si desea agregar máquinas virtuales posteriores al laboratorio, también puede elegir una *base* que sea una [imagen personalizada](devtest-lab-create-template.md) o una [fórmula](devtest-lab-manage-formulas.md). En este tutorial se explica cómo usar Azure Portal para agregar una máquina virtual a un laboratorio de DevTest Labs.

En este artículo se explica cómo administrar los artefactos de una máquina virtual de un laboratorio.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Pasos para agregar una máquina virtual a un laboratorio de Azure DevTest Labs
1. Inicie sesión en el [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
1. En la lista de laboratorios, seleccione aquel en el que desea crear la máquina virtual.  
1. En el panel **Información general** del laboratorio, seleccione **+ Agregar**.  

    ![Botón Agregar VM](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. En el panel **Elegir una base**, seleccione una base para la máquina virtual.
1. En el panel **Máquina virtual**, el **nombre de máquina virtual** se rellena previamente con un nombre único generado automáticamente. El nombre corresponde al nombre de usuario dentro de la dirección de correo electrónico, seguido por un número único de 3 dígitos. Esta característica permite ahorrarse tiempo de pensar en un nombre de máquina y escribirlo cada vez que cree una máquina. Puede invalidar el nombre automático de este campo con el nombre que elija. Para invalidar el nombre automático de la máquina virtual, escriba un nombre en el cuadro de texto **Nombre de máquina virtual**. 

    ![Panel de VM del laboratorio](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. El **nombre de usuario** de la máquina se rellena previamente con un nombre único generado automáticamente. El nombre corresponde al nombre de usuario dentro de la dirección de correo electrónico. Esta característica reduce el tiempo necesario decidir cuál es el nombre de usuario cada vez que se crea una nueva máquina. Puede invalidar el nombre automático de este campo con el nombre que elija. Para invalidar el nombre de usuario automático, escriba un valor en el cuadro de texto **Nombre de usuario**. A este usuario se le conceden privilegios de **administrador** en la máquina virtual.     
1. En la **Contraseña**:
    
    Si va a crear la primera máquina virtual del laboratorio, escriba una contraseña en el cuadro de texto **Escribir un valor**. Para guardar esta contraseña como contraseña predeterminada en el almacén de claves de Azure asociado al laboratorio, seleccione **Guardar como contraseña predeterminada**. La contraseña predeterminada se guarda en el almacén de claves con el nombre: **VmPassword**. Cuando después se intenta crear máquinas virtuales en el laboratorio, **VmPassword** se selecciona automáticamente como **Contraseña**. Para invalidar el valor, desactive la casilla **Usar un secreto guardado** y escriba una contraseña. 

    Puede también guardar primero los secretos en el almacén de claves y, después, usarlos al crear una máquina virtual en el laboratorio. Para más información, consulte [Almacenamiento de secretos en un almacén de claves](devtest-lab-store-secrets-in-key-vault.md). Para usar una contraseña almacenada en un almacén de claves, seleccione **Usar un secreto guardado** y especifique un valor de clave que corresponda a su secreto (contraseña). 
3. El valor **Virtual machine disk type** (Tipo de disco de máquina virtual) determina qué tipo de disco de almacenamiento se admite para las máquinas virtuales del laboratorio.
4. Seleccione **Tamaño de máquina virtual** y seleccione uno de los elementos predefinidos que especifican los núcleos del procesador, el tamaño de RAM y el tamaño de la unidad de disco duro de la máquina virtual que se va a crear.
5. Seleccione **Artefactos** y, en la lista de artefactos, seleccione y configure los artefactos que desea agregar a la imagen base.
    **Nota:** Si no está familiarizado con DevTest Labs o la configuración de artefactos, vaya a la sección [Incorporación de un artefacto existente a una máquina virtual](#add-an-existing-artifact-to-a-vm) y vuelva aquí cuando haya finalizado.
6. Seleccione **Configuración avanzada** para configurar las opciones de expiración y las opciones de red de la máquina virtual. 

   Para establecer una opción de expiración, elija el icono del calendario para especificar una fecha en la que la VM se eliminará automáticamente.  De forma predeterminada, la VM nunca expirará. 
1. Si desea ver o copiar la plantilla de Azure Resource Manager, vaya a la sección [Almacenamiento de una plantilla de Azure Resource Manager](#save-azure-resource-manager-template) y vuelva aquí cuando haya finalizado.
1. Seleccione **Crear** para agregar la máquina virtual especificada al laboratorio.
1. En el panel del laboratorio se muestra el estado de creación de la máquina virtual; primero como **Creating** (En creación) y luego como **Running** (En ejecución), una vez que se haya iniciado la máquina virtual.

> [!NOTE]
> [Agregar una máquina virtual reclamable](devtest-lab-add-claimable-vm.md) muestra cómo convertir una máquina virtual en reclamable para que esté disponible con el fin de que pueda usarlo cualquier usuario del laboratorio.
>
>

## <a name="add-an-existing-artifact-to-a-vm"></a>Incorporación de un artefacto existente a una máquina virtual
Al crear una máquina virtual, puede agregar artefactos existentes. Cada laboratorio incluye artefactos procedentes del repositorio de artefactos de DevTest Labs público, así como los artefactos que ha creado y agregado a su propio repositorio de artefactos.

* Los *artefactos* de Azure DevTest Labs permiten especificar las *acciones* que se realizan al aprovisionarse la máquina virtual, como, por ejemplo, la ejecución de scripts de Windows PowerShell, la ejecución de comandos Bash y la instalación de software.
* Los *parámetros* del artefacto le permiten personalizar el artefacto para su escenario en particular.

Para descubrir cómo crear artefactos, consulte el artículo [Creación de artefactos personalizados para la máquina virtual de DevTest Labs](devtest-lab-artifact-author.md).

1. Inicie sesión en el [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
1. En la lista de laboratorios, seleccione el que contiene la máquina virtual con la que desea trabajar.  
1. Seleccione **My virtual machines** (Mis máquinas virtuales).
1. Seleccione la máquina virtual deseada.
1. Seleccione **Administrar artefactos**. 
1. Seleccione **Apply artifacts** (Aplicar artefactos).
1. En el panel **Aplicar artefactos**, seleccione el artefacto que desea agregar a la máquina virtual.
1. En el panel **Agregar artefacto**, especifique los valores de parámetros necesarios y cualquier otro parámetro opcional que precise.  
1. Seleccione **Agregar** para agregar el artefacto y vuelva al panel **Aplicar artefactos**.
1. Siga agregando los artefactos que necesite para la máquina virtual.
1. Cuando haya agregado los artefactos, puede [cambiar el orden en que se ejecutan](#change-the-order-in-which-artifacts-are-run). También puede volver atrás para [ver o modificar un artefacto](#view-or-modify-an-artifact).
1. Cuando haya terminado de agregar artefactos, seleccione **Aplicar**.

## <a name="change-the-order-in-which-artifacts-are-run"></a>Cambio del orden en que se ejecutan los artefactos
De forma predeterminada, las acciones de los artefactos se ejecutan en el orden en que se agregan a la máquina virtual. Los siguientes pasos muestran cómo cambiar el orden en que se ejecutan los artefactos.

1. En la parte superior del panel **Aplicar artefactos**, seleccione el vínculo que indica cuántos artefactos se han agregado a la máquina virtual.
   
    ![Número de artefactos que se agregan a la máquina virtual](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. En el panel **Artefactos seleccionados**, arrastre los artefactos y colóquelos en el orden deseado. **Nota:** Si tiene problemas para arrastrar el artefacto, asegúrese de que lo arrastra desde el lado izquierdo del artefacto. 
1. Seleccione **Aceptar** cuando haya terminado.  

## <a name="view-or-modify-an-artifact"></a>ver o modificar un artefacto
Los siguientes pasos muestran cómo ver o modificar los parámetros de un artefacto:

1. En la parte superior del panel **Aplicar artefactos**, seleccione el vínculo que indica cuántos artefactos se han agregado a la máquina virtual.
   
    ![Número de artefactos que se agregan a la máquina virtual](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. En el panel **Artefactos seleccionados**, seleccione el artefacto que desea ver o modificar.  
1. En el panel **Agregar artefacto**, realice los cambios necesarios y seleccione **Aceptar** para cerrar el panel **Agregar artefacto**.
1. Seleccione **Aceptar** para cerrar el panel **Artefactos seleccionados**.

## <a name="save-azure-resource-manager-template"></a>Almacenamiento de una plantilla de Azure Resource Manager
Una plantilla de Azure Resource Manager proporciona una manera declarativa de definir una implementación repetible. Los siguientes pasos explican cómo guardar la plantilla de Azure Resource Manager para la máquina virtual que se va a crear.
Una vez guardada, puede utilizar la plantilla de Azure Resource Manager para [implementar nuevas máquinas virtuales con Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. En el panel **Máquina virtual**, seleccione **Ver plantilla de Azure Resource Manager**.
2. En el panel **Ver plantilla de Azure Resource Manager**, seleccione el texto de la plantilla.
3. Copie el texto seleccionado en el Portapapeles.
4. Seleccione **Aceptar** para cerrar el panel **Ver plantilla de Azure Resource Manager**.
5. Abra un editor de texto.
6. Pegue el texto de la plantilla desde el Portapapeles.
7. Guarde el archivo para su uso posterior.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Pasos siguientes
* Una vez creada la máquina virtual, puede conectarse a ella seleccionando **Conectar** en el panel de la máquina virtual.
* Aprenda a [crear artefactos personalizados para la máquina virtual de DevTest Labs](devtest-lab-artifact-author.md).
* Explore la [galería de plantillas de inicio rápido de Azure Resource Manager de DevTest Labs](https://github.com/Azure/azure-devtestlab/tree/master/Samples).

---
title: 'Guía de inicio rápido: Creación de una máquina virtual Windows en Azure Portal | Microsoft Docs'
description: En esta guía de inicio rápido aprenderá a usar Azure Portal para crear una máquina virtual Windows.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/03/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f5a92d421bbf7bfe485252c148d5f64ae2fb8e23
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916122"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-the-azure-portal"></a>Guía de inicio rápido: Creación de una máquina virtual Windows en Azure Portal

Las máquinas virtuales de Azure pueden crearse mediante Azure Portal. Este método proporciona una interfaz de usuario basada en explorador para crear máquinas virtuales y sus recursos asociados. En esta guía de inicio rápido se muestra cómo usar Azure Portal para implementar una máquina virtual (VM) de Azure que ejecuta Windows Server 2016. Para ver la máquina virtual en acción, conéctese a la máquina virtual mediante RDP e instale al servidor web IIS.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-virtual-machine"></a>Crear máquina virtual

1. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.

2. En el cuadro de búsqueda encima de la lista de recursos de Azure Marketplace, busque y seleccione **Windows Server 2016 Datacenter** y, a continuación, elija **Crear**.

3. Proporcione un nombre de máquina virtual, como *myVM*, deje el tipo de disco como *SSD* y proporcione un nombre de usuario, como *azureuser*. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](faq.md#what-are-the-password-requirements-when-creating-a-vm).

    ![Especificación de la información básica de la máquina virtual en la hoja del Portal](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)

5. Elija **Crear nuevo** para el grupo de recursos y proporcione un nombre, como *myResourceGroup*. Elija la **ubicación** y seleccione **Aceptar**.

4. Seleccione un tamaño para la máquina virtual. Puede filtrar por *Tipo de proceso* o *Tipo de disco*, por ejemplo. Se recomienda un tamaño de máquina virtual de *D2s_v3*. Haga clic en **Seleccionar** después de elegir un tamaño.

    ![Captura de pantalla que muestra los tamaños de máquina virtual](./media/quick-create-portal/create-windows-vm-portal-sizes.png)

5. En la página **Configuración**, en **Red** > **Grupo de seguridad de red** > **Seleccionar puertos de entrada públicos**, seleccione **HTTP** y **RDP (3389)** en el menú desplegable. Deje el resto de valores predeterminados y seleccione **Aceptar**.

6. En la página de resumen, seleccione **Crear** para iniciar la implementación de la máquina virtual.

7. La máquina virtual se ancla al panel de Azure Portal. Una vez completada la implementación, se abrirá automáticamente el resumen de la máquina virtual.

## <a name="connect-to-virtual-machine"></a>Conexión a la máquina virtual

Cree una conexión a Escritorio remoto en la máquina virtual. Estas instrucciones indican cómo conectarse a la máquina virtual desde un equipo Windows. En un equipo Mac, necesita un cliente RDP como este [Cliente de Escritorio remoto](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) de Mac App Store.

1. Haga clic en el botón **Conectar** en la página de propiedades de la máquina virtual. 

    ![Conexión a una máquina virtual de Azure desde el portal](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png)
    
2. En la página **Connect to virtual machine** (Conexión a una máquina virtual), mantenga las opciones predeterminadas para conectarse por nombre DNS a través del puerto 3389 y haga clic en **Descargar archivo RDP**.

2. Abra el archivo RDP que descargó y haga clic en **Conectar** cuando se le solicite. 

3. En la ventana **Seguridad de Windows**, seleccione **Más opciones** y, después, **Usar otra cuenta**. Escriba el nombre de usuario con el formato *nombreDeMV*\\*nombreDeUsuario*, escriba la contraseña que creó para la máquina virtual y luego haga clic en **Aceptar**.

4. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Haga clic en **Sí** o en **Continuar** para crear la conexión.

## <a name="install-web-server"></a>Instalación del servidor web

Para ver la máquina virtual en acción, instale el servidor web IIS. Abra un símbolo del sistema de PowerShell en la máquina virtual y ejecute el siguiente comando:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Cuando haya terminado, cierre la conexión RDP con la máquina virtual.


## <a name="view-the-iis-welcome-page"></a>Página principal de IIS

En el portal, seleccione la maquina virtual y, en su información general, utilice el botón **Haga clic para copiar** a la derecha de la dirección IP para copiarla y pegarla en una pestaña del explorador. Se abrirá la página de bienvenida de IIS predeterminada, y debería tener el siguiente aspecto:

![Sitio predeterminado de IIS](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede eliminar el grupo de recursos, la máquina virtual y todos los recursos relacionados. Para ello, seleccione el grupo de recursos de la máquina virtual, seleccione **Eliminar** y luego confirme el nombre del grupo de recursos para eliminar.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, implementará una máquina virtual sencilla, abrirá un puerto de red para el tráfico web e instalará un servidor web básico. Para más información acerca de las máquinas virtuales de Azure, continúe con el tutorial de máquinas virtuales Windows.

> [!div class="nextstepaction"]
> [Tutoriales de máquinas virtuales Windows de Azure](./tutorial-manage-vm.md)

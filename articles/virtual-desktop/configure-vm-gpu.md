---
title: 'Configuración de GPU para Windows Preview de Escritorio Virtual: Azure'
description: Cómo habilitar la aceleración por GPU de representación y la codificación en la vista previa de Escritorio Virtual de Windows.
services: virtual-desktop
author: gundarev
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/06/2019
ms.author: denisgun
ms.openlocfilehash: a6a67c89253a1b16f9266d7917655d1b1104022e
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159576"
---
# <a name="configure-graphics-processing-unit-gpu-acceleration-for-windows-virtual-desktop-preview"></a>Configurar la aceleración de unidad (GPU) de Windows Vista previa del escritorio Virtual de procesamiento de gráficos

Vista previa de Escritorio Virtual de Windows es compatible con aceleración por GPU de representación y una codificación para escalabilidad y rendimiento mejorado de la aplicación. Aceleración de GPU es especialmente importante para las aplicaciones de gráficos.

Siga las instrucciones de este artículo para crear una máquina virtual de Azure GPU optimizada, agregarlo al grupo de host y configurarlo para usar aceleración de GPU para representar y codificación. En este artículo se da por supuesto que ya tiene un inquilino de Escritorio Virtual de Windows configurado.

## <a name="select-a-gpu-optimized-azure-virtual-machine-size"></a>Seleccione un tamaño de máquina virtual de Azure GPU optimizada

Azure ofrece una serie de [tamaños de máquinas virtuales optimizadas para GPU](/azure/virtual-machines/windows/sizes-gpu). La elección correcta para el grupo host depende de una serie de factores, incluidos sus cargas de trabajo de aplicación en particular, calidad deseados de la experiencia del usuario y el costo. En general, más grandes y más capaces de GPU ofrecen una mejor experiencia de usuario con una densidad de un usuario determinado.

## <a name="create-a-host-pool-provision-your-virtual-machine-and-configure-an-app-group"></a>Crear un grupo host, aprovisionar la máquina virtual y configurar un grupo de aplicaciones

Cree un nuevo grupo de host con una máquina virtual del tamaño seleccionado. Para obtener instrucciones, consulte [Tutorial: Crear un grupo host con Azure Marketplace](/azure/virtual-desktop/create-host-pools-azure-marketplace).

Vista previa de Escritorio Virtual de Windows es compatible con aceleración por GPU de representación y la codificación en los siguientes sistemas operativos:

* Windows 10 versión 1511 o posterior
* Windows Server 2016 o posterior

También debe configurar un grupo de aplicaciones, o usa el grupo de la aplicación de escritorio predeterminada (denominado "Grupo de aplicaciones de escritorio") que se crea automáticamente cuando se crea un nuevo grupo host. Para obtener instrucciones, consulte [Tutorial: Administrar grupos de aplicaciones para Windows Vista previa del escritorio Virtual](/azure/virtual-desktop/manage-app-groups).

>[!NOTE]
>Vista previa de Escritorio Virtual de Windows solo admite el tipo de grupo de aplicación de "Escritorio" para los grupos host habilitadas para GPU. No se admiten grupos de aplicaciones de tipo "RemoteApp" para los grupos host habilitadas para GPU.

## <a name="install-supported-graphics-drivers-in-your-virtual-machine"></a>Instalar los controladores de gráficos compatibles en la máquina virtual

Para aprovechar las funcionalidades de GPU serie N de VM de Azure en vista previa de Escritorio Virtual de Windows, debe instalar a los controladores de gráficos NVIDIA. Siga las instrucciones de [controladores de GPU de NVIDIA instalar en máquinas virtuales de serie N que se ejecuta Windows](/azure/virtual-machines/windows/n-series-driver-setup) para instalar controladores, ya sea manualmente o mediante el [extensión de controlador de GPU de NVIDIA](/azure/virtual-machines/extensions/hpccompute-gpu-windows).

Tenga en cuenta que solo [controladores de NVIDIA GRID](/azure/virtual-machines/windows/n-series-driver-setup#nvidia-grid-drivers) distribuido por Azure son compatibles con Windows Vista previa del escritorio Virtual.

Después de la instalación del controlador, se requiere un reinicio de máquina virtual. Utilice los pasos de comprobación en las instrucciones anteriores para confirmar que los controladores de gráficos se han instalado correctamente.

## <a name="configure-gpu-accelerated-app-rendering"></a>Configurar la representación de aceleración por GPU de aplicación

De forma predeterminada, las aplicaciones y escritorios que ejecuten en las configuraciones de sesión múltiples se representan con la CPU y no aprovechan las GPU disponibles para la representación. Configurar la directiva de grupo para el host de sesión habilitar la aceleración por GPU de representación:

1. Conectar con el escritorio de la máquina virtual con una cuenta con privilegios de administrador local.
2. Abra el inicio y escriba "gpedit.msc" para abrir el Editor de directivas de grupo.
3. Navegar por el árbol para **configuración del equipo** > **plantillas administrativas** > **componentes de Windows**  >   **Servicios de escritorio remoto** > **Host de sesión de escritorio remoto** > **entorno de la sesión remota**.
4. Seleccione la directiva **utilizar el adaptador de gráficos de hardware predeterminado para todas las sesiones de servicios de escritorio remoto** y establecer esta directiva en **habilitado** para habilitar la representación de GPU en la sesión remota.

## <a name="configure-gpu-accelerated-frame-encoding"></a>Configurar la codificación de aceleración por GPU de marco

Escritorio remoto codifica todos los gráficos que representan las aplicaciones y los equipos de escritorio (si va a representar con GPU o CPU) para su transmisión a clientes de escritorio remoto. De forma predeterminada, escritorio remoto no aprovecha las GPU disponibles para esta codificación. Configurar la directiva de grupo para el host de sesión habilitar la codificación del marco de aceleración por GPU. Continuar con los pasos anteriores:

1. Seleccione la directiva **modo priorizar h.264/AVC 444 gráficos para las conexiones de escritorio remoto** y establecer esta directiva en **habilitado** para forzar el códec 444 h.264/AVC en la sesión remota.
2. Seleccione la directiva **configurar, h.264/AVC codificación de hardware para las conexiones de escritorio remoto** y establecer esta directiva en **habilitado** para habilitar la codificación de hardware para AVC/H.264 en la sesión remota.

    >[!NOTE]
    >En Windows Server 2016, establezca la opción **prefiere codificación de Hardware AVC** a **intento siempre**.

3. Ahora que se han editado las directivas de grupo, forzar una actualización de directiva de grupo. Abra el símbolo del sistema y escriba:

    ```batch
    gpupdate.exe /force
    ```

4. Cierre la sesión de la sesión de escritorio remoto.

## <a name="verify-gpu-accelerated-app-rendering"></a>Compruebe la representación acelerada por GPU de aplicación

Para comprobar que las aplicaciones usan la GPU para la representación, lleve a cabo cualquiera de las siguientes acciones:

* Use la `nvidia-smi` utilidad, como se describe en [comprobar la instalación del controlador](/azure/virtual-machines/windows/n-series-driver-setup#verify-driver-installation) para comprobar la utilización de GPU al ejecutar las aplicaciones.
* En las versiones de sistema operativo compatible, puede usar el Administrador de tareas para comprobar si la utilización de GPU. Seleccione la GPU en la pestaña "Rendimiento" para ver si las aplicaciones que usan la GPU.

## <a name="verify-gpu-accelerated-frame-encoding"></a>Comprobar la codificación de aceleración por GPU de marco

Para comprobar que Escritorio remoto utiliza la codificación de aceleración por GPU:

1. Conectar con el escritorio de la máquina virtual con el cliente de Escritorio Virtual de Windows.
2. Inicie el Visor de eventos y navegue hasta el siguiente nodo: **Registros de aplicaciones y servicios** > **Microsoft** > **Windows** > **RemoteDesktopServices-RdpCoreTS**  >  **Operativa**
3. Para determinar si se utiliza la codificación de aceleración por GPU, busque el evento Id. de 170. Si ve "codificador de AVC hardware habilitado: se usa 1", a continuación, la codificación de GPU.
4. Para determinar si se usa el modo de AVC 444, busque el evento ID 162. Si ve "AVC disponible: 1 perfil inicial: se usa 2048", a continuación, 444 AVC.

## <a name="next-steps"></a>Pasos siguientes

Estas instrucciones deben tener le en marcha con aceleración de GPU en una máquina virtual del host de sesión único. Algunas consideraciones adicionales para habilitar la aceleración de GPU a través de un grupo más grande de host:

* Considere el uso de la [extensión de controlador de GPU de NVIDIA](/azure/virtual-machines/extensions/hpccompute-gpu-windows) para simplificar la instalación de controladores y las actualizaciones a través de un número de máquinas virtuales.
* Considere el uso de directiva de grupo de Active Directory para simplificar la configuración de directiva de grupo a través de un número de máquinas virtuales. Para obtener información sobre cómo implementar la directiva de grupo en el dominio de Active Directory, vea [trabajar con objetos de directiva de grupo](https://go.microsoft.com/fwlink/p/?LinkId=620889).

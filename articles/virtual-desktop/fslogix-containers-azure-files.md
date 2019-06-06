---
title: Contenedores de perfil FSLogix y Azure Files en el escritorio Virtual de Windows - Azure
description: Este artículo describe los contenedores de perfil FSLogix dentro de los archivos de Escritorio Virtual de Windows y Azure.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: v-chjenk
ms.openlocfilehash: b3032aa796b3c79572bbf8b2beb85efc252ff73b
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497532"
---
# <a name="fslogix-profile-containers-and-azure-files"></a>Contenedores de perfiles de FSLogix y archivos de Azure

El servicio de Windows Vista previa del escritorio Virtual, recomienda FSLogix contenedores de perfil como una solución de perfil de usuario. FSLogix está diseñado para los perfiles en entornos de trabajo remoto, como el escritorio Virtual de Windows se mueven. Un perfil de usuario completa almacena en un único contenedor. Al iniciar sesión, este contenedor dinámicamente se adjunta al entorno informático mediante el disco duro Virtual de Hyper-V y de disco duro Virtual (VHD) compatible de forma nativa (VHDX). El perfil de usuario está disponible inmediatamente y aparece en el sistema exactamente igual que un perfil de usuario nativa.

En este artículo, describiremos los contenedores de perfil FSLogix puede utilizados con archivos de Azure. La información está en el contexto de Escritorio Virtual de Windows, que era [anunciadas en/21/3](https://www.microsoft.com/microsoft-365/blog/2019/03/21/windows-virtual-desktop-public-preview/).

## <a name="user-profiles"></a>Perfiles de usuario

Un perfil de usuario contiene los elementos de datos sobre una persona, incluida la información de configuración como la configuración del escritorio, conexiones de red persistentes y configuración de la aplicación. De forma predeterminada, Windows crea un perfil de usuario local que se integra estrechamente con el sistema operativo.

Un perfil de usuario remoto proporciona una partición entre el sistema operativo y datos de usuario. Permite que el sistema operativo, reemplazar o cambiar sin afectar a los datos de usuario. En el Host de sesión de escritorio remoto (RDSH) y las infraestructuras de Escritorio Virtual (VDI), el sistema operativo puede reemplazarse por las razones siguientes:

- Una actualización del sistema operativo
- Un reemplazo de una máquina Virtual existente (VM)
- Un usuario que forman parte de un entorno agrupado de RDSH o VDI (no persistente)

Productos de Microsoft funcionan con varias tecnologías para los perfiles de usuario remoto, incluidas estas tecnologías:
- Perfiles de usuario móvil (RUP)
- Discos de perfil de usuario (UPD)
- De Enterprise state roaming (ESR)

UPD y RUP son las tecnologías más usadas para perfiles de usuario en entornos de Host de sesión de escritorio remoto (RDSH) y el disco duro Virtual (VHD).

### <a name="challenges-with-previous-user-profile-technologies"></a>Desafíos con tecnologías anteriores de perfil de usuario

Soluciones de Microsoft existentes y heredadas para perfiles de usuario incluida con varios desafíos. No hay ninguna solución anterior controla todas las necesidades de perfil de usuario que vienen con un entorno RDSH o VDI. Por ejemplo, UPD no puede controlar grandes archivos OST y RUP no conservar la configuración modernas.

#### <a name="functionality"></a>Funcionalidad

La siguiente tabla muestra las ventajas y limitaciones de las tecnologías de perfil de usuario anterior.

| Technology | Configuración de modernas | Configuración de Win32 | Configuración del sistema operativo | Datos de usuario | Admite la SKU de servidor | Almacenamiento de back-end en Azure | Almacenamiento back-end de forma local | Compatibilidad de versiones | Inicios de sesión posteriores en el tiempo |Notas|
| ---------- | :-------------: | :------------: | :---------: | --------: | :---------------------: | :-----------------------: | :--------------------------: | :-------------: | :---------------------: |-----|
| **Discos de perfil de usuario (UPD)** | Sí | Sí | Sí | Sí | Sí | Sin | Sí | Win 7 o posterior | Sí | |
| **Perfil de usuario móvil (RUP), modo de mantenimiento** | No | Sí | Sí | Sí | Sí| Sin | Sí | Win 7 o posterior | Sin | |
| **Enterprise State Roaming (ESR)** | Sí | Sin | Sí | Sin | Vea las notas | Sí | Sin | Windows 10 | Sin | Funciones usadas en las SKU de servidor, pero ninguna interfaz de usuario auxiliares |
| **User Experience Virtualization (UE-V)** | Sí | Sí | Sí | No | Sí | Sin | Sí | Win 7 o posterior | Sin |  |
| **Archivos de OneDrive en la nube** | Sin | No | No | Sí | Vea las notas | Vea las notas  | Vea las notas | Win 10 RS3 | Sin | No se ha probado en SKU de servidor. Almacenamiento de back-end en Azure depende del cliente de sincronización. Almacenamiento back-end local, necesita a un cliente de sincronización. |

#### <a name="performance"></a>Rendimiento

Requiere UPD [espacios de almacenamiento directo (S2D)](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment) para satisfacer los requisitos de rendimiento. UPD usa el protocolo bloque de mensajes del servidor (SMB). Copia el perfil a la máquina virtual en el que se está registrando el usuario. UPD con S2D era la solución recomendado al equipo RDS para escritorios virtuales de Windows durante la versión preliminar del servicio.  

#### <a name="cost"></a>Coste

Mientras que S2D clústeres logran el rendimiento necesario, el costo es caro para los clientes de empresa, pero especialmente costosa para clientes de empresas pequeñas y medianas (SMB). Para esta solución, las empresas pagan por discos de premium storage, junto con el costo de las máquinas virtuales que usan los discos para un recurso compartido.

#### <a name="administrative-overhead"></a>Sobrecarga administrativa

Los clústeres de S2D requieren un sistema operativo que es revisado, actualizado y mantiene en un estado seguro. Estos procesos y la complejidad de la configuración de la recuperación ante desastres de S2D hacen S2D viables solo para las empresas con un personal de TI dedicado.

## <a name="fslogix-profile-containers"></a>Contenedores de perfil FSLogix

19 de noviembre de 2018, [Microsoft adquirió FSLogix](https://blogs.microsoft.com/blog/2018/11/19/microsoft-acquires-fslogix-to-enhance-the-office-365-virtualization-experience/). FSLogix aborda muchos desafíos de contenedor de perfil. La clave entre ellos son:

- **Rendimiento:** El [contenedores de perfil FSLogix](https://fslogix.com/products/profile-containers) son de alto rendimiento y resolver los problemas de rendimiento que históricamente han bloqueado en modo de exchange en caché.
- **OneDrive:** Sin contenedores de perfil FSLogix, OneDrive para la empresa no se admite en entornos de RDSH o VDI no persistentes. [OneDrive para los procedimientos recomendados de negocio y FSLogix](https://fslogix.com/products/technical-faqs/284-onedrive-for-business-and-fslogix-best-practices) se describe cómo interactúan. Para obtener más información, consulte [usar el cliente de sincronización en escritorios virtuales](https://docs.microsoft.com/deployoffice/rds-onedrive-business-vdi).
- **Carpetas adicionales:** FSLogix proporciona la capacidad para ampliar los perfiles de usuario para incluir las carpetas adicionales.

Desde la adquisición, Microsoft inició reemplazando las soluciones existentes de perfil de usuario, como UPD, con contenedores de perfil FSLogix.

## <a name="azure-files-integration-with-azure-active-directory"></a>Integración de los archivos de Azure con Azure Active Directory

Características y rendimiento de contenedores de perfil FSLogix aprovechan las ventajas de la nube. 24 de septiembre de 2018, Microsoft Azure Files anunció una versión preliminar pública de [archivos de Azure que admiten la autenticación de Azure Active Directory](https://azure.microsoft.com/blog/azure-active-directory-integration-for-smb-access-now-in-public-preview/). Al tratar de costo y sobrecarga administrativa, archivos de Azure con autenticación de Azure Active Directory es una solución de premium para perfiles de usuario en el nuevo servicio de Escritorio Virtual de Windows.

## <a name="best-practices-for-windows-virtual-desktop"></a>Procedimientos recomendados para el escritorio Virtual de Windows

Escritorio Virtual de Windows ofrece un control total sobre el tamaño, tipo y número de máquinas virtuales que están siendo utilizados por los clientes. Para obtener más información, consulte [¿qué es Windows Vista previa del escritorio Virtual?](https://docs.microsoft.com/azure/virtual-desktop/overview).

Para asegurarse de su escritorio Virtual Windows entorno sigue los procedimientos recomendados:

- Debe ser la cuenta de almacenamiento de Azure Files en la misma región que el host de sesión de las máquinas virtuales.
- Permisos de archivos de Azure deben coincidir con los permisos descritos en [requisitos - perfil contenedores](https://docs.fslogix.com/display/20170529/Requirements+-+Profile+Containers).
- Cada grupo host se debe generar del mismo tipo y tamaño de máquina virtual basada en la misma imagen maestra.
- Cada máquina virtual del grupo host debe estar en el mismo grupo de recursos para facilitar la administración, escalado y la actualización.
- Para obtener un rendimiento óptimo, la solución de almacenamiento y el contenedor de perfil debe estar en los mismos datos de FSLogix ubicación del centro.
- La cuenta de almacenamiento que contiene la imagen maestra debe estar en la misma región y suscripción donde se aprovisionan las máquinas virtuales.

## <a name="next-steps"></a>Pasos siguientes

Utilice las siguientes instrucciones para configurar un entorno de Escritorio Virtual de Windows.

- Para empezar a compilar la solución de virtualización de escritorio, consulte [crear un inquilino en el escritorio de Windows Virtual](https://docs.microsoft.com/azure/virtual-desktop/tenant-setup-azure-active-directory).
- Para crear un grupo host dentro de su inquilino de Escritorio Virtual de Windows, consulte [crear un grupo host con Azure Marketplace](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace).
- Para configurar totalmente administrado recursos compartidos de archivos en la nube, consulte [configuración de recurso compartido de archivos de Azure](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-enable).
- Para configurar los contenedores de perfil FSLogix, consulte [configura un recurso compartido de perfil de usuario para un grupo host](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-user-profile).
- Para asignar usuarios a un grupo host, consulte [administrar grupos de aplicaciones de Escritorio Virtual de Windows](https://docs.microsoft.com/azure/virtual-desktop/manage-app-groups).
- Para obtener acceso a los recursos de Escritorio Virtual de Windows desde un explorador web, consulte [conectar con el escritorio de Windows Virtual](https://docs.microsoft.com/azure/virtual-desktop/connect-web).

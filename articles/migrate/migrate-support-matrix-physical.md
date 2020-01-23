---
title: Compatibilidad para la evaluación de servidores físicos con Azure Migrate
description: Obtenga información sobre la compatibilidad para la evaluación de servidores físicos con Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 32080605217cde78bd648ca6192f73d1025dea4c
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028761"
---
# <a name="support-matrix-for-physical-server-assessment"></a>Matriz de compatibilidad para la evaluación de servidores físicos 

Puede usar el [servicio Azure Migrate](migrate-overview.md) para evaluar y migrar máquinas a la nube de Microsoft Azure. En este artículo se resumen los valores de compatibilidad y las limitaciones para evaluar y migrar servidores físicos locales.


## <a name="overview"></a>Información general

A fin de evaluar los equipos locales para la migración a Azure con este artículo, debe agregar la herramienta Azure Migrate: Server Assessment a un proyecto de Azure Migrate. Tendrá que implementar el [dispositivo de Azure Migrate](migrate-appliance.md). El dispositivo detecta los equipos locales de forma continuada y envía metadatos y datos de configuración y rendimiento a Azure. Después de la detección, recopile en grupos los equipos detectados y ejecute una valoración del grupo.

## <a name="limitations"></a>Limitaciones

**Soporte técnico** | **Detalles**
--- | ---
**Límites de evaluación**| Descubra y evalúe hasta 35 000 servidores físicos en un solo [proyecto](migrate-support-matrix.md#azure-migrate-projects).
**Límites del proyecto** | Puede crear varios proyectos en una suscripción a Azure. Un proyecto puede incluir servidores físicos y máquinas virtuales de VMware y de Hyper-V, hasta los límites de evaluación.
**Detección** | El dispositivo de Azure Migrate puede detectar hasta 250 servidores físicos.
**Valoración** | Puede agregar hasta 35 000 máquinas en un solo grupo.<br/><br/> Puede evaluar hasta 35 000 máquinas en una sola evaluación.

[Más información](concepts-assessment-calculation.md) sobre las evaluaciones.




## <a name="physical-server-requirements"></a>Requisitos del servidor físico

| **Soporte técnico**                | **Detalles**               
| :-------------------       | :------------------- |
| **Implementación de servidor físico**       | El servidor físico puede ser independiente o implementarse en un clúster. |
| **Permisos**           | **Windows:** Configure una cuenta de usuario local en todos los servidores de Windows que quiera incluir en la detección. Es necesario agregar la cuenta de usuario a estos grupos: Usuarios de escritorio remoto, Usuarios de Monitor de rendimiento y Usuarios del registro de rendimiento. <br/> **Linux:** Necesita una cuenta raíz en los servidores Linux que desee detectar. |
| **Sistema operativo** | Se admiten todos los sistemas operativos [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) y [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) excepto los siguientes:<br/> Windows Server 2003 <br/> SUSE Linux|


## <a name="azure-migrate-appliance-requirements"></a>Requisitos del dispositivo de Azure Migrate

Azure Migrate usa el [dispositivo de Azure Migrate](migrate-appliance.md) para la detección y la evaluación. El dispositivo para los servidores físicos se puede ejecutar en una máquina virtual o en una máquina física. Se configura mediante un script de PowerShell que se descarga desde Azure Portal.

- Obtenga información sobre los [requisitos del dispositivo](migrate-appliance.md#appliance---physical) para los servidores físicos.
- Obtenga información sobre las [direcciones URL](migrate-appliance.md#url-access) a las que tiene que acceder el dispositivo.

## <a name="port-access"></a>Acceso a puertos

En la tabla siguiente se resumen los requisitos de los puertos para la evaluación.

**Dispositivo** | **Connection**
--- | ---
**Dispositivo** | Conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del Escritorio remoto al dispositivo.<br/> Conexiones entrantes en el puerto 44368 para tener acceso de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: ``` https://<appliance-ip-or-name>:44368 ```<br/> Conexiones salientes en los puertos 443, 5671 y 5672 para enviar los metadatos de detección y rendimiento a Azure Migrate.
**Servidores físicos** | **Windows:** Conexiones entrantes en los puertos 443, los puertos WinRM 5985 (HTTP) y 5986 (HTTPS) para extraer los metadatos de configuración y rendimiento de los servidores de Windows. <br/> **Linux:**  Conexiones entrantes en el puerto 22 (UDP) para extraer los metadatos de configuración y rendimiento de los servidores Linux. |


## <a name="next-steps"></a>Pasos siguientes

[Preparación de la valoración de servidores físicos](tutorial-prepare-physical.md).

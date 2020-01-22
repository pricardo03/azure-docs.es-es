---
title: Compatibilidad con la evaluación de Hyper-V en Azure Migrate
description: Obtenga información sobre la compatibilidad de la evaluación de Hyper-V con Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 20bdbb16d2f0610f6519424141b09190eae3cc42
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028789"
---
# <a name="support-matrix-for-hyper-v-assessment"></a>Matriz de compatibilidad para la evaluación de Hyper-V

En este artículo se resumen los valores de compatibilidad y las limitaciones para evaluar máquinas virtuales de Hyper-V con [Azure Migrate: Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool). Si quiere obtener información sobre cómo migrar máquinas virtuales de Hyper-V a Azure, revise la [matriz de compatibilidad de migración](migrate-support-matrix-hyper-v-migration.md).

## <a name="overview"></a>Información general

Para evaluar las máquinas virtuales locales para la migración a Azure con este artículo, debe agregar la herramienta Azure Migrate: Server Assessment a un proyecto de Azure Migrate. Tendrá que implementar el [dispositivo de Azure Migrate](migrate-appliance.md). El dispositivo detecta las máquinas locales de forma continuada y envía datos de configuración y rendimiento a Azure. Después de la detección de la máquina, debe recopilar las máquinas detectadas en grupos y ejecutar una evaluación de un grupo.


## <a name="limitations"></a>Limitaciones

**Soporte técnico** | **Detalles**
--- | ---
**Límites de evaluación**| Detecte y evalúe hasta 35 000 máquinas virtuales de Hyper-V en un único [proyecto](migrate-support-matrix.md#azure-migrate-projects).
**Límites del proyecto** | Puede crear varios proyectos en una suscripción a Azure. Un proyecto puede incluir servidores físicos y máquinas virtuales de VMware y de Hyper-V, hasta los límites de evaluación.
**Detección** | El dispositivo de Azure Migrate puede detectar hasta 5000 máquinas virtuales de Hyper-V.<br/><br/> Un dispositivo puede conectarse a un total de 300 hosts de Hyper-V.
**Valoración** | Puede agregar hasta 35 000 máquinas en un solo grupo.<br/><br/> Puede acceder hasta a 35 000 máquinas virtuales en una única evaluación.

[Más información](concepts-assessment-calculation.md) sobre las evaluaciones.



## <a name="hyper-v-host-requirements"></a>Requisitos del host de Hyper-V:

| **Soporte técnico**                | **Detalles**               
| :-------------------       | :------------------- |
| **Implementación del host**       | El host de Hyper-V puede ser independiente o implementarse en un clúster. |
| **Permisos**           | Necesita permisos de administrador en el host de Hyper-V. <br/> Como alternativa, si no quiere asignar permisos de administrador, cree una cuenta de usuario local o de dominio y agregue el usuario a estos grupos: Usuarios de administración remota, Administradores de Hyper-V y Usuarios del monitor de sistema. |
| **Sistema operativo host** | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2.<br/> No puede evaluar VM que se encuentran en hosts de Hyper-V que ejecutan Windows Server 2012. |
| **Comunicación remota de PowerShell**   | Debe estar habilitada en cada host. |
| **Réplica de Hyper-V**       | Si usa la réplica de Hyper-V (o tiene varias VM con los mismos identificadores de VM) y detecta las VM originales y las replicadas mediante Azure Migrate, es posible que la evaluación generada por Azure Migrate no sea precisa. |


## <a name="hyper-v-vm-requirements"></a>Requisitos de la máquina virtual de Hyper-V

| **Soporte técnico**                  | **Detalles**               
| :----------------------------- | :------------------- |
| **Sistema operativo** | Azure admite todos los sistemas operativos [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) y [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros). |
| **Servicio de integración**       | Los [servicio de integración de Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) deben ejecutarse en las VM que evalúe, con el fin de capturar la información del sistema operativo. |


## <a name="azure-migrate-appliance-requirements"></a>Requisitos del dispositivo de Azure Migrate

Azure Migrate usa el [dispositivo de Azure Migrate](migrate-appliance.md) para la detección y la evaluación. El dispositivo para Hyper-V se ejecuta en una máquina virtual de Hyper-V, y se implementa mediante un disco duro virtual de Hyper-V comprimido que se descarga de Azure Portal. 

- Obtenga información sobre los [requisitos del dispositivo](migrate-appliance.md#appliance---hyper-v) para Hyper-V.
- Obtenga información sobre las [direcciones URL](migrate-appliance.md#url-access) a las que tiene que acceder el dispositivo.

## <a name="port-access"></a>Acceso a puertos

En la tabla siguiente se resumen los requisitos de los puertos para la evaluación.

**Dispositivo** | **Connection**
--- | ---
**Dispositivo** | Conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del Escritorio remoto al dispositivo.<br/> Conexiones entrantes en el puerto 44368 para tener acceso de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: ``` https://<appliance-ip-or-name>:44368 ```<br/> Conexiones salientes en los puertos 443, 5671 y 5672 para enviar los metadatos de detección y rendimiento a Azure Migrate.
**Host o clúster de Hyper-V** | Conexiones entrantes en los puertos de WinRM 5985 (HTTP) y 5986 (HTTPS) para extraer los metadatos de configuración y rendimiento de las VM de Hyper-V mediante una sesión de Modelo de información común (CIM).



## <a name="next-steps"></a>Pasos siguientes

[Preparación de la evaluación de máquinas virtuales de Hyper-V](tutorial-prepare-hyper-v.md)

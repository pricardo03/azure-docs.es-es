---
title: Matriz de compatibilidad de Azure Migrate para la evaluación y migración de Hyper-V
description: Resume la configuración y las limitaciones para la evaluación y migración de Hyper-V mediante el servicio Azure Migrate.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: raynew
ms.openlocfilehash: dd100e2390cdd2731df498379e376bde4cf2b87d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498691"
---
# <a name="support-matrix-for-hyper-v-assessment-and-migration"></a>Matriz de compatibilidad para la evaluación y migración de Hyper-V

Puede usar el [servicio Azure Migrate](migrate-overview.md) para evaluar y migrar máquinas a la nube de Microsoft Azure. En este artículo se resumen los valores de compatibilidad y las limitaciones para evaluar y migrar VM de Hyper-V locales.



## <a name="hyper-v-scenarios"></a>Escenarios de Hyper-V

En la tabla se resumen los escenarios admitidos para las VM de Hyper-V.

**Implementación** | **Detalles***
--- | ---
**Evaluación de VM de Hyper-V locales** | [Configure](tutorial-prepare-hyper-v.md) la primera evaluación.<br/><br/> [Ejecute](scale-hyper-v-assessment.md) una evaluación a gran escala.
**Migración de máquinas virtuales de Hyper-V a Azure** | [Pruebe](tutorial-migrate-hyper-v.md) a migrar a Azure.

## <a name="azure-migrate-projects"></a>Proyectos de Azure Migrate

**Soporte técnico** | **Detalles**
--- | ---
Permisos de Azure | Necesita permisos de Colaborador o Propietario en la suscripción para crear un proyecto de Azure Migrate.
Máquinas virtuales de Hyper-V | Evalúe hasta 35 000 máquinas virtuales de Hyper-V en un único proyecto. Puede tener varios proyectos en una suscripción a Azure. Un proyecto puede incluir máquinas virtuales de VMware y máquinas virtuales de Hyper-V, hasta los límites de evaluación.
Geography | [Revisión](migrate-support-matrix.md#supported-geographies) de las ubicaciones geográficas admitidas.


## <a name="assessment-hyper-v-host-requirements"></a>Evaluación de los requisitos del host de Hyper-V

| **Soporte técnico**                | **Detalles**               
| :-------------------       | :------------------- |
| **Implementación del host**       | El host de Hyper-V puede ser independiente o implementarse en un clúster. |
| **Permisos**           | Necesita permisos de administrador en el host de Hyper-V. <br/> Como alternativa, si no quiere asignar permisos de administrador, cree una cuenta de usuario local o de dominio y agregue el usuario a estos grupos: Usuarios de administración remota, Administradores de Hyper-V y Usuarios del monitor de sistema. |
| **Sistema operativo host** | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2.<br/> No puede evaluar VM que se encuentran en hosts de Hyper-V que ejecutan Windows Server 2012. |
| **Comunicación remota de PowerShell**   | Debe estar habilitada en cada host. |
| **Réplica de Hyper-V**       | Si usa la réplica de Hyper-V (o tiene varias VM con los mismos identificadores de VM) y detecta las VM originales y las replicadas mediante Azure Migrate, es posible que la evaluación generada por Azure Migrate no sea precisa. |


## <a name="assessment-hyper-v-vm-requirements"></a>Evaluación de los requisitos de VM de Hyper-V

| **Soporte técnico**                  | **Detalles**               
| :----------------------------- | :------------------- |
| **Sistema operativo** | Azure admite todos los sistemas operativos [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) y [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros). |
| **Servicio de integración**       | Los [servicio de integración de Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) deben ejecutarse en las VM que evalúe, con el fin de capturar la información del sistema operativo. |



## <a name="assessment-appliance-requirements"></a>Evaluación: requisitos del dispositivo

Para la evaluación, Azure Migrate ejecuta un dispositivo ligero que detecta VM de Hyper-V y envía los metadatos de las VM y los datos de rendimiento a Azure Migrate. El dispositivo se ejecuta en una VM de Hyper-V, y lo configura mediante un disco duro virtual de Hyper-V comprimido que se descarga de Azure Portal. En la tabla siguiente se resumen los requisitos del dispositivo.

| **Soporte técnico**                | **Detalles**               
| :-------------------       | :------------------- |
| **Implementación del dispositivo**   |  El dispositivo se implementa como VM de Hyper-V.<br/> La máquina virtual del dispositivo proporcionada por Azure Migrate es la versión 5.0 de la máquina virtual de Hyper-V.<br/> El host de Hyper-V debe ejecutarse en Windows Server 2012 R2 o posterior.<br/> El host necesita espacio suficiente para asignar 16 GB de RAM, 8 vCPU, alrededor de 80 GB de espacio de almacenamiento y conmutador externo para la máquina virtual del dispositivo.<br/> El dispositivo requiere una dirección IP estática o dinámica y acceso a Internet.
| **Proyecto de Azure Migrate**  |  Un dispositivo solo puede estar asociado a un proyecto.<br/> Se puede asociar cualquier número de dispositivos a un solo proyecto.<br/> Puede evaluar hasta 35 000 VM en un proyecto.
| **Hosts de Hyper-V**          | Un dispositivo puede conectarse hasta a 300 hosts de Hyper-V.
| **Detección**              | Un solo dispositivo puede detectar hasta 5000 máquinas virtuales.
| **Grupo de evaluación**       | Puede agregar hasta 35 000 máquinas en un solo grupo.
| **Valoración**             | Puede acceder hasta a 35 000 máquinas virtuales en una única evaluación.



## <a name="assessment-appliance-url-access"></a>Evaluación: acceso a la dirección URL del dispositivo

Para evaluar las VM, el dispositivo de Azure Migrate necesita conectividad a Internet.

- Al implementar el dispositivo, Azure Migrate hace una comprobación de conectividad con las direcciones URL que se resumen en la tabla siguiente.
- Si usa un proxy basado en URL, permita el acceso a las direcciones URL de la tabla, y asegúrese de que el proxy resuelva los registros CNAME recibidos al buscar las direcciones URL.
- Si tiene un proxy de interceptación, es posible que tenga que importar el certificado de servidor del servidor proxy al dispositivo.


**URL** | **Detalles**  
--- | ---
*.portal.azure.com | Navegación a Azure Portal
\* .windows.net <br/> *.msftauth.net <br/> *.msauth.net <br/> *.microsoft.com <br/> *.live.com  | Inicie sesión en la suscripción de Azure
*.microsoftonline.com <br/> *.microsoftonline-p.com | Creación de aplicaciones de Azure Active Directory para las comunicaciones entre el dispositivo y el servicio.
management.azure.com | Creación de aplicaciones de Azure Active Directory para las comunicaciones entre el dispositivo y el servicio.
dc.services.visualstudio.com | Registro y supervisión
*.vault.azure.net | Administración de secretos en Azure Key Vault al comunicarse entre el dispositivo y el servicio.
aka.ms/* | Permiso de acceso a vínculos aka.
https://download.microsoft.com/download/* | Permiso de descarga del sitio de descarga de Microsoft.



## <a name="assessment-port-requirements"></a>Evaluación: requisitos de puertos

En la tabla siguiente se resumen los requisitos de los puertos para la evaluación.

**Dispositivo** | **Connection**
--- | ---
**Dispositivo** | Conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del Escritorio remoto al dispositivo.<br/> Conexiones entrantes en el puerto 44368 para tener acceso de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: ``` https://<appliance-ip-or-name>:44368 ```<br/> Conexiones salientes en los puertos 443, 5671 y 5672 para enviar los metadatos de detección y rendimiento a Azure Migrate.
**Host o clúster de Hyper-V** | Conexiones entrantes en los puertos de WinRM 5985 (HTTP) y 5986 (HTTPS) para extraer los metadatos de configuración y rendimiento de las VM de Hyper-V mediante una sesión de Modelo de información común (CIM).

## <a name="migration-limitations"></a>Migración: limitaciones
Puede seleccionar hasta 10 máquinas virtuales a la vez para la replicación. Si quiere migrar más máquinas, replique en grupos de 10.

## <a name="migration-hyper-v-host-requirements"></a>Migración: requisitos del host de Hyper-V

| **Soporte técnico**                | **Detalles**               
| :-------------------       | :------------------- |
| **Implementación del host**       | El host de Hyper-V puede ser independiente o implementarse en un clúster. |
| **Permisos**           | Necesita permisos de administrador en el host de Hyper-V. |
| **Sistema operativo host** | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2. |

## <a name="migration-hyper-v-vm-requirements"></a>Migración: requisitos de las VM de Hyper-V

| **Soporte técnico**                  | **Detalles**               
| :----------------------------- | :------------------- |
| **Sistema operativo** | Azure admite todos los sistemas operativos [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) y [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros). |
| **Permisos**           | Necesita permisos de administrador en cada VM de Hyper-V que quiera evaluar. |
| **Servicio de integración**       | Los [servicio de integración de Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) deben ejecutarse en las VM que evalúe, con el fin de capturar la información del sistema operativo. |
| **Cambios necesarios para Azure** | Es posible que algunas máquinas virtuales requieran cambios para poder ejecutarse en Azure. Azure Migrate hace estos cambios automáticamente en los siguientes sistemas operativos:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> - Debian 7, 8<br/><br/> En el caso de otros sistemas operativos, debe hacer los ajustes manualmente antes de la migración. Los artículos pertinentes contienen instrucciones sobre cómo hacerlo. |
| **Arranque de Linux**                 | Si/boot está en una partición dedicada, debe residir en el disco del sistema operativo y no distribuirse en varios discos.<br/> Si/boot forma parte de la partición raíz (/), la partición "/" debe estar en el disco del sistema operativo y no abarcar otros discos. |
| **Arranque UEFI**                  | La máquina virtual migrada en Azure se convertirá automáticamente en una VM de arranque del BIOS. La máquina virtual debe estar ejecutando Windows Server 2012 o una versión posterior. El disco del sistema operativo debe tener un máximo de cinco particiones y el tamaño del disco del sistema operativo debe ser inferior a 300 GB.
  |
| **Tamaño del disco**                  | 2 TB para el disco del sistema operativo y 4 TB para los discos de datos.
| **Número de discos** | Un máximo de 16 discos por VM.
| **Discos/volúmenes cifrados**    | No se admiten para la migración. |
| **Discos RDM/de acceso directo**      | No se admiten para la migración. |
| **Disco compartido** | Las VM que usan discos compartidos no se admiten para la migración.
| **NFS**                        | Los volúmenes NFS montados como volúmenes en las máquinas virtuales no se replicarán. |
| **ISCSI**                      | Las VM con destinos iSCSI no se admiten para la migración.
| **Disco de destino**                | Puede migrar las VM de Azure con discos administrados únicamente. |
| **IPv6** | No compatible.
| **Formación de equipos NIC** | No compatible.
| **Azure Site Recovery** | No se puede replicar mediante la migración del servidor de Azure Migrate si la VM está habilitada para la replicación con Azure Site Recovery.





## <a name="migration-hyper-v-host-url-access"></a>Migración: acceso a la dirección URL del host de Hyper-V

En la tabla siguiente se resumen los requisitos de acceso de la dirección URL para los hosts de Hyper-V.

**URL** | **Detalles**  
--- | ---
login.microsoftonline.com | Control de acceso y administración de identidades mediante Active Directory.
*.backup.windowsazure.com | Transferencia y coordinación de datos de replicación.
*.hypervrecoverymanager.windowsazure.com | Conectarse a las direcciones URL del servicio Azure Migrate.
\* .blob.core.windows.net | Carga de datos en las cuentas de almacenamiento.
dc.services.visualstudio.com | Carga de los registros de aplicaciones que se usan para la supervisión interna.
time.windows.com | Verificación de la sincronización de la hora entre el sistema y la hora global.

## <a name="migration-port-access"></a>Migración: acceso a puertos

En la tabla siguiente se resumen los requisitos de puertos en hosts y VM de Hyper-V para la migración de VM.

**Dispositivo** | **Connection**
--- | ---
Hosts/VM de Hyper-V | Conexiones salientes en el puerto HTTPS 443 para enviar datos de replicación de VM a Azure Migrate.




## <a name="next-steps"></a>Pasos siguientes

[Preparación de la evaluación de VM de Hyper-V](tutorial-prepare-hyper-v.md) para la migración.

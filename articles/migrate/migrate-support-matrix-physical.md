---
title: Matriz de soporte de Azure Migrate para la evaluación y migración del servidor físico
description: Resume la configuración y las limitaciones de la evaluación y la migración del servidor físico con el servicio de Azure Migrate.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: 55ada93e6a77dd7e31640988bad95034f77c90dd
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74091890"
---
# <a name="support-matrix-for-physical-server-assessment-and-migration"></a>Matriz de compatibilidad para la evaluación y migración de servidores físicos

Puede usar el [servicio Azure Migrate](migrate-overview.md) para evaluar y migrar máquinas a la nube de Microsoft Azure. En este artículo se resumen los valores de compatibilidad y las limitaciones para evaluar y migrar servidores físicos locales.

> [!NOTE]
> Si aquí se mencionan características que aún no aparecen en el portal de Azure Migrate, espere. Aparecerán aproximadamente la semana siguiente.


## <a name="physical-server-scenarios"></a>Escenarios de servidor físico

En la tabla se resumen los escenarios admitidos para los servidores físicos.

**Implementación** | **Detalles***
--- | ---
**Evaluación de servidores físicos locales** | [Configure](tutorial-prepare-physical.md) la primera evaluación.<br/><br/> [Ejecute](tutorial-assess-physical.md) una evaluación.
**Migración de servidores físicos a Azure** | [Pruebe](tutorial-migrate-physical-virtual-machines.md) a migrar a Azure.


## <a name="azure-migrate-projects"></a>Proyectos de Azure Migrate

**Soporte técnico** | **Detalles**
--- | ---
**Permisos de Azure** | Necesita permisos de colaborador o propietario en la suscripción para crear un proyecto de Azure Migrate.
**Servidores físicos** | Evalúe hasta 35 000 servidores físicos en un solo proyecto. Puede tener varios proyectos en una suscripción a Azure. Un proyecto puede incluir servidores físicos, máquinas virtuales de VMware y máquinas virtuales de Hyper-V, hasta los límites de evaluación.
**Geografía** | Puede crear proyectos de Azure Migrate en varias zonas geográficas. Aunque puede crear proyectos en geografías específicas, puede migrar máquinas o acceder a ellas, para otras ubicaciones de destino. La geografía del proyecto solo se usa para almacenar los metadatos detectados.

  **Geografía** | **Ubicación de almacenamiento de metadatos**
  --- | ---
  Azure Government | Gobierno de EE. UU. - Virginia
  Asia Pacífico | Asia Oriental o Sudeste Asiático
  Australia | Este de Australia o Sudeste de Australia
  Brasil | Sur de Brasil
  Canadá | Centro de Canadá o Este de Canadá
  Europa | Norte de Europa y Oeste de Europa
  Francia | Centro de Francia
  India | India central o India meridional
  Japón |  Este de Japón u Oeste de Japón
  Corea | Centro de Corea del Sur o Sur de Corea del Sur
  Reino Unido | Sur de Reino Unido u Oeste de Reino Unido
  Estados Unidos | Centro de EE. UU. u Oeste de EE. UU. 2


 > [!NOTE]
 > La compatibilidad con Azure Government solo está disponible actualmente para la [versión anterior](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) de Azure Migrate.


## <a name="assessment-physical-server-requirements"></a>Evaluación: requisitos del servidor físico

| **Soporte técnico**                | **Detalles**               
| :-------------------       | :------------------- |
| **Implementación de servidor físico**       | El servidor físico puede ser independiente o implementarse en un clúster. |
| **Permisos**           | **Windows:** Configure una cuenta de usuario local en todos los servidores de Windows que desee incluir en la detección. Es necesario agregar la cuenta de usuario a estos grupos: Usuarios de escritorio remoto, Usuarios del monitor de rendimiento y Usuarios del registro de rendimiento. <br/> **Linux:** Necesita una cuenta raíz en los servidores Linux que desee detectar. |
| **Sistema operativo** | Se admiten todos los sistemas operativos [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) y [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) excepto los siguientes:<br/> Windows Server 2003 <br/> SUSE Linux|


## <a name="assessment-appliance-requirements"></a>Evaluación: requisitos del dispositivo

Para la evaluación, Azure Migrate ejecuta una aplicación ligera para detectar servidores físicos y envía metadatos de servidor y datos de rendimiento a Azure Migrate. El dispositivo se puede ejecutar en un servidor físico o en una máquina virtual y se configura mediante un script de PowerShell que se descarga desde el Azure Portal. En la tabla siguiente se resumen los requisitos del dispositivo.

| **Soporte técnico**                | **Detalles**               
| :-------------------       | :------------------- |
| **Implementación del dispositivo**   |  La aplicación se implementa en un servidor físico o una máquina virtual.<br/>  El equipo host debe estar ejecutando Windows Server 2012 R2 o posterior.<br/> El host necesita espacio suficiente para asignar 16 GB de RAM, 8 vCPU, alrededor de 80 GB de espacio de almacenamiento y conmutador externo para la máquina virtual del dispositivo.<br/> El dispositivo requiere una dirección IP estática o dinámica y acceso a Internet.
| **Proyecto de Azure Migrate**  |  Un dispositivo solo puede estar asociado a un proyecto.<br/> Se puede asociar cualquier número de dispositivos a un solo proyecto.<br/> Puede evaluar hasta 35 000 máquinas en un proyecto.
| **Detección**              | Un solo dispositivo puede detectar hasta 250 servidores.
| **Grupo de evaluación**       | Puede agregar hasta 35 000 máquinas en un solo grupo.
| **Valoración**             | Puede evaluar hasta 35 000 máquinas en una sola evaluación.


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
**Servidores físicos** | **Windows:** Conexiones entrantes en los puertos 443 y 5989 para extraer los metadatos de configuración y rendimiento de los servidores de Windows. <br/> **Linux:**  Conexiones entrantes en el puerto 22 (UDP) para extraer los metadatos de configuración y rendimiento de los servidores Linux. |


## <a name="next-steps"></a>Pasos siguientes

[Preparación para la evaluación física del servidor](tutorial-prepare-physical.md) para la evaluación y la migración del servidor físico.

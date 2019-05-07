---
title: Solución de VMware de Azure por CloudSimple - CloudSimple mantenimiento y actualizaciones
description: Describe el proceso de servicio de CloudSimple de mantenimiento programado y actualizaciones
author: sharaths-cs
ms.author: dikamath
ms.date: 04/30/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4dde358f10e9ac5054297ff68a0971404c0dc135
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160251"
---
# <a name="cloudsimple-maintenance-and-updates"></a>Actualizaciones y mantenimiento CloudSimple

El entorno de nube privada está diseñado para no tener ningún punto único de error:

* Clústeres de ESXi están configurados con una alta disponibilidad de vSphere. Los clústeres tienen un tamaño para que tenga al menos un nodo de repuesto para proporcionar resistencia.
* VSAN, lo que requiere al menos tres nodos para proporcionar protección frente a un único error proporciona redundancia de almacenamiento principal. vSAN puede configurarse para proporcionar mayor resistencia para clústeres más grandes.
* vCenter PSC, NSX Manager máquinas virtuales y se configuran con la directiva de almacenamiento de RAID 10 para protegerse frente a errores de almacenamiento. Las máquinas virtuales están protegidas frente a errores de red o nodo por vSphere alta disponibilidad.
* Hosts ESXi tienen ventiladores redundantes y NIC.
* Conmutadores de TOR y la espina se configuran en pares de alta disponibilidad para proporcionar resistencia.

CloudSimple continuamente supervisa las siguientes máquinas virtuales de actividad y disponibilidad y proporciona los SLA de disponibilidad:

* Hosts ESXi
* vCenter
* PSC
* NSX Manager

CloudSimple también supervisa los siguientes errores de forma continua:

* Discos duros
* Puertos de NIC física
* Servidores
* aficionados
* Potencia
* Conmutadores
* Puertos de conmutador

Si se produce un error en un disco o nodo, se agrega automáticamente un nuevo nodo en el clúster de VMware afectado para devolverlos al estado inmediatamente.

CloudSimple realiza copias de seguridad, mantiene y actualiza estos elementos de VMware en las nubes privadas:

* ESXi
* Servicios de plataforma de vCenter
* Controller
* vSAN
* NSX

## <a name="back-up-and-restore"></a>Copia de seguridad y restauración

Copia de seguridad de CloudSimple incluye:

* Copias de seguridad incrementales por la noche de vCenter, PSC y DVS reglas.
* Uso de vCenter las API nativas para realizar una copia de seguridad de componentes en el nivel de aplicación.
* Copia de seguridad automática antes de cualquier actualización o la actualización de software de administración de VMware.
* Cifrado de datos en el origen de vCenter, antes de transfieren datos a través de un canal cifrado TLS 1.2 en Azure. Los datos se almacenan en un blob de Azure donde se replica entre regiones.

Puede solicitar una restauración, abra un [solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="maintenance"></a>Mantenimiento 

CloudSimple hace varios tipos de mantenimiento planeado.

### <a name="backendinternal-maintenance"></a>Mantenimiento de back-end/interno

Este mantenimiento normalmente implica volver a configurar los recursos físicos o instalar las revisiones de software. Esto no afecta el consumo de los recursos de proceso de mantenimiento normal. Con NIC redundantes en cada bastidor físico, el tráfico de red normal y las operaciones de nube privada no están afectadas. Es posible que observe un impacto en el rendimiento solo si su organización espera que se usará el ancho de banda totalmente redundante durante el intervalo de mantenimiento.

### <a name="cloudsimple-portal-maintenance"></a>Mantenimiento del portal CloudSimple

Tiempo de inactividad limitado del servicio es necesario cuando se actualiza el plano de control CloudSimple o infraestructura. Actualmente, los intervalos de mantenimiento pueden ser tan frecuentes como una vez al mes. Se espera que la frecuencia de rechazar con el tiempo. CloudSimple proporciona una notificación de mantenimiento del portal y mantiene el intervalo más breve posible. Durante un intervalo de mantenimiento del portal, siguen funcionando sin afectar los siguientes servicios:

* Las aplicaciones y el plano de administración de VMware
* acceso a vCenter
* Todas las redes y almacenamiento
* Todo el tráfico de Azure

### <a name="vmware-infrastructure-maintenance"></a>Mantenimiento de la infraestructura de VMware

En ocasiones, es necesario realizar cambios en la configuración de la infraestructura de VMware.  Actualmente, estos intervalos pueden producirse cada 1 ó 2 meses, pero se espera que la frecuencia de rechazar con el tiempo. Este tipo de mantenimiento normalmente puede realizarse sin interrumpir el consumo de los servicios CloudSimple normal. Durante un intervalo de mantenimiento de VMware, seguirán funcionando sin afectar los siguientes servicios:

* Las aplicaciones y el plano de administración de VMware
* acceso a vCenter
* Todas las redes y almacenamiento
* Todo el tráfico de Azure

## <a name="updates-and-upgrades"></a>Las actualizaciones

CloudSimple es responsable de la administración del ciclo de vida del software de VMware (ESXi, vCenter, PSC y NSX) en la nube privada.

Actualizaciones de software incluyen:

* **Las revisiones**. Las revisiones de seguridad o correcciones publicadas por VMware.
* **Actualizaciones**. Cambio de versión secundaria de un componente de la pila de VMware.
* **Las actualizaciones**. Cambio de versión principal de un componente de la pila de VMware.

CloudSimple comprueba una revisión de seguridad críticas en cuanto esté disponible de VMware. Por el SLA, CloudSimple implementa la revisión de seguridad en entornos de nube privada dentro de una semana.

CloudSimple proporciona actualizaciones de mantenimiento trimestrales a componentes de software de VMware. Cuando está disponible una nueva versión principal del software de VMware, CloudSimple trabaja con clientes para coordinar una ventana de mantenimiento adecuado para la actualización.

## <a name="next-steps"></a>Pasos siguientes

[Copia de seguridad de máquinas virtuales de carga de trabajo con Veeam](https://docs.azure.cloudsimple.com/backup-workloads-veeam/).
---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: barclayn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/19/2019
ms.author: barclayn
ms.custom: include file
ms.openlocfilehash: 60d4957f65c124de683c6d156561de3b52aaec94
ms.sourcegitcommit: a7a9d7f366adab2cfca13c8d9cbcf5b40d57e63a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71164083"
---
En este artículo se incluyen recomendaciones de seguridad para Azure Virtual Machines. La implementación de estas recomendaciones le ayudará a cumplir con las obligaciones de seguridad que se describen en nuestro modelo de responsabilidad compartido y mejorará la seguridad general de sus soluciones de aplicación de web. Para más información sobre lo que Microsoft hace para cumplir las responsabilidades del proveedor de servicios, consulte [Responsabilidades compartidas de la informática en la nube](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf).

Algunas de las recomendaciones incluidas en este artículo se pueden supervisar automáticamente mediante Azure Security Center. Azure Security Center es la primera línea de defensa en la protección de los recursos de Azure. Se encarga de analizar el estado de seguridad de los recursos de Azure para identificar posibles vulnerabilidades de seguridad. Después, proporciona recomendaciones sobre cómo abordarlas.

- Para más información sobre las recomendaciones de seguridad de Azure Security Center, consulte [Recomendaciones de seguridad en Azure Security Center](../articles/security-center/security-center-recommendations.md).
- Para más información sobre Azure Security Center, consulte [¿Qué es Azure Security Center?](../articles/security-center/security-center-intro.md)

## <a name="recommendations"></a>Recomendaciones

| Category | Recomendación | Comentarios | Security Center |
|-|-|----|--|
| General | Al crear imágenes de máquina virtual personalizadas, use las actualizaciones más recientes. | Antes de crear las imágenes, asegúrese de que todas las actualizaciones más recientes están instaladas para el sistema operativo, así como todas las aplicaciones que formarán parte de la imagen.  | - |
| General | Mantener actualizadas las máquinas virtuales | Puede usar la solución [Update Management](../articles/automation/automation-update-management.md) de Azure Automation para administrar las actualizaciones del sistema operativo de los equipos Windows y Linux de Azure. | [Sí](../articles/security-center/security-center-apply-system-updates.md) |
| General | Creación de copias de seguridad de las máquinas virtuales | [Azure Backup](../articles/backup/backup-overview.md) ayuda a proteger los datos de la aplicación con unos costos operativos mínimos. Los errores de una aplicación pueden dañar los datos, y los errores humanos pueden crear errores en las aplicaciones. Con Azure Backup, las máquinas virtuales que ejecutan Windows y Linux están protegidas. | - |
| General | Uso de varias máquinas virtuales para mayor resistencia y disponibilidad | Si la máquina virtual ejecuta aplicaciones que necesitan tener una alta disponibilidad, debe tener varias máquinas virtuales o [conjuntos de disponibilidad](../articles/virtual-machines/windows/manage-availability.md). | - |
| General | Adopción de una estrategia de continuidad empresarial y recuperación ante desastres (BCDR) | Azure Site Recovery le permite elegir entre diferentes opciones diseñadas para satisfacer sus necesidades de continuidad empresarial. Admite diferentes escenarios de replicación y de conmutación por error. Para más información, consulte [Acerca de Site Recovery](../articles/site-recovery/site-recovery-overview.md). | - |
| Administración de identidades y acceso | Centralización de la autenticación de la máquina virtual | Puede centralizar la autenticación de las máquinas virtuales Windows y Linux con la [autenticación de Azure AD](../articles/active-directory/develop/authentication-scenarios.md). | - |
| Seguridad de los datos | Cifrado de discos del sistema operativo | [Azure Disk Encryption](../articles/security/azure-security-disk-encryption-overview.md) le ayuda a cifrar discos de las máquinas virtuales IaaS con Windows y Linux. El cifrado de los discos hace que el contenido sea ilegible sin las claves necesarias. El cifrado de disco protege los datos almacenados del acceso no autorizado que, de lo contrario, sería posible si se copiara el disco.| [Sí](../articles/security-center/security-center-apply-disk-encryption.md) |
| Seguridad de los datos | Cifrado de los discos de datos | [Azure Disk Encryption](../articles/security/azure-security-disk-encryption-overview.md) le ayuda a cifrar discos de las máquinas virtuales IaaS con Windows y Linux. El cifrado de los discos hace que el contenido sea ilegible sin las claves necesarias. El cifrado de disco protege los datos almacenados del acceso no autorizado que, de lo contrario, sería posible si se copiara el disco.| -  |
| Seguridad de los datos | Límite del software instalado | Limitar el software instalado a lo que se necesita para implementar correctamente la solución ayuda a reducir la superficie expuesta a ataques de la solución. | - |
| Seguridad de los datos | Uso de antimalware y antivirus | En Azure, puede usar el software antimalware de proveedores de seguridad como Microsoft, Symantec, Trend Micro y Kaspersky. Este software le ayudará a proteger las máquinas virtuales de archivos malintencionados, adware y otras amenazas. Puede implementar la protección de antimalware de Microsoft en función de las cargas de trabajo de aplicaciones, con configuración de protección básica o personalizada avanzada que incluye supervisión antimalware. Para más información sobre Microsoft Antimalware para Azure, consulte [Microsoft antimalware para Azure Cloud Services y Virtual Machines](../articles/security/azure-security-antimalware.md). | - |
| Seguridad de los datos | Almacenamiento seguro de claves y secretos | La simplificación de la administración de los secretos y claves al proporcionar a los propietarios de la aplicación una opción segura administrada centralmente le permite reducir el riesgo de que se produzcan riesgos o fugas accidentales. Azure Key Vault puede guardar de manera segura las claves en módulos de seguridad de hardware (HSM) que tienen la certificación FIPS 140-2 nivel 2. Si necesita almacenar las claves y secretos mediante el nivel 3 de FIPs 140,2, puede usar [Azure Dedicated HSM](../articles/dedicated-hsm/overview.md). | - |
| Redes | Restricción del acceso a los puertos de administración | Los atacantes analizan los intervalos de IP de la nube pública en busca de puertos de administración abiertos e intentan realizar ataques "sencillos", como contraseñas comunes y vulnerabilidades conocidas sin revisiones. [El acceso a las máquinas virtuales Just-In-Time (JIT)](../articles/security-center/security-center-just-in-time.md) se puede usar para bloquear el tráfico entrante a las máquinas virtuales de Azure, lo que reduce la exposición a ataques y, al mismo tiempo, se proporciona acceso sencillo para conectarse a las máquinas virtuales cuando sea necesario. | - |
| Redes | Límite de acceso a la red | Los grupos de seguridad de red le permiten restringir el acceso a la red y controlar el número de puntos de conexión expuestos. Para más información, consulte [Crear, modificar o eliminar un grupo de seguridad de red ](../articles/virtual-network/manage-network-security-group.md). | - |
| Supervisión | Supervisión de las máquinas virtuales | Puede usar [Azure Monitor para máquinas virtuales](../articles/azure-monitor/insights/vminsights-overview.md) para supervisar el estado de las máquinas virtuales y los conjuntos de escalado de máquinas virtuales de Azure. Los problemas de rendimiento con una máquina virtual pueden provocar la interrupción del servicio, lo que infringe el principio de seguridad de disponibilidad. | - |

## <a name="next-steps"></a>Pasos siguientes

Póngase en contacto con el proveedor de la aplicación para ver si hay más requisitos de seguridad. Para obtener más información sobre el desarrollo de aplicaciones seguras, vea [Documentación sobre desarrollo seguro](../articles/security/fundamentals/abstract-develop-secure-apps.md).
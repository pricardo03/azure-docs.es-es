---
title: Servicios y tecnologías de seguridad de Azure | Microsoft Docs
description: El artículo proporciona una lista exclusiva de las tecnologías y los servicios de seguridad de Azure.
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: a5a7f60a-97e2-49b4-a8c5-7c010ff27ef8
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2018
ms.author: barclayn
ms.openlocfilehash: 32174af9edcd6da7b87b6b38c0824fa7f07aa8f9
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49384894"
---
# <a name="security-services-and-technologies-available-on-azure"></a>Servicios y tecnologías de seguridad disponibles en Azure

En nuestras conversaciones con los clientes de Azure actuales y futuros, se nos pregunta a menudo si tenemos una lista de todos los servicios y tecnologías relacionados con la seguridad que ofrece Azure.

Al evaluar las opciones del proveedor de servicios en la nube, es útil tener esta información. Por este motivo, hemos preparado esta lista para que pueda empezar.

Con el tiempo, esta lista cambiará y aumentará, igual que lo hace Azure. Asegúrese de que consultar esta página periódicamente para mantenerse informado sobre nuestros servicios relacionados con la seguridad y las tecnologías.

## <a name="general-azure-security"></a>Seguridad general de Azure
|Servicio|DESCRIPCIÓN|
|--------|--------|
|[Azure&nbsp;Security&nbsp;Center](../security-center/security-center-intro.md)| Solución de protección que proporciona administración de la seguridad y protección avanzada contra amenazas para cargas de trabajo en la nube híbrida.|
|[Azure Key Vault](../key-vault/key-vault-overview.md)| Almacén de secretos seguro para las contraseñas, las cadenas de conexión y otra información que necesita para mantener sus aplicaciones en funcionamiento. |
|[Log Analytics](../log-analytics/log-analytics-overview.md)|Servicio de supervisión que recopila datos de telemetría y otros datos, y proporciona un motor de lenguaje de consultas y análisis para proporcionar información detallada sobre sus aplicaciones y recursos. Puede utilizarse solo o con otros servicios como Security Center. |
|[Documentación de Azure Dev/Test Lab](../devtest-lab/devtest-lab-overview.md)|Servicio que ayuda a los desarrolladores y evaluadores a crear rápidamente entornos de Azure al tiempo que se optimizan los recursos y se controlan los costos.  |

<!---|[Azure&nbsp;Disk&nbsp;Encryption](azure-security-disk-encryption-overview.md)| THIS WILL GO TO THE NEW OVERVIEW TOPIC MEGHAN STEWART IS WRITING|--->

## <a name="storage-security"></a>Seguridad para almacenamiento
|Servicio|DESCRIPCIÓN|
|------|--------|
| [Azure&nbsp;Storage&nbsp;Service&nbsp;Encryption](../storage/common/storage-service-encryption.md)|Característica de seguridad que permite cifrar automáticamente los datos en Azure Storage.   |
|[Documentación de StorSimple](../storsimple/storsimple-ova-overview.md)| Solución de almacenamiento integrada que administra las tareas de almacenamiento entre los dispositivos locales y el almacenamiento en la nube de Azure.|
|[Cifrado del lado de cliente y Almacén de claves de Azure para el Almacenamiento de Microsoft Azure](../storage/common/storage-client-side-encryption.md)| Solución de cifrado en lado de cliente que cifra los datos en aplicaciones de cliente antes de cargarlos en Azure Storage. También descifra los datos mientras se descargan. |
| [Firmas de acceso compartido, Parte 1: Descripción del modelo SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md)|Una firma de acceso compartido ofrece acceso delegado a recursos en la cuenta de almacenamiento.  |
|[Acerca de las cuentas de Azure Storage](../storage/common/storage-create-storage-account.md)| Método de control de acceso para Azure Storage que se utiliza para la autenticación cuando se accede a la cuenta de almacenamiento. |
|[Introducción a Almacenamiento de archivos de Azure en Windows](../storage/files/storage-files-introduction.md)|Tecnología de seguridad de red que habilita el cifrado de red automático para el protocolo de uso compartido de archivos Bloque de mensajes del servidor (SMB). |
|[Análisis de Azure Storage](https://docs.microsoft.com/rest/api/storageservices/Storage-Analytics)| Tecnología de generación y registro de métricas para los datos de la cuenta de almacenamiento. |

<!------>

## <a name="database-security"></a>Seguridad de bases de datos
|Servicio|DESCRIPCIÓN|
|------|--------|
| [Azure&nbsp;SQL&nbsp;Firewall](../sql-database/sql-database-firewall-configure.md)|Característica de control de acceso de red que protege frente a ataques basados en red a una base de datos. |
|[Cifrado de&nbsp;nivel de celda de&nbsp;Azure&nbsp;SQL](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)| Tecnología de seguridad de base de datos que proporciona cifrado en un nivel más pormenorizado.  |
| [Cifrado de conexión de&nbsp;Azure&nbsp;SQL](../sql-database/sql-database-control-access.md)|Para proporcionar seguridad, SQL Database controla el acceso con reglas de firewall que limitan la conectividad por dirección IP, con mecanismos de autenticación que requieren a los usuarios que demuestren su identidad y con mecanismos de autorización que limitan a los usuarios el acceso a datos y acciones específicos. |
| [Always Encrypted (Database Engine)](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)|Protege la información confidencial, como números de tarjetas de crédito o números de identificación nacionales (por ejemplo, números de la seguridad social de EE. UU.), almacenados en bases de datos de Azure SQL Database o SQL Server.  |
| [Cifrado de datos transparente de&nbsp;Azure&nbsp;SQL](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current)| Característica de seguridad de base de datos que cifra el almacenamiento de una base de datos completa. |
| [Introducción a la auditoría de SQL Database](../sql-database/sql-database-auditing.md)|Característica de auditoría de bases de datos que realiza un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de su cuenta de Azure Storage.  |


## <a name="identity-and-access-management"></a>Administración de identidades y acceso
|Servicio|DESCRIPCIÓN|
|------|--------|
| [Control de acceso&nbsp;basado en&nbsp;rol de&nbsp;Azure](../active-directory/role-based-access-control-configure.md)|Característica de control de acceso diseñada para que los usuarios accedan únicamente a los recursos necesarios en función de sus roles dentro de la organización.  |
| [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)|Repositorio de autenticación basado en la nube que admite un directorio en la nube de varios inquilinos y varios servicios de administración de identidades en Azure.  |
| [Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-overview.md)|Servicio de administración de identidades que permite controlar la manera en que los clientes se registran, inician sesión y administran sus perfiles al usar las aplicaciones de Azure.   |
| [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md)| Una versión en la nube y administrada de Active Directory Domain Services. |
| [Azure Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md)| Aprovisionamiento de seguridad que utiliza diferentes formas de autenticación y comprobación antes de permitir el acceso a información protegida. |

## <a name="backup-and-disaster-recovery"></a>Copia de seguridad y recuperación ante desastres
|Servicio|DESCRIPCIÓN|
|------|--------|
| [Azure&nbsp;Backup](../backup/backup-introduction-to-azure-backup.md)| Servicio de Azure que se usa para realizar copias de seguridad y restaurar los datos en la nube de Azure. |
| [Azure&nbsp;Site&nbsp;Recovery](../site-recovery/site-recovery-overview.md)|Servicio en línea que replica las cargas de trabajo que se ejecutan en máquinas físicas y virtuales desde un sitio principal a una ubicación secundaria para poder recuperar los servicios después de un error. |

## <a name="networking"></a>Redes
|Servicio|DESCRIPCIÓN|
|------|--------|
| [Grupos de&nbsp;seguridad de&nbsp;red](../virtual-network/virtual-networks-nsg.md)| Característica de control de acceso basado en la red que utiliza una tupla de 5 elementos para permitir o denegar las decisiones.  |
| [Acerca de VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)| Dispositivo de red que se usa como un punto de conexión VPN para permitir el acceso entre entornos locales a las redes virtuales de Azure.  |
| [Introducción a Puerta de enlace de aplicaciones](../application-gateway/application-gateway-introduction.md)|Equilibrador de carga de aplicación web avanzado que puede enrutar en función de la dirección URL y realizar descargas SSL. |
| [Equilibrador de carga de Azure](../load-balancer/load-balancer-overview.md)|Equilibrador de carga de red para aplicaciones TCP/UDP. |
| [Información técnica de ExpressRoute](../expressroute/expressroute-introduction.md)| Vínculo WAN dedicado entre las redes locales y las redes virtuales de Azure. |
| [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)| Equilibrador de carga de DNS global.|
| [Habilitación del proxy de la aplicación de Azure AD](../active-directory/active-directory-application-proxy-get-started.md)| Servidor front-end de autenticación usado para proteger el acceso remoto a las aplicaciones web hospedadas en los entornos locales. |
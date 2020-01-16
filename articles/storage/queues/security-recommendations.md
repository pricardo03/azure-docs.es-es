---
title: Recomendaciones de seguridad para Queue Storage
titleSuffix: Azure Storage
description: Obtenga información sobre recomendaciones para Queue Storage. La implementación de esta guía le ayudará a cumplir sus obligaciones de seguridad, tal y como se describe en nuestro modelo de responsabilidad compartida.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: tamram
ms.custom: security-recommendations
ms.openlocfilehash: 1315e1f81ee32a544b623b7f3ffad61a7e7275d8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75476926"
---
# <a name="security-recommendations-for-queue-storage"></a>Recomendaciones de seguridad para Queue Storage

Este artículo contiene recomendaciones de seguridad para Queue Storage. La implementación de estas recomendaciones le ayudará a cumplir sus obligaciones de seguridad, tal y como se describe en nuestro modelo de responsabilidad compartida. Para más información sobre lo que Microsoft hace para cumplir las responsabilidades del proveedor de servicios, consulte [Responsabilidades compartidas de la informática en la nube](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/225366/1/Shared%20Responsibility%20for%20Cloud%20Computing-2019-10-25.pdf).

Algunas de las recomendaciones incluidas en este artículo se pueden supervisar automáticamente mediante Azure Security Center. Azure Security Center es la primera línea de defensa en la protección de los recursos de Azure. Para más información sobre Azure Security Center, consulte [¿Qué es Azure Security Center?](../../security-center/security-center-intro.md)

Azure Security Center analiza periódicamente el estado de seguridad de los recursos de Azure para identificar posibles vulnerabilidades de seguridad. Después, proporciona recomendaciones sobre cómo abordarlas. Para más información sobre las recomendaciones de seguridad de Azure Security Center, consulte [Recomendaciones de seguridad en Azure Security Center](../../security-center/security-center-recommendations.md).

## <a name="data-protection"></a>Protección de los datos

| Recomendación | Comentarios | Security Center |
|-|----|--|
| Usar el modelo de implementación de Azure Resource Manager | Cree nuevas cuentas de almacenamiento mediante el modelo de implementación de Azure Resource Manager para mejoras de seguridad importantes, incluido el control de acceso (RBAC) y la auditoría, gobernanza e implementación basados en Resource Manager, acceso a identidades administradas, acceso Azure Key Vault para secretos y autenticación basada en Azure AD y autorización para acceder a datos y recursos de Azure Storage. Si es posible, migre las cuentas de almacenamiento existentes que usan el modelo de implementación clásica para usar Azure Resource Manager. Para más información sobre Azure Resource Manager, consulte [Información general de Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview). | - |
| Habilitar la opción **Se requiere transferencia segura** en las cuentas de almacenamiento | Cuando habilite la opción **Se requiere transferencia segura**, todas las solicitudes realizadas en la cuenta de almacenamiento deben realizarse a través de conexiones seguras. Cualquier solicitud realizada a través de HTTP dará error. Para más información, consulte [Requerir transferencia segura en Azure Storage](../common/storage-require-secure-transfer.md). | [Sí](../../security-center/security-center-sql-service-recommendations.md) |
| Habilitar Advanced Threat Protection para todas las cuentas de almacenamiento | Advanced Threat Protection para Azure Storage proporciona un nivel adicional de inteligencia de seguridad que detecta intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de almacenamiento o vulnerarlas. Las alertas de seguridad se desencadenan en Azure Security Center cuando se producen anomalías en la actividad y también se envían por correo electrónico a los administradores de las suscripciones, con detalles de la actividad sospechosa y recomendaciones sobre cómo investigar y solucionar las amenazas. Para más información, consulte [Advanced Threat Protection para Azure Storage](../common/storage-advanced-threat-protection.md). | [Sí](../../security-center/security-center-sql-service-recommendations.md) |
| Limitar los tokens de firma de acceso compartido (SAS) solo a conexiones HTTPS | Requerir HTTPS cuando un cliente usa un token de SAS para acceder a los datos de la cola ayuda a minimizar el riesgo de espionaje. Para obtener más información, consulte [Otorgar acceso limitado a recursos de Azure Storage con firmas de acceso compartido (SAS)](../common/storage-sas-overview.md). | - |

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

| Recomendación | Comentarios | Security Center |
|-|----|--|
| Usar Azure Active Directory (Azure AD) para autorizar el acceso a los datos de la cola | Azure AD proporciona seguridad superior y facilidad de uso sobre la clave compartida para autorizar solicitudes en Queue Storage. Para más información, consulte [Autenticación del acceso a blobs y colas de Azure con Azure Active Directory](../common/storage-auth-aad.md). | - |
| Tener en cuenta la entidad de seguridad de menor privilegio al asignar permisos a una entidad de seguridad de Azure AD a través de RBAC | Al asignar un rol a un usuario, grupo o aplicación, conceda a esa entidad de seguridad solo aquellos permisos que sean necesarios para que puedan realizar sus tareas. Limitar el acceso a los recursos ayuda a prevenir el mal uso involuntario y malintencionado de los datos. | - |
| Proteger las claves de acceso de su cuenta con Azure Key Vault | Microsoft recomienda usar Azure AD para autorizar solicitudes a Azure Storage. Sin embargo, si debe usar la autorización de clave compartida, proteja sus claves de cuenta con Azure Key Vault. Puede recuperar las claves del almacén de claves en tiempo de ejecución, en lugar de guardarlas con la aplicación. | - |
| Regenerar la claves de cuenta periódicamente | La rotación periódica de las claves de la cuenta reduce el riesgo de exponer los datos a actores malintencionados. | - |
| Tener en cuenta la entidad de seguridad de menor privilegio al asignar permisos a una SAS | Al crear una SAS, especifique solo aquellos permisos que el cliente requiere para realizar su función. Limitar el acceso a los recursos ayuda a prevenir el mal uso involuntario y malintencionado de los datos. | - |
| Tener un plan de revocación para cualquier SAS que emita a los clientes | Si una SAS se ve comprometida, querrá revocarla lo antes posible. Para revocar una SAS de delegación de usuario, revoque la clave de delegación de usuario para invalidar rápidamente todas las firmas asociadas con esa clave. Para revocar una SAS de servicio que está asociada con una directiva de acceso almacenado, puede eliminar esta, cambiar el nombre de la directiva o cambiar su tiempo de vencimiento a un tiempo pasado. Para obtener más información, consulte [Otorgar acceso limitado a recursos de Azure Storage con firmas de acceso compartido (SAS)](../common/storage-sas-overview.md).  | - |
| Si una SAS de servicio no está asociada con una directiva de acceso almacenado, establezca el tiempo de vencimiento en una hora o menos | Una SAS de servicio que no está asociada con una directiva de acceso almacenada no se puede revocar. Por esta razón, se recomienda limitar el tiempo de expiración para que la SAS sea válida durante una hora o menos. | - |

## <a name="networking"></a>Redes

| Recomendación | Comentarios | Security Center |
|-|----|--|
| Habilitar reglas de firewall | Configure las reglas de firewall para limitar el acceso a la cuenta de almacenamiento a las solicitudes que se originan a partir de direcciones IP o intervalos especificados o a partir de una lista de subredes en una red virtual de Azure (VNet). Para más información sobre cómo configurar reglas de firewall, consulte [Configuración del proxy y el firewall de Azure File Sync](../files/storage-sync-files-firewall-and-proxy.md). | - |
| Permitir que los servicios de Microsoft de confianza accedan a la cuenta de almacenamiento | La activación de las reglas de firewall para la cuenta de almacenamiento bloquea las solicitudes entrantes para los datos de forma predeterminada, a menos que las solicitudes procedan de un servicio que funcione en una instancia de Azure Virtual Network (VNet) o desde direcciones IP públicas permitidas. Las solicitudes que bloquean incluyen aquellas de otros servicios de Azure, desde Azure Portal, desde los servicios de registro y de métricas, etc. Puede permitir solicitudes de otros servicios de Azure agregando una excepción para permitir que servicios de Microsoft de confianza accedan a la cuenta de almacenamiento. Para obtener más información sobre cómo agregar una excepción para servicios confiables de Microsoft, consulte [Configuración del proxy y el firewall de Azure File Sync](../files/storage-sync-files-firewall-and-proxy.md).| - |
| Usar puntos de conexión privados | Un punto de conexión privado asigna una dirección IP privada de Azure Virtual Network (red virtual) a la cuenta de almacenamiento. Asegura todo el tráfico entre su red virtual y la cuenta de almacenamiento a través de un enlace privado. Para más información sobre puntos de conexión privados, consulte [Conexión privada a una cuenta de almacenamiento mediante el punto de conexión privado de Azure](../../private-link/create-private-endpoint-storage-portal.md). | - |
| Limitar el acceso de red a redes específicas | Limitar el acceso a la red a redes que hospedan clientes que requieren acceso reduce la exposición de sus recursos a ataques a la red. | [Sí](../../security-center/security-center-sql-service-recommendations.md) |

## <a name="loggingmonitoring"></a>Registro y supervisión

| Recomendación | Comentarios | Security Center |
|-|----|--|
| Hacer un seguimiento de cómo se autorizan las solicitudes | Habilite el registro de Azure Storage para realizar un seguimiento de cómo se autorizó cada solicitud realizada en Azure Storage. Los registros indican si una solicitud se realizó de forma anónima, mediante un token OAuth 2.0, mediante una clave compartida o mediante una firma de acceso compartido (SAS). Para más información, vea [Registro de Azure Storage Analytics](../common/storage-analytics-logging.md). | - |

## <a name="next-steps"></a>Pasos siguientes

- [Documentación de Azure Security Center](https://docs.microsoft.com//azure/security/)
- [Documentación sobre desarrollo seguro](https://docs.microsoft.com/azure/security/develop/)

---
title: 'Seguridad y privacidad de los datos: Azure Search'
description: Azure Search es compatible con SOC 2, HIPAA y otras certificaciones. Conexión y cifrado de datos, autenticación y acceso a identidades mediante identificadores de seguridad de usuarios y grupos en los filtros de Azure Search.
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: fbad9624d6b76593ac4e77283f63904e9c006bcd
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647795"
---
# <a name="security-and-data-privacy-in-azure-search"></a>Seguridad y privacidad de datos en Azure Search

Los controles de acceso y las características de seguridad integrales se integran en Azure Search para garantizar que el contenido privado se mantenga privado. En este artículo se enumeran las características de seguridad y el cumplimiento de normas integrados en Azure Search.

La arquitectura de seguridad de Azure Search abarca seguridad física, transmisiones cifradas, almacenamiento cifrado y cumplimiento de normas en toda la plataforma. Azure Search solo acepta solicitudes autenticadas en sus operaciones. De modo opcional, puede agregar controles de acceso por usuario en el contenido mediante filtros de seguridad. En este artículo se trata la seguridad en cada capa, pero se centra principalmente en cómo se protegen los datos y las operaciones en Azure Search.

## <a name="standards-compliance-iso-27001-soc-2-hipaa"></a>Cumplimiento de estándares: ISO 27001, SOC 2, HIPAA

Azure Search está certificada por los siguientes estándares, tal como se [anunció en junio de 2018](https://azure.microsoft.com/blog/azure-search-is-now-certified-for-several-levels-of-compliance/):

+ [ISO 27001:2013](https://www.iso.org/isoiec-27001-information-security.html) 
+ [Cumplimiento de SOC 2 tipo 2](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) Para ver el informe completo, vaya a [Azure - and Azure Government SOC 2 Type II Report](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports) (Informe de SOC 2 de tipo II de Azure y Azure Government). 
+ [Ley de Portabilidad y Contabilidad de los Seguros de Salud (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)
+ [GxP (21 CFR Part 11)](https://en.wikipedia.org/wiki/Title_21_CFR_Part_11) (GxP [21 CFR Parte 11])
+ [HITRUST](https://en.wikipedia.org/wiki/HITRUST)
+ [PCI DSS Level 1](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard) (PCI DSS de nivel 1)
+ [Australia IRAP Unclassified DLM](https://asd.gov.au/infosec/irap/certified_clouds.htm) (DLM sin clasificar del programa IRAP en Australia)

El cumplimiento de estándares se aplica a las características disponibles con carácter general. Las características en versión preliminar se certifican cuando pasan a disponibilidad general y no se deben usar en soluciones que tengan requisitos de estándares estrictos. La certificación de cumplimiento está documentada en [Overview of Microsoft Azure compliance](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) (Información general del cumplimiento de Microsoft Azure) y el [Centro de confianza](https://www.microsoft.com/en-us/trustcenter). 

## <a name="encrypted-transmission-and-storage"></a>Transmisión y almacenamiento cifrados

El cifrado se extiende a lo largo de la canalización de indización: desde las conexiones, pasando por la transmisión y hasta los datos indizados almacenados en Azure Search.

| Nivel de seguridad | DESCRIPCIÓN |
|----------------|-------------|
| Cifrado en tránsito <br>(HTTPS/SSL/TLS) | Azure Search escucha en el puerto HTTPS 443. Las conexiones a los servicios de Azure están cifradas en toda la plataforma. <br/><br/>Todas las interacciones de Azure Search de cliente a servicio son compatibles con los protocolos SSL o TLS 1.2.  Asegúrese de usar TLSv1.2 para las conexiones SSL con el servicio.|
| Cifrado en reposo <br>Claves administradas por Microsoft | El cifrado se internaliza totalmente en el proceso de indexación, sin impacto cuantificable a la hora de indexar el tiempo que tarda en completarse ni el tamaño de indexación. Se produce automáticamente en todas las indexaciones, incluidas las actualizaciones incrementales a un índice que no esté totalmente cifrado (creado antes de enero de 2018).<br><br>Internamente, el cifrado se basa en el [cifrado del servicio de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), que usa [cifrado AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) de 256 bits.<br><br> El cifrado es interno para Azure Search, con certificados y claves de cifrado que Microsoft administra internamente, y se aplica universalmente. No se puede activar ni desactivar el cifrado, administrar ni sustituir sus claves, ni ver la configuración de cifrado en el portal o mediante programación.<br><br>El cifrado en reposo se anunció el 24 de enero de 2018 y se aplica a todos los niveles de servicio, incluidos los servicios (gratis) compartidos, y en todas las regiones. Para el cifrado completo, los índices creados antes de esa fecha deben quitarse y volver a generarse para que se produzca el cifrado. En caso contrario, solo se cifrarán los datos nuevos a partir del 24 de enero.|
| Cifrado en reposo <br>Claves administradas por el cliente | El cifrado con claves administradas por el cliente es una característica en **vista previa** (GB) que no está disponible para servicios gratuitos. En el caso de servicios de pago, solo está disponible para los servicios de búsqueda creados en enero de 2019 o posteriormente mediante la versión de API de versión preliminar más reciente (api-version=2019-05-06-Preview).<br><br>Las asignaciones de sinónimos y los índices de Azure Search ya se pueden cifran en reposo con claves administradas por el cliente en Azure Key Vault. Para obtener más información, consulte [Administración de claves de cifrado en Azure Search](search-security-manage-encryption-keys.md).<br>Esta característica no reemplaza el cifrado en reposo de forma predeterminada, sino que se aplica además de este.<br>Si habilita esta característica, aumentará el tamaño del índice y reducirá el rendimiento de las consultas. Según las observaciones hechas hasta la fecha, puede esperar un aumento del 30 al 60% en los tiempos de consultas, aunque el rendimiento real variará según la definición del índice y los tipos de consultas. Debido a este impacto en el rendimiento, se recomienda habilitar esta característica solo en los índices que realmente la necesitan.

## <a name="azure-wide-user-access-controls"></a>Controles de acceso de usuario en todo Azure

Hay disponibles varios mecanismos de seguridad en todo Azure que, por tanto, están disponibles automáticamente para los recursos de Azure Search que cree.

+ [Bloqueos en el nivel de suscripción y recurso para evitar la eliminación](../azure-resource-manager/resource-group-lock-resources.md)
+ [Control de acceso basado en roles (RBAC) para controlar el acceso a la información y las operaciones administrativas](../role-based-access-control/overview.md)

Todos los servicios de Azure admiten controles de acceso basado en roles (RBAC) para establecer niveles de acceso de forma coherente en todos los servicios. Por ejemplo, ver información confidencial como la clave de administración está restringido a los roles de Colaborador y Propietario, mientras que la visualización del estado del servicio está disponible para los miembros de cualquier rol. RBAC proporciona los roles de Propietario, Colaborador y Lector. De forma predeterminada, todos los administradores de servicios son miembros del rol de propietario.

<a name="service-access-and-authentication"></a>

## <a name="service-access-and-authentication"></a>Autenticación y acceso al servicio

Mientras Azure Search hereda las medidas de seguridad de la plataforma Azure, también proporciona su propia autenticación basada en claves. Una clave de API es una cadena que se compone de letras y números generados aleatoriamente. El tipo de clave (administrador o consulta) determina el nivel de acceso. El envío de una clave válida se considera una prueba de que la solicitud se origina desde una entidad de confianza. 

Hay dos niveles de acceso al servicio de búsqueda, habilitados por dos tipos de claves:

* Acceso de administración (válido para cualquier operación de lectura y escritura en el servicio)
* Acceso de consulta (válido para operaciones de solo lectura, como las consultas en la colección de documentos de un índice)

Las *claves de administración* se crean cuando se aprovisiona el servicio. Hay dos claves de administración, designadas como *principal* y *secundaria*, pero en realidad son intercambiables. Todos los servicios tienen dos claves de administración, con el fin de que se pueda dejar de usar una de ellas sin perder el acceso al servicio. También puede [regenerar la clave de administrador](search-security-api-keys.md#regenerate-admin-keys) periódicamente para seguir los procedimientos recomendados de seguridad de Azure, pero no se puede agregar al número total de claves de administrador. Hay un máximo de dos claves de administrador por servicio de búsqueda.

Las *claves de consulta* se crean a medida que son necesarias y están diseñadas para las aplicaciones cliente que emiten consultas. Puede crear hasta 50 claves de consulta. En el código de la aplicación, especifique la dirección URL de búsqueda y una clave de API de consulta para permitir el acceso de solo lectura a la colección de documentos de un índice específico. Juntos, el punto de conexión, una clave de API para el acceso de solo lectura y un índice de destino definen el nivel de acceso y ámbito de la conexión desde la aplicación cliente.

Se requiere autenticación en cada solicitud, y cada solicitud se compone de una clave obligatoria, una operación y un objeto. Cuando se encadenan, los dos niveles de permisos (completo y de solo lectura) y el contexto (por ejemplo, una operación de consulta en un índice) son suficientes para proporcionar seguridad en la gama completa de las operaciones del servicio. Para más información acerca de las claves, vea [Create and manage api-keys](search-security-api-keys.md) (Creación y administración de claves de API).

## <a name="index-access"></a>Acceso a los índices

En Azure Search, un índice individual no es un objeto protegible. En su lugar, el acceso a un índice se determina en la capa de servicio (acceso de lectura o escritura), junto con el contexto de una operación.

Para el acceso del usuario final, puede estructurar las solicitudes de consulta para conectarse con una clave de consulta, lo que hace que cualquier solicitud sea de solo lectura e incluir el índice específico utilizado por la aplicación. En una solicitud de consulta, no existe el concepto de combinación de índices ni de acceso simultáneo a varios índices para que todas las solicitudes tengan un único índice de destino por definición. Por lo tanto, la construcción de la solicitud de consulta misma (una clave y un índice único de destino) define el límite de seguridad.

Los accesos de administrador y de desarrollador a los índices no se diferencian: ambos necesitan tener acceso de escritura para crear, eliminar y actualizar objetos administrados por el servicio. Cualquier persona con una clave de administración del servicio puede leer, modificar o eliminar cualquier índice en el mismo servicio. Para la protección contra la eliminación accidental o malintencionada de índices, su control de código fuente interno para los recursos de código es la solución para revertir una eliminación o modificación de índices no deseada. Azure Search tiene conmutación por error dentro del clúster para garantizar la disponibilidad, pero no almacena ni ejecuta el código propietario utilizado para crear o cargar los índices.

Para soluciones multiinquilino que requieren límites de seguridad en el nivel de índice, estas soluciones suelen incluir un nivel intermedio que los clientes utilizan para controlar el aislamiento de índices. Para más información sobre casos de uso para varios inquilinos, consulte [Modelos de diseño de aplicaciones SaaS para varios inquilinos y Azure Search](search-modeling-multitenant-saas-applications.md).

## <a name="admin-access"></a>Acceso de administrador

El [acceso basado en roles (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) determina si tiene acceso a los controles del servicio y su contenido. Si es propietario o colaborador en un servicio de Azure Search, puede usar el módulo **Az.Search** de PowerShell o el portal para crear, actualizar o eliminar objetos en el servicio. También puede usar la [API REST de administración de Azure Search](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api).

## <a name="user-access"></a>Acceso de usuarios

De forma predeterminada, el acceso de usuario a un índice viene determinado por la clave de acceso en la solicitud de consulta. La mayoría de los desarrolladores crean y asignan [*claves de consulta*](search-security-api-keys.md) para las solicitudes de búsqueda del lado cliente. Una clave de consulta concede acceso de lectura a todo el contenido del índice.

Si necesita tener un control por usuario pormenorizado del contenido, puede crear filtros de seguridad en las consultas, que devuelven documentos asociados con una identidad de seguridad determinada. En lugar de funciones predefinidas y asignaciones de roles, el control de acceso basado en identidades se implementa como un *filtro* que recorta los resultados de búsqueda de documentos y contenido en función de las identidades. La tabla siguiente describe dos enfoques para recortar el contenido no autorizado de los resultados de la búsqueda.

| Enfoque | DESCRIPCIÓN |
|----------|-------------|
|[Recorte de seguridad basado en filtros de identidad](search-security-trimming-for-azure-search.md)  | Documenta el flujo de trabajo básico para implementar el control de acceso de identidades de usuario. Trata la incorporación de identificadores de seguridad a un índice y, a continuación, se explica el filtrado por ese campo para no incluir el contenido prohibido en los resultados. |
|[Recorte de seguridad basado en identidades de Azure Active Directory](search-security-trimming-for-azure-search-with-aad.md)  | En este artículo se amplía el artículo anterior y se proporcionan los pasos necesarios para recuperar identidades de Azure Active Directory (AAD), uno de los [servicios gratuitos](https://azure.microsoft.com/free/) de la plataforma de nube de Azure. |

## <a name="table-permissioned-operations"></a>Tabla: Operaciones con permisos

En la tabla siguiente se resumen las operaciones permitidas en Azure Search y la clave que desbloquea el acceso a una operación determinada.

| Operación | Permisos |
|-----------|-------------------------|
| Crear un servicio | Titular de la suscripción de Azure|
| Escalar un servicio | Clave de administración, RBAC propietario o colaborador en el recurso  |
| Eliminar un servicio | Clave de administración, RBAC propietario o colaborador en el recurso |
| Crear, modificar y eliminar objetos en el servicio: <br>Índices y partes de componentes (incluidas las definiciones del analizador, los perfiles de puntuación y las opciones de CORS), indexadores, orígenes de datos, sinónimos, proveedores de sugerencias. | Clave de administración, RBAC propietario o colaborador en el recurso  |
| Consultar un índice | Clave de administrador o de consulta (RBAC no es aplicable) |
| Consultar la información del sistema, como devolver estadísticas, recuentos y listas de objetos. | Clave de administración, RBAC en el recurso (propietario, colaborador y lector) |
| Administrar claves de administración | Clave de administración, RBAC propietario o colaborador en el recurso |
| Administrar claves de consulta |  Clave de administración, RBAC propietario o colaborador en el recurso  |

## <a name="physical-security"></a>Seguridad física

Los centros de datos de Microsoft proporcionan una seguridad física líder en la industria y son compatibles con una amplia gama de normas y reglas. Para más información, vaya a la página [Centros de datos globales](https://www.microsoft.com/cloud-platform/global-datacenters) o vea un vídeo corto sobre la seguridad de los centros de datos.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]


## <a name="see-also"></a>Otras referencias

+ [Introducción a .NET (se muestra el uso de una clave de administración para crear un índice)](search-create-index-dotnet.md)
+ [Introducción a REST (se muestra el uso de una clave de administración para crear un índice)](search-create-index-rest-api.md)
+ [Control de acceso basado en identidades mediante filtros de Azure Search](search-security-trimming-for-azure-search.md)
+ [Control de acceso basado en identidades de Active Directory mediante filtros de Azure Search](search-security-trimming-for-azure-search-with-aad.md)
+ [Filtros de Azure Search](search-filters.md)
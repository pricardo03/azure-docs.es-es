---
title: Instalaciones de Azure, entornos locales y seguridad física | Microsoft Docs
description: El artículo describe los centros de datos de Azure, incluida la infraestructura física, la seguridad y las ofertas de cumplimiento.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: a6a9b1d6e12dabb09cde684c34481b4b3442c1b8
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101819"
---
# <a name="azure-facilities-premises-and-physical-security"></a>Instalaciones de Azure, entornos locales y seguridad física
La nube de Microsoft está formada por una [infraestructura de centros de datos distribuida globalmente](https://azure.microsoft.com/global-infrastructure/) que da soporte a miles de servicios en línea y abarca más de 100 instalaciones de alta seguridad en todo el mundo.

La infraestructura está diseñada para acercar las aplicaciones a usuarios de todo el mundo. De este modo, mantiene la residencia de los datos y ofrece a los clientes opciones muy completas de cumplimiento normativo y resistencia. Azure tiene 52 regiones en todo el mundo y está disponible en 140 países.

Una región es un conjunto de centros de datos que están conectados entre sí mediante una red masiva y resistente. De forma predeterminada, la red incluye distribución de contenido, equilibrio de carga, redundancia y cifrado. Con más regiones globales que cualquier otro proveedor de servicios en la nube, Azure ofrece a los clientes la flexibilidad de implementar aplicaciones donde es necesario.

Las regiones de Azure se organizan por zonas geográficas. Una zona geográfica de Azure garantiza que se cumplan los requisitos de residencia, soberanía, cumplimiento normativo y resistencia de los datos dentro de las fronteras geográficas.

Las zonas geográficas permiten a los clientes con necesidades específicas de residencia de datos y cumplimiento normativo mantener sus datos y aplicaciones cerca. Las zonas geográficas son tolerantes a errores hasta el punto de resistir una interrupción total del funcionamiento de una región gracias a su conexión con nuestra infraestructura de red dedicada de alta capacidad.

Las zonas de disponibilidad son ubicaciones separadas físicamente dentro de una región de Azure. Cada zona de disponibilidad consta de uno o varios centros de datos equipados con alimentación, refrigeración y redes independientes. Las zonas de disponibilidad permiten a los clientes ejecutar aplicaciones críticas con alta disponibilidad y replicación con baja latencia.

La siguiente ilustración muestra cómo la infraestructura global de Azure combina una región con zonas de disponibilidad que estén dentro de los mismos límites de residencia de datos para obtener alta disponibilidad, recuperación ante desastres y copias de seguridad.

![Límites de la residencia de datos][1]

Una gran extensión de centros de datos distribuidos geográficamente permite a Microsoft estar más cerca de los clientes con el fin de reducir la latencia de red y permitir funcionalidades de conmutación por error y copia de seguridad con redundancia geográfica.

## <a name="physical-security"></a>Seguridad física
Microsoft diseña, crea y opera los centros de datos de forma que se controla estrictamente el acceso físico a las áreas donde se almacenan los datos de los clientes. Microsoft reconoce la importancia de proteger los datos de los clientes y se compromete a ayudar a proteger los centros de datos que contienen sus datos. Contamos con toda una división en Microsoft dedicada al diseño, creación y operación de las instalaciones físicas que dan soporte a Azure. Este equipo se dedica a mantener una seguridad física de última generación.

Microsoft adopta un enfoque por capas para la seguridad física con el fin de reducir el riesgo de que los usuarios no autorizados obtengan acceso físico a los datos y los recursos del centro de datos. Los centros de datos administrados por Microsoft tienen numerosas capas de protección: autorización del acceso en el perímetro de la instalación, en el perímetro del edificio, dentro del edificio y en la planta del centro de datos. Las capas de seguridad física son:

- Solicitud de acceso y aprobación: debe solicitar acceso antes de llegar al centro de datos. Se le pedirá que proporcione una justificación comercial válida para su visita, por ejemplo, con fines de cumplimiento o auditoría. Los empleados de Microsoft aprueban las solicitudes según la necesidad de acceso. La autorización por necesidad de acceso ayuda a reducir al mínimo el número de personas necesarias para completar una tarea en los centros de datos. Una vez que se concede el permiso, solo una persona tiene acceso al área del centro de datos pertinente según la justificación empresarial aprobada. Los permisos se limitan a un determinado período de tiempo y expiran una vez transcurrido el período de tiempo permitido.

- Perímetro de la instalación: cuando llegue a un centro de datos, se le pedirá que vaya a un punto de acceso bien definido. Normalmente, una alta valla de acero y hormigón rodea cada centímetro del perímetro. Hay cámaras en torno a los centros de datos, con un equipo de seguridad que supervisa los vídeos ininterrumpidamente todos días del año.

- Entrada al edificio: la entrada de centro de datos está protegida por profesionales de seguridad que han recibido rigurosos cursos de aprendizaje y comprobaciones de antecedentes. Estos responsables de seguridad patrullan habitualmente el centro de datos mientras también supervisan los vídeos de las cámaras situadas dentro del centro de datos, de forma ininterrumpida todos los días del año.

- Dentro del edificio: después de entrar al edificio, debe pasar por una autenticación en dos fases con datos biométricos para continuar avanzando por el centro de datos. Si se valida la identidad, puede entrar en la parte del centro de datos a la que se le ha concedido acceso. Puede permanecer allí solo durante el tiempo aprobado.

- Planta del centro de datos: solo se le permitirá entrar en la planta para la que se le haya autorizado. Deberá pasar por un detector de metales de cuerpo completo. Para reducir el riesgo de que datos no autorizados entren o salgan del centro de datos sin nuestro conocimiento, solo podrán entrar en el centro de datos los dispositivos aprobados. Además, hay cámaras de vídeo que supervisan la parte frontal y posterior de cada bastidor de servidores. Al salir de la planta del centro de datos, deberá volver a pasar por un detector de metales de cuerpo completo. Para salir del centro de datos, deberá pasar por un examen de seguridad adicional.

Los visitantes tienen que entregar las tarjetas al salir de cualquier instalación de Microsoft.

## <a name="physical-security-reviews"></a>Revisiones de la seguridad física
Periódicamente se realizan revisiones de la seguridad física de las instalaciones para garantizar que los centros de datos aborden correctamente los requisitos de seguridad de Microsoft Azure. El personal del proveedor de hospedaje del centro de datos no realiza la administración de los servicios de Microsoft Azure. El personal no tiene acceso físico a las jaulas ni a sala de colocación de Azure, ni acceso de inicio de sesión a los sistemas de Azure.

## <a name="data-bearing-devices"></a>Dispositivos que contienen datos
Microsoft usa los procedimientos recomendados y una solución de borrado que [cumple la norma NIST 800-88](https://csrc.nist.gov/publications/detail/sp/800-88/archive/2006-09-01). En el caso de las unidades de disco duro que no se pueden borrar, se usa un proceso de destrucción que lo destruye y hace que sea imposible recuperar la información. El proceso de destrucción puede ser desintegración, triturado, pulverización o incineración. El método adecuado de eliminación viene determinado por el tipo de recurso. Los registros de la destrucción se conservan.  

## <a name="equipment-disposal"></a>Eliminación de equipos
Microsoft Azure implementa este principio en nombre de los clientes. Al final del ciclo de vida de un sistema, el personal operativo de Microsoft sigue unos rigurosos procedimientos de control de los datos y de eliminación del hardware que garantizan que ningún elemento del hardware que contenga datos del cliente estará disponible para terceros que no son de confianza. Se sigue un método de eliminación segura (mediante el firmware de la unidad de disco duro) para unidades compatibles. En el caso de las unidades de disco duro que no se pueden borrar, se usa un proceso de destrucción que lo destruye y hace que sea imposible recuperar la información. El proceso de destrucción puede ser desintegración, triturado, pulverización o incineración. El método adecuado de eliminación viene determinado por el tipo de recurso. Los registros de la destrucción se conservan. Todos los servicios de Microsoft Azure usan servicios aprobados de almacenamiento de elementos multimedia y administración de los desechos.

## <a name="compliance"></a>Cumplimiento normativo
La infraestructura de Azure está diseñada y se administra para satisfacer un amplio conjunto de normas internacionales y específicas del sector, tales como ISO 27001, HIPAA, FedRAMP, SOC 1 y SOC 2. También se cumplen los estándares específicos del país, incluidos IRAP en Australia, G-Cloud en Reino Unido y MTCs en Singapur. Auditorías de terceros rigurosas, como las del Instituto Británico de Normalización, confirman que Azure se adhiere a los estrictos controles de seguridad que estos estándares exigen.

Consulte la [ofertas de cumplimiento](https://www.microsoft.com/trustcenter/compliance/complianceofferings) para ver una lista completa de las normas que Azure cumple.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre lo que hace Microsoft para proteger la infraestructura de Azure, consulte:

- [Disponibilidad de la infraestructura de Azure](azure-infrastructure-availability.md)
- [Componentes y límites del sistema de información de Azure](azure-infrastructure-components.md)
- [Arquitectura de red de Azure](azure-infrastructure-network.md)
- [Red de producción de Azure](azure-production-network.md)
- [Características de seguridad de Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Operaciones de producción y administración de Azure](azure-infrastructure-operations.md)
- [Supervisión de la infraestructura de Azure](azure-infrastructure-monitoring.md)
- [Integridad de la infraestructura de Azure](azure-infrastructure-integrity.md)
- [Protección de datos del cliente en Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-physical-security/data-residency-boundary.png

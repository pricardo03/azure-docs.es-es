---
title: Explicación de la seguridad de Azure Australia
description: Información y preguntas más frecuentes sobre las regiones de Australia y el cumplimiento de los requisitos específicos de las directivas, los reglamentos y la legislación de la administración pública de Australia.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 212d14e31c152e50c216f2f34fb225c29fe054b5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571112"
---
# <a name="azure-australia-security-explained"></a>Explicación de la seguridad de Azure Australia

En este artículo se abordan algunas de las preguntas más frecuentes y áreas de interés para los organismos gubernamentales de Australia que investigan, diseñan e implementan en Microsoft Azure Australia.

## <a name="irap-and-certified-cloud-services-list-ccsl-documents"></a>Documentos del programa IRAP y la lista de servicios en la nube certificados (CCSL)

El Australian Cyber Security Centre (ACSC) proporciona una carta de certificación, un informe de certificación y una guía del consumidor para el servicio cuando se agrega a la CCSL.

Microsoft figura actualmente en la lista CCSL para Azure, Office 365 y Dynamics 365 CRM.

Microsoft pone a disposición de sus clientes y socios los documentos de auditoría, evaluación y certificación del ACSC en una página específica para Australia del [Portal de confianza de servicios de Microsoft](https://aka.ms/au-irap).

## <a name="dissemination-limiting-markers-dlm-and-protected-certification"></a>Marcadores de limitación de difusión (DLM) y certificación de nivel PROTEGIDO

El proceso para disponer de sistemas, incluidos servicios en la nube, aprobados para su uso por organizaciones gubernamentales se define en el [Manual de Seguridad de la Información (ISM)](https://acsc.gov.au/infosec/ism/) producido y publicado por el Australian Cyber Security Centre (ACSC). El Australian Cyber Security Centre (ACSC) es la entidad de la ASD responsable de la certificación en la nube y la ciberseguridad.

El proceso de aprobación consta de dos pasos:

1. Evaluación de la seguridad (IRAP): proceso en el que profesionales registrados evalúan los sistemas, servicios y soluciones con controles técnicos en el ISM y evalúan si los controles se han implementado de forma eficaz. La evaluación también identifica cualquier riesgo específico que deba tener en cuenta la autoridad de aprobación antes de emitir una autorización para operar (ATO).

1. Autorización para operar: proceso en el que un oficial superior de una agencia gubernamental reconoce y acepta formalmente el riesgo residual de un sistema para la información que procesa, almacena y comunica.  Una entrada de este proceso es la evaluación de la seguridad.

La evaluación de los servicios de Azure en el nivel PROTEGIDO identifica que se ha confirmado que la implementación de los controles de seguridad necesarios para el almacenamiento y el procesamiento de los datos de nivel PROTEGIDO y de nivel inferior está en vigor y funciona de forma eficaz.

## <a name="australian-data-classification-changes"></a>Cambios en la clasificación de datos australianos

El 1 de octubre de 2018 el, la Fiscalía General de Australia anunció públicamente cambios en el marco de la directiva de seguridad de protección (PSPF), específicamente un nuevo [sistema de información confidencial y clasificada](https://www.protectivesecurity.gov.au/information/sensitive-classified-information/Pages/default.aspx).

![Clasificaciones de PSPF revisadas](media/pspf-classifications.png)

Estos cambios afectan a todas las agencias y organizaciones australianas que operan conforme al PSPF. El cambio principal que afecta a nuestras certificaciones actuales de IRAP/CCSL es que se han retirado los actuales marcadores de limitación de difusión (DLM). El marcador **OFICIAL: Confidencial** reemplaza los distintos DLM utilizados para la protección de la información confidencial. El cambio también incorporó tres marcadores de administración de información que se pueden aplicar a toda la información oficial en todos los niveles de confidencialidad y clasificación. La clasificación de nivel **PROTEGIDO** no ha experimentado modificaciones.

El término Sin clasificar se ha eliminado del nuevo sistema y el término No oficial se aplica a la información no gubernamental, aunque no requiere un marcador formal.

## <a name="choosing-an-azure-region-for-my-official-sensitive-and-protected-workloads"></a>Elección de una región de Azure para cargas de trabajo de nivel OFICIAL: Confidencial y PROTEGIDO

Los servicios de Azure con la certificación **OFICIAL: Confidencial** y **PROTEGIDO** se implementan en las cuatro regiones de centro de datos de Australia (Este de Australia, Sudeste de Australia, Centro de Australia y Centro de Australia 2); sin embargo, el informe de certificación emitido por el ACSC recomienda que los datos de nivel **PROTEGIDO** se implementen en las regiones de Azure Government en Canberra si hay un servicio disponible allí. Puede encontrar información más detallada sobre los servicios de Azure con la certificación de nivel **PROTEGIDO** en la [página de Australia de la STP](https://aka.ms/au-irap).

>[!NOTE]
>Microsoft recomienda que los datos gubernamentales de todas las clasificaciones y niveles de confidencialidad se implementen en las regiones Centro de Australia y Centro de Australia 2, ya que están diseñadas y funcionan específicamente para satisfacer las necesidades de la administración pública.

Puede encontrar más información sobre la naturaleza especial de las regiones australianas de Azure en [Regiones centrales de Australia en Azure](https://azure.microsoft.com/global-infrastructure/australia/).

## <a name="how-microsoft-separates-classified-and-official-data"></a>Cómo separa Microsoft los datos clasificados y oficiales

Microsoft trabaja con Azure y Office 365 en Australia como si todos los datos fueran confidenciales o clasificados, elevando los controles de seguridad a ese nivel.

La infraestructura que respalda Azure puede servir datos de varias clasificaciones.  No obstante, al presuponer que los datos del cliente son clasificados, se mantienen en vigor los controles adecuados correspondientes a este nivel. Microsoft ha adoptado una postura de seguridad de base para nuestros servicios que cumple los requisitos de PSPF para almacenar y procesar información clasificada de nivel **PROTEGIDO**.

Para garantizar a los clientes que un inquilino de Azure no está expuesto a un riesgo de otros inquilinos, Microsoft implementa controles completos de defensa en profundidad.

Además de las funcionalidades implementadas en la plataforma Microsoft Azure, los controles adicionales configurables por el cliente, como el cifrado con claves administradas por el cliente, la virtualización anidada y el acceso administrativo Just-in-Time, pueden reducir aún más el riesgo. Dentro de las regiones australianas de Azure Government de Canberra, hay en vigor un proceso para incluir en listas blancas formales solo organizaciones de infraestructura críticas nacional y gubernamentales de Australia y Nueva Zelanda. Esta nube de comunidad proporciona una garantía adicional para las organizaciones que son sensibles a riesgos de coinquilinos.

El informe de certificación de nivel **PROTEGIDO** de Microsoft Azure confirma que estos controles son eficaces para el almacenamiento y el procesamiento de datos clasificados de nivel **PROTEGIDO** y su aislamiento.

## <a name="relevance-of-irapccsl-to-state-government-and-critical-infrastructure-providers"></a>Relevancia de IRAP/CCSL para los proveedores de infraestructuras gubernamentales y críticas

Muchos de los proveedores de infraestructuras gubernamentales y críticas incorporan los requisitos gubernamentales en su directiva y marco de seguridad. Estas organizaciones también administran datos clasificados de nivel **OFICIAL**, **OFICIAL: Confidencial** y cierta cantidad de datos de nivel **PROTEGIDO**, ya sea mediante su interacción con el gobierno federal o por sí mismas.

La administración pública de Australia cada vez centra más su política y legislación en la protección de datos no gubernamentales que afectan fundamentalmente a la seguridad y la prosperidad económica de Australia. Como tal, las regiones de Azure de Australia y la certificación CCSL son pertinentes para todos esos sectores.

![Sectores de infraestructura crítica](media/nci-sectors.png)

Las certificaciones de Microsoft demuestran que los servicios de Azure se han sometido a una evaluación exhaustiva, rigurosa y formal de las protecciones de seguridad en vigor y que se han aprobado para el tratamiento de datos altamente confidenciales.

## <a name="location-and-control-of-microsoft-data-centres"></a>Ubicación y control de los centros de datos de Microsoft

Es un requisito obligatorio de la administración pública y los proveedores de infraestructura crítica conocer explícitamente la ubicación de los centros de datos y la propiedad de los servicios en la nube que procesan sus datos.  Microsoft es el único proveedor de servicios en la nube a hiperescala que proporciona una amplia información sobre estas ubicaciones y la propiedad.

Las regiones de Azure de Australia de Microsoft (Centro de Australia y Centro de Australia 2) operan en las instalaciones de los centros de datos de CDC.  La propiedad de los centros de datos de CDC es australiana, con un 48 % bajo control de la Commonwealth Superannuation Corporation, otro 48 % propiedad de Infratil (un fondo de activos a largo plazo con sede en Nueva Zelanda, que cotiza tanto en el mercado de valores australiano como en el neozelandés) y el 4 % restante en manos de la administración australiana.  

La administración de los centros de datos de CDC tiene garantías contractuales en vigor con la administración pública de Australia que restringen la transferencia futura de la propiedad y el control. Esta transparencia de la cadena de suministro y la propiedad mediante la asociación de Microsoft con los centros de datos de CDC está en la línea de los principios de la [estrategia de hospedaje integral de la administración pública de Australia](https://www.dta.gov.au/our-projects/whole-government-hosting-strategy) y la definición de **centro de datos soberano certificado**.

## <a name="the-azure-services-that-are-included-in-the-current-ccsl-certification"></a>Servicios de Azure que se incluyen en la certificación CCSL actual

En junio de 2017, el ACSC certificó 41 servicios de Azure para el almacenamiento y el procesamiento de datos a nivel **Sin clasificar: DLM**. En abril de 2018, 24 de esos servicios se certificaron para datos clasificados de nivel **PROTEGIDO**.

La disponibilidad de los servicios de Azure certificados por el ACSC en las regiones de Azure en Australia es la siguiente (en negrita, los certificados a nivel **PROTEGIDO**):

|Regiones centrales de Australia en Azure|Servicios no regionales y otras regiones|
|---|---|
|API Management, App Gateway, App Services, **Automation, Azure Portal, Backup, Batch, Cloud Services**, Cosmos DB, Event Hubs, **ExpressRoute**, HDInsight, **Key Vault**, Load Balancer, Log Analytics, **Multi-factor Authentication**, Redis Cache,  **Resource Manager, Service Bus, Service Fabric, Site Recovery, SQL Database, Storage**, Traffic Manager, **Virtual Machines, Virtual Network, VPN Gateway**|**Azure Active Directory**, CDN, Data Catalog, **Import Export, Information Protection, IOT Hub**, Machine Learning, Media Services, **Notification Hubs**, Power BI, **Security Centre, Scheduler**, Search, Stream Analytics|
|

Microsoft publica el documento [Información general sobre el cumplimiento de Microsoft Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/44/Microsoft%20Azure%20Compliance%20Offerings.pdf), que enumera todos los servicios incluidos de todos los procesos de cumplimiento y evaluación globales, gubernamentales, regionales y de la industria que pasa Azure, incluido IRAP/CCSL.

## <a name="azure-service-not-listed-or-assessed-at-a-lower-level-than-i-need"></a>Servicio de Azure no incluido o evaluado a un nivel inferior al necesario

Los servicios no certificados o con el certificado **OFICIAL: Confidencial** pero no **PROTEGIDO** se pueden usar conjuntamente o como parte de una solución que hospede datos de nivel **PROTEGIDO** siempre que:
1. los servicios no almacenen ni procesen datos de nivel **PROTEGIDO** sin cifrar, o
1. haya realizado una evaluación de riesgos y aprobado personalmente el servicio para que almacene datos de nivel **PROTEGIDO**.

Si desea usar un servicio que no está incluido en CCSL para almacenar y procesar datos de nivel **OFICIAL**, tiene la posibilidad de hacerlo, si bien el ISM requiere que notifique a ACSC por escrito que lo va a hacer, antes de suscribir o renovar un contrato con un proveedor de servicios en la nube.

Cualquier servicio que use una agencia para cargas de trabajo de nivel **PROTEGIDO** debe evaluarse para determinar su seguridad y aprobarse de conformidad con los procesos descritos en el ISM y en el proceso de evaluación de IRAP administrado por la agencia en la [estrategia de nube segura de la DTA](https://www.dta.gov.au/files/cloud-strategy/secure-cloud-strategy.pdf).

![Proceso de certificación de la estrategia de nube segura de la DTA](media/certification.png)

Microsoft evalúa continuamente sus servicios para garantizar que la plataforma sea segura y se ajuste a la finalidad del uso de la administración pública de Australia. Por tanto, póngase en contacto con Microsoft si necesita ayuda con un servicio que no figure actualmente en la lista CCSL en el nivel **PROTEGIDO**.

Dado que Microsoft tiene una gama de servicios certificados en la lista CCSL en las clasificaciones **DLM sin clasificar** y **PROTEGIDO**, el ISM requiere que se realice una evaluación IRAP de sus servicios al menos cada dos años. Microsoft se somete a una evaluación anual, que también ofrece la oportunidad de incluir servicios adicionales para su consideración.

## <a name="certified-protected-gateway-in-azure"></a>Puerta de enlace con certificación de nivel PROTEGIDO en Azure

Microsoft no utiliza una puerta de enlace de Internet segura (SIG) con certificación gubernamental debido a las restricciones en el número de SIG permitidas en el programa de consolidación de puertas de enlace.  Sin embargo, las funcionalidades previstas y necesarias de una SIG se pueden configurar en Microsoft Azure.

Mediante la certificación de nivel **PROTEGIDO** de los servicios de Azure, el ACSC ofrece recomendaciones específicas a las agencias para conectarse a Azure y para implementar segmentación de redes entre dominios de seguridad, por ejemplo entre el nivel **PROTEGIDO** e Internet. Estas recomendaciones incluyen el uso de grupos de seguridad de red, firewalls y redes privadas virtuales.  El ACSC recomienda el uso de una aplicación de puerta de enlace virtual. Existen varias aplicaciones virtuales disponibles en Azure que tienen un equivalente físico en la lista de productos evaluados de ASD o que se han evaluado con los perfiles de protección de criterios comunes y figuran en el portal de criterios comunes. Estos productos se reconocen mutuamente por el ASD como signatario del acuerdo de reconocimiento de criterios comunes (CCRA).

Microsoft ha creado guías sobre la implementación de funcionalidades basadas en Azure que proporcionan las funciones de seguridad necesarias para proteger el límite entre distintos dominios de seguridad, que, cuando se combinan, conforman el equivalente de una SIG certificada. Existen varios asociados que pueden ayudar en el diseño y la implementación de estas funcionalidades, así como una serie de soluciones de asociados disponibles para lo mismo.

## <a name="security-clearances-and-citizenship-of-microsoft-support-personnel"></a>Permisos de seguridad y nacionalidad del personal de soporte técnico de Microsoft

Microsoft ofrece sus servicios a nivel global con personal de seguridad entrenado y rigurosamente seleccionado.  El personal con acceso físico sin acompañamiento a las instalaciones de Sydney y Melbourne tiene permisos de seguridad básicos de la administración pública de Australia. El personal de las regiones Centro de Australia y Centro de Australia 2 tiene un permiso mínimo de veto negativo 1 (NV1) (según corresponda para datos de nivel **SECRETO**). Esto ofrece a los clientes la garantía adicional de que el personal que trabaja en los centros de datos de Azure es de extrema confianza.

Microsoft tiene una directiva de acceso de cero permanentes con acceso concedido a través de un sistema de administración Just-in-Time y suficiente basado en controles de acceso basados en rol. En la gran mayoría de los casos, nuestros administradores no requieren acceso ni privilegios en los datos de los clientes para solucionar y mantener el servicio.  El alto nivel de automatización y scripting de tareas para ejecución remota elimina la necesidad de acceder directamente a los datos de los clientes.

La Fiscalía General de Australia ha confirmado que los procedimientos y las directivas de seguridad del personal de Microsoft en Azure son coherentes con la intención del acceso de PSPF a las disposiciones de información en INFOSEC-9.

## <a name="storing-international-traffic-of-arms-regulations-itar-or-export-administration-regulations-ear-data"></a>Almacenamiento de datos del reglamento internacional sobre el tráfico de armas (ITAR) o el reglamento de administración de exportaciones (EAR)

Los controles técnicos de Azure que ayudan a los clientes a cumplir sus obligaciones en lo que respecta a los datos sujetos a controles de exportación son los mismos a nivel global en Azure. Cabe señalar que no se trata de marcar una casilla para el ITAR o EAR, ya que no hay ningún marco de evaluación y certificación formal para los datos sujetos a controles de exportación.

Para Azure Government y Office 365 US Government for Defense, hemos establecido medidas contractuales y procedimentales adicionales para ofrecer asistencia a los clientes sujetos a controles de exportación. Estas cláusulas contractuales adicionales y la asistencia y administración garantizadas a nivel de EE. UU. de las regiones de Azure no están en vigor para Australia.

Esto no significa que no se pueda usar Azure en Australia para ITAR/EAR, pero debe comprender claramente las restricciones impuestas por su licencia de exportación y debe implementar medidas de protección adicionales para cumplir esas obligaciones antes de usar Azure para almacenar esos datos. Por ejemplo, puede que necesite establecer la nacionalidad como un atributo en Azure Active Directory, usar Azure Information Protection para aplicar las reglas de cifrado en los datos y limitarlos solo a EE. UU. y a cualquier otra nacionalidad que esté incluida en la licencia de exportación, cifrar todos los datos en el entorno local antes de almacenarlos en Azure, usar la clave de cliente o mantener su propia clave para los datos de ITAR, etc.

Dado que ITAR no es una certificación formal, debe comprender cuáles son las restricciones y limitaciones asociadas a la licencia de exportación y, a continuación, determinar si hay suficientes controles en Azure para cumplir esos requisitos. En este caso, uno de los problemas que se deben tener en cuenta es el acceso por parte de nuestros ingenieros que pueden tener una nacionalidad no aprobada en la licencia de exportación.

## <a name="next-steps"></a>Pasos siguientes

Consulte el artículo sobre [Azure VPN Gateway](vpn-gateway.md) para la configuración y la implementación compatibles con ISM de la conectividad VPN en Azure Australia

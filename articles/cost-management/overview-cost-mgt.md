---
title: Introducción a Azure Cost Management | Microsoft Docs
description: Azure Cost Management es una solución de administración de costos que ayuda a supervisar y controlar el gasto de Azure y a optimizar el uso de los recursos.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/14/2019
ms.topic: overview
ms.service: cost-management-billing
manager: benshy
ms.custom: ''
ms.openlocfilehash: 90d2646aa554a20a823d29cde06537d05415b603
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230034"
---
# <a name="what-is-azure-cost-management"></a>¿Qué es Administración de costos de Azure?

La administración de costos es el proceso en el que se planifican y controlan eficazmente los costos implicados en su negocio. Normalmente, los equipos de finanzas, administración y aplicación realizan las tareas de administración de costos. Azure Cost Management ayuda a las organizaciones a planear teniendo en cuenta los costos. También ayuda a analizar eficazmente los costos y a tomar medidas para optimizar el gasto en la nube. Para obtener más información sobre cómo abordar la administración de costos como organización, eche un vistazo al artículo [Azure Cost Management best practices](cost-mgt-best-practices.md) (Procedimientos recomendados de Azure Cost Management).

Vea el [vídeo de información general de Azure Cost Management](https://www.youtube.com/watch?v=el4yN5cHsJ0) para consultar una breve introducción sobre cómo Azure Cost Management puede ayudarle a ahorrar dinero en Azure.

>[!VIDEO https://www.youtube.com/embed/el4yN5cHsJ0]

Aunque están relacionados, la facturación difiere de administración de costos. La facturación es el proceso de facturar a los clientes por bienes o servicios y administrar la relación comercial.  Los equipos de adquisiciones y finanzas son los que normalmente realizan las tareas de facturación.

Cost Management muestras los costos de organización y los patrones de uso con análisis avanzados. Los informes de Cost Management muestra los costos basados en el uso consumidos por los servicios de Azure y las ofertas de Marketplace de terceros. Los costos dependen de los precios negociados y del factor de reserva y los descuentos de Ventaja híbrida de Azure. En conjunto, en los informes se muestran los costos internos y externos del uso y los cargos de Azure Marketplace. Otros cargos, como las compras de reservas, el soporte técnico y los impuestos no se muestran todavía en los informes. Los informes le ayudan a comprender los gastos y el uso de recursos, y pueden ayudar a encontrar anomalías de gastos. Los análisis predictivos también están disponibles. Cost Management usa grupos de administración de Azure, presupuestos y recomendaciones para mostrar claramente cómo se organizan los gastos y cómo se pueden reducir los costos.

Puede usar Azure Portal o varias API para la automatización de la exportación, de manera que se integren los datos de costos con los procesos y sistemas externos. También están disponibles la exportación de datos de facturación automatizada y los informes programados.

## <a name="plan-and-control-expenses"></a>Planificación y control de los gastos

A continuación se mencionan las formas mediante las que Cost Management le ayuda a planear y controlar los costos: análisis de costos, presupuestos, recomendaciones y exportación de datos de administración de costos.

Los análisis de costos se usan para explorar y analizar los costos de su organización. Puede ver los costos agregados por organización para saber dónde se acumulan estos e identificar las tendencias de gasto. Además, puede ver los costos acumulados con el tiempo para estimar las tendencias de costos mensual, trimestral o incluso anualmente con respecto a un presupuesto.

Los presupuestos le ayudarán a planear y cumplir con las responsabilidades financieras de la organización. Ayudan a evitar que se superen los umbrales o los límites de costos. Los presupuestos también pueden ayudar a informar a otros usuarios sobre sus gastos para que administren de manera proactiva los costos. Y con ellos, puede ver cómo los gastos crecen con el tiempo.

Las recomendaciones le muestran cómo optimizar y mejorar la eficiencia mediante la identificación de recursos inactivos e infrautilizados. O bien, pueden mostrar las opciones de recurso menos costosas. Al implementar las recomendaciones, puede cambiar la forma en que usa sus recursos para ahorrar dinero. Para hacerlo, primero debe ver las recomendaciones de optimización de costos, para así ver posibles deficiencias de uso. A continuación, puede implementar una recomendación al cambiar su uso del recurso de Azure por una opción más rentable. Después, puede verificar la acción para asegurarse de que el cambio se realizó correctamente.

Si usa los sistemas externos para acceder o revisar los datos de administración de costos, puede exportar los datos fácilmente desde Azure. Y puede establecer una exportación programada diariamente en formato CSV y almacenar los archivos de datos en el almacenamiento de Azure. A continuación, puede tener acceso a los datos desde el sistema externo.

## <a name="consider-cloudyn"></a>Considere la posibilidad de Cloudyn

[Cloudyn](overview.md) es un servicio de Azure relacionado con Cost Management. Con Cloudyn, puede realizar un seguimiento de los gastos y el uso de la nube para los recursos de Azure. También admite otros proveedores de nube, como AWS y Google. Los sencillos informes del panel proporcionan ayuda con la asignación de costos, los contracargos y la visibilidad de los gastos. Actualmente, Cost Management no es compatible con la visualización ni los contracargos, ni con otros proveedores de servicios en la nube. No obstante, Cloudyn es una opción que _sí_ es compatible. Actualmente, Cost Management no admite las cuentas del Proveedor de servicios en la nube (CSP) de Microsoft, pero Cloudyn sí lo hace. Si tiene las cuentas de CSP o si quiere usar visualización y contracargo de costos, puede usar Cloudyn para administrar los costos.

Vea el [vídeo de Azure Cost Management y Cloudyn](https://www.youtube.com/watch?v=PmwFWwSluh8) para obtener recomendaciones de cuándo debe usar Azure Cost Management o Cloudyn, según sus necesidades empresariales.

>[!VIDEO https://www.youtube.com/embed/PmwFWwSluh8]

## <a name="additional-azure-tools"></a>Herramientas de Azure adicionales

Azure tiene otras herramientas que no forman parte del conjunto de características de Azure Cost Management. Sin embargo, tienen un rol importante en el proceso de administración de costos. Para aprender más sobre dichas herramientas, consulte los siguientes vínculos.

- [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/): utilice esta herramienta para calcular los en la nube costos por adelantado.
- [Azure Migrate](../migrate/migrate-overview.md): evalué la carga de trabajo actual de su centro de datos para obtener información sobre lo que se necesita de la solución de reemplazo de Azure.
- [Azure Advisor](../advisor/advisor-overview.md): identifique las máquinas virtuales no utilizadas y reciba recomendaciones sobre la compra de instancias reservadas de Azure.
- [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/): use sus licencias locales de Windows Server o SQL Server para máquinas virtuales en Azure para el guardado.


## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con Cost Management, el siguiente paso es que empiece a usar el servicio.

- Empiece por usar Azure Cost Management para [analizar los costos](quick-acm-cost-analysis.md).
- También puede leer más sobre [Azure Cost Management best practices](cost-mgt-best-practices.md) (Procedimientos recomendados de Azure Cost Management).

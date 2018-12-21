---
title: Introducción a Azure Cost Management | Microsoft Docs
description: Azure Cost Management es una solución de administración de costos que ayuda a supervisar y controlar el gasto de Azure y a optimizar el uso de los recursos.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 12/05/2018
ms.topic: overview
ms.service: cost-management
manager: benshy
ms.custom: ''
ms.openlocfilehash: a90ef531cedb5e4c32a8f0af8b6cca86a93fb39a
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52997125"
---
# <a name="what-is-azure-cost-management"></a>¿Qué es Administración de costos de Azure?

La administración de costos es el proceso en el que se planifican y controlan eficazmente los costos implicados en su negocio. Normalmente, los equipos de finanzas, administración y aplicación realizan las tareas de administración de costos. Azure Cost Management ayuda a las organizaciones con Contratos Enterprise (EA) de Microsoft a planificar con los costos en mente. También ayuda a analizar eficazmente los costos y a tomar medidas para optimizar el gasto en la nube. Para obtener más información sobre cómo abordar la administración de costos como organización, eche un vistazo al artículo [Azure Cost Management best practices](cost-mgt-best-practices.md) (Procedimientos recomendados de Azure Cost Management).

Aunque están relacionados, la facturación difiere de administración de costos. La facturación es el proceso de facturar a los clientes por bienes o servicios y administrar la relación comercial.  Los equipos de adquisiciones y finanzas son los que normalmente realizan las tareas de facturación.

Cost Management muestras los costos de organización y los patrones de uso con análisis avanzados. Los informes de Cost Management muestran los costos de Azure, el uso, las instancias reservadas y el uso de la ventaja híbrida de Azure. En conjunto, en los informes se muestran los costos internos y externos del uso y los cargos de Azure Marketplace. Otros cargos, como las compras de reservas, el soporte técnico y los impuestos no se muestran todavía en los informes. Los informes le ayudan a comprender los gastos y el uso de recursos, y pueden ayudar a encontrar anomalías de gastos. Los análisis predictivos también están disponibles. Cost Management usa grupos de administración de Azure, presupuestos y recomendaciones para mostrar claramente cómo se organizan los gastos y cómo se pueden reducir los costos.

Puede usar Azure Portal o varias API para la automatización de la exportación, de manera que se integren los datos de costos con los procesos y sistemas externos. También están disponibles la exportación de datos de facturación automatizada y los informes programados.

## <a name="plan-and-control-expenses"></a>Planificación y control de los gastos

A continuación se mencionan las formas mediante las que Cost Management le ayuda a planear y controlar los costos: análisis de costos, presupuestos, recomendaciones y exportación de datos de administración de costos.

Los análisis de costos se usan para explorar y analizar los costos de su organización. Puede ver los costos agregados por organización para saber dónde se acumulan estos e identificar las tendencias de gasto. Además, puede ver los costos acumulados con el tiempo para estimar las tendencias de costos mensual, trimestral o incluso anualmente con respecto a un presupuesto.

Los presupuestos le ayudarán a planear y cumplir con las responsabilidades financieras de la organización. Ayudan a evitar que se superen los umbrales o los límites de costos. Los presupuestos también pueden ayudar a informar a otros usuarios sobre sus gastos para que administren de manera proactiva los costos. Y con ellos, puede ver cómo los gastos crecen con el tiempo.

Las recomendaciones le muestran cómo optimizar y mejorar la eficiencia mediante la identificación de recursos inactivos e infrautilizados. O bien, pueden mostrar las opciones de recurso menos costosas. Al implementar las recomendaciones, puede cambiar la forma en que usa sus recursos para ahorrar dinero. Para hacerlo, primero debe ver las recomendaciones de optimización de costos, para así ver posibles deficiencias de uso. A continuación, puede implementar una recomendación al cambiar su uso del recurso de Azure por una opción más rentable. Después, puede verificar la acción para asegurarse de que el cambio se realizó correctamente.

Si usa los sistemas externos para acceder o revisar los datos de administración de costos, puede exportar los datos fácilmente desde Azure. Y puede establecer una exportación programada diariamente en formato CSV y almacenar los archivos de datos en el almacenamiento de Azure. A continuación, puede tener acceso a los datos desde el sistema externo.

## <a name="consider-cloudyn"></a>Considere la posibilidad de Cloudyn

[Cloudyn](overview.md) es un servicio de Azure relacionado con Cost Management. Con Cloudyn, puede realizar un seguimiento de los gastos y el uso de la nube para los recursos de Azure. También admite otros proveedores de nube, como AWS y Google. Los sencillos informes del panel proporcionan ayuda con la asignación de costos, los contracargos y la visibilidad de los gastos. Actualmente, Cost Management no es compatible con la visualización ni los contracargos, ni con otros proveedores de servicios en la nube. No obstante, Cloudyn es una opción que _sí_ es compatible. Actualmente, Cost Management solo es compatible con las cuentas de Azure EA. Aunque no es compatible con cuentas individuales o de pago por uso, ni con cuentas de proveedor de servicios de nube de Microsoft, Cloudyn sí lo es. Si tiene una de esas cuentas, puede utilizar Cloudyn para ayudar a administrar los costos.

## <a name="additional-azure-tools"></a>Herramientas de Azure adicionales

Azure tiene otras herramientas que no forman parte del conjunto de características de Azure Cost Management. Sin embargo, cumplen un papel importante en el proceso de administración de costos. Para aprender más sobre dichas herramientas, consulte los siguientes vínculos.

- [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/): utilice esta herramienta para calcular los en la nube costos por adelantado.
- [Azure Migrate](../migrate/migrate-overview.md): evalué la carga de trabajo actual de su centro de datos para obtener información sobre lo que se necesita de la solución de reemplazo de Azure.
- [Azure Advisor](../advisor/advisor-overview.md): identifique las máquinas virtuales no utilizadas y reciba recomendaciones sobre la compra de instancias reservadas de Azure.
- [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/): use sus licencias locales de Windows Server o SQL Server para máquinas virtuales en Azure para el guardado.


## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con Cost Management, el siguiente paso es que empiece a usar el servicio.

- Empiece por usar Azure Cost Management para [analizar los costos](quick-acm-cost-analysis.md).
- También puede leer más sobre [Azure Cost Management best practices](cost-mgt-best-practices.md) (Procedimientos recomendados de Azure Cost Management).

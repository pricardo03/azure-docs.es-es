---
title: Administrar los costos de AWS y el uso en Azure Cost Management
description: En este artículo le ayudará a comprender cómo usar análisis de costos y los presupuestos en Cost Management para administrar los costos de AWS y uso.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: 007b6c409dde248a4dde7a15fd16b543add234bc
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870318"
---
# <a name="manage-aws-costs-and-usage-in-azure"></a>Administrar los costos de AWS y el uso de Azure

Una vez que haya instalado y configurado la integración de informes de uso y costos de AWS para Azure Cost Management, está listo para empezar a administrar los costos de AWS y uso. En este artículo le ayudará a comprender cómo usar análisis de costos y los presupuestos en Cost Management para administrar los costos de AWS y uso.

Si no ha configurado la integración, consulte [establecer configuración y configurar la integración de informes de uso de AWS](aws-integration-set-up-configure.md).

_Antes de comenzar_: Si no está familiarizado con el análisis de costos, consulte el [Explore y analice los costos con análisis de costos](quick-acm-cost-analysis.md) Guía de inicio rápido. Y, si no está familiarizado con los presupuestos en Azure, consulte el [crear y administrar presupuestos Azure](tutorial-acm-create-budgets.md) tutorial.

## <a name="view-aws-costs-in-cost-analysis"></a>Ver los costos AWS en análisis de costos

Los costos de AWS están disponibles en análisis de costos en los siguientes ámbitos:

- Cuentas AWS vinculado en un grupo de administración
- Costos de cuenta AWS vinculado
- Costos de cuenta AWS consolidado

Las secciones siguientes describen cómo utilizar los ámbitos para que vea datos de uso y costo para cada uno de ellos.

### <a name="view-aws-linked-accounts-under-a-management-group"></a>Ver cuentas de AWS vinculado en un grupo de administración

Ver los costos mediante el ámbito de grupo de administración es la única manera de ver los costos agregados procedentes de distintas suscripciones y cuentas vinculadas. Uso de un grupo de administración proporciona una vista de toda la nube.

En el análisis de costos, abra el selector de ámbito y seleccione el grupo de administración que contiene las cuentas de AWS vinculado. Esta es una imagen de ejemplo en el portal de Azure:

![Ejemplo de la vista de ámbito seleccione](./media/aws-integration-manage/select-scope01.png)



Este es un ejemplo que muestra el coste de grupo de administración de análisis de costos, agrupados por proveedor (Azure y AWS).

![Ejemplo que muestra los costos de Azure y AWS para análisis de costos en un trimestre](./media/aws-integration-manage/cost-analysis-aws-azure.png)

### <a name="view-aws-linked-account-costs"></a>Ver los costos de la cuenta AWS vinculado

Para ver los costos AWS vínculo cuenta, abra el selector de ámbito y seleccione el AWS cuenta vinculada. Tenga en cuenta que las cuentas vinculadas están asociadas a un grupo de administración, tal como se define en el conector AWS.

Este es un ejemplo que se muestra la selección de AWS vinculado el ámbito de la cuenta.

![Ejemplo de la vista de ámbito seleccione](./media/aws-integration-manage/select-scope02.png)



### <a name="view-aws-consolidated-account-costs"></a>Vista AWS consolidados los costos de la cuenta

Para ver AWS cuenta consolidada de los costos, abra el selector de ámbito y seleccione el AWS cuenta consolidada. Este es un ejemplo que se muestra la selección de AWS consolidados de ámbito de la cuenta.

![Ejemplo de la vista de ámbito seleccione](./media/aws-integration-manage/select-scope03.png)



Este ámbito proporciona que una vista agregada de todos los AWS vinculado cuentas asociadas con la cuenta de AWS consolidado. Este es un ejemplo que muestra los costos de AWS consolidado cuenta, agrupado por nombre de servicio.

![Ejemplo que muestra AWS consolidan los costos en análisis de costos](./media/aws-integration-manage/cost-analysis-aws-consolidated.png)

### <a name="dimensions-available-for-filtering-and-grouping"></a>Dimensiones disponibles para el filtrado y agrupación

La tabla siguiente describe las dimensiones disponibles para agrupar y filtrar por en el análisis de costos.

| Dimension Data | Amazon CUR header | Ámbitos | Comentarios |
| --- | --- | --- | --- |
| Zona de disponibilidad | lineitem/AvailabilityZone | Todo |   |
| Location | producto o región | Todo |   |
| Medidor |   | Todo |   |
| Categoría del medidor | lineItem/ProductCode | Todo |   |
| Subcategoría del medidor | lineitem/UsageType | Todo |   |
| Operación | lineItem/Operation | Todo |   |
| Recurso | lineItem/ResourceId | Todo |   |
| Tipo de recurso | product/instanceType | Todo | Si el producto/instanceType es null, se usa lineItem/UsageType. |
| ResourceGuid | N/D | Todo | GUID de medidor de Azure. |
| Nombre del servicio | producto/ProductName | Todo | Si el producto/ProductName es null, se usa lineItem/ProductCode. |
| Nivel de servicio |   |   |   |
| Id. de suscripción | lineItem/UsageAccountId | Grupo de cuenta y la administración consolidado |   |
| Nombre de la suscripción | N/D | Grupo de cuenta y la administración consolidado | Los nombres de cuentas se recopilan mediante la API de la organización de AWS. |
| Etiqueta | resourceTags/\* | Todo | El _usuario:_ prefijo se quita de las etiquetas definidas por el usuario para permitir que las etiquetas de toda la nube. El _aws:_ prefijo se deje intacto. |
| Id. de cuenta de facturación | bill/PayerAccountId | Grupo de administración |   |
| Nombre de la cuenta de facturación | N/D | Grupo de administración | Los nombres de cuentas se recopilan mediante la API de la organización de AWS. |
| Proveedor | N/D | Grupo de administración | O bien, AWS o Azure. |

## <a name="set-budgets-on-aws-scopes"></a>Presupuestos de conjunto de ámbitos AWS

Use los presupuestos para administrar de manera proactiva la responsabilidad de los costos y unidad de su organización. Los presupuestos se establecen en la cuenta AWS consolidado y AWS ámbitos cuenta vinculada. Este es un ejemplo de presupuestos para una cuenta de AWS consolidados se muestra en la administración de costos:

![Cuenta consolidada de ejemplo que muestra los presupuestos de AWS](./media/aws-integration-manage/budgets-aws-consolidated-account01.png)

## <a name="aws-data-collection-process"></a>Proceso de recopilación de datos AWS

Después de configurar el conector AWS, inician la recopilación de datos y procesos de detección. Puede tardar unas horas en recopilar todos los datos de uso. La duración depende de:

- El tiempo necesario para procesar los archivos CUR que se encuentran en el cubo de AWS S3.
- El tiempo necesario para crear la cuenta de AWS consolidado y ámbitos de la cuenta vinculada de AWS.
- El tiempo y la frecuencia de AWS escribe los archivos de informe de uso y costo en el depósito de S3

## <a name="aws-integration-pricing"></a>Precios de integración de AWS

Cada conector AWS obtiene 90 días de prueba gratuita. Durante la versión preliminar pública, no hay ningún cargo.

El precio de venta es de 1% de los costos mensuales de AWS. Cada mes se cobra en función de los costos facturados del mes anterior.

Obtener acceso a las API de AWS puede suponer costos adicionales.

## <a name="aws-integration-limitations"></a>Limitaciones de la integración de AWS

- Administración de costos no es compatible con los informes de costos que contienen varios tipos de moneda. Se muestra un mensaje de error si selecciona un ámbito que tenga varias monedas.
- Los conectores de nube no admiten AWS GovCloud (US), AWS Gov o AWS China.
- Administración de costos muestra AWS _los costos de uso_ solo. Aún no se admite los impuestos, soporte técnico, reembolsos, RI, créditos o cualquier otro tipo de cargo.

## <a name="troubleshooting-aws-integration"></a>Solucionar problemas de integración de AWS

Use la siguiente información de solución de problemas para resolver problemas comunes.

### <a name="no-permission-to-aws-linked-accounts"></a>No tiene permiso para las cuentas vinculadas de AWS

Hay dos maneras de obtener permisos para acceder a los costos de las cuentas AWS vinculado:

- Obtenga acceso al grupo de administración que tenga las cuentas vinculadas de AWS.
- Alguien tiene te dé permiso a la cuenta de AWS vinculado.

De forma predeterminada, el creador del conector AWS es el propietario de todos los objetos creados por el conector. Incluido el AWS cuenta consolidado y el AWS cuenta vinculada.

### <a name="collection-failed-with-assumerole"></a>Error con AssumeRole de recopilación

Este error significa que no se puede llamar a la API de AWS AssumeRole Cost Management. Este problema puede ocurrir debido a un problema con la definición de función. Compruebe que las condiciones siguientes son ciertas:

- El identificador externo es el mismo que el de la definición de función y la definición del conector.
- El tipo de rol se establece en **AWS otra cuenta que pertenecen a usted o terceros 3rd.**
- El **exigencia de MFA** opción está desactivada.
- Es la cuenta de AWS confianza en el rol de AWS _432263259397_.

### <a name="collection-failed-with-access-denied"></a>Error con acceso denegado de recopilación

Este mensaje de error significa que no se puede acceder a los archivos CUR almacenados en el depósito de Amazon S3 Cost Management. Asegúrese de que la directiva de AWS JSON asociada a la función es similar al ejemplo que se muestra en la parte inferior de la [crear un rol y una directiva en AWS](aws-integration-set-up-configure.md#create-a-role-and-policy-in-aws) sección.

### <a name="connector-error-with-failedtofindreport"></a>Error del conector con FailedToFindReport

Este error significa que Cost Management no puede encontrar el informe de costo y uso que se definió en el conector. Asegúrese de no se elimine y que la directiva de AWS JSON asociada a la función es similar al ejemplo que se muestra en la parte inferior de la [crear un rol y una directiva en AWS](aws-integration-set-up-configure.md#create-a-role-and-policy-in-aws) sección.

## <a name="next-steps"></a>Pasos siguientes

- Si ya no ha configurado el entorno de Azure con grupos de administración, consulte [configuración de grupos de administración inicial](../governance/management-groups/index.md#initial-setup-of-management-groups).

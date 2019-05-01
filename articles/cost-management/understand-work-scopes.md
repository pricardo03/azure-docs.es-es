---
title: Entender y trabajar con los ámbitos de Azure Cost Management | Microsoft Docs
description: En este artículo le ayudará a comprender la facturación y recursos ámbitos de administración disponibles en Azure y cómo usar los ámbitos de API y la administración de costos.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/13/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 4e7956e8873b552fcd73c51a51f51d99f21af324
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61003018"
---
# <a name="understand-and-work-with-scopes"></a>Descripción y uso de ámbitos

En este artículo le ayudará a comprender la facturación y recursos ámbitos de administración disponibles en Azure y cómo usar los ámbitos de API y la administración de costos.

## <a name="scopes"></a>Ámbitos

Un _ámbito_ es un nodo de la jerarquía de recursos de Azure, donde los usuarios de Azure AD acceden y administrar servicios. Recursos de Azure más se crean e implementan en grupos de recursos, que forman parte de las suscripciones. Microsoft también ofrece dos jerarquías por encima de las suscripciones de Azure que se han especializado a roles para administrar datos de facturación:
- Datos de facturación, como los pagos y las facturas
- Servicios de nube, como el costo y la directiva de gobierno

Los ámbitos son donde administrar datos de facturación, tienen funciones específicas de los pagos, ver las facturas y llevar a cabo la administración de cuentas general. Roles de cuentas y facturación se administran por separado desde los que se usan para la administración de recursos, que usan [RBAC de Azure](../role-based-access-control/overview.md). Para distinguir claramente la intención de los ámbitos independientes, incluidas las diferencias de control de acceso, estos se conocen como _facturación ámbitos_ y _ámbitos RBAC_, respectivamente.

## <a name="how-cost-management-uses-scopes"></a>Cómo Cost Management utiliza ámbitos

En todos los ámbitos funciona la administración de costos por encima de los recursos permite que las organizaciones a administrar los costos en el nivel en el que tiene acceso, ya sea un único grupo de recursos o de toda la cuenta de facturación. Aunque los ámbitos de facturación varían en función de su contrato de Microsoft (tipo de suscripción), los ámbitos RBAC no lo hacen.

## <a name="azure-rbac-scopes"></a>Ámbitos de RBAC de Azure

Azure admite tres ámbitos para la administración de recursos. Cada ámbito es compatible con la administración de acceso y gobierno, incluyendo pero sin carácter limitativo, administración de costos.

- [**Grupos de administración** ](../governance/management-groups/index.md) -contenedores jerárquicos, hasta ocho niveles, para organizar las suscripciones de Azure.

    Tipo de recurso: [Microsoft.Management/managementGroups](/rest/api/resources/managementgroups)

- **Suscripciones** -contenedores principales para los recursos de Azure.

    Tipo de recurso: [Microsoft.Resources/subscriptions](/rest/api/resources/subscriptions)

- [**Grupos de recursos** ](../azure-resource-manager/resource-group-overview.md#resource-groups) -agrupaciones lógicas de los recursos relacionados de una solución de Azure que comparten el mismo ciclo de vida. Por ejemplo los recursos que se implementan y eliminar juntos.

    Tipo de recurso: [Microsoft.Resources/subscriptions/resourceGroups](/rest/api/resources/resourcegroups)

Grupos de administración permiten organizar las suscripciones en una jerarquía. Por ejemplo, podría crear una jerarquía de organización lógica mediante grupos de administración. A continuación, asigne a las suscripciones de los equipos para cargas de trabajo de desarrollo y pruebas y producción. Y, a continuación, cree grupos de recursos en las suscripciones para administrar cada subsistema o componente.

Creación de que una jerarquía organizativa permite costo y cumplimiento de directivas acumulación de una organización. A continuación, cada directriz puede ver y analizar los costos actuales. Y, a continuación, pueden crear los presupuestos para criar malos patrones de gastos y optimizar los costos con las recomendaciones de Advisor en el nivel más bajo.

Conceder acceso para ver los costos y, opcionalmente, administrar la configuración de costo, como los presupuestos y las exportaciones, se realiza en los ámbitos de gobierno mediante RBAC en Azure. Use RBAC de Azure para conceder a los usuarios de Azure AD y obtener acceso grupos para llevar a cabo un conjunto predefinido de acciones que se definen en un rol en un ámbito específico y, a continuación. Por ejemplo, un rol asignado a un ámbito de grupo de administración concede también los mismos permisos a grupos de recursos y suscripciones anidadas.

Administración de costos es compatible con los siguientes roles integrados para cada uno de los siguientes ámbitos:

- [**Propietario** ](../role-based-access-control/built-in-roles.md#owner) : puede ver los costos y administrar todo, incluida la configuración de costo.
- [**Colaborador** ](../role-based-access-control/built-in-roles.md#contributor) : puede ver los costos y administrar todo, incluida la configuración de costo, pero sin incluir el control de acceso.
- [**Lector** ](../role-based-access-control/built-in-roles.md#reader) : puede ver todo, incluidos los datos de costo y la configuración, pero no realizar cambios.
- [**Colaborador de la administración de costos** ](../role-based-access-control/built-in-roles.md#cost-management-contributor) : puede ver los costos y administrar la configuración de costo.
- [**Lector de administración de costos** ](../role-based-access-control/built-in-roles.md#cost-management-reader) : puede ver datos de costos y la configuración.

Colaborador de administración de costos es el rol con privilegios mínimos recomendado. Permite a los usuarios acceso para crear y administrar presupuestos y exporta más eficaz de supervisar y notificar sobre los costos. Colaboradores de la administración de costos también pueden requerir funciones adicionales para admitir escenarios de administración de costos to-end. Considere los siguientes escenarios:

- **Actuar cuando se superan los presupuestos** – colaboradores de la administración de costos también necesitan tener acceso a crear o administrar grupos de acciones para responder automáticamente a un uso excesivo. Considere la posibilidad de conceder [colaborador de supervisión](../role-based-access-control/built-in-roles.md#monitoring-contributor) a un grupo de recursos que contiene el grupo de acciones para usar cuando se superan los umbrales de presupuesto. Automatización de acciones específicas requiere roles adicionales para los servicios específicos que se usa como Automation y Azure Functions.
- **Exportación de datos de costo de programación** – colaboradores de la administración de costos también necesiten acceder para administrar las cuentas de almacenamiento para programar una exportación para copiar datos en una cuenta de almacenamiento. Considere la posibilidad de conceder [colaborador de la cuenta de almacenamiento](../role-based-access-control/built-in-roles.md#storage-account-contributor) a un grupo de recursos que contiene el almacenamiento se exporta la cuenta donde los datos de costo.
- **Visualización de recomendaciones de ahorro de costos** : costo de administración de lectores y colaboradores no tienen acceso a las recomendaciones de forma predeterminada. Acceso a recomendaciones requiere acceso de lectura a los recursos individuales. Considere la posibilidad de conceder [lector](../role-based-access-control/built-in-roles.md#reader) o un [rol específicos del servicio](../role-based-access-control/built-in-roles.md#built-in-role-descriptions).

## <a name="enterprise-agreement-scopes"></a>Ámbitos del contrato Enterprise

Cuentas de facturación de contrato Enterprise (EA), también denominadas inscripciones, tienen los siguientes ámbitos:

- [**Cuenta de facturación** ](../billing/billing-view-all-accounts.md) -representa una inscripción de EA. Las facturas se generan en este ámbito. Las compras que no están basados en uso, como Marketplace y reservas, solo están disponibles en este ámbito. Que no se representan en departamentos o las cuentas de inscripción.

    Tipo de recurso: `Microsoft.Billing/billingAccounts (accountType = Enrollment)`
- **Departamento** : opcional de agrupación de las cuentas de inscripción.

    Tipo de recurso: `Billing/billingAccounts/departments`

- **Cuenta de inscripción** -representa un único propietario. No admite la concesión de acceso a varias personas.

    Tipo de recurso: `Microsoft.Billing/billingAccounts/enrollmentAccounts`

Aunque los ámbitos de gobierno están enlazados a un único directorio, no son ámbitos de facturación de EA. Una cuenta de facturación de EA puede tener suscripciones a través de cualquier número de directorios de Azure AD.

Los ámbitos de facturación de EA admiten los siguientes roles:

- **Administrador de empresa** : puede administrar la configuración de la cuenta de facturación y acceso, puede ver todos los costos y puede administrar la configuración de costo. Por ejemplo, presupuestos y exporta. En la función, el ámbito de facturación de EA es igual a [rol RBAC de Azure Cost Management colaborador](../role-based-access-control/built-in-roles.md#cost-management-contributor).
- **Usuario de solo lectura de la empresa** : puede ver la configuración de la cuenta de facturación, los datos de costo y la configuración de costo. Por ejemplo, presupuestos y exporta. En la función, el ámbito de facturación de EA es el mismo que el [rol RBAC de Azure Cost Management lector](../role-based-access-control/built-in-roles.md#cost-management-reader).
- **Administrador de departamento** : puede administrar la configuración de departamento, por ejemplo, el centro de costo y puede tener acceso, ver todos los costos y administrar la configuración de costo. Por ejemplo, presupuestos y exporta.  El **vea los cargos** configuración de la cuenta de facturación debe estar habilitada para que los administradores de departamento y usuarios de solo lectura para ver los costos. Si **vea los cargos** está deshabilitado, los usuarios del departamento no pueden ver los costos en cualquier nivel, incluso si son un propietario de cuenta o suscripción.
- **Usuario de solo lectura de departamento** : puede ver la configuración de departamento, datos de costos y la configuración de costo. Por ejemplo, presupuestos y exporta. Si **vea los cargos** está deshabilitado, los usuarios del departamento no pueden ver los costos en cualquier nivel, incluso si son un propietario de cuenta o suscripción.
- **Propietario de la cuenta** : puede administrar la configuración de la cuenta de inscripción (por ejemplo, el centro de costo), ver todos los costos y administrar la configuración de costo (por ejemplo, los presupuestos y las exportaciones) para la cuenta de inscripción. El **vea los cargos** configuración de la cuenta de facturación debe estar habilitada para que los propietarios de cuentas y usuarios RBAC para ver los costos.

Usuarios de cuentas de facturación de EA no tienen acceso directo a las facturas. Las facturas están disponibles desde un sistema de licencias de volumen externo.

Las suscripciones de Azure se anidan bajo cuentas de inscripción. La facturación a los usuarios tengan acceso a los datos de costo para las suscripciones y grupos de recursos que están bajo sus respectivos ámbitos. No tiene acceso para ver o administrar los recursos en Azure portal. Facturación a los usuarios pueden ver los costos, vaya a **Cost Management + facturación** en la lista de portal de Azure de servicios. A continuación, pueden filtrar los costos a los que necesitan para informar sobre los grupos de recursos y suscripciones específicos.

Los usuarios de facturación no tienen acceso a grupos de administración ya que no estén explícitamente en una cuenta de facturación específica. Debe conceder acceso a grupos de administración explícitamente. Grupos de administración de costos acumulados de suscripciones anidados. Sin embargo, solo incluye las compras basada en uso. No incluyen las compras, como las reservas y las ofertas de Marketplace de terceros. Para ver estos costos, use la cuenta de facturación de EA.

## <a name="individual-agreement-pay-as-you-go-scopes"></a>Ámbitos de contratos individuales (pago por uso)

Las suscripciones de pago por uso (PAYG), incluidos los tipos relacionados como gratis o de evaluación y ofertas de desarrollo y pruebas, no tienen un ámbito explícito de la cuenta de facturación. En su lugar, cada suscripción tiene un administrador de cuenta, como propietario de la cuenta EA o de propietario de la cuenta.

- [**Cuenta de facturación** ](../billing/billing-view-all-accounts.md) -representa un único propietario para una o varias suscripciones de Azure. Actualmente no admite la concesión de acceso a varias personas o acceso a las vistas de costos agregados.

    Tipo de recurso: No aplicable

Administradores de cuentas de suscripción de pago por uso. puede ver y administrar datos de facturación, como facturas y pagos, desde el [centro de cuentas de Azure](https://account.azure.com/subscriptions). Sin embargo, no se puede ver los datos de costos o administrar los recursos en Azure portal. Para conceder acceso al administrador de la cuenta, use las funciones de administración de costos que se ha mencionado anteriormente.

A diferencia de EA, administradores de cuentas de suscripción de pago por uso. puede ver sus facturas en el portal de Azure. Tenga en cuenta que los roles de lector de administración de costos y colaborador de la administración de costos no proporcionan acceso a las facturas. Para obtener más información, consulte [cómo conceder acceso a las facturas de pago por uso](../billing/billing-manage-access.md#give-access-to-billing).

## <a name="customer-agreement-scopes"></a>Ámbitos del contrato de cliente

Las cuentas de facturación de contrato de cliente de Microsoft tienen los siguientes ámbitos:

- **Cuenta de facturación** -representa un contrato para clientes varios productos y servicios de Microsoft. Las cuentas de facturación de contrato de cliente no son funcionalmente igual que las inscripciones de EA. Las inscripciones de EA se alinean más estrechamente a los perfiles de facturación.

    Tipo de recurso: `Microsoft.Billing/billingAccounts (accountType = Organization)`

- **Perfil de facturación** -define las suscripciones que se incluyen en una factura. Perfiles de facturación son el equivalente funcional de una inscripción de EA, ya que es el ámbito que las facturas se generan en. De forma similar, las compras que no están basados en uso (por ejemplo, Marketplace y reservas) solo están disponibles en este ámbito. No están incluidas en las secciones de la factura.

    Tipo de recurso: `Microsoft.Billing/billingAccounts/billingProfiles`

- **Sección de factura** -representa un grupo de suscripciones en una factura o el perfil de facturación. Las secciones de la factura son como los departamentos de TI, varias personas pueden tener acceso a una sección de la factura.

    Tipo de recurso: `Microsoft.Billing/billingAccounts/invoiceSections`

A diferencia de EA de facturación de los ámbitos, contrato de cuentas de facturación _son_ enlazado a un único directorio y no pueden tener suscripciones en varios directorios de Azure AD.

Ámbitos de facturación de contrato de cliente admiten los siguientes roles:

- **Propietario** : puede administrar la configuración de facturación y acceso, ver todos los costos y administrar la configuración de costo. Por ejemplo, presupuestos y exporta. En función, este contrato de facturación de ámbito es el mismo que el [rol RBAC de Azure Cost Management colaborador](../role-based-access-control/built-in-roles.md#cost-management-contributor).
- **Colaborador** : puede administrar la configuración de facturación, excepto el acceso, ver todos los costos y administrar la configuración de costo. Por ejemplo, presupuestos y exporta. En función, este contrato de facturación de ámbito es el mismo que el [rol RBAC de Azure Cost Management colaborador](../role-based-access-control/built-in-roles.md#cost-management-contributor).
- **Lector** : puede ver la configuración de facturación, los datos de costo y la configuración de costo. Por ejemplo, presupuestos y exporta. En función, este contrato de facturación de ámbito es el mismo que el [rol RBAC de Azure Cost Management lector](../role-based-access-control/built-in-roles.md#cost-management-reader).
- **Administrador de facturas** : puede ver y pagar las facturas y puede ver los datos y la configuración de costo. Por ejemplo, presupuestos y exporta. En función, este contrato de facturación de ámbito es el mismo que el [rol RBAC de Azure Cost Management lector](../role-based-access-control/built-in-roles.md#cost-management-reader).
- **Creador de la suscripción de Azure** : puede crear suscripciones de Azure, ver los costos y administrar la configuración de costo. Por ejemplo, presupuestos y exporta. En la función, este ámbito de facturación de contrato de cliente es el mismo que el rol de propietario de cuenta de inscripción de EA.

Las suscripciones de Azure están anidadas en las secciones de la factura, como cómo están bajo cuentas de inscripción de EA. La facturación a los usuarios tengan acceso a los datos de costo para las suscripciones y grupos de recursos que están bajo sus respectivos ámbitos. Sin embargo, no tienen acceso para ver o administrar los recursos en Azure portal. Facturación a los usuarios pueden ver los costos, vaya a **Cost Management + facturación** en la lista de portal de Azure de servicios. A continuación, filtrar los costos a los que necesitan para informar sobre los grupos de recursos y suscripciones específicos.

Los usuarios de facturación no tienen acceso a los grupos de administración ya que no estén explícitamente en la cuenta de facturación. Sin embargo, cuando se habilitan los grupos de administración para la organización, todos los costos de suscripción están consolidadas para la cuenta de facturación y el grupo de administración raíz porque ambos están limitados a un único directorio. Grupos de administración solo incluyen las compras que están basados en uso. Las compras, como las reservas y las ofertas de Marketplace de terceros no están incluidas en grupos de administración. Por lo tanto, el grupo de administración raíz y la cuenta de facturación puede notificar totales diferentes. Para ver estos costos, use la cuenta de facturación o el correspondiente perfil de facturación.

## <a name="cloud-solution-provider-csp-scopes"></a>Ámbitos de proveedor de soluciones (CSP) en la nube

Asociados de proveedor de soluciones (CSP) en la nube no se admiten en Cost Management hoy mismo. En su lugar, puede usar [centro de partners](https://docs.microsoft.com/azure/cloud-solution-provider/overview/partner-center-overview).

## <a name="switch-between-scopes-in-cost-management"></a>Cambiar entre los ámbitos en Cost Management

Todas las vistas de administración de costos en Azure portal incluyen una **ámbito** pastilla en la parte superior izquierda de la vista. Puede usarlo para cambiar rápidamente el ámbito. Haga clic en el **ámbito** pastilla para abrir el selector de ámbito. Muestra las cuentas de facturación, el grupo de administración raíz y todas las suscripciones que no están anidadas en el grupo de administración raíz. Para seleccionar un ámbito, haga clic en el fondo para resaltarla y, a continuación, haga clic en **seleccione** en la parte inferior. Para la exploración en profundidad en los ámbitos anidados, como grupos de recursos en una suscripción, haga clic en el vínculo del nombre de ámbito. Para seleccionar el ámbito primario en cualquier nivel anidado, haga clic en **Seleccione esta opción &lt;ámbito&gt;**  en la parte superior del selector de ámbito.

## <a name="identify-the-resource-id-for-a-scope"></a>Identificar el identificador de recurso para un ámbito

Cuando se trabaja con las API de administración de costos, sabiendo que el ámbito es fundamental. Use la siguiente información para crear el URI del ámbito adecuado para las API de administración de costos.

### <a name="billing-accounts"></a>Cuentas de facturación

1. Abra el portal de Azure y, a continuación, vaya a **Cost Management + facturación** en la lista de servicios.
2. Seleccione **propiedades** en el menú de la cuenta de facturación.
3. Copie el identificador de cuenta facturación.
4. El ámbito es: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}"`

### <a name="billing-profiles"></a>Perfiles de facturación

1. Abra el portal de Azure y, a continuación, vaya a **Cost Management + facturación** en la lista de servicios.
2. Seleccione **perfiles facturación** en el menú de la cuenta de facturación.
3. Haga clic en el nombre del perfil de facturación deseado.
4. Seleccione **propiedades** en el menú de perfil de facturación.
5. Copie la cuenta de facturación y el Id. de perfil de facturación.
6. El ámbito es: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}"`

### <a name="invoice-sections"></a>Secciones de factura

1. Abra el portal de Azure y, a continuación, vaya a **Cost Management + facturación** en la lista de servicios.
2. Seleccione **factura secciones** en el menú de la cuenta de facturación.
3. Haga clic en el nombre de la sección de factura que desee.
4. Seleccione **propiedades** en el menú de la sección de factura.
5. Copie la cuenta de facturación y los identificadores de la sección de factura.
6. El ámbito es: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/invoiceSections/{invoiceSectionId}"`

### <a name="ea-departments"></a>Departamentos de EA

1. Abra el portal de Azure y, a continuación, vaya a **Cost Management + facturación** en la lista de servicios.
2. Seleccione **departamentos** en el menú de la cuenta de facturación.
3. Haga clic en el nombre del departamento deseado.
4. Seleccione **propiedades** en el menú de departamento.
5. Copie los identificadores de cuenta y el departamento de facturación.
6. El ámbito es: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/departments/{departmentId}"`

### <a name="ea-enrollment-account"></a>Cuenta de inscripción de EA

1. Abra el portal de Azure y vaya a **Cost Management + facturación** en la lista de servicios.
2. Seleccione **las cuentas de inscripción** en el menú de la cuenta de facturación.
3. Haga clic en el nombre de la cuenta de inscripción deseado.
4. Seleccione **propiedades** en el menú de la cuenta de inscripción.
5. Copie la cuenta de facturación y los identificadores de cuenta de inscripción.
6. El ámbito es: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/enrollmentAccounts/{enrollmentAccountId}"`

### <a name="management-group"></a>Grupo de administración

1. Abra el portal de Azure y vaya a **grupos de administración** en la lista de servicios.
2. Desplácese hasta el grupo de administración que desea.
3. Copie el identificador de grupo de administración de la tabla.
4. El ámbito es: `"/providers/Microsoft.Management/managementGroups/{id}"`

### <a name="subscription"></a>Subscription

1. Abra el portal de Azure y vaya a **suscripciones** en la lista de servicios.
2. Copie el identificador de suscripción de la tabla.
3. El ámbito es: `"/subscriptions/{id}"`

### <a name="resource-groups"></a>Grupos de recursos

1. Abra el portal de Azure y vaya a **grupos de recursos** en la lista de servicios.
2. Haga clic en el nombre del grupo de recursos deseado.
3. Seleccione **propiedades** en el menú de grupo de recursos.
4. Copie el valor del campo de Id. de recurso.
5. El ámbito es: `"/subscriptions/{id}/resourceGroups/{name}"`

Administración de costos se admite actualmente en [Azure Global](https://management.azure.com) y [Azure Government](https://management.usgovcloudapi.net). Para obtener más información sobre Azure Government, consulte [puntos de conexión Global de Azure y API de gobierno](../azure-government/documentation-government-developer-guide.md#endpoint-mapping)_._

## <a name="next-steps"></a>Pasos siguientes

- Si aún no ha completado la primera guía rápida de Cost Management, léala en [Start analyzing costs](quick-acm-cost-analysis.md) (Comenzar a analizar los costos).

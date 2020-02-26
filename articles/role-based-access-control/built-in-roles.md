---
title: Roles integrados en los recursos de Azure | Microsoft Docs
description: En este tema se describen los roles integrados para el control de acceso basado en rol (RBAC) y los recursos de Azure. Se enumeran las acciones, notActions, dataActions y notDataActions.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/13/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: it-pro
ms.openlocfilehash: 9858f7ac25f2063e62dce0322f1859a0a7fcf83b
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198673"
---
# <a name="built-in-roles-for-azure-resources"></a>Roles integrados en los recursos de Azure

El [control de acceso basado en rol (RBAC)](overview.md) tiene varios roles integrados para recursos de Azure que se pueden asignar a usuarios, grupos, entidades de servicio e identidades administradas. Las asignaciones de roles sirven para controlar el acceso a los recursos de Azure. Si los roles integrados no cumplen las necesidades específicas de su organización, puede crear sus propios [roles personalizados para los recursos de Azure](custom-roles.md).

En este artículo se enumeran los roles integrados para los recursos de Azure, que están en constante evolución. Para obtener los últimos roles, use [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) o [az role definition list](/cli/azure/role/definition#az-role-definition-list). Si desea ver los roles de administrador de Azure Active Directory, consulte [Permisos de roles de administrador en Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="built-in-role-descriptions"></a>Descripciones de rol integrado

En la tabla siguiente se proporciona una breve descripción de cada rol integrado. Haga clic en el nombre del rol para ver la lista de `Actions`, `NotActions`, `DataActions` y `NotDataActions` para cada rol. Para obtener información sobre lo que significan estas acciones y cómo se aplican a la administración y a los planos de datos, consulte [Descripción de definiciones de roles para los recursos de Azure](role-definitions.md).


> [!div class="mx-tableFixed"]
> | Rol integrado | Descripción | Identificador |
> | --- | --- | --- |
> | [Propietario](#owner) | Permite administrarlo todo, incluso el acceso a los recursos. | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | [Colaborador](#contributor) | Permite administrarlo todo excepto la concesión de acceso a los recursos. | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | [Lector](#reader) | Permite ver todo el contenido, pero no realizar cambios. | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | [AcrDelete](#acrdelete) | Eliminar artefacto | c2f4ef07-c644-48eb-af81-4b1b4947fb11 |
> | [AcrImageSigner](#acrimagesigner) | Firmante de imagen de ACR | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | [AcrPull](#acrpull) | extracción de ACR | 7f951dda-4ed3-4680-a7ca-43fe172d538d |
> | [AcrPush](#acrpush) | inserción de ACR | 8311e382-0749-4cb8-b61a-304f252e45ec |
> | [AcrQuarantineReader](#acrquarantinereader) | Lector de datos de cuarentena de ACR | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | [AcrQuarantineWriter](#acrquarantinewriter) | Escritura de datos de cuarentena de ACR | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | [Colaborador de servicio de administración de API](#api-management-service-contributor) | Puede administrar servicios y las API. | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | [Rol del operador del servicio API Management](#api-management-service-operator-role) | Puede administrar el servicio, pero no las API. | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | [Rol de lector del servicio API Management](#api-management-service-reader-role) | Acceso de solo lectura al servicio y las API. | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | [Propietario de los datos de App Configuration](#app-configuration-data-owner) | Permite el acceso completo a los datos de App Configuration. | 5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b |
> | [Lector de los datos de App Configuration](#app-configuration-data-reader) | Permite el acceso de lectura a los datos de App Configuration. | 516239f1-63e1-4d78-a4de-a74fb236a071 |
> | [Colaborador de componentes de Application Insights](#application-insights-component-contributor) | Puede administrar los componentes de Application Insights | ae349356-3a1b-4a5e-921d-050484c6347e |
> | [Depurador de instantáneas de Application Insights](#application-insights-snapshot-debugger) | Concede permiso al usuario para ver y descargar las instantáneas de depuración que se recopilan con Snapshot Debugger de Application Insights. Tenga en cuenta que estos permisos no se incluyen en los roles [Propietario](#owner) ni [Colaborador](#contributor). | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | [Operador de trabajos de Automation](#automation-job-operator) | Permite crear y administrar trabajos con los runbooks de Automation. | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | [Operador de Automation](#automation-operator) | Los operadores de automatización pueden iniciar, detener, suspender y reanudar trabajos. | d3881f73-407a-4167-8283-e981cbba0404 |
> | [Operador de runbooks de Automation](#automation-runbook-operator) | Permite leer las propiedades de runbook para poder crear trabajos del runbook. | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | [Colaborador de Avere](#avere-contributor) | Puede crear y administrar un clúster de Avere vFXT. | 4f8fab4f-1852-4a58-a46a-8eaf358af14a |
> | [Operador de Avere](#avere-operator) | Lo usa el clúster de Avere vFXT para su administración. | c025889f-8102-4ebf-b32c-fc0c6f0c6bd9 |
> | [Incorporación de Azure Connected Machine](#azure-connected-machine-onboarding) | Puede incorporar máquinas conectadas a Azure. | b64e21ea-ac4e-4cdf-9dc9-5b892992bee7 |
> | [Administrador de recursos de Azure Connected Machine](#azure-connected-machine-resource-administrator) | Puede leer, escribir, eliminar y volver a incorporar máquinas conectadas a Azure. | cd570a14-e51a-42ad-bac8-bafd67325302 |
> | [Propietario de los datos de Azure Event Hubs](#azure-event-hubs-data-owner) | Concede acceso total a los recursos de Azure Event Hubs. | f526a384-b230-433a-b45c-95f59c4a2dec |
> | [Receptor de datos de Azure Event Hubs](#azure-event-hubs-data-receiver) | Concede acceso de recepción a los recursos de Azure Event Hubs. | a638d3c7-ab3a-418d-83e6-5f17a39d4fde |
> | [Emisor de datos de Azure Event Hubs](#azure-event-hubs-data-sender) | Concede acceso de emisión a los recursos de Azure Event Hubs. | 2b629674-e913-4c01-ae53-ef4638d8f975 |
> | [Rol de administrador de clúster de Azure Kubernetes Service](#azure-kubernetes-service-cluster-admin-role) | Enumerar la acción de credenciales administrativas del clúster. | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | [Rol de usuario de clúster de Azure Kubernetes Service](#azure-kubernetes-service-cluster-user-role) | Enumerar la acción de credenciales de usuario del clúster. | 4abbcc35-e782-43D8-92c5-2d3f1bd2253f |
> | [Lector de datos de Azure Maps (versión preliminar)](#azure-maps-data-reader-preview) | Concede acceso de lectura a los datos de los mapas de una cuenta de Azure Maps. | 423170ca-a8f6-4b0f-8487-9e4eb8f49bfa |
> | [Colaborador de Azure Sentinel](#azure-sentinel-contributor) | Colaborador de Azure Sentinel | ab8e14d6-4a74-4a29-9ba8-549422addade |
> | [Lector de Azure Sentinel](#azure-sentinel-reader) | Lector de Azure Sentinel | 8d289c81-5878-46d4-8554-54e1e3d8b5cb |
> | [Respondedor de Azure Sentinel](#azure-sentinel-responder) | Respondedor de Azure Sentinel | 3e150937-b8fe-4cfb-8069-0eaf05ecd056 |
> | [Propietario de los datos de Azure Service Bus](#azure-service-bus-data-owner) | Concede acceso total a los recursos de Azure Service Bus. | 090c5cfd-751d-490a-894a-3ce6f1109419 |
> | [Receptor de datos de Azure Service Bus](#azure-service-bus-data-receiver) | Concede acceso de recepción a los recursos de Azure Service Bus. | 4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0 |
> | [Emisor de datos de Azure Service Bus](#azure-service-bus-data-sender) | Concede acceso de emisión a los recursos de Azure Service Bus. | 69a216fc-b8fb-44d8-bc22-1f3c2cd27a39 |
> | [Propietario del registro de Azure Stack](#azure-stack-registration-owner) | Permite administrar los registros de Azure Stack. | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | [Colaborador de copias de seguridad](#backup-contributor) | Permite administrar el servicio de copias de seguridad, pero no puede crear almacenes ni conceder acceso a otros usuarios | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | [Operador de copias de seguridad](#backup-operator) | Permite administrar los servicios de copias de seguridad, excepto la eliminación de copias de seguridad, la creación de almacenes y la concesión de acceso a otros usuarios | 00c29273-979b-4161-815c-10b084fb9324 |
> | [Lector de copias de seguridad](#backup-reader) | Puede ver servicios de copia de seguridad, pero no puede realizar cambios. | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | [Lector de facturación](#billing-reader) | Permite acceso de lectura a los datos de facturación. | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | [Colaborador de BizTalk](#biztalk-contributor) | Permite administrar los servicios de BizTalk, pero no acceder a ellos. | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | [Acceso al nodo de miembro de la cadena de bloques (versión preliminar)](#blockchain-member-node-access-preview) | Permite acceder a los nodos de miembro de la cadena de bloques. | 31a002a1-acaf-453e-8a5b-297c9ca1ea24 |
> | [Colaborador de plano técnico](#blueprint-contributor) | Puede administrar las definiciones del plano técnico, pero no asignarlas. | 41077137-e803-4205-871c-5a86e6a753b4 |
> | [Operador del plano técnico](#blueprint-operator) | Puede asignar los planos técnicos publicados existentes, pero no puede crear nuevos. Nota: Esto solo funciona si la asignación se realiza con una identidad administrada asignada por el usuario. | 437d2ced-4a38-4302-8479-ed2bcb43d090 |
> | [Colaborador de punto de conexión de CDN](#cdn-endpoint-contributor) | Puede administrar puntos de conexión de CDN, pero no conceder acceso a otros usuarios. | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | [Lector de punto de conexión de CDN](#cdn-endpoint-reader) | Puede ver puntos de conexión de CDN, pero no hacer cambios. | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | [Colaborador de perfil de CDN](#cdn-profile-contributor) | Puede administrar perfiles de CDN y sus puntos de conexión, pero no conceder acceso a otros usuarios. | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | [Lector de perfil de CDN](#cdn-profile-reader) | Puede ver perfiles de CDN y sus puntos de conexión, pero no hacer cambios. | 8f96442b-4075-438f-813d-ad51ab4019af |
> | [Colaborador de la red clásica](#classic-network-contributor) | Permite administrar las redes clásicas, pero no acceder a ellas. | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | [Colaborador de cuentas de almacenamiento clásico](#classic-storage-account-contributor) | Permite administrar cuentas de almacenamiento clásicas, pero no acceder a ellas. | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | [Rol de servicio de operador de claves de cuentas de almacenamiento clásicas](#classic-storage-account-key-operator-service-role) | Los operadores de claves de cuentas de almacenamiento clásicas pueden enumerar y regenerar claves en cuentas de almacenamiento clásicas | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | [Colaborador de la máquina virtual clásica](#classic-virtual-machine-contributor) | Permite administrar máquinas virtuales clásicas, pero no acceder a ellas, ni tampoco a la red virtual ni a la cuenta de almacenamiento a las que están conectadas. | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | [Colaborador de Cognitive Services](#cognitive-services-contributor) | Le permite crear, leer, actualizar, eliminar y administrar las claves de Cognitive Services. | 25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68 |
> | [Lector de datos de Cognitive Services (versión preliminar)](#cognitive-services-data-reader-preview) | Permite leer los datos de Cognitive Services. | b59867f0-fa02-499b-be73-45a86b5b3e1c |
> | [Usuario de Cognitive Services](#cognitive-services-user) | Le permite leer y mostrar las claves de Cognitive Services. | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | [Rol de lector de cuentas de Cosmos DB](#cosmos-db-account-reader-role) | Puede leer los datos de cuentas de Azure Cosmos DB. Vea [Colaborador de cuenta de DocumentDB](#documentdb-account-contributor) para administrar cuentas de Azure Cosmos DB. | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | [Operador de Cosmos DB](#cosmos-db-operator) | Permite administrar las cuentas de Azure Cosmos DB, pero no acceder a los datos que contienen. Evita el acceso a las claves de cuenta y a las cadenas de conexión. | 230815da-be43-4aae-9cb4-875f7bd000aa |
> | [CosmosBackupOperator](#cosmosbackupoperator) | Puede enviar una solicitud de restauración para una base de datos de Cosmos DB o un contenedor de una cuenta | db7b14f2-5adf-42da-9f96-f2ee17bab5cb |
> | [Colaborador de Cost Management](#cost-management-contributor) | Puede ver los costos y administrar la configuración de estos (por ejemplo, presupuestos, exportaciones) | 434105ed-43f6-45c7-a02f-909b2ba83430 |
> | [Lector de Cost Management](#cost-management-reader) | Puede ver los datos de costo y la configuración (por ejemplo, presupuestos, exportaciones) | 72fafb9e-0641-4937-9268-a91bfd8191a3 |
> | [Colaborador de Data Box](#data-box-contributor) | Permite administrarlo todo en el servicio Data Box, excepto dar acceso a otros usuarios. | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | [Lector de Data Box](#data-box-reader) | Permite administrar el servicio Data Box excepto la creación o edición de detalles de pedido y dar acceso a otros usuarios. | 028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027 |
> | [Colaborador de Factoría de datos](#data-factory-contributor) | Crea y administra factorías de datos, así como recursos secundarios dentro de ellas. | 673868aa-7521-48a0-acc6-0f60742d39f5 |
> | [Desarrollador de Data Lake Analytics](#data-lake-analytics-developer) | Le permite enviar, supervisar y administrar sus propios trabajos, pero no crear ni eliminar cuentas de Data Lake Analytics. | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | [Purgador de datos](#data-purger) | Puede purgar datos de análisis. | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | [Usuario de DevTest Labs](#devtest-labs-user) | Permite conectarse a sus máquinas virtuales, así como iniciarlas, reiniciarlas y apagarlas, en su instancia de Azure DevTest Labs. | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | [Colaborador de zona DNS](#dns-zone-contributor) | Permite administrar zonas y conjuntos de registros DNS en Azure DNS, pero no controlar los usuarios que tienen acceso. | befefa01-2a29-4197-83a8-272ff33ce314 |
> | [Colaborador de cuenta de DocumentDB](#documentdb-account-contributor) | Puede administrar cuentas de Azure Cosmos DB. Azure Cosmos DB se llamaba anteriormente DocumentDB. | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | [Colaborador de EventGrid EventSubscription](#eventgrid-eventsubscription-contributor) | Permite administrar las operaciones de suscripción de eventos de EventGrid. | 428e0ff0-5e57-4d9c-a221-2c70d0e0a443 |
> | [Lector de EventGrid EventSubscription](#eventgrid-eventsubscription-reader) | Permite leer las suscripciones de eventos de EventGrid. | 2414bbcf-6497-4faf-8c65-045460748405 |
> | [Operador de clústeres de HDInsight](#hdinsight-cluster-operator) | Permite leer y modificar las configuraciones de clúster de HDInsight. | 61ed4efc-Fab3-44fd-b111-e24485cc132a |
> | [Colaborador de Domain Services para HDInsight](#hdinsight-domain-services-contributor) | Puede leer, crear, modificar y eliminar operaciones relacionadas con Domain Services para HDInsight Enterprise Security Package | 8d8d5a11-05d3-4bda-a417-a08778121c7c |
> | [Colaborador de la cuenta de Sistemas inteligentes](#intelligent-systems-account-contributor) | Permite administrar las cuentas de Intelligent Systems, pero no acceder a ellas. | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | [Colaborador de almacén de claves](#key-vault-contributor) | Le permite administrar almacenes de claves, pero no acceder a ellos. | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | [Creador de laboratorio](#lab-creator) | Le permite crear, administrar y eliminar los laboratorios administrados en sus cuentas de Azure Lab. | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | [Colaborador de Log Analytics](#log-analytics-contributor) | Un colaborador de Log Analytics puede leer todos los datos de supervisión y editar la configuración de supervisión. La edición de la configuración de supervisión incluye la posibilidad de añadir la extensión de máquina virtual a las máquinas virtuales, leer las claves de las cuentas de almacenamiento para poder configurar la recopilación de registros de Azure Storage, crear y configurar cuentas de Automation, añadir soluciones y configurar Azure Diagnostics en todos los recursos de Azure. | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | [Lector de Log Analytics](#log-analytics-reader) | Un lector de Log Analytics puede ver y buscar todos los datos de supervisión, así como consultar la configuración de supervisión, incluida la de Azure Diagnostics en todos los recursos de Azure. | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | [Colaborador de aplicación lógica](#logic-app-contributor) | Le permite administrar aplicaciones lógicas, pero no cambiar el acceso a ellas. | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | [Operador de aplicación lógica](#logic-app-operator) | Le permite leer, habilitar y deshabilitar aplicaciones lógicas, pero no permite editarlas ni actualizarlas. | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | [Rol de operador de aplicación administrada](#managed-application-operator-role) | Permite leer y realizar acciones en los recursos de aplicación administrada. | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | [Lector de aplicaciones administradas](#managed-applications-reader) | Le permite leer los recursos de una aplicación administrada y solicitar acceso JIT. | b9331d33-8a36-4f8c-b097-4f54124fdb44 |
> | [Colaborador de identidad administrada](#managed-identity-contributor) | Le permite crear, leer, actualizar y eliminar identidades asignadas por el usuario. | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | [Operador de identidad administrada](#managed-identity-operator) | Le permite leer y asignar identidades asignadas por el usuario. | f1a07417-d97a-45cb-824c-7a7467783830 |
> | [Rol para eliminar la asignación de registros de servicios administrados](#managed-services-registration-assignment-delete-role) | El rol para eliminar la asignación de registros de servicios administrados permite que los usuarios que administran el inquilino eliminen la asignación de registros asignada a su inquilino. | 91c1777a-f3dc-4fae-b103-61d183457e46 |
> | [Colaborador de grupo de administración](#management-group-contributor) | Rol de colaborador de grupo de administración | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | [Lector de grupo de administración](#management-group-reader) | Rol de lector de grupo de administración | ac63b705-f282-497d-ac71-919bf39d939d |
> | [Colaborador de supervisión](#monitoring-contributor) | Puede leer todos los datos de supervisión y editar la configuración de supervisión. Consulte también [Introducción a roles, permisos y seguridad con Azure Monitor](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | [Supervisión del publicador de métricas](#monitoring-metrics-publisher) | Permite publicar las métricas de los recursos de Azure. | 3913510d-42f4-4e42-8a64-420c390055eb |
> | [Lector de supervisión](#monitoring-reader) | Puede leer todos los datos de supervisión (métricas, registros, etc.). Consulte también [Introducción a roles, permisos y seguridad con Azure Monitor](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | [Colaborador de la red](#network-contributor) | Permite administrar redes, pero no acceder a ellas. | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | [Colaborador de la cuenta de NewRelic APM](#new-relic-apm-account-contributor) | Le permite administrar las aplicaciones y cuentas de Application Performance Management de New Relic, pero no acceder a ellas. | 5d28c62d-5b37-4476-8438-e587778df237 |
> | [Escritor de datos de Policy Insights (versión preliminar)](#policy-insights-data-writer-preview) | Permite el acceso de lectura a las directivas de los recursos y el acceso de escritura a los eventos de directiva de los componentes de los recursos. | 66bb4e9e-b016-4a94-8249-4c0511c2be84 |
> | [Lector y acceso a los datos](#reader-and-data-access) | Permite ver todo el contenido, pero no eliminar ni crear una cuenta de almacenamiento ni un recurso incluido. También permitirá el acceso de lectura o escritura para todos los datos incluidos en una cuenta de almacenamiento a través del acceso a las claves de la cuenta de almacenamiento. | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | [Colaborador de la memoria caché de Redis](#redis-cache-contributor) | Permite administrar cachés de Redis, pero no acceder a ellas. | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | [Colaborador de directivas de recursos](#resource-policy-contributor) | Los usuarios con derechos para crear o modificar la directiva de recursos pueden crear solicitudes de soporte técnico y leer los recursos o la jerarquía. | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | [Colaborador de colecciones de trabajos de Scheduler](#scheduler-job-collections-contributor) | Permite administrar colecciones de trabajos de Scheduler, pero no acceder a ellas. | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |
> | [Colaborador del servicio Search](#search-service-contributor) | Permite administrar los servicios de Búsqueda, pero no acceder a ellos. | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | [Administrador de seguridad](#security-admin) | Solo en Security Center: puede ver las directivas de seguridad, los estados de seguridad, editar las directivas de seguridad, ver alertas y recomendaciones, y descartar alertas y recomendaciones. | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | [Administrador de seguridad (heredado)](#security-manager-legacy) | Se trata de un rol heredado. En su lugar, use el Administrador de seguridad. | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | [Lector de seguridad](#security-reader) | Solo en Security Center: puede ver las recomendaciones y alertas, ver las directivas de seguridad y ver los estados de seguridad, pero no puede realizar cambios | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | [Colaborador de Site Recovery](#site-recovery-contributor) | Permite administrar el servicio Site Recovery, excepto la creación de almacenes y la asignación de roles. | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | [Operador de Site Recovery](#site-recovery-operator) | Permite realizar una conmutación por error o una conmutación por recuperación, pero no otras operaciones de administración de Site Recovery. | 494ae006-db33-4328-bf46-533a6560a3ca |
> | [Lector de Site Recovery](#site-recovery-reader) | Permite visualizar el estado de Site Recovery, pero no realizar otras operaciones de administración. | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | [Colaborador de la cuenta de Spatial Anchors](#spatial-anchors-account-contributor) | Permite administrar los anclajes espaciales en su cuenta, pero no eliminarlos. | 8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827 |
> | [Propietario de la cuenta de Spatial Anchors](#spatial-anchors-account-owner) | Permite administrar los anclajes espaciales en su cuenta y eliminarlos. | 70bbe301-9835-447d-afdd-19eb3167307c |
> | [Lector de la cuenta de Spatial Anchors](#spatial-anchors-account-reader) | Permite encontrar y leer propiedades de los anclajes espaciales en la cuenta. | 5d51204f-eb77-4b1c-b86a-2ec626c49413 |
> | [Colaborador de Base de datos de SQL](#sql-db-contributor) | Permite administrar las bases de datos de SQL, pero no acceder a ellas. Además, no puede administrar sus directivas relacionadas con la seguridad ni los servidores SQL primarios. | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | [Colaborador de Instancia administrada de SQL](#sql-managed-instance-contributor) | Permite administrar las instancias administradas de SQL y requiere configuración de red, pero no puede conceder acceso a otros usuarios. | 4939a1f6-9ae0-4e48-a1e0-f2cbe897382d |
> | [Administrador de seguridad SQL](#sql-security-manager) | Permite administrar las directivas relacionadas con seguridad de bases de datos y servidores SQL, pero no acceder a ellas. | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | [Colaborador de SQL Server](#sql-server-contributor) | Permite administrar bases de datos y servidores SQL, pero no acceder a ellos, ni a sus directivas relacionadas con la seguridad. | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | [Colaborador de la cuenta de almacenamiento](#storage-account-contributor) | Permite la administración de cuentas de almacenamiento. Proporciona acceso a la clave de cuenta, que puede usarse para tener acceso a datos a través de la autorización de clave compartida. | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | [Rol de servicio de operador de claves de cuentas de almacenamiento](#storage-account-key-operator-service-role) | Permite enumerar y regenerar claves de acceso de la cuenta de almacenamiento. | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | [Colaborador de datos de blobs de almacenamiento](#storage-blob-data-contributor) | Lee, escribe y elimina blobs y contenedores de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | [Propietario de datos de blobs de almacenamiento](#storage-blob-data-owner) | Proporciona acceso total a los contenedores de blobs y los datos de Azure Storage, incluida la asignación de control de acceso POSIX. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | b7e6dc6d-f1e8-4753-8033-0f276bb0955b |
> | [Lector de datos de blobs de almacenamiento](#storage-blob-data-reader) | Lee y enumera blobs y contenedores de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | [Delegador de Blob Storage](#storage-blob-delegator) | Obtiene una clave de delegación de usuarios, que se puede usar a continuación para crear una firma de acceso compartido para un contenedor o un blob firmado con credenciales de Azure AD. Para más información, vea [Creación de SAS de delegación de usuarios](https://docs.microsoft.com/rest/api/storageservices/create-user-delegation-sas). | db58b8e5-c6ad-4a2a-8342-4190687cbf4a |
> | [Colaborador de recursos compartidos de SMB de datos de archivos de Storage](#storage-file-data-smb-share-contributor) | Permite el acceso de lectura, escritura y eliminación a los archivos y directorios de los recursos compartidos de Azure. Este rol no tiene ningún equivalente integrado en los servidores de archivos de Windows. | 0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb |
> | [Colaborador elevado de recursos compartidos de SMB de datos de archivos de Storage](#storage-file-data-smb-share-elevated-contributor) | Permite el acceso de lectura, escritura, eliminación y modificación de ACL en los archivos y directorios de los recursos compartidos de Azure. Este rol es equivalente a una ACL de recurso compartido de cambio en los servidores de archivos de Windows. | a7264617-510b-434b-a828-9731dc254ea7 |
> | [Lector de recursos compartidos de SMB de datos de archivos de Storage](#storage-file-data-smb-share-reader) | Permite el acceso de lectura a los archivos y directorios de los recursos compartidos de Azure. Este rol es equivalente a una ACL de recurso compartido de lectura en los servidores de archivos de Windows. | aba4ae5f-2193-4029-9191-0cb91df5e314 |
> | [Colaborador de datos de la cola de Storage](#storage-queue-data-contributor) | Lee, escribe y elimina los mensajes de la cola y a la cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | [Procesador de mensajes de datos de la cola de Storage](#storage-queue-data-message-processor) | Consulta, recupera y elimina un mensaje de una cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | 8a0f0c08-91a1-4084-bc3d-661d67233fed |
> | [Emisor de mensajes de datos de la cola de Storage](#storage-queue-data-message-sender) | Agrega mensaje a una cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | c6a89b2d-59bc-44d0-9896-0f6e12d7b80a |
> | [Lector de datos de la cola de Storage](#storage-queue-data-reader) | Lee y enumera los mensajes de la cola y las colas de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | 19e7f393-937e-4f77-808e-94535e297925 |
> | [Colaborador de la solicitud de soporte técnico](#support-request-contributor) | Permite crear y administrar solicitudes de soporte técnico. | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | [Colaborador de Traffic Manager](#traffic-manager-contributor) | Le permite administrar perfiles de Traffic Manager, pero no controlar los usuarios que tienen acceso a ellos. | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | [Administrador de acceso de usuario](#user-access-administrator) | Permite administrar el acceso de usuario a los recursos de Azure. | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | [Inicio de sesión de administrador de Virtual Machine](#virtual-machine-administrator-login) | Visualización de máquinas virtuales en el portal e inicio de sesión como administrador | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | [Colaborador de la máquina virtual](#virtual-machine-contributor) | Permite administrar máquinas virtuales, pero no acceder a ellas, ni tampoco a la red virtual ni la cuenta de almacenamiento a las que están conectadas. | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | [Inicio de sesión de usuario de Virtual Machine](#virtual-machine-user-login) | Visualización de máquinas virtuales en el portal e inicio de sesión como usuario normal. | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | [Colaborador de plan web](#web-plan-contributor) | Permite administrar los planes web para sitios web, pero no acceder a ellos. | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | [Colaborador de sitio web](#website-contributor) | Permite administrar los sitios web (no planes web), pero no acceder a ellos. | de139f84-1756-47ae-9be6-808fbbe84772 |


## <a name="owner"></a>Propietario
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrarlo todo, incluso el acceso a los recursos. |
> | **Id** | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | **Acciones** |  |
> | * | Crear y administrar recursos de todos los tipos |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="contributor"></a>Colaborador
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrarlo todo excepto la concesión de acceso a los recursos. |
> | **Id** | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | **Acciones** |  |
> | * | Crear y administrar recursos de todos los tipos |
> | **NotActions** |  |
> | Microsoft.Authorization/*/Delete | Eliminar roles, asignaciones de directivas, definiciones de directiva y definiciones del conjunto de directivas |
> | Microsoft.Authorization/*/Write | Crear roles, asignaciones de roles, asignaciones de directivas, definiciones de directiva y definiciones del conjunto de directivas |
> | Microsoft.Authorization/elevateAccess/Action | Concede al llamador acceso de administrador de acceso de usuario en el ámbito de inquilinos |
> | Microsoft.Blueprint/blueprintAssignments/write | Crear o actualizar cualquier asignación de planos técnicos |
> | Microsoft.Blueprint/blueprintAssignments/delete | Eliminar cualquier asignación de planos técnicos |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="reader"></a>Lector
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite ver todo el contenido, pero no realizar cambios. |
> | **Id** | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | **Acciones** |  |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="acrdelete"></a>AcrDelete
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Eliminar artefacto |
> | **Id** | c2f4ef07-c644-48eb-af81-4b1b4947fb11 |
> | **Acciones** |  |
> | Microsoft.ContainerRegistry/registries/artifacts/delete | Eliminar artefacto de un registro de contenedor. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="acrimagesigner"></a>AcrImageSigner
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Firmante de imagen de ACR |
> | **Id** | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | **Acciones** |  |
> | Microsoft.ContainerRegistry/registries/sign/write | Inserta o extrae metadatos de confianza en el contenido para un registro de contenedor. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="acrpull"></a>AcrPull
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | extracción de ACR |
> | **Id** | 7f951dda-4ed3-4680-a7ca-43fe172d538d |
> | **Acciones** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | Extrae u obtiene imágenes de un registro de contenedor. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="acrpush"></a>AcrPush
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | inserción de ACR |
> | **Id** | 8311e382-0749-4cb8-b61a-304f252e45ec |
> | **Acciones** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | Extrae u obtiene imágenes de un registro de contenedor. |
> | Microsoft.ContainerRegistry/registries/push/write | Inserta o escribe imágenes en un registro de contenedor. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="acrquarantinereader"></a>AcrQuarantineReader
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Lector de datos de cuarentena de ACR |
> | **Id** | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | **Acciones** |  |
> | Microsoft.ContainerRegistry/registries/quarantine/read | Extrae u obtiene imágenes en cuarentena de un registro de contenedor |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="acrquarantinewriter"></a>AcrQuarantineWriter
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Escritura de datos de cuarentena de ACR |
> | **Id** | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | **Acciones** |  |
> | Microsoft.ContainerRegistry/registries/quarantine/read | Extrae u obtiene imágenes en cuarentena de un registro de contenedor |
> | Microsoft.ContainerRegistry/registries/quarantine/write | Escribe o modifica el estado de cuarentena de las imágenes que estén en cuarentena |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="api-management-service-contributor"></a>Colaborador de servicio de administración de API
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede administrar servicios y las API. |
> | **Id** | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | **Acciones** |  |
> | Microsoft.ApiManagement/service/* | Crear y administrar servicio API Management |
> | Microsoft.Authorization/*/read | Leer autorización |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="api-management-service-operator-role"></a>Rol del operador del servicio API Management
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede administrar el servicio, pero no las API. |
> | **Id** | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | **Acciones** |  |
> | Microsoft.ApiManagement/service/*/read | Lectura de instancias del servicio API Management |
> | Microsoft.ApiManagement/service/backup/action | Realiza una copia de seguridad del servicio API Management en el contenedor especificado de una cuenta de almacenamiento proporcionada por el usuario |
> | Microsoft.ApiManagement/service/delete | Elimina una instancia del servicio API Management |
> | Microsoft.ApiManagement/service/managedeployments/action | Cambia SKU y unidades, y agrega o quita las implementaciones regionales del servicio API Management |
> | Microsoft.ApiManagement/service/read | Lectura de los metadatos de una instancia del servicio API Management |
> | Microsoft.ApiManagement/service/restore/action | Restauración del servicio API Management desde el contenedor especificado de una cuenta de almacenamiento proporcionada por el usuario |
> | Microsoft.ApiManagement/service/updatecertificate/action | Carga el certificado SSL de un servicio API Management |
> | Microsoft.ApiManagement/service/updatehostname/action | Configura, actualiza o elimina los nombres de dominio personalizado de un servicio API Management |
> | Microsoft.ApiManagement/service/write | Creación de una nueva instancia del servicio API Management |
> | Microsoft.Authorization/*/read | Leer autorización |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Obtener las claves asociadas con el usuario. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="api-management-service-reader-role"></a>Rol de lector del servicio API Management
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Acceso de solo lectura al servicio y las API. |
> | **Id** | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | **Acciones** |  |
> | Microsoft.ApiManagement/service/*/read | Lectura de instancias del servicio API Management |
> | Microsoft.ApiManagement/service/read | Lectura de los metadatos de una instancia del servicio API Management |
> | Microsoft.Authorization/*/read | Leer autorización |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Obtener las claves asociadas con el usuario. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="app-configuration-data-owner"></a>Propietario de los datos de App Configuration
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite el acceso completo a los datos de App Configuration. |
> | **Id** | 5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.AppConfiguration/configurationStores/*/read |  |
> | Microsoft.AppConfiguration/configurationStores/*/write |  |
> | Microsoft.AppConfiguration/configurationStores/*/delete |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="app-configuration-data-reader"></a>Lector de los datos de App Configuration
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite el acceso de lectura a los datos de App Configuration. |
> | **Id** | 516239f1-63e1-4d78-a4de-a74fb236a071 |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.AppConfiguration/configurationStores/*/read |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="application-insights-component-contributor"></a>Colaborador de componentes de Application Insights
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede administrar los componentes de Application Insights |
> | **Id** | ae349356-3a1b-4a5e-921d-050484c6347e |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas clásicas |
> | Microsoft.Insights/metricAlerts/* | Crear y administrar nuevas reglas de alertas |
> | Microsoft.Insights/components/* | Crear y administrar componentes de Insights |
> | Microsoft.Insights/webtests/* | Crear y administrar pruebas web |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="application-insights-snapshot-debugger"></a>Depurador de instantáneas de Application Insights
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Concede permiso al usuario para ver y descargar las instantáneas de depuración que se recopilan con Snapshot Debugger de Application Insights. Tenga en cuenta que estos permisos no se incluyen en los roles [Propietario](#owner) ni [Colaborador](#contributor). |
> | **Id** | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="automation-job-operator"></a>Operador de trabajos de Automation
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite crear y administrar trabajos con los runbooks de Automation. |
> | **Id** | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Lee los recursos de Hybrid Runbook Worker |
> | Microsoft.Automation/automationAccounts/jobs/read | Obtiene un trabajo de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Reanuda un trabajo de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Detiene un trabajo de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Obtiene un flujo de trabajos de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Suspende un trabajo de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/write | Crea un trabajo de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Obtiene la salida de un trabajo |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="automation-operator"></a>Operador de Automation
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Los operadores de automatización pueden iniciar, detener, suspender y reanudar trabajos. |
> | **Id** | d3881f73-407a-4167-8283-e981cbba0404 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Lee los recursos de Hybrid Runbook Worker |
> | Microsoft.Automation/automationAccounts/jobs/read | Obtiene un trabajo de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Reanuda un trabajo de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Detiene un trabajo de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Obtiene un flujo de trabajos de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Suspende un trabajo de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobs/write | Crea un trabajo de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobSchedules/read | Obtiene una programación de trabajos de Azure Automation |
> | Microsoft.Automation/automationAccounts/jobSchedules/write | Crea una programación de trabajos de Azure Automation |
> | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Obtiene el área de trabajo vinculada a la cuenta de Automation. |
> | Microsoft.Automation/automationAccounts/read | Obtiene una cuenta de Azure Automation |
> | Microsoft.Automation/automationAccounts/runbooks/read | Obtiene un runbook de Azure Automation |
> | Microsoft.Automation/automationAccounts/schedules/read | Obtiene un recurso de programación de Azure Automation |
> | Microsoft.Automation/automationAccounts/schedules/write | Crea o actualiza un recurso de programación de Azure Automation |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Obtiene la salida de un trabajo |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="automation-runbook-operator"></a>Operador de runbooks de Automation
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite leer las propiedades de runbook para poder crear trabajos del runbook. |
> | **Id** | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Automation/automationAccounts/runbooks/read | Obtiene un runbook de Azure Automation |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="avere-contributor"></a>Colaborador de Avere
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede crear y administrar un clúster de Avere vFXT. |
> | **Id** | 4f8fab4f-1852-4a58-a46a-8eaf358af14a |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Compute/*/read |  |
> | Microsoft.Compute/availabilitySets/* |  |
> | Microsoft.Compute/virtualMachines/* |  |
> | Microsoft.Compute/disks/* |  |
> | Microsoft.Network/*/read |  |
> | Microsoft.Network/networkInterfaces/* |  |
> | Microsoft.Network/virtualNetworks/read | Obtiene la definición de red virtual |
> | Microsoft.Network/virtualNetworks/subnets/read | Obtiene una definición de subred de red virtual |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Se une a una red virtual. No genera alertas. |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Combina un recurso como una cuenta de almacenamiento o una instancia de SQL Database con una subred. No genera alertas. |
> | Microsoft.Network/networkSecurityGroups/join/action | Se une a un grupo de seguridad de red. No genera alertas. |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Storage/*/read |  |
> | Microsoft.Storage/storageAccounts/* |  |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Resources/subscriptions/resourceGroups/resources/read | Obtiene los recursos del grupo de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Devuelve el resultado de la eliminación de un blob. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Devuelve un blob o una lista de blobs. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Devuelve el resultado de la escritura de un blob. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="avere-operator"></a>Operador de Avere
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Lo usa el clúster de Avere vFXT para su administración. |
> | **Id** | c025889f-8102-4ebf-b32c-fc0c6f0c6bd9 |
> | **Acciones** |  |
> | Microsoft.Compute/virtualMachines/read | Obtiene las propiedades de una máquina virtual |
> | Microsoft.Network/networkInterfaces/read | Obtiene una definición de interfaz de red.  |
> | Microsoft.Network/networkInterfaces/write | Crea una interfaz de red o actualiza una interfaz de red existente.  |
> | Microsoft.Network/virtualNetworks/read | Obtiene la definición de red virtual |
> | Microsoft.Network/virtualNetworks/subnets/read | Obtiene una definición de subred de red virtual |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Se une a una red virtual. No genera alertas. |
> | Microsoft.Network/networkSecurityGroups/join/action | Se une a un grupo de seguridad de red. No genera alertas. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Devuelve el resultado de la eliminación de un contenedor. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Devuelve una lista de contenedores. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Devuelve el resultado del contenedor de blobs de colocación. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Devuelve el resultado de la eliminación de un blob. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Devuelve un blob o una lista de blobs. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Devuelve el resultado de la escritura de un blob. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-connected-machine-onboarding"></a>Incorporación de Azure Connected Machine
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede incorporar máquinas conectadas a Azure. |
> | **Id** | b64e21ea-ac4e-4cdf-9dc9-5b892992bee7 |
> | **Acciones** |  |
> | Microsoft.HybridCompute/machines/read | Leer cualquier máquina de Azure Arc |
> | Microsoft.HybridCompute/machines/write | Escribir una máquina de Azure Arc |
> | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | Obtiene la asignación de configuración de invitado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-connected-machine-resource-administrator"></a>Administrador de recursos de Azure Connected Machine
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede leer, escribir, eliminar y volver a incorporar máquinas conectadas a Azure. |
> | **Id** | cd570a14-e51a-42ad-bac8-bafd67325302 |
> | **Acciones** |  |
> | Microsoft.HybridCompute/machines/read | Leer cualquier máquina de Azure Arc |
> | Microsoft.HybridCompute/machines/write | Escribir una máquina de Azure Arc |
> | Microsoft.HybridCompute/machines/delete | Eliminar una máquina de Azure Arc |
> | Microsoft.HybridCompute/machines/reconnect/action | Volver a conectar una máquina de Azure Arc |
> | Microsoft.HybridCompute/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-event-hubs-data-owner"></a>Propietario de los datos de Azure Event Hubs
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Concede acceso total a los recursos de Azure Event Hubs. |
> | **Id** | f526a384-b230-433a-b45c-95f59c4a2dec |
> | **Acciones** |  |
> | Microsoft.EventHub/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.EventHub/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-event-hubs-data-receiver"></a>Receptor de datos de Azure Event Hubs
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Concede acceso de recepción a los recursos de Azure Event Hubs. |
> | **Id** | a638d3c7-ab3a-418d-83e6-5f17a39d4fde |
> | **Acciones** |  |
> | Microsoft.EventHub/*/eventhubs/consumergroups/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.EventHub/*/receive/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-event-hubs-data-sender"></a>Emisor de datos de Azure Event Hubs
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Concede acceso de emisión a los recursos de Azure Event Hubs. |
> | **Id** | 2b629674-e913-4c01-ae53-ef4638d8f975 |
> | **Acciones** |  |
> | Microsoft.EventHub/*/eventhubs/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.EventHub/*/send/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-kubernetes-service-cluster-admin-role"></a>Rol de administrador de clúster de Azure Kubernetes Service
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Enumerar la acción de credenciales administrativas del clúster. |
> | **Id** | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | **Acciones** |  |
> | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Muestra la credencial clusterAdmin de un clúster administrado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-kubernetes-service-cluster-user-role"></a>Rol de usuario de clúster de Azure Kubernetes Service
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Enumerar la acción de credenciales de usuario del clúster. |
> | **Id** | 4abbcc35-e782-43D8-92c5-2d3f1bd2253f |
> | **Acciones** |  |
> | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Muestra la credencial clusterUser de un clúster administrado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-maps-data-reader-preview"></a>Azure Maps Data Reader (versión preliminar)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Concede acceso de lectura a los datos de los mapas de una cuenta de Azure Maps. |
> | **Id** | 423170ca-a8f6-4b0f-8487-9e4eb8f49bfa |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Maps/accounts/data/read | Concede acceso de lectura de datos a una cuenta de Maps. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-sentinel-contributor"></a>Colaborador de Azure Sentinel
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Colaborador de Azure Sentinel |
> | **Id** | ab8e14d6-4a74-4a29-9ba8-549422addade |
> | **Acciones** |  |
> | Microsoft.SecurityInsights/* |  |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Realiza búsquedas mediante el nuevo motor. |
> | Microsoft.OperationalInsights/workspaces/read | Obtiene un área de trabajo existente |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* |  |
> | Microsoft.OperationsManagement/solutions/read | Obtiene una solución OMS ya existente |
> | Microsoft.OperationalInsights/workspaces/query/read | Ejecuta consultas en los datos del área de trabajo. |
> | Microsoft.OperationalInsights/workspaces/query/*/read |  |
> | Microsoft.OperationalInsights/workspaces/dataSources/read | Obtiene los orígenes de datos en un área de trabajo. |
> | Microsoft.Insights/workbooks/* |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-sentinel-reader"></a>Lector de Azure Sentinel
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Lector de Azure Sentinel |
> | **Id** | 8d289c81-5878-46d4-8554-54e1e3d8b5cb |
> | **Acciones** |  |
> | Microsoft.SecurityInsights/*/read |  |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Realiza búsquedas mediante el nuevo motor. |
> | Microsoft.OperationalInsights/workspaces/read | Obtiene un área de trabajo existente |
> | Microsoft.OperationalInsights/workspaces/savedSearches/read | Obtiene una consulta de búsqueda guardada |
> | Microsoft.OperationsManagement/solutions/read | Obtiene una solución OMS ya existente |
> | Microsoft.OperationalInsights/workspaces/query/read | Ejecuta consultas en los datos del área de trabajo. |
> | Microsoft.OperationalInsights/workspaces/query/*/read |  |
> | Microsoft.OperationalInsights/workspaces/dataSources/read | Obtiene los orígenes de datos en un área de trabajo. |
> | Microsoft.Insights/workbooks/read | Lee un libro. |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-sentinel-responder"></a>Respondedor de Azure Sentinel
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Respondedor de Azure Sentinel |
> | **Id** | 3e150937-b8fe-4cfb-8069-0eaf05ecd056 |
> | **Acciones** |  |
> | Microsoft.SecurityInsights/*/read |  |
> | Microsoft.SecurityInsights/cases/* |  |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Realiza búsquedas mediante el nuevo motor. |
> | Microsoft.OperationalInsights/workspaces/read | Obtiene un área de trabajo existente |
> | Microsoft.OperationalInsights/workspaces/dataSources/read | Obtiene los orígenes de datos en un área de trabajo. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/read | Obtiene una consulta de búsqueda guardada |
> | Microsoft.OperationsManagement/solutions/read | Obtiene una solución OMS ya existente |
> | Microsoft.OperationalInsights/workspaces/query/read | Ejecuta consultas en los datos del área de trabajo. |
> | Microsoft.OperationalInsights/workspaces/query/*/read |  |
> | Microsoft.OperationalInsights/workspaces/dataSources/read | Obtiene los orígenes de datos en un área de trabajo. |
> | Microsoft.Insights/workbooks/read | Lee un libro. |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-service-bus-data-owner"></a>Propietario de los datos de Azure Service Bus
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Concede acceso total a los recursos de Azure Service Bus. |
> | **Id** | 090c5cfd-751d-490a-894a-3ce6f1109419 |
> | **Acciones** |  |
> | Microsoft.ServiceBus/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.ServiceBus/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-service-bus-data-receiver"></a>Receptor de datos de Azure Service Bus
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Concede acceso de recepción a los recursos de Azure Service Bus. |
> | **Id** | 4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0 |
> | **Acciones** |  |
> | Microsoft.ServiceBus/*/queues/read |  |
> | Microsoft.ServiceBus/*/topics/read |  |
> | Microsoft.ServiceBus/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.ServiceBus/*/receive/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-service-bus-data-sender"></a>Emisor de datos de Azure Service Bus
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Concede acceso de emisión a los recursos de Azure Service Bus. |
> | **Id** | 69a216fc-b8fb-44d8-bc22-1f3c2cd27a39 |
> | **Acciones** |  |
> | Microsoft.ServiceBus/*/queues/read |  |
> | Microsoft.ServiceBus/*/topics/read |  |
> | Microsoft.ServiceBus/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.ServiceBus/*/send/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="azure-stack-registration-owner"></a>Propietario del registro de Azure Stack
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar los registros de Azure Stack. |
> | **Id** | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | **Acciones** |  |
> | Microsoft.AzureStack/registrations/products/*/action |  |
> | Microsoft.AzureStack/registrations/products/read | Obtiene las propiedades de un producto de Marketplace de Azure Stack. |
> | Microsoft.AzureStack/registrations/read | Obtiene las propiedades de un registro de Azure Stack. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="backup-contributor"></a>Colaborador de copias de seguridad
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar el servicio de copias de seguridad, pero no puede crear almacenes ni conceder acceso a otros usuarios |
> | **Id** | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Network/virtualNetworks/read | Obtiene la definición de red virtual |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Administrar los resultados de la operación de administración de copias de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Crear y administrar contenedores de copias de seguridad dentro de tejidos de copia de seguridad del almacén de Recovery Services |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Actualiza la lista de contenedores |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Crear y administrar trabajos de copia de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exporta trabajos |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Crear y administrar resultados de operaciones de administración de copias de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/* | Crear y administrar directivas de copia de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Crear y administrar elementos de los que se puede realizar una copia de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Crear y administrar elementos de los que se ha realizado una copia de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Crear y administrar contenedores que incluyen elementos de copia de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devuelve resúmenes de los elementos y servidores protegidos para un almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/certificates/* | Crear y administrar certificados relacionados con copias de seguridad en el almacén de Recovery Services |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Crear y administrar información ampliada relacionada con el almacén |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtiene las alertas del almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Crear y administrar identidades registradas |
> | Microsoft.RecoveryServices/Vaults/usages/* | Crear y administrar el uso del almacén de Recovery Services |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Storage/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Valida la operación en el elemento protegido. |
> | Microsoft.RecoveryServices/Vaults/write | La operación Create Vault crea un recurso de Azure del tipo "almacén" |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Devuelve el estado de la operación de Backup para el almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Devuelve todos los servidores de administración de copia de seguridad que se registraron con el almacén. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Obtiene todos los contenedores que se pueden proteger. |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Comprueba el estado de la copia de seguridad de los almacenes de Recovery Services. |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Valida las características. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Resuelve la alerta. |
> | Microsoft.RecoveryServices/operations/read | La operación devuelve la lista de operaciones de un proveedor de recursos. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Obtiene el estado de la operación para una operación determinada. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Enumera todas las intenciones de protección de la copia de seguridad. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="backup-operator"></a>Operador de copias de seguridad
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar los servicios de copias de seguridad, excepto la eliminación de copias de seguridad, la creación de almacenes y la concesión de acceso a otros usuarios |
> | **Id** | 00c29273-979b-4161-815c-10b084fb9324 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Network/virtualNetworks/read | Obtiene la definición de red virtual |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Devuelve el estado de la operación |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Obtiene los resultados de la operación realizada en el contenedor de protección. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Hace una copia de seguridad del elemento protegido. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Obtiene el resultado de la operación realizada en los elementos protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Devuelve el estado de la operación realizada en los elementos protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Devuelve detalles de objeto del elemento protegido |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Aprovisiona una recuperación de elementos instantánea para los elementos protegidos |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Obtiene los puntos de recuperación de los elementos protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Restaura los puntos de recuperación de los elementos protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Revoca la recuperación de elementos instantánea para los elementos protegidos |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Crea un elemento protegido de copia de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Devuelve todos los contenedores registrados |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Actualiza la lista de contenedores |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Crear y administrar trabajos de copia de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exporta trabajos |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Crear y administrar resultados de operaciones de administración de copias de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Obtiene los resultados de la operación de directiva. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Devuelve todas las directivas de protección |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Crear y administrar elementos de los que se puede realizar una copia de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Devuelve la lista de todos los elementos protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Devuelve todos los contenedores que pertenecen a la suscripción |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devuelve resúmenes de los elementos y servidores protegidos para un almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | La operación Actualizar certificado de recursos permite actualizar el certificado de credencial de recursos o almacenes. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | La operación Obtener información adicional obtiene la información adicional de un objeto que representa el recurso de Azure de tipo ?almacén? |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/write | La operación Obtener información adicional obtiene la información adicional de un objeto que representa el recurso de Azure de tipo ?almacén? |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtiene las alertas del almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | La operación Obtener resultados de la operación se puede usar para obtener el estado y el resultado de la operación enviada de forma asincrónica |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | La operación Obtener contenedores se puede usar para obtener los contenedores registrados para un recurso. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | La operación Registrar contenedor de servicios se puede usar para registrar un contenedor con servicio de recuperación. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Storage/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Valida la operación en el elemento protegido. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Devuelve el estado de la operación de Backup para el almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Obtiene el estado de la operación de directiva. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Crea un contenedor registrado. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Realiza consultas para las cargas de trabajo de un contenedor. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Devuelve todos los servidores de administración de copia de seguridad que se registraron con el almacén. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Crea una intención de protección de la copia de seguridad. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Obtiene una intención de protección de la copia de seguridad. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Obtiene todos los contenedores que se pueden proteger. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Obtiene todos los elementos de un contenedor. |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Comprueba el estado de la copia de seguridad de los almacenes de Recovery Services. |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Valida las características. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Resuelve la alerta. |
> | Microsoft.RecoveryServices/operations/read | La operación devuelve la lista de operaciones de un proveedor de recursos. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Obtiene el estado de la operación para una operación determinada. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Enumera todas las intenciones de protección de la copia de seguridad. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="backup-reader"></a>Lector de copias de seguridad
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede ver servicios de copia de seguridad, pero no puede realizar cambios. |
> | **Id** | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp es una operación interna que el servicio usa |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Devuelve el estado de la operación |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Obtiene los resultados de la operación realizada en el contenedor de protección. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Obtiene el resultado de la operación realizada en los elementos protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Devuelve el estado de la operación realizada en los elementos protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Devuelve detalles de objeto del elemento protegido |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Obtiene los puntos de recuperación de los elementos protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Devuelve todos los contenedores registrados |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Devuelve el resultado de la operación de trabajo. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Devuelve todos los objetos de trabajo |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exporta trabajos |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Devuelve el resultado de la operación de Backup para el almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Obtiene los resultados de la operación de directiva. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Devuelve todas las directivas de protección |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Devuelve la lista de todos los elementos protegidos. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Devuelve todos los contenedores que pertenecen a la suscripción |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Devuelve resúmenes de los elementos y servidores protegidos para un almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | La operación Obtener información adicional obtiene la información adicional de un objeto que representa el recurso de Azure de tipo ?almacén? |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtiene las alertas del almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | La operación Obtener resultados de la operación se puede usar para obtener el estado y el resultado de la operación enviada de forma asincrónica |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | La operación Obtener contenedores se puede usar para obtener los contenedores registrados para un recurso. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Devuelve la configuración de almacenamiento del almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/backupconfig/read | Devuelve la configuración del almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Devuelve el estado de la operación de Backup para el almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Obtiene el estado de la operación de directiva. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Devuelve todos los servidores de administración de copia de seguridad que se registraron con el almacén. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Obtiene una intención de protección de la copia de seguridad. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Obtiene todos los elementos de un contenedor. |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Comprueba el estado de la copia de seguridad de los almacenes de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Resuelve la alerta. |
> | Microsoft.RecoveryServices/operations/read | La operación devuelve la lista de operaciones de un proveedor de recursos. |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Obtiene el estado de la operación para una operación determinada. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Enumera todas las intenciones de protección de la copia de seguridad. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="billing-reader"></a>Lector de facturación
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite acceso de lectura a los datos de facturación. |
> | **Id** | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Billing/*/read | Leer la información de facturación |
> | Microsoft.Commerce/*/read |  |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.Management/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | Microsoft.CostManagement/*/read |  |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="biztalk-contributor"></a>Colaborador de BizTalk
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar los servicios de BizTalk, pero no acceder a ellos. |
> | **Id** | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.BizTalkServices/BizTalk/* | Crear y administrar los servicios de BizTalk |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="blockchain-member-node-access-preview"></a>Acceso al nodo de miembro de la cadena de bloques (versión preliminar)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite acceder a los nodos de miembro de la cadena de bloques. |
> | **Id** | 31a002a1-acaf-453e-8a5b-297c9ca1ea24 |
> | **Acciones** |  |
> | Microsoft.Blockchain/blockchainMembers/transactionNodes/read | Obtiene o enumera los nodos de transacción de miembro de la cadena de bloques existentes. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action | Se conecta a un nodo de transacción de miembro de la cadena de bloques. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="blueprint-contributor"></a>Colaborador de plano técnico
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede administrar las definiciones del plano técnico, pero no asignarlas. |
> | **Id** | 41077137-e803-4205-871c-5a86e6a753b4 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Blueprint/blueprints/* | Crea y administra definiciones de plano técnico o artefactos de plano técnico. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="blueprint-operator"></a>Operador del plano técnico
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede asignar los planos técnicos publicados existentes, pero no puede crear nuevos. Nota: Esto solo funciona si la asignación se realiza con una identidad administrada asignada por el usuario. |
> | **Id** | 437d2ced-4a38-4302-8479-ed2bcb43d090 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Blueprint/blueprintAssignments/* | Crea y administra asignaciones de plano técnico. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cdn-endpoint-contributor"></a>Colaborador de punto de conexión de CDN
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede administrar puntos de conexión de CDN, pero no conceder acceso a otros usuarios. |
> | **Id** | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/* |  |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cdn-endpoint-reader"></a>Lector de punto de conexión de CDN
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede ver puntos de conexión de CDN, pero no hacer cambios. |
> | **Id** | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/*/read |  |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cdn-profile-contributor"></a>Colaborador de perfil de CDN
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede administrar perfiles de CDN y sus puntos de conexión, pero no conceder acceso a otros usuarios. |
> | **Id** | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/* |  |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cdn-profile-reader"></a>Lector de perfil de CDN
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede ver perfiles de CDN y sus puntos de conexión, pero no hacer cambios. |
> | **Id** | 8f96442b-4075-438f-813d-ad51ab4019af |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/*/read |  |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="classic-network-contributor"></a>Colaborador de la red clásica
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar las redes clásicas, pero no acceder a ellas. |
> | **Id** | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer autorización |
> | Microsoft.ClassicNetwork/* | Crear y administrar redes clásicas |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="classic-storage-account-contributor"></a>Colaborador de cuentas de almacenamiento clásico
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar cuentas de almacenamiento clásicas, pero no acceder a ellas. |
> | **Id** | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer autorización |
> | Microsoft.ClassicStorage/storageAccounts/* | Crear y administrar cuentas de almacenamiento |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="classic-storage-account-key-operator-service-role"></a>Rol de servicio de operador de claves de cuentas de almacenamiento clásicas
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Los operadores de claves de cuentas de almacenamiento clásicas pueden enumerar y regenerar claves en cuentas de almacenamiento clásicas |
> | **Id** | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | **Acciones** |  |
> | Microsoft.ClassicStorage/storageAccounts/listkeys/action | Enumera las claves de acceso de las cuentas de almacenamiento. |
> | Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Regenera las claves de acceso existentes de la cuenta de almacenamiento. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="classic-virtual-machine-contributor"></a>Colaborador de la máquina virtual clásica
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar máquinas virtuales clásicas, pero no acceder a ellas, ni tampoco a la red virtual ni a la cuenta de almacenamiento a las que están conectadas. |
> | **Id** | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer autorización |
> | Microsoft.ClassicCompute/domainNames/* | Crear y administrar nombres de dominio de proceso clásico |
> | Microsoft.ClassicCompute/virtualMachines/* | Crear y administrar máquinas virtuales |
> | Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
> | Microsoft.ClassicNetwork/reservedIps/link/action | Vincula una IP reservada |
> | Microsoft.ClassicNetwork/reservedIps/read | Obtiene las IP reservadas |
> | Microsoft.ClassicNetwork/virtualNetworks/join/action | Une la red virtual. |
> | Microsoft.ClassicNetwork/virtualNetworks/read | Obtiene la red virtual. |
> | Microsoft.ClassicStorage/storageAccounts/disks/read | Devuelve el disco de la cuenta de almacenamiento. |
> | Microsoft.ClassicStorage/storageAccounts/images/read | Devuelve la imagen de la cuenta de almacenamiento. (En desuso. Use "Microsoft.ClassicStorage/storageAccounts/vmImages"). |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Enumera las claves de acceso de las cuentas de almacenamiento. |
> | Microsoft.ClassicStorage/storageAccounts/read | Devuelve la cuenta de almacenamiento con la cuenta especificada. |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cognitive-services-contributor"></a>Colaborador de Cognitive Services
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite crear, leer, actualizar, eliminar y administrar las claves de Cognitive Services. |
> | **Id** | 25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.CognitiveServices/* |  |
> | Microsoft.Features/features/read | Obtiene las características de una suscripción. |
> | Microsoft.Features/providers/features/read | Obtiene la característica de una suscripción de un proveedor de recursos determinado. |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Insights/diagnosticSettings/* | Crea, actualiza o lee la configuración de diagnóstico de Analysis Server. |
> | Microsoft.Insights/logDefinitions/read | Lee definiciones de registro |
> | Microsoft.Insights/metricdefinitions/read | Lee definiciones de métricas |
> | Microsoft.Insights/metrics/read | Lee métricas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/deployments/operations/read | Obtiene o enumera las operaciones de implementación. |
> | Microsoft.Resources/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | Microsoft.Resources/subscriptions/read | Obtiene la lista de suscripciones. |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cognitive-services-data-reader-preview"></a>Lector de datos de Cognitive Services (versión preliminar)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite leer los datos de Cognitive Services. |
> | **Id** | b59867f0-fa02-499b-be73-45a86b5b3e1c |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cognitive-services-user"></a>Usuario de Cognitive Services
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite leer y mostrar las claves de Cognitive Services. |
> | **Id** | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | **Acciones** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | Microsoft.CognitiveServices/accounts/listkeys/action | Enumera las claves |
> | Microsoft.Insights/alertRules/read | Lee una alerta de métrica clásica. |
> | Microsoft.Insights/diagnosticSettings/read | Lee la configuración de diagnóstico de un recurso. |
> | Microsoft.Insights/logDefinitions/read | Lee definiciones de registro |
> | Microsoft.Insights/metricdefinitions/read | Lee definiciones de métricas |
> | Microsoft.Insights/metrics/read | Lee métricas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/operations/read | Obtiene o enumera las operaciones de implementación. |
> | Microsoft.Resources/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | Microsoft.Resources/subscriptions/read | Obtiene la lista de suscripciones. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cosmos-db-account-reader-role"></a>Rol de lector de cuentas de Cosmos DB
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede leer los datos de cuentas de Azure Cosmos DB. Vea [Colaborador de cuenta de DocumentDB](#documentdb-account-contributor) para administrar cuentas de Azure Cosmos DB. |
> | **Id** | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles, puede leer permisos dados a cada usuario |
> | Microsoft.DocumentDB/*/read | Leer cualquier colección |
> | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Lee las claves de solo lectura de la cuenta de base de datos. |
> | Microsoft.Insights/MetricDefinitions/read | Lee definiciones de métricas |
> | Microsoft.Insights/Metrics/read | Lee métricas |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cosmos-db-operator"></a>Operador de Cosmos DB
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar las cuentas de Azure Cosmos DB, pero no acceder a los datos que contienen. Evita el acceso a las claves de cuenta y a las cadenas de conexión. |
> | **Id** | 230815da-be43-4aae-9cb4-875f7bd000aa |
> | **Acciones** |  |
> | Microsoft.DocumentDb/databaseAccounts/* |  |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Combina un recurso como una cuenta de almacenamiento o una instancia de SQL Database con una subred. No genera alertas. |
> | **NotActions** |  |
> | Microsoft.DocumentDB/databaseAccounts/readonlyKeys/* |  |
> | Microsoft.DocumentDB/databaseAccounts/regenerateKey/* |  |
> | Microsoft.DocumentDB/databaseAccounts/listKeys/* |  |
> | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cosmosbackupoperator"></a>CosmosBackupOperator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede enviar una solicitud de restauración para una base de datos de Cosmos DB o un contenedor de una cuenta |
> | **Id** | db7b14f2-5adf-42da-9f96-f2ee17bab5cb |
> | **Acciones** |  |
> | Microsoft.DocumentDB/databaseAccounts/backup/action | Envía una solicitud para configurar la copia de seguridad. |
> | Microsoft.DocumentDB/databaseAccounts/restore/action | Envía una solicitud de restauración. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cost-management-contributor"></a>Colaborador de Cost Management
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede ver los costos y administrar la configuración de estos (por ejemplo, presupuestos, exportaciones) |
> | **Id** | 434105ed-43f6-45c7-a02f-909b2ba83430 |
> | **Acciones** |  |
> | Microsoft.Consumption/* |  |
> | Microsoft.CostManagement/* |  |
> | Microsoft.Billing/billingPeriods/read |  |
> | Microsoft.Resources/subscriptions/read | Obtiene la lista de suscripciones. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Advisor/configurations/read | Obtener configuraciones |
> | Microsoft.Advisor/recommendations/read | Lee las recomendaciones |
> | Microsoft.Management/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="cost-management-reader"></a>Lector de Cost Management
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede ver los datos de costo y la configuración (por ejemplo, presupuestos, exportaciones) |
> | **Id** | 72fafb9e-0641-4937-9268-a91bfd8191a3 |
> | **Acciones** |  |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.CostManagement/*/read |  |
> | Microsoft.Billing/billingPeriods/read |  |
> | Microsoft.Resources/subscriptions/read | Obtiene la lista de suscripciones. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Advisor/configurations/read | Obtener configuraciones |
> | Microsoft.Advisor/recommendations/read | Lee las recomendaciones |
> | Microsoft.Management/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="data-box-contributor"></a>Colaborador de Data Box
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrarlo todo en el servicio Data Box, excepto dar acceso a otros usuarios. |
> | **Id** | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Databox/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="data-box-reader"></a>Lector de Data Box
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar el servicio Data Box excepto la creación o edición de detalles de pedido y dar acceso a otros usuarios. |
> | **Id** | 028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Databox/*/read |  |
> | Microsoft.Databox/jobs/listsecrets/action |  |
> | Microsoft.Databox/jobs/listcredentials/action | Enumera las credenciales sin cifrar relacionadas con el pedido. |
> | Microsoft.Databox/locations/availableSkus/action | Este método devuelve la lista de SKU disponibles. |
> | Microsoft.Databox/locations/validateAddress/action | Valida la dirección de envío y proporciona direcciones alternativas, si existen. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="data-factory-contributor"></a>Colaborador de Factoría de datos
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Crea y administra factorías de datos, así como recursos secundarios dentro de ellas. |
> | **Id** | 673868aa-7521-48a0-acc6-0f60742d39f5 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.DataFactory/dataFactories/* | Crear y administrar factorías de datos y recursos secundarios dentro de ellos. |
> | Microsoft.DataFactory/factories/* | Crear y administrar factorías de datos y recursos secundarios dentro de ellos. |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="data-lake-analytics-developer"></a>Desarrollador de Data Lake Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite enviar, supervisar y administrar sus propios trabajos, pero no crear ni eliminar cuentas de Data Lake Analytics. |
> | **Id** | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.BigAnalytics/accounts/* |  |
> | Microsoft.DataLakeAnalytics/accounts/* |  |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | Microsoft.DataLakeAnalytics/accounts/Delete | Elimina la cuenta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Concede permisos para cancelar trabajos que enviaron otros usuarios. |
> | Microsoft.DataLakeAnalytics/accounts/Write | Crea o actualiza una cuenta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Crea o actualiza una cuenta vinculada de DataLakeStore en la cuenta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Anula la vinculación de una cuenta de DataLakeStore a la cuenta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Crea o actualiza una cuenta de almacenamiento vinculada a una cuenta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Anula la vinculación de una cuenta de almacenamiento a la cuenta de DataLakeAnalytics. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Crea o actualiza una regla de firewall. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Elimina una regla de firewall. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Crea o actualiza una directiva de proceso. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Elimina una directiva de proceso. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="data-purger"></a>Purgador de datos
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede purgar datos de análisis. |
> | **Id** | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | **Acciones** |  |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Insights/components/purge/action | Purga datos de Application Insights. |
> | Microsoft.OperationalInsights/workspaces/*/read |  |
> | Microsoft.OperationalInsights/workspaces/purge/action | Elimina los datos especificados del área de trabajo. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="devtest-labs-user"></a>Usuario de DevTest Labs
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite conectarse a sus máquinas virtuales, así como iniciarlas, reiniciarlas y apagarlas, en su instancia de Azure DevTest Labs. |
> | **Id** | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Compute/availabilitySets/read | Obtiene las propiedades de un conjunto de disponibilidad |
> | Microsoft.Compute/virtualMachines/*/read | Leer las propiedades de una máquina virtual (tamaños de máquinas virtuales, el estado de tiempo de ejecución, extensiones de máquina virtual, etc.) |
> | Microsoft.Compute/virtualMachines/deallocate/action | Apaga la máquina virtual y libera los recursos de proceso |
> | Microsoft.Compute/virtualMachines/read | Obtiene las propiedades de una máquina virtual |
> | Microsoft.Compute/virtualMachines/restart/action | Reinicia la máquina virtual |
> | Microsoft.Compute/virtualMachines/start/action | Inicia la máquina virtual |
> | Microsoft.DevTestLab/*/read | Leer las propiedades de un laboratorio |
> | Microsoft.DevTestLab/labs/claimAnyVm/action | Reclama una máquina virtual aleatoria en el laboratorio. |
> | Microsoft.DevTestLab/labs/createEnvironment/action | Crea máquinas virtuales en un laboratorio. |
> | Microsoft.DevTestLab/labs/ensureCurrentUserProfile/action | Asegúrese de que el usuario actual tiene un perfil válido en el laboratorio. |
> | Microsoft.DevTestLab/labs/formulas/delete | Elimina fórmulas. |
> | Microsoft.DevTestLab/labs/formulas/read | Lee fórmulas. |
> | Microsoft.DevTestLab/labs/formulas/write | Agrega o modifica fórmulas. |
> | Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Evalúa la directiva de laboratorio. |
> | Microsoft.DevTestLab/labs/virtualMachines/claim/action | Toma la propiedad de una máquina virtual existente |
> | Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action | Muestra, si las hay, las programaciones de inicio y detención aplicables. |
> | Microsoft.DevTestLab/labs/virtualMachines/getRdpFileContents/action | Obtiene una cadena que representa el contenido del archivo RDP para la máquina virtual. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Se une a un grupo de direcciones de back-end del equilibrador de carga. No genera alertas. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Se une a una regla NAT de entrada del equilibrador de carga. No genera alertas. |
> | Microsoft.Network/networkInterfaces/*/read | Leer las propiedades de una interfaz de red (por ejemplo, todos los equilibradores de carga de los que forma parte de la interfaz de red) |
> | Microsoft.Network/networkInterfaces/join/action | Une una máquina virtual a una interfaz de red. No genera alertas. |
> | Microsoft.Network/networkInterfaces/read | Obtiene una definición de interfaz de red.  |
> | Microsoft.Network/networkInterfaces/write | Crea una interfaz de red o actualiza una interfaz de red existente.  |
> | Microsoft.Network/publicIPAddresses/*/read | Leer las propiedades de una dirección IP pública |
> | Microsoft.Network/publicIPAddresses/join/action | Se une a una dirección IP pública. No genera alertas. |
> | Microsoft.Network/publicIPAddresses/read | Obtiene una definición de la dirección ip pública. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Se une a una red virtual. No genera alertas. |
> | Microsoft.Resources/deployments/operations/read | Obtiene o enumera las operaciones de implementación. |
> | Microsoft.Resources/deployments/read | Obtiene o enumera implementaciones. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | **NotActions** |  |
> | Microsoft.Compute/virtualMachines/vmSizes/read | Enumera los tamaños disponibles a los que se puede actualizar la máquina virtual |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="dns-zone-contributor"></a>Colaborador de zona DNS
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar zonas y conjuntos de registros DNS en Azure DNS, pero no controlar los usuarios que tienen acceso. |
> | **Id** | befefa01-2a29-4197-83a8-272ff33ce314 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.Network/dnsZones/* | Crear y administrar registros y zonas DNS |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar vales de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="documentdb-account-contributor"></a>Colaborador de cuenta de DocumentDB
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede administrar cuentas de Azure Cosmos DB. Azure Cosmos DB se llamaba anteriormente DocumentDB. |
> | **Id** | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.DocumentDb/databaseAccounts/* | Crear y administrar cuentas de Azure Cosmos DB |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Combina un recurso como una cuenta de almacenamiento o una instancia de SQL Database con una subred. No genera alertas. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="eventgrid-eventsubscription-contributor"></a>Colaborador de EventGrid EventSubscription
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar las operaciones de suscripción de eventos de EventGrid. |
> | **Id** | 428e0ff0-5e57-4d9c-a221-2c70d0e0a443 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.EventGrid/eventSubscriptions/* |  |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | Enumera las suscripciones de eventos globales por tipo de tema. |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | Enumera las suscripciones de eventos regionales. |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | Enumera las suscripciones de eventos regionales por tipo de tema. |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="eventgrid-eventsubscription-reader"></a>Lector de EventGrid EventSubscription
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite leer las suscripciones de eventos de EventGrid. |
> | **Id** | 2414bbcf-6497-4faf-8c65-045460748405 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.EventGrid/eventSubscriptions/read | Lee una clase eventSubscription. |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | Enumera las suscripciones de eventos globales por tipo de tema. |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | Enumera las suscripciones de eventos regionales. |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | Enumera las suscripciones de eventos regionales por tipo de tema. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="hdinsight-cluster-operator"></a>Operador de clústeres de HDInsight
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite leer y modificar las configuraciones de clúster de HDInsight. |
> | **Id** | 61ed4efc-Fab3-44fd-b111-e24485cc132a |
> | **Acciones** |  |
> | Microsoft.HDInsight/*/read |  |
> | Microsoft.HDInsight/clusters/getGatewaySettings/action | Obtiene la configuración de puerta de enlace para el clúster de HDInsight. |
> | Microsoft.HDInsight/clusters/updateGatewaySettings/action | Actualiza la configuración de puerta de enlace para el clúster de HDInsight. |
> | Microsoft.HDInsight/clusters/configurations/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Resources/deployments/operations/read | Obtiene o enumera las operaciones de implementación. |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="hdinsight-domain-services-contributor"></a>Colaborador de Domain Services para HDInsight
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede leer, crear, modificar y eliminar operaciones relacionadas con Domain Services para HDInsight Enterprise Security Package |
> | **Id** | 8d8d5a11-05d3-4bda-a417-a08778121c7c |
> | **Acciones** |  |
> | Microsoft.AAD/*/read |  |
> | Microsoft.AAD/domainServices/*/read |  |
> | Microsoft.AAD/domainServices/oucontainer/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="intelligent-systems-account-contributor"></a>Colaborador de la cuenta de Sistemas inteligentes
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar las cuentas de Intelligent Systems, pero no acceder a ellas. |
> | **Id** | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.IntelligentSystems/accounts/* | Crear y administrar cuentas de sistemas inteligentes |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="key-vault-contributor"></a>Colaborador de almacén de claves
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite administrar almacenes de claves, pero no acceder a ellos. |
> | **Id** | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.KeyVault/* |  |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | Microsoft.KeyVault/locations/deletedVaults/purge/action | Purga un almacén de claves eliminado temporalmente |
> | Microsoft.KeyVault/hsmPools/* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="lab-creator"></a>Creador de laboratorio
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite crear, administrar y eliminar los laboratorios administrados en sus cuentas de Azure Lab. |
> | **Id** | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.LabServices/labAccounts/*/read |  |
> | Microsoft.LabServices/labAccounts/createLab/action | Crea un laboratorio en una cuenta de laboratorio. |
> | Microsoft.LabServices/labAccounts/sizes/getRegionalAvailability/action |  |
> | Microsoft.LabServices/labAccounts/getRegionalAvailability/action | Obtiene información sobre la disponibilidad regional para cada categoría de tamaño configurada en una cuenta de laboratorio. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="log-analytics-contributor"></a>Colaborador de Log Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Un colaborador de Log Analytics puede leer todos los datos de supervisión y editar la configuración de supervisión. La edición de la configuración de supervisión incluye la posibilidad de añadir la extensión de máquina virtual a las máquinas virtuales, leer las claves de las cuentas de almacenamiento para poder configurar la recopilación de registros de Azure Storage, crear y configurar cuentas de Automation, añadir soluciones y configurar Azure Diagnostics en todos los recursos de Azure. |
> | **Id** | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | **Acciones** |  |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | Microsoft.Automation/automationAccounts/* |  |
> | Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Enumera las claves de acceso de las cuentas de almacenamiento. |
> | Microsoft.Compute/virtualMachines/extensions/* |  |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Insights/diagnosticSettings/* | Crea, actualiza o lee la configuración de diagnóstico de Analysis Server. |
> | Microsoft.OperationalInsights/* |  |
> | Microsoft.OperationsManagement/* |  |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="log-analytics-reader"></a>Lector de Log Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Un lector de Log Analytics puede ver y buscar todos los datos de supervisión, así como consultar la configuración de supervisión, incluida la de Azure Diagnostics en todos los recursos de Azure. |
> | **Id** | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **Acciones** |  |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Realiza búsquedas mediante el nuevo motor. |
> | Microsoft.OperationalInsights/workspaces/search/action | Ejecuta una consulta de búsqueda |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Recupera las claves compartidas del área de trabajo. Estas claves se utilizan para conectar los agentes de Microsoft Operational Insights al área de trabajo. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="logic-app-contributor"></a>Colaborador de aplicación lógica
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite administrar aplicaciones lógicas, pero no cambiar el acceso a ellas. |
> | **Id** | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Enumera las claves de acceso de las cuentas de almacenamiento. |
> | Microsoft.ClassicStorage/storageAccounts/read | Devuelve la cuenta de almacenamiento con la cuenta especificada. |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Insights/metricAlerts/* |  |
> | Microsoft.Insights/diagnosticSettings/* | Crea, actualiza o lee la configuración de diagnóstico de Analysis Server. |
> | Microsoft.Insights/logdefinitions/* | Este permiso es necesario para los usuarios que necesitan acceder a registros de actividades a través del portal. Enumere las categorías de registro del registro de actividad. |
> | Microsoft.Insights/metricDefinitions/* | Leer definiciones de métrica (lista de tipos de métricas disponibles para un recurso). |
> | Microsoft.Logic/* | Administra los recursos de Logic Apps. |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Storage/storageAccounts/listkeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | Microsoft.Storage/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Web/connectionGateways/* | Crea y administra una puerta de enlace de conexión. |
> | Microsoft.Web/connections/* | Crea y administra una conexión. |
> | Microsoft.Web/customApis/* | Crea y administra una API personalizada. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Obtiene las propiedades de un plan de App Service |
> | Microsoft.Web/sites/functions/listSecrets/action | Lista de secretos de función. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="logic-app-operator"></a>Operador de aplicación lógica
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite leer, habilitar y deshabilitar aplicaciones lógicas, pero no permite editarlas ni actualizarlas. |
> | **Id** | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/*/read | Permite leer reglas de alerta de Insights. |
> | Microsoft.Insights/metricAlerts/*/read |  |
> | Microsoft.Insights/diagnosticSettings/*/read | Obtiene la configuración de diagnóstico de Logic Apps. |
> | Microsoft.Insights/metricDefinitions/*/read | Obtiene las métricas disponibles para Logic Apps. |
> | Microsoft.Logic/*/read | Lee los recursos de Logic Apps. |
> | Microsoft.Logic/workflows/disable/action | Deshabilita el flujo de trabajo. |
> | Microsoft.Logic/workflows/enable/action | Habilita el flujo de trabajo. |
> | Microsoft.Logic/workflows/validate/action | Valida el flujo de trabajo. |
> | Microsoft.Resources/deployments/operations/read | Obtiene o enumera las operaciones de implementación. |
> | Microsoft.Resources/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Web/connectionGateways/*/read | Lee las puertas de enlace de conexión. |
> | Microsoft.Web/connections/*/read | Lee las conexiones. |
> | Microsoft.Web/customApis/*/read | Lee la API personalizada. |
> | Microsoft.Web/serverFarms/read | Obtiene las propiedades de un plan de App Service |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="managed-application-operator-role"></a>Rol de operador de aplicación administrada
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite leer y realizar acciones en los recursos de aplicación administrada. |
> | **Id** | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | **Acciones** |  |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | Microsoft.Solutions/applications/read | Recupera una lista de aplicaciones. |
> | Microsoft.Solutions/*/action |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="managed-applications-reader"></a>Lector de aplicaciones administradas
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite leer los recursos de una aplicación administrada y solicitar acceso JIT. |
> | **Id** | b9331d33-8a36-4f8c-b097-4f54124fdb44 |
> | **Acciones** |  |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Solutions/jitRequests/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="managed-identity-contributor"></a>Colaborador de identidad administrada
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite crear, leer, actualizar y eliminar identidades asignadas por el usuario. |
> | **Id** | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | **Acciones** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/read | Obtiene la identidad asignada a un usuario existente. |
> | Microsoft.ManagedIdentity/userAssignedIdentities/write | Crea una nueva identidad asignada a un usuario o actualiza las etiquetas asociadas a una identidad asignada a un usuario existente. |
> | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Elimina la identidad asignada a un usuario existente. |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="managed-identity-operator"></a>Operador de identidad administrada
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite leer y asignar identidades asignadas por el usuario. |
> | **Id** | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Acciones** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="managed-services-registration-assignment-delete-role"></a>Rol para eliminar la asignación de registros de servicios administrados
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | El rol para eliminar la asignación de registros de servicios administrados permite que los usuarios que administran el inquilino eliminen la asignación de registros asignada a su inquilino. |
> | **Id** | 91c1777a-f3dc-4fae-b103-61d183457e46 |
> | **Acciones** |  |
> | Microsoft.ManagedServices/registrationAssignments/read | Recupera una lista de las asignaciones del registro de servicios administrados. |
> | Microsoft.ManagedServices/registrationAssignments/delete | Quita la asignación del registro de servicios administrados. |
> | Microsoft.ManagedServices/operationStatuses/read | Lee el estado de la operación de los recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="management-group-contributor"></a>Colaborador de grupo de administración
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Rol de colaborador de grupo de administración |
> | **Id** | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | **Acciones** |  |
> | Microsoft.Management/managementGroups/delete | Elimina un grupo de administración. |
> | Microsoft.Management/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | Microsoft.Management/managementGroups/subscriptions/delete | Anula la asociación de la suscripción con el grupo de administración. |
> | Microsoft.Management/managementGroups/subscriptions/write | Asocia la suscripción existente con el grupo de administración. |
> | Microsoft.Management/managementGroups/write | Crea o actualiza un grupo de administración. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="management-group-reader"></a>Lector de grupo de administración
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Rol de lector de grupo de administración |
> | **Id** | ac63b705-f282-497d-ac71-919bf39d939d |
> | **Acciones** |  |
> | Microsoft.Management/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="monitoring-contributor"></a>Colaborador de supervisión
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede leer todos los datos de supervisión y editar la configuración de supervisión. Consulte también [Introducción a roles, permisos y seguridad con Azure Monitor](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). |
> | **Id** | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | **Acciones** |  |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | Microsoft.AlertsManagement/alerts/* |  |
> | Microsoft.AlertsManagement/alertsSummary/* |  |
> | Microsoft.Insights/actiongroups/* |  |
> | Microsoft.Insights/activityLogAlerts/* |  |
> | Microsoft.Insights/AlertRules/* | Reglas de alerta de lectura, escritura y eliminación. |
> | Microsoft.Insights/components/* | Leer, escribir o eliminar los componentes de Application Insights. |
> | Microsoft.Insights/DiagnosticSettings/* | Configuración de diagnóstico de lectura, escritura y eliminación. |
> | Microsoft.Insights/eventtypes/* | Enumerar eventos del registro de actividades (eventos de administración) de una suscripción. Este permiso es aplicable para el acceso mediante programación y mediante el portal al registro de actividades. |
> | Microsoft.Insights/LogDefinitions/* | Este permiso es necesario para los usuarios que necesitan acceder a registros de actividades a través del portal. Enumere las categorías de registro del registro de actividad. |
> | Microsoft.Insights/metricalerts/* |  |
> | Microsoft.Insights/MetricDefinitions/* | Leer definiciones de métrica (lista de tipos de métricas disponibles para un recurso). |
> | Microsoft.Insights/Metrics/* | Leer las métricas de un recurso. |
> | Microsoft.Insights/Register/Action | Registra el proveedor de Microsoft Insights. |
> | Microsoft.Insights/scheduledqueryrules/* |  |
> | Microsoft.Insights/webtests/* | Leer, escribir o eliminar pruebas web de Application Insights. |
> | Microsoft.Insights/workbooks/* |  |
> | Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Lee, escribe o elimina paquetes de soluciones de Log Analytics. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* | Lee, escribe o elimina búsquedas guardadas de Log Analytics. |
> | Microsoft.OperationalInsights/workspaces/search/action | Ejecuta una consulta de búsqueda |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Recupera las claves compartidas del área de trabajo. Estas claves se utilizan para conectar los agentes de Microsoft Operational Insights al área de trabajo. |
> | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Lee, escribe o elimina configuraciones de visión de almacenamiento de Log Analytics. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.WorkloadMonitor/monitors/* |  |
> | Microsoft.WorkloadMonitor/notificationSettings/* |  |
> | Microsoft.AlertsManagement/smartDetectorAlertRules/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="monitoring-metrics-publisher"></a>Supervisión del publicador de métricas
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite publicar las métricas de los recursos de Azure. |
> | **Id** | 3913510d-42f4-4e42-8a64-420c390055eb |
> | **Acciones** |  |
> | Microsoft.Insights/Register/Action | Registra el proveedor de Microsoft Insights. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Insights/Metrics/Write | Escribe métricas. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="monitoring-reader"></a>Lector de supervisión
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Puede leer todos los datos de supervisión (métricas, registros, etc.). Consulte también [Introducción a roles, permisos y seguridad con Azure Monitor](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). |
> | **Id** | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | **Acciones** |  |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | Microsoft.OperationalInsights/workspaces/search/action | Ejecuta una consulta de búsqueda |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="network-contributor"></a>Colaborador de la red
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar redes, pero no acceder a ellas. |
> | **Id** | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.Network/* | Crear y administrar redes |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="new-relic-apm-account-contributor"></a>Colaborador de la cuenta de NewRelic APM
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite administrar las aplicaciones y cuentas de Application Performance Management de New Relic, pero no acceder a ellas. |
> | **Id** | 5d28c62d-5b37-4476-8438-e587778df237 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | NewRelic.APM/accounts/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="policy-insights-data-writer-preview"></a>Escritor de datos de Policy Insights (versión preliminar)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite el acceso de lectura a las directivas de los recursos y el acceso de escritura a los eventos de directiva de los componentes de los recursos. |
> | **Id** | 66bb4e9e-b016-4a94-8249-4c0511c2be84 |
> | **Acciones** |  |
> | Microsoft.Authorization/policyassignments/read | Obtiene información sobre una asignación de directiva. |
> | Microsoft.Authorization/policydefinitions/read | Obtiene información sobre una definición de directiva. |
> | Microsoft.Authorization/policysetdefinitions/read | Obtiene información sobre una definición de un conjunto de directivas. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.PolicyInsights/checkDataPolicyCompliance/action | Compruebe el estado de cumplimiento de un componente determinado en relación con las directivas de datos. |
> | Microsoft.PolicyInsights/policyEvents/logDataEvents/action | Registra los eventos de directivas de componentes del recurso. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="reader-and-data-access"></a>Lector y acceso a los datos
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite ver todo el contenido, pero no eliminar ni crear una cuenta de almacenamiento ni un recurso incluido. También permitirá el acceso de lectura o escritura para todos los datos incluidos en una cuenta de almacenamiento a través del acceso a las claves de la cuenta de almacenamiento. |
> | **Id** | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | **Acciones** |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | Microsoft.Storage/storageAccounts/ListAccountSas/action | Devuelve el token de SAS de la cuenta de almacenamiento especificada. |
> | Microsoft.Storage/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="redis-cache-contributor"></a>Colaborador de la memoria caché de Redis
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar cachés de Redis, pero no acceder a ellas. |
> | **Id** | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Cache/redis/* | Crear y administrar memorias caché de Redis |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="resource-policy-contributor"></a>Colaborador de directivas de recursos
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Los usuarios con derechos para crear o modificar la directiva de recursos pueden crear solicitudes de soporte técnico y leer los recursos o la jerarquía. |
> | **Id** | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | **Acciones** |  |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | Microsoft.Authorization/policyassignments/* | Crear y administrar asignaciones de directivas |
> | Microsoft.Authorization/policydefinitions/* | Crear y administrar definiciones de directivas |
> | Microsoft.Authorization/policysetdefinitions/* | Crear y administrar conjuntos de directivas |
> | Microsoft.PolicyInsights/* |  |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="scheduler-job-collections-contributor"></a>Colaborador de colecciones de trabajos de Scheduler
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar colecciones de trabajos de Scheduler, pero no acceder a ellas. |
> | **Id** | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Scheduler/jobcollections/* | Crear y administrar colecciones de trabajos |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="search-service-contributor"></a>Colaborador del servicio de búsqueda
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar los servicios de Búsqueda, pero no acceder a ellos. |
> | **Id** | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Search/searchServices/* | Crear y administrar servicios de búsqueda |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="security-admin"></a>Administrador de seguridad
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Solo en Security Center: puede ver las directivas de seguridad, los estados de seguridad, editar las directivas de seguridad, ver alertas y recomendaciones, y descartar alertas y recomendaciones. |
> | **Id** | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Authorization/policyAssignments/* | Crear y administrar asignaciones de directivas |
> | Microsoft.Authorization/policyDefinitions/* | Crear y administrar definiciones de directivas |
> | Microsoft.Authorization/policySetDefinitions/* | Crear y administrar conjuntos de directivas |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.Management/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | Microsoft.operationalInsights/workspaces/*/read | Consulta datos de Log Analytics. |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Security/* |  |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="security-manager-legacy"></a>Administrador de seguridad (heredado)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Se trata de un rol heredado. En su lugar, use el Administrador de seguridad. |
> | **Id** | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.ClassicCompute/*/read | Leer información de configuración de máquinas virtuales clásicas |
> | Microsoft.ClassicCompute/virtualMachines/*/write | Escribir configuración de máquinas virtuales clásicas |
> | Microsoft.ClassicNetwork/*/read | Leer información de configuración acerca de la red clásica |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Security/* | Crear y administrar las directivas y los componentes de seguridad |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="security-reader"></a>Lector de seguridad
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Solo en Security Center: puede ver las recomendaciones y alertas, ver las directivas de seguridad y ver los estados de seguridad, pero no puede realizar cambios |
> | **Id** | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.operationalInsights/workspaces/*/read | Consulta datos de Log Analytics. |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Security/*/read | Leer directivas y componentes de seguridad |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Management/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="site-recovery-contributor"></a>Colaborador de Site Recovery
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar el servicio Site Recovery, excepto la creación de almacenes y la asignación de roles. |
> | **Id** | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.Network/virtualNetworks/read | Obtiene la definición de red virtual |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp es una operación interna que el servicio usa |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp es una operación interna que el servicio usa |
> | Microsoft.RecoveryServices/Vaults/certificates/write | La operación Actualizar certificado de recursos permite actualizar el certificado de credencial de recursos o almacenes. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Crear y administrar información ampliada relacionada con el almacén |
> | Microsoft.RecoveryServices/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Crear y administrar identidades registradas |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Crea o actualiza la configuración de las alertas de replicación |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Lee todos los evento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/* | Crea y administra los tejidos de replicación |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Crea y administra los trabajos de replicación |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/* | Crea y administra las directivas de replicación |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Crea y administra planes de recuperación |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* | Crea y administra la configuración de almacenamiento del almacén de Recovery Services |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | La operación Token de almacén se puede usar para obtener el token de almacén de las operaciones back-end a nivel de almacén. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Lee las alertas del almacén de Recovery Services |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Storage/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | Microsoft.RecoveryServices/vaults/replicationOperationStatus/read | Lee los estados de operación de replicación del almacén. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="site-recovery-operator"></a>Operador de Site Recovery
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite realizar una conmutación por error o una conmutación por recuperación, pero no otras operaciones de administración de Site Recovery. |
> | **Id** | 494ae006-db33-4328-bf46-533a6560a3ca |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.Network/virtualNetworks/read | Obtiene la definición de red virtual |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp es una operación interna que el servicio usa |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp es una operación interna que el servicio usa |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | La operación Obtener información adicional obtiene la información adicional de un objeto que representa el recurso de Azure de tipo ?almacén? |
> | Microsoft.RecoveryServices/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | La operación Obtener resultados de la operación se puede usar para obtener el estado y el resultado de la operación enviada de forma asincrónica |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | La operación Obtener contenedores se puede usar para obtener los contenedores registrados para un recurso. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Lee todas las configuraciones de alerta |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Lee todos los evento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Comprueba la coherencia del tejido |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Lee todas las fábricas |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Vuelve a asociar la puerta de enlace |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Renueva un certificado para Fabric. |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Lee todas las redes |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Lee todas las asignaciones de redes |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Lee todos los contenedores de protección |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Lee todos los elementos que se pueden proteger |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Aplica un punto de recuperación |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Confirma la conmutación por error |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Conmutación por error planeada |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Lee todos los elementos protegidos |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lee los puntos de recuperación de todas las replicaciones |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Repara una replicación |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Vuelva a proteger el elemento protegido |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Cambia los contenedores de protección |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Test Failover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Prueba la limpieza de la conmutación por error |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Conmutación por error |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Actualiza Mobility Service |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Lee todas las asignaciones de los contenedores de protección |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Lee todos los proveedores de Recovery Services |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Actualiza el proveedor |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Lee todas las clasificaciones de almacenamiento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Lee todas las asignaciones de clasificaciones de almacenamiento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Lee todos los vCenters. |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Crea y administra los trabajos de replicación |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Lee todas las directivas |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Plan de recuperación de confirmación de la conmutación por error |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Plan de recuperación de conmutación por error planeado |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Lee todos los planes de recuperación |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Vuelve a proteger el plan de recuperación |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Prueba el plan de recuperación de conmutación por error |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Prueba el plan de recuperación de limpieza de la conmutación por error |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Plan de recuperación de conmutación por error |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Lee las alertas del almacén de Recovery Services |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | La operación Token de almacén se puede usar para obtener el token de almacén de las operaciones back-end a nivel de almacén. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Storage/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="site-recovery-reader"></a>Lector de Site Recovery
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite visualizar el estado de Site Recovery, pero no realizar otras operaciones de administración. |
> | **Id** | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp es una operación interna que el servicio usa |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | La operación Obtener información adicional obtiene la información adicional de un objeto que representa el recurso de Azure de tipo ?almacén? |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Obtiene las alertas del almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | La operación Obtener resultados de la operación se puede usar para obtener el estado y el resultado de la operación enviada de forma asincrónica |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | La operación Obtener contenedores se puede usar para obtener los contenedores registrados para un recurso. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Lee todas las configuraciones de alerta |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Lee todos los evento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Lee todas las fábricas |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Lee todas las redes |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Lee todas las asignaciones de redes |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Lee todos los contenedores de protección |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Lee todos los elementos que se pueden proteger |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Lee todos los elementos protegidos |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lee los puntos de recuperación de todas las replicaciones |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Lee todas las asignaciones de los contenedores de protección |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Lee todos los proveedores de Recovery Services |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Lee todas las clasificaciones de almacenamiento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Lee todas las asignaciones de clasificaciones de almacenamiento |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Lee todos los vCenters. |
> | Microsoft.RecoveryServices/vaults/replicationJobs/read | Lee todos los trabajos |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Lee todas las directivas |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Lee todos los planes de recuperación |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | La operación Token de almacén se puede usar para obtener el token de almacén de las operaciones back-end a nivel de almacén. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="spatial-anchors-account-contributor"></a>Colaborador de la cuenta de Spatial Anchors
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar los anclajes espaciales en su cuenta, pero no eliminarlos. |
> | **Id** | 8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827 |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Crea anclajes espaciales. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Detecta anclajes espaciales próximos. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Obtiene las propiedades de los anclajes espaciales. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Ubica anclajes espaciales. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Envía datos de diagnóstico para ayudar a mejorar la calidad del servicio Azure Spatial Anchors. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Actualiza las propiedades de los anclajes espaciales. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="spatial-anchors-account-owner"></a>Propietario de la cuenta de Spatial Anchors
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar los anclajes espaciales en su cuenta y eliminarlos. |
> | **Id** | 70bbe301-9835-447d-afdd-19eb3167307c |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Crea anclajes espaciales. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Elimina anclajes espaciales. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Detecta anclajes espaciales próximos. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Obtiene las propiedades de los anclajes espaciales. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Ubica anclajes espaciales. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Envía datos de diagnóstico para ayudar a mejorar la calidad del servicio Azure Spatial Anchors. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Actualiza las propiedades de los anclajes espaciales. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="spatial-anchors-account-reader"></a>Lector de la cuenta de Spatial Anchors
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite encontrar y leer propiedades de los anclajes espaciales en la cuenta. |
> | **Id** | 5d51204f-eb77-4b1c-b86a-2ec626c49413 |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Detecta anclajes espaciales próximos. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Obtiene las propiedades de los anclajes espaciales. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Ubica anclajes espaciales. |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Envía datos de diagnóstico para ayudar a mejorar la calidad del servicio Azure Spatial Anchors. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="sql-db-contributor"></a>Colaborador de Base de datos de SQL
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar las bases de datos de SQL, pero no acceder a ellas. Además, no puede administrar sus directivas relacionadas con la seguridad ni los servidores SQL primarios. |
> | **Id** | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alertas |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/databases/* | Crear y administrar bases de datos SQL |
> | Microsoft.Sql/servers/read | Devuelve la lista de servidores u obtiene las propiedades de un servidor específico. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Insights/metrics/read | Lee métricas |
> | Microsoft.Insights/metricDefinitions/read | Lee definiciones de métricas |
> | **NotActions** |  |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Edita directivas de auditoría |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Edita la configuración de auditoría |
> | Microsoft.Sql/servers/databases/auditRecords/read | Recupera los registros de auditoría de blobs de bases de datos |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Edita directivas de conexión |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Edita directivas de enmascaramiento |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Edita las directivas de alerta de seguridad |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Edita las métricas de seguridad |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="sql-managed-instance-contributor"></a>Colaborador de Instancia administrada de SQL
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar las instancias administradas de SQL y requiere configuración de red, pero no puede conceder acceso a otros usuarios. |
> | **Id** | 4939a1f6-9ae0-4e48-a1e0-f2cbe897382d |
> | **Acciones** |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Network/networkSecurityGroups/* |  |
> | Microsoft.Network/routeTables/* |  |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/managedInstances/* |  |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Network/virtualNetworks/subnets/* |  |
> | Microsoft.Network/virtualNetworks/* |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Insights/metrics/read | Lee métricas |
> | Microsoft.Insights/metricDefinitions/read | Lee definiciones de métricas |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="sql-security-manager"></a>Administrador de seguridad SQL
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar las directivas relacionadas con seguridad de bases de datos y servidores SQL, pero no acceder a ellas. |
> | **Id** | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer autorización de Microsoft |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Combina un recurso como una cuenta de almacenamiento o una instancia de SQL Database con una subred. No genera alertas. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | Crear y administrar directivas de auditoría de SQL Server |
> | Microsoft.Sql/servers/auditingSettings/* | Crear y administrar configuración de auditoría de SQL Server |
> | Microsoft.Sql/servers/extendedAuditingSettings/read | Recupera los detalles de la directiva de auditoría de blobs del servidor extendido que está configurada en un servidor determinado. |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Crear y administrar directivas de auditoría de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Crear y administrar configuración de auditoría de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/auditRecords/read | Leer registros de auditoría |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Crear y administrar directivas de conexión de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Crear y administrar directivas de enmascaramiento de datos de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Recupera los detalles de la directiva de auditoría de blobs extendida y configurada en una base de datos determinada. |
> | Microsoft.Sql/servers/databases/read | Devuelve la lista de bases de datos u obtiene las propiedades de una base de datos específica. |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/read | Obtiene un esquema de la base de datos. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Obtiene una columna de la base de datos. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/read | Obtiene una tabla de la base de datos. |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Crear y administrar directivas de alerta de seguridad de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Crear y administrar métricas de seguridad de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/transparentDataEncryption/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/firewallRules/* |  |
> | Microsoft.Sql/servers/read | Devuelve la lista de servidores u obtiene las propiedades de un servidor específico. |
> | Microsoft.Sql/servers/securityAlertPolicies/* | Crear y administrar directivas de alerta de seguridad de SQL Server |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="sql-server-contributor"></a>Colaborador de SQL Server
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar bases de datos y servidores SQL, pero no acceder a ellos, ni a sus directivas relacionadas con la seguridad. |
> | **Id** | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/* | Crear y administrar servidores de SQL Server |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Insights/metrics/read | Lee métricas |
> | Microsoft.Insights/metricDefinitions/read | Lee definiciones de métricas |
> | **NotActions** |  |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | Edita las directivas de auditoría de SQL Server |
> | Microsoft.Sql/servers/auditingSettings/* | Edita la configuración de auditoría de SQL Server |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Edita las directivas de auditoría de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Edita la configuración de auditoría de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/auditRecords/read | Leer registros de auditoría |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Edita las directivas de conexión de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Edita las directivas de enmascaramiento de datos de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Edita las directivas de alerta de seguridad de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Edita las métricas de seguridad de bases de datos de SQL Server |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/securityAlertPolicies/* | Edita las directivas de alerta de seguridad de SQL Server |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-account-contributor"></a>Colaborador de la cuenta de almacenamiento
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite la administración de cuentas de almacenamiento. Proporciona acceso a la clave de cuenta, que puede usarse para tener acceso a datos a través de la autorización de clave compartida. |
> | **Id** | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer toda la autorización |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Insights/diagnosticSettings/* | Administrar la configuración de diagnóstico |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Combina un recurso como una cuenta de almacenamiento o una instancia de SQL Database con una subred. No genera alertas. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Storage/storageAccounts/* | Crear y administrar cuentas de almacenamiento |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-account-key-operator-service-role"></a>Rol de servicio de operador de claves de cuentas de almacenamiento
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite enumerar y regenerar claves de acceso de la cuenta de almacenamiento. |
> | **Id** | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | **Acciones** |  |
> | Microsoft.Storage/storageAccounts/listkeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | Microsoft.Storage/storageAccounts/regeneratekey/action | Regenera las claves de acceso de la cuenta de almacenamiento especificada. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-blob-data-contributor"></a>Colaborador de datos de blobs de almacenamiento
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Lee, escribe y elimina blobs y contenedores de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). |
> | **Id** | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | **Acciones** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Elimina un contenedor. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Devuelve un contenedor o una lista de contenedores. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Modifica los metadatos o las propiedades de un contenedor. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Devuelve una clave de delegación de usuarios para la instancia de Blob service. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Eliminar un blob. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Devuelve un blob o una lista de blobs. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Escribe en un blob. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-blob-data-owner"></a>Propietario de datos de blobs de almacenamiento
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Proporciona acceso total a los contenedores de blobs y los datos de Azure Storage, incluida la asignación de control de acceso POSIX. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). |
> | **Id** | b7e6dc6d-f1e8-4753-8033-0f276bb0955b |
> | **Acciones** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/* | Todos los permisos en los contenedores. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Devuelve una clave de delegación de usuarios para la instancia de Blob service. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/* | Todos los permisos en los blobs. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-blob-data-reader"></a>Lector de datos de blobs de almacenamiento
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Lee y enumera blobs y contenedores de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). |
> | **Id** | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | **Acciones** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Devuelve un contenedor o una lista de contenedores. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Devuelve una clave de delegación de usuarios para la instancia de Blob service. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Devuelve un blob o una lista de blobs. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-blob-delegator"></a>Delegador de Blob Storage
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Obtiene una clave de delegación de usuarios, que se puede usar a continuación para crear una firma de acceso compartido para un contenedor o un blob firmado con credenciales de Azure AD. Para más información, vea [Creación de SAS de delegación de usuarios](https://docs.microsoft.com/rest/api/storageservices/create-user-delegation-sas). |
> | **Id** | db58b8e5-c6ad-4a2a-8342-4190687cbf4a |
> | **Acciones** |  |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Devuelve una clave de delegación de usuarios para la instancia de Blob service. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-file-data-smb-share-contributor"></a>Colaborador de recursos compartidos de SMB de datos de archivos de Storage
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite el acceso de lectura, escritura y eliminación a los archivos y directorios de los recursos compartidos de Azure. Este rol no tiene ningún equivalente integrado en los servidores de archivos de Windows. |
> | **Id** | 0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Devuelve un archivo o una carpeta, o bien una lista de archivos o carpetas. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write | Devuelve el resultado de escribir un archivo o de crear una carpeta. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete | Devuelve el resultado de eliminar un archivo o una carpeta. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-file-data-smb-share-elevated-contributor"></a>Colaborador elevado de recursos compartidos de SMB de datos de archivos de Storage
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite el acceso de lectura, escritura, eliminación y modificación de ACL en los archivos y directorios de los recursos compartidos de Azure. Este rol es equivalente a una ACL de recurso compartido de cambio en los servidores de archivos de Windows. |
> | **Id** | a7264617-510b-434b-a828-9731dc254ea7 |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Devuelve un archivo o una carpeta, o bien una lista de archivos o carpetas. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write | Devuelve el resultado de escribir un archivo o de crear una carpeta. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete | Devuelve el resultado de eliminar un archivo o una carpeta. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action | Devuelve el resultado de modificar el permiso en un archivo o una carpeta. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-file-data-smb-share-reader"></a>Lector de recursos compartidos de SMB de datos de archivos de Storage
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite el acceso de lectura a los archivos y directorios de los recursos compartidos de Azure. Este rol es equivalente a una ACL de recurso compartido de lectura en los servidores de archivos de Windows. |
> | **Id** | aba4ae5f-2193-4029-9191-0cb91df5e314 |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Devuelve un archivo o una carpeta, o bien una lista de archivos o carpetas. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-queue-data-contributor"></a>Colaborador de datos de la cola de Storage
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Lee, escribe y elimina los mensajes de la cola y a la cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). |
> | **Id** | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | **Acciones** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Elimina una cola. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Devuelve una cola o una lista de colas. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/write | Modifica las propiedades o los metadatos de la cola. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Elimina uno o más mensajes de una cola. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Consulta o recupera uno o más mensajes de una cola. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Agrega un mensaje a una cola. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-queue-data-message-processor"></a>Procesador de mensajes de datos de la cola de Storage
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Consulta, recupera y elimina un mensaje de una cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). |
> | **Id** | 8a0f0c08-91a1-4084-bc3d-661d67233fed |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Consulta un mensaje. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Recupera y elimina un mensaje. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-queue-data-message-sender"></a>Emisor de mensajes de datos de la cola de Storage
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Agrega mensaje a una cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). |
> | **Id** | c6a89b2d-59bc-44d0-9896-0f6e12d7b80a |
> | **Acciones** |  |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Agrega un mensaje a una cola. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="storage-queue-data-reader"></a>Lector de datos de la cola de Storage
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Lee y enumera los mensajes de la cola y las colas de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). |
> | **Id** | 19e7f393-937e-4f77-808e-94535e297925 |
> | **Acciones** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Devuelve una cola o una lista de colas. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Consulta o recupera uno o más mensajes de una cola. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="support-request-contributor"></a>Colaborador de la solicitud de soporte técnico
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite crear y administrar solicitudes de soporte técnico. |
> | **Id** | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer autorización |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="traffic-manager-contributor"></a>Colaborador de Traffic Manager
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Le permite administrar perfiles de Traffic Manager, pero no controlar los usuarios que tienen acceso a ellos. |
> | **Id** | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer roles y asignaciones de roles |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Network/trafficManagerProfiles/* |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="user-access-administrator"></a>Administrador de acceso de usuario
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar el acceso de usuario a los recursos de Azure. |
> | **Id** | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Acciones** |  |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | Microsoft.Authorization/* | Administrar la autorización |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="virtual-machine-administrator-login"></a>Inicio de sesión de administrador de Virtual Machine
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Visualización de máquinas virtuales en el portal e inicio de sesión como administrador |
> | **Id** | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | **Acciones** |  |
> | Microsoft.Network/publicIPAddresses/read | Obtiene una definición de la dirección ip pública. |
> | Microsoft.Network/virtualNetworks/read | Obtiene la definición de red virtual |
> | Microsoft.Network/loadBalancers/read | Obtiene una definición del equilibrador de carga |
> | Microsoft.Network/networkInterfaces/read | Obtiene una definición de interfaz de red.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Iniciar sesión en una máquina virtual como usuario habitual |
> | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Iniciar de sesión en una máquina virtual con privilegios de administrador de Windows o de usuario raíz de Linux |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="virtual-machine-contributor"></a>Colaborador de la máquina virtual
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar máquinas virtuales, pero no acceder a ellas, ni tampoco a la red virtual ni la cuenta de almacenamiento a las que están conectadas. |
> | **Id** | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer autorización |
> | Microsoft.Compute/availabilitySets/* | Crear y administrar conjuntos de disponibilidad de proceso |
> | Microsoft.Compute/locations/* | Crear y administrar ubicaciones de proceso |
> | Microsoft.Compute/virtualMachines/* | Crear y administrar máquinas virtuales |
> | Microsoft.Compute/virtualMachineScaleSets/* | Crear y administrar conjuntos de escalado de máquinas virtuales |
> | Microsoft.DevTestLab/schedules/* |  |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Se une a un grupo de direcciones de back-end de una puerta de enlace de aplicaciones. No genera alertas. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Se une a un grupo de direcciones de back-end del equilibrador de carga. No genera alertas. |
> | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Se une a conjuntos NAT de entrada del equilibrador de carga. No genera alertas. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Se une a una regla NAT de entrada del equilibrador de carga. No genera alertas. |
> | Microsoft.Network/loadBalancers/probes/join/action | Permite usar sondeos de un equilibrador de carga. Por ejemplo, con este permiso, la propiedad healthProbe de un conjunto de escalado de máquinas virtuales puede hacer referencia al sondeo. No genera alertas. |
> | Microsoft.Network/loadBalancers/read | Obtiene una definición del equilibrador de carga |
> | Microsoft.Network/locations/* | Crear y administrar ubicaciones de red |
> | Microsoft.Network/networkInterfaces/* | Crear y administrar interfaces de red |
> | Microsoft.Network/networkSecurityGroups/join/action | Se une a un grupo de seguridad de red. No genera alertas. |
> | Microsoft.Network/networkSecurityGroups/read | Obtiene una definición de grupo de seguridad de red |
> | Microsoft.Network/publicIPAddresses/join/action | Se une a una dirección IP pública. No genera alertas. |
> | Microsoft.Network/publicIPAddresses/read | Obtiene una definición de la dirección ip pública. |
> | Microsoft.Network/virtualNetworks/read | Obtiene la definición de red virtual |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Se une a una red virtual. No genera alertas. |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Crea una intención de protección de la copia de seguridad. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Devuelve detalles de objeto del elemento protegido |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Crea un elemento protegido de copia de seguridad |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Devuelve todas las directivas de protección |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Crea una directiva de protección |
> | Microsoft.RecoveryServices/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | Microsoft.RecoveryServices/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/write | La operación Create Vault crea un recurso de Azure del tipo "almacén" |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.SqlVirtualMachine/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | Microsoft.Storage/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="virtual-machine-user-login"></a>Inicio de sesión de usuario de Virtual Machine
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Visualización de máquinas virtuales en el portal e inicio de sesión como usuario normal. |
> | **Id** | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Acciones** |  |
> | Microsoft.Network/publicIPAddresses/read | Obtiene una definición de la dirección ip pública. |
> | Microsoft.Network/virtualNetworks/read | Obtiene la definición de red virtual |
> | Microsoft.Network/loadBalancers/read | Obtiene una definición del equilibrador de carga |
> | Microsoft.Network/networkInterfaces/read | Obtiene una definición de interfaz de red.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Iniciar sesión en una máquina virtual como usuario habitual |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="web-plan-contributor"></a>Colaborador de plan web
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar los planes web para sitios web, pero no acceder a ellos. |
> | **Id** | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer autorización |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Web/serverFarms/* | Crear y administrar granjas de servidores |
> | Microsoft.Web/hostingEnvironments/Join/Action | Se une a App Service Environment. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="website-contributor"></a>Colaborador de sitio web
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Descripción** | Permite administrar los sitios web (no planes web), pero no acceder a ellos. |
> | **Id** | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Acciones** |  |
> | Microsoft.Authorization/*/read | Leer autorización |
> | Microsoft.Insights/alertRules/* | Crear y administrar reglas de alerta de Insights |
> | Microsoft.Insights/components/* | Crear y administrar componentes de Insights |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | Microsoft.Resources/deployments/* | Crear y administrar implementaciones de grupos de recursos |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.Support/* | Crear y administrar incidencias de soporte técnico |
> | Microsoft.Web/certificates/* | Crear y administrar certificados de sitios web |
> | Microsoft.Web/listSitesAssignedToHostName/read | Obtiene los nombres de sitios asignados al nombre de host. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Obtiene las propiedades de un plan de App Service |
> | Microsoft.Web/sites/* | Crear y administrar sitios web (la creación de sitios también requiere permisos de escritura para el plan de App Service asociado) |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

## <a name="next-steps"></a>Pasos siguientes

- [Coincidencia del proveedor de recursos con el servicio](../azure-resource-manager/management/azure-services-resource-providers.md)
- [Roles personalizados en los recursos de Azure](custom-roles.md)
- [Permisos en Azure Security Center](../security-center/security-center-permissions.md)

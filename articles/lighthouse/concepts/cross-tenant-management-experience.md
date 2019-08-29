---
title: Experiencias de administración entre inquilinos con Azure Lighthouse
description: La administración de recursos delegados de Azure habilita una experiencia de administración entre inquilinos.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: f6468e62e9969aa87d51e25ed220bf7d4bed989e
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69971570"
---
# <a name="cross-tenant-management-experiences"></a>Experiencias de administración entre inquilinos

En este artículo se describen los escenarios que usted, como proveedor de servicios, puede usar con la [administración de recursos delegados](../concepts/azure-delegated-resource-management.md) para administrar los recursos de Azure para varios clientes desde su propio inquilino en [Azure Portal](https://portal.azure.com).

> [!NOTE]
> La administración de recursos delegados de Azure también se puede usar en una empresa que tenga varios inquilinos propios para simplificar la administración entre inquilinos.

## <a name="understanding-customer-tenants"></a>Descripción de los inquilinos de cliente

Un inquilino de Azure Active Directory (Azure AD) es una representación de una organización. Se trata de una instancia dedicada de Azure AD que una organización recibe cuando crea una relación con Microsoft al registrarse en Azure, Microsoft 365 u otros servicios. Cada inquilino de Azure AD es distinto e independiente de los demás inquilinos de Azure AD y tiene su propio identificador de inquilino (un GUID). Para más información, consulte [¿Qué es Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)

Normalmente, para administrar los recursos de Azure para un cliente, los proveedores de servicios tendrían que iniciar sesión en Azure Portal mediante una cuenta asociada al inquilino de ese cliente, lo que obliga a que un administrador del inquilino del cliente cree y administre cuentas de usuario. para el proveedor de servicios.

Con la administración de recursos delegados de Azure, el proceso de incorporación especifica los usuarios del inquilino del proveedor de servicios que podrán acceder a las suscripciones, los grupos de recursos y los recursos del inquilino del cliente y administrarlos. A continuación, estos usuarios pueden iniciar sesión en Azure Portal con sus propias credenciales. En Azure Portal, pueden administrar los recursos que pertenecen a todos los clientes a los que tienen acceso. Puede hacerse desde la página [Mis clientes](../how-to/view-manage-customers.md) de Azure Portal o trabajando directamente en el contexto de la suscripción de ese cliente, ya sea en Azure Portal o a través de las API.

La administración de recursos delegados de Azure permite una mayor flexibilidad para administrar los recursos de varios clientes sin tener que iniciar sesión en diferentes cuentas en distintos inquilinos. Por ejemplo, un proveedor de servicios puede tener tres clientes con diferentes responsabilidades y niveles de acceso, como se muestra aquí:

![Tres inquilinos de cliente que muestran las responsabilidades del proveedor de servicios](../media/azure-delegated-resource-management-customer-tenants.jpg)

Con la administración de recursos delegados de Azure, los usuarios autorizados pueden iniciar sesión en el inquilino del proveedor de servicios para acceder a estos recursos, como se muestra aquí:

![Recursos de cliente administrados a través de un inquilino de proveedor de servicios](../media/azure-delegated-resource-management-service-provider-tenant.jpg)

## <a name="supported-services-and-scenarios"></a>Servicios y escenarios admitidos

Actualmente, la experiencia de administración entre inquilinos admite los siguientes escenarios con recursos de cliente delegados:

[Azure Automation](https://docs.microsoft.com/azure/automation/):

- use cuentas de Automation para tener acceso a recursos de clientes delegados y trabajar con ellos

[Azure Backup](https://docs.microsoft.com/azure/backup/):

- haga una copia de seguridad y restaure los datos de los inquilinos de clientes

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com//azure/aks/):

- administre los entornos de Kubernetes hospedados e implemente y administre aplicaciones en contenedores en inquilinos de clientes

[Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/):

- consulte las alertas de las suscripciones delegadas en Azure Portal o mediante programación a través de llamadas a la API REST con la posibilidad de ver alertas en todas las suscripciones
- Ver los detalles del registro de actividad para las suscripciones delegadas
- Log analytics: consulte datos de áreas de trabajo de clientes remotos en varios inquilinos

[Azure Policy](https://docs.microsoft.com/azure/governance/policy/):

- las instantáneas de cumplimiento muestran detalles para las directivas asignadas en las suscripciones delegadas
- Crear y editar definiciones de directivas en una suscripción delegada
- Asignar definiciones de directivas definidas por el cliente en una suscripción delegada
- Los clientes ven las directivas creadas por el proveedor de servicios junto con las directivas que hayan creado ellos mismos.
- Se pueden corregir las asignaciones deployIfNotExists dentro de los inquilinos de cliente si el cliente ha configurado la identidad administrada y *roleDefinitionIds* para esa asignación de directiva.

[Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/)

- Ahora incluye el identificador de inquilino en los resultados de la consulta devueltos, lo que le permite identificar si una suscripción pertenece al inquilino del cliente o del proveedor de servicios.

[Azure Security Center](https://docs.microsoft.com/azure/security-center/):

- Visibilidad entre inquilinos
  - Supervise el cumplimiento de las directivas de seguridad y garantice la cobertura de seguridad en todos los recursos de los inquilinos.
  - Supervisión continua del cumplimiento normativo en varios clientes en una sola vista
  - Supervisión, evaluación de prioridades y priorización de recomendaciones de seguridad procesables con el cálculo de puntuaciones seguras
- Administración de posiciones de seguridad entre inquilinos
  - Administrar directivas de seguridad
  - Acciones sobre los recursos que no cumplen las recomendaciones de seguridad procesables
  - Recopilación y almacenamiento de datos relacionados con la seguridad
- Detección y protección de amenazas entre inquilinos
  - Detección de amenazas entre los recursos de los inquilinos
  - Aplicación de controles de protección contra amenazas avanzados, como el acceso a VM Just-in-Time (JIT)
  - Protección de la configuración de grupos de seguridad de red con refuerzo de redes adaptable
  - Comprobación de que los servidores ejecutan solo las aplicaciones y los procesos que deben con controles de aplicaciones adaptables
  - Supervisar los cambios en archivos importantes y entradas del registro con la supervisión de la integridad de los archivos (FIM)

[Azure Service Health](https://docs.microsoft.com/azure/service-health/):

- Supervisar el estado de los recursos del cliente con Azure Resource Health
- Hacer un seguimiento del estado de los servicios de Azure que usan los clientes

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/):

- Administrar opciones de recuperación ante desastres para máquinas virtuales de Azure en inquilinos de clientes (tenga en cuenta que no puede usar cuentas de ejecución para copiar extensiones de VM)

[Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/):

- Usar extensiones de máquina virtual para la configuración posterior a la implementación y las tareas de automatización en VM de Azure en los inquilinos del cliente
- Uso de diagnósticos de arranque para solucionar problemas de VM de Azure en inquilinos de clientes
- Acceso a VM con la consola serie en inquilinos de clientes
- Tenga en cuenta que no puede usar Azure Active Directory para el inicio de sesión remoto en una VM y no puede integrar una VM con una instancia de Key Vault para contraseñas, secretos o claves criptográficas para el cifrado de discos.

[Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/):

- Implementación y administración de redes virtuales y tarjetas de interfaz de red virtual (VNIC) en inquilinos de cliente

Solicitudes de soporte técnico:

- Abra solicitudes de soporte técnico para los recursos delegados en la hoja **Ayuda y soporte técnico** de Azure Portal (selección del plan de soporte técnico disponible para el ámbito delegado)

## <a name="current-limitations"></a>Limitaciones actuales
Con todos los escenarios, tenga en cuenta las siguientes limitaciones actuales:

- Las solicitudes controladas por Azure Resource Manager pueden realizarse mediante la administración de recursos delegados de Azure. Los URI de operación de estas solicitudes comienzan por `https://management.azure.com`. Sin embargo, las solicitudes que se administran mediante una instancia de un tipo de recurso (por ejemplo, acceso a los secretos de KeyVault o acceso a datos de almacenamiento) no son compatibles con la administración de recursos delegados de Azure. Los URI de la operación para estas solicitudes suelen empezar con una dirección que es única para la instancia, como `https://myaccount.blob.core.windows.net` o `https://mykeyvault.vault.azure.net/`. Esta última también suelen ser operaciones de datos, en lugar de operaciones de administración. 
- Las asignaciones de roles deben usar [roles integrados](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) de control de acceso basado en rol (RBAC). Actualmente, todos los roles integrados se admiten con la administración de recursos delegados de Azure, excepto el rol Propietario, Administrador de acceso de usuario y los roles integrados con el permiso [DataActions](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#dataactions). Tampoco se admiten los roles personalizados y [los roles de administrador de la suscripción clásica](https://docs.microsoft.com/azure/role-based-access-control/classic-administrators).
- Actualmente, no se puede incorporar una suscripción (o un grupo de recursos de una suscripción) para la administración de recursos delegados de Azure si la suscripción usa Azure Databricks. Del mismo modo, si una suscripción se ha registrado para la incorporación con el proveedor de recursos **Microsoft.ManagedServices**, en este momento, no podrá crear un área de trabajo de Databricks para la suscripción.

## <a name="using-apis-and-management-tools-with-cross-tenant-management"></a>Uso de API y herramientas de administración con administración entre inquilinos

En el caso de los servicios y escenarios admitidos mencionados anteriormente, puede realizar tareas de administración directamente en el portal o mediante API y herramientas de administración (como la CLI de Azure y Azure PowerShell). Todas las API existentes se pueden usar al trabajar con recursos delegados (para los servicios que se admiten).

También hay API específicas para realizar tareas de administración de recursos delegados de Azure. Para obtener más información, consulte la sección **Referencia**.


## <a name="next-steps"></a>Pasos siguientes

- Incorpore los clientes a la administración de recursos delegados de Azure, ya sea [mediante plantillas de Azure Resource Manager](../how-to/onboard-customer.md) o [publicando una oferta de servicios administrados privada o pública en Azure Marketplace](../how-to/publish-managed-services-offers.md).
- Puede [ver y administrar clientes](../how-to/view-manage-customers.md) desde **Mis clientes**, en Azure Portal.

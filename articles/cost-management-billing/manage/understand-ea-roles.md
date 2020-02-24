---
title: Roles de administrador para Enterprise en Azure | Microsoft Docs
description: Conozca los roles de administrador de empresa en Azure.
author: bandersmsft
ms.reviewer: adwise
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: banders
ms.openlocfilehash: 9fab4d8ba0cf2e6f684a1b9de177084f8ce31604
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462197"
---
# <a name="understand-azure-enterprise-agreement-administrative-roles-in-azure"></a>Roles administrativos del Contrato Enterprise de Azure en Azure

Para ayudar a administrar el uso y el gasto de su organización, los clientes de Azure con un Contrato Enterprise (EA) pueden asignar cinco roles de administrador diferentes:

- Administrador de empresa
- Administrador de empresa (solo lectura)<sup>1</sup>
- Administrador de departamentos
- Administrador de departamento (solo lectura)
- Propietario de cuenta

<sup>1</sup> El contacto de facturación del Contrato Enterprise estará bajo este rol.

Estos roles son específicos de la administración de Contratos Enterprise de Azure y son adicionales a los roles integrados que tiene Azure para controlar el acceso a los recursos. Para más información, consulte [Roles integrados en los recursos de Azure](../../role-based-access-control/built-in-roles.md).

En las secciones siguientes se describen las limitaciones y funcionalidades de cada rol.

## <a name="user-limit-for-admin-roles"></a>Límite de usuarios para roles de administrador

|Role| Límite de usuarios|
|---|---|
|Administrador de empresa|Sin límite|
|Administrador de empresa (solo lectura)|Sin límite|
|Administrador de departamentos|Sin límite|
|Administrador de departamento (solo lectura)|Sin límite|
|Propietario de cuenta|Uno por cuenta<sup>2</sup>|

<sup>2</sup> Cada cuenta requiere una única cuenta de Microsoft o una cuenta profesional o educativa.

## <a name="organization-structure-and-permissions-by-role"></a>Estructura de organización y permisos por rol

|Tareas| Administrador de empresa|Administrador de empresa (solo lectura)|Administrador de departamentos|Administrador de departamento (solo lectura)|Propietario de cuenta|
|---|---|---|---|---|---|
|Ver administradores de empresa|✔|✔|✘|✘|✘|
|Agregar o quitar administradores de empresa|✔|✘|✘|✘|✘|
|Ver contactos de notificación<sup>3</sup> |✔|✔|✘|✘|✘|
|Agregar o quitar contactos de notificación<sup>3</sup> |✔|✘|✘|✘|✘|
|Crear y administrar departamentos |✔|✘|✘|✘|✘|
|Ver administradores de departamento|✔|✔|✔|✔|✘|
|Agregar o quitar administradores de departamento|✔|✘|✔|✘|✘|
|Ver cuentas en la inscripción |✔|✔|✔<sup>4</sup>|✔<sup>4</sup>|✘|
|Agregar cuentas a la inscripción y cambiar el propietario de la cuenta|✔|✘|✔<sup>4</sup>|✘|✘|
|Crear y administrar suscripciones y permisos de suscripción|✘|✘|✘|✘|✔|

- <sup>3</sup>A los contactos de notificación se les envían comunicaciones por correo electrónico sobre el Contrato Enterprise de Azure.
- <sup>4</sup> La tarea se limita a las cuentas de su departamento.


## <a name="usage-and-costs-access-by-role"></a>Acceso de uso y costos por rol

|Tareas| Administrador de empresa|Administrador de empresa (solo lectura)|Administrador de departamentos|Administrador de departamento (solo lectura) |Propietario de cuenta|
|---|---|---|---|---|---|
|Ver saldo de crédito, incluido el compromiso monetario|✔|✔|✘|✘|✘|
|Ver cuotas de gastos de departamento|✔|✔|✘|✘|✘|
|Establecer cuotas de gasto de departamento|✔|✘|✘|✘|✘|
|Ver la hoja de precios de EA de la organización|✔|✔|✘|✘|✘|
|Ver los detalles de uso y costo|✔|✔|✔<sup>5</sup>|✔<sup>5</sup>|✔<sup>6</sup>|
|Administrar recursos en Azure Portal|✘|✘|✘|✘|✔|

- <sup>5</sup> Requiere que el administrador de empresa habilite la directiva de **visualización de cargos del administrador de departamento** en Enterprise Portal. El administrador de departamento puede entonces ver los detalles de costo del departamento.
- <sup>6</sup> Requiere que el administrador de empresa habilite la directiva de **visualización de cargos del propietario de la cuenta** en Enterprise Portal. El propietario de la cuenta puede ver entonces los detalles del costo de la cuenta.


## <a name="pricing-in-azure-portal"></a>Precios en Azure Portal

Es posible que vea un precio diferente en Azure Portal según su rol administrativo y cómo el administrador de empresa haya establecido las directivas de visualización de los cargos. Las dos directivas de Enterprise Portal que afectan a los precios que ve en Azure Portal son:

- Visualización de cargos del AD
- Visualización de cargos del PC

Para saber cómo establecer estas directivas, consulte [Administración del acceso a la información de facturación en Azure](manage-billing-access.md).

En la tabla siguiente se muestra la relación entre los roles de administrador del Contrato Enterprise, la directiva de visualización de cargos, el control de acceso basado en rol (RBAC) en Azure Portal y los precios que ve en Azure Portal. El administrador de empresa siempre ve los detalles de uso según los precios de EA de la organización. Sin embargo, el administrador de departamento y el propietario de la cuenta tienen vistas de precios diferentes según la directiva de visualización de cargos y su rol de RBAC. El rol de administrador de departamento que se muestra en la tabla siguiente hace referencia a los roles de administrador de departamento y administrador de departamento (solo lectura).

|Rol de administrador de Contrato Enterprise|Directiva de visualización de cargos por rol|Rol de RBAC|Vista de precios|
|---|---|---|---|
|Propietario de la cuenta o administrador de departamento|✔ Habilitado|Propietario|Precios de EA de la organización|
|Propietario de la cuenta o administrador de departamento|✘ Deshabilitado|Propietario|Precio de venta|
|Propietario de la cuenta o administrador de departamento|✔ Habilitado |None|No hay precios|
|Propietario de la cuenta o administrador de departamento|✘ Deshabilitado |None|No hay precios|
|None|No aplicable |Propietario|Precio de venta|

El rol de administrador de Enterprise y las directivas de visualización de cargos se establecen en Enterprise Portal. El rol de RBAC puede actualizarse en Azure Portal. Para saber más, vea [Administración del acceso mediante RBAC y Azure Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Pasos siguientes

- [Administración del acceso a la información de facturación de Azure](manage-billing-access.md)
- [Administración del acceso mediante RBAC y Azure Portal](../../role-based-access-control/role-assignments-portal.md)
- [Roles integrados en los recursos de Azure](../../role-based-access-control/built-in-roles.md)

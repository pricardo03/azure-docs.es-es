---
title: Descripción de roles administrativos de facturación de contratos de cliente de Microsoft - Azure
description: Obtenga información acerca de las funciones de facturación para la facturación de las cuentas de Azure para contratos del cliente de Microsoft.
services: billing
documentationcenter: ''
author: amberbhargava
manager: amberbhargava
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: 780870cc71e95507a52ba6a9338026f895a96ac1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60370974"
---
# <a name="understand-microsoft-customer-agreement-administrative-roles-in-azure"></a>Comprender las funciones administrativas de contrato de cliente de Microsoft en Azure

Para administrar su cuenta de facturación de un contrato de cliente de Microsoft, use las funciones descritas en las secciones siguientes. Estos roles son adicionales a los roles integrados de que Azure tiene que controlar el acceso a los recursos. Para más información, consulte [Roles integrados en los recursos de Azure](../role-based-access-control/built-in-roles.md).

En este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. Compruebe si tiene acceso a un contrato de cliente de Microsoft.

## <a name="billing-role-definitions"></a>Definiciones de roles de facturación

En la tabla siguiente describe las funciones de facturación que usa para administrar su cuenta de facturación, perfiles de facturación y secciones de la factura.

|Rol|DESCRIPCIÓN|
|---|---|
|Propietario de la cuenta de facturación|Administrar todo el contenido de la cuenta de facturación|
|Colaborador de la cuenta de facturación|Administrar todo, excepto los permisos en la cuenta de facturación|
|Lector de la cuenta de facturación|Vista de solo lectura de todo el contenido en la cuenta de facturación|
|Propietario del perfil de facturación|Administrar todo el contenido para el perfil de facturación|
|Colaborador del perfil de facturación|Administrar todo, excepto los permisos en el perfil de facturación|
|Lector del perfil de facturación|Vista de solo lectura de todo el contenido en el perfil de facturación|
|Administrador de facturación|Ver y pagar las facturas para el perfil de facturación|
|Propietario de las secciones de factura|Administrar todo el contenido de la sección de factura|
|Colaborador de las secciones de factura|Administrar todo, excepto los permisos en la sección de factura|
|Lector de las secciones de factura|Vista de solo lectura de todo el contenido en la sección de factura|
|Creador de la suscripción de Azure|Crear suscripciones de Azure|

## <a name="billing-account-roles-and-tasks"></a>Tareas y roles de la cuenta de facturación

Una cuenta de facturación le permite administrar la facturación de su organización. Use la cuenta de facturación para organizar los costos, los cargos de monitor y facturas y controlar el acceso de facturación para su organización. Para obtener más información, consulte [comprender la cuenta de facturación](billing-mca-overview.md#understand-billing-account).

Las siguientes tablas muestran qué rol debe completar las tareas en el contexto de la cuenta de facturación.

### <a name="manage-billing-account-permissions-and-properties"></a>Administrar las propiedades y permisos de cuenta de facturación

|Tarea|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación|
|---|---|---|---|
|Ver los permisos existentes para la cuenta de facturación|✔|✔|✔|
|Conceder a otros usuarios permisos para ver y administrar la cuenta de facturación|✔|✘|✘|
|Propiedades de cuenta de facturación de la vista como el nombre de la compañía, dirección etc.|✔|✔|✔|

### <a name="manage-billing-profiles-for-billing-account"></a>Administrar perfiles de facturación para la cuenta de facturación

|Tarea|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación|
|---|---|---|---|
|Ver todos los perfiles de facturación de la cuenta|✔|✔|✔|

### <a name="manage-invoices-for-billing-account"></a>Administrar las facturas de la cuenta de facturación

|Tarea|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación|
|---|---|---|---|
|Ver todas las facturas de la cuenta|✔|✔|✔|
|Descargar facturas, los archivos de uso y los cargos de Azure, las hojas de precios y documentos en la cuenta de impuestos|✔|✔|✔|

### <a name="manage-invoice-sections-for-billing-account"></a>Administrar las secciones de la factura para la cuenta de facturación

|Tarea|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación|
|---|---|---|---|
|Ver todas las secciones de la factura de la cuenta|✔|✔|✔|

### <a name="manage-transactions-for-billing-account"></a>Administrar las transacciones para la cuenta de facturación

|Tarea|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación|
|---|---|---|---|
|Ver todas las transacciones de facturación para la cuenta|✔|✔|✔|
|Ver todos los productos que ha comprado para la cuenta|✔|✔|✔|

### <a name="manage-subscriptions-for-billing-account"></a>Administrar suscripciones para la cuenta de facturación

|Tarea|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación|
|---|---|---|---|
|Ver todas las suscripciones de Azure en la cuenta de facturación|✔|✔|✔|

## <a name="billing-profile-roles-and-tasks"></a>Tareas y roles del perfil de facturación

Un perfil de facturación le permite administrar sus facturas y los métodos de pago. Se genera una factura mensual de las suscripciones de Azure y otros productos adquiridos mediante el perfil de facturación. Utilice los métodos de pago para pagar la factura. Para obtener más información, consulte [perfiles facturación](billing-mca-overview.md#understand-billing-profiles).

Las siguientes tablas muestran qué rol debe completar las tareas en el contexto del perfil de facturación.

### <a name="manage-billing-profile-permissions-properties-and-policies"></a>Administrar las directivas, propiedades y permisos de perfil de facturación

|Tarea|Propietario del perfil de facturación|Colaborador del perfil de facturación|Lector del perfil de facturación|Administrador de facturas|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación
|---|---|---|---|---|---|---|---|
|Ver los permisos existentes para el perfil de facturación|✔|✔|✔|✔|✔|✔|✔|
|Conceder a otros usuarios permisos para ver y administrar el perfil de facturación|✔|✘|✘|✘|✘|✘|✘|
|Propiedades de perfil de facturación de la vista como el número de pedido de compra, preferencias de la factura de correo electrónico y mucho más|✔|✔|✔|✔|✔|✔|✔|
|Actualizar propiedades de perfil de facturación |✔|✔|✘|✘|✘|✘|✘|
|Ver directivas aplicadas en el perfil de facturación, como habilitar las compras de reservas de Azure, habilite las compras de Azure marketplace etc.|✔|✔|✔|✔|✔|✔|✔|
|Aplicar directivas en el perfil de facturación |✔|✔|✘|✘|✘|✘|✘|

### <a name="manage-invoices-for-billing-profile"></a>Administración de facturas para el perfil de facturación

|Tarea|Propietario del perfil de facturación|Colaborador del perfil de facturación|Lector del perfil de facturación|Administrador de facturas|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación
|---|---|---|---|---|---|---|---|
|Ver todas las facturas para el perfil de facturación|✔|✔|✔|✔|✔|✔|✔|
|Descargar facturas, los archivos de uso y los cargos de Azure, las hojas de precios y documentos para el perfil de facturación de impuestos|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-invoice-sections-for-billing-profile"></a>Administrar las secciones de la factura para el perfil de facturación

|Tarea|Propietario del perfil de facturación|Colaborador del perfil de facturación|Lector del perfil de facturación|Administrador de facturas|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación
|---|---|---|---|---|---|---|---|
|Ver todas las secciones de la factura para el perfil de facturación|✔|✔|✔|✔|✔|✔|✔|
|Crear nueva sección de factura para el perfil de facturación|✔|✔|✘|✘|✘|✘|✘|

### <a name="manage-transactions-for-billing-profile"></a>Administrar las transacciones para el perfil de facturación

|Tarea|Propietario del perfil de facturación|Colaborador del perfil de facturación|Lector del perfil de facturación|Administrador de facturas|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación
|---|---|---|---|---|---|---|---|
|Ver todas las transacciones de facturación para el perfil de facturación|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-payment-methods-for-billing-profile"></a>Administrar métodos de pago para el perfil de facturación

|Tarea|Propietario del perfil de facturación|Colaborador del perfil de facturación|Lector del perfil de facturación|Administrador de facturas|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación
|---|---|---|---|---|---|---|---|
|Ver los métodos de pago para el perfil de facturación|✔|✔|✔|✔|✔|✔|✔|
|Realizar un seguimiento del saldo de créditos de Azure para el perfil de facturación|✔|✔|✔|✔|✔|✔|✔|

### <a name="manage-subscriptions-for-billing-profile"></a>Administrar suscripciones para el perfil de facturación

|Tarea|Propietario del perfil de facturación|Colaborador del perfil de facturación|Lector del perfil de facturación|Administrador de facturas|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación
|---|---|---|---|---|---|---|---|
|Ver todas las suscripciones de Azure para el perfil de facturación|✔|✔|✔|✔|✔|✔|✔|

## <a name="invoice-section-roles-and-tasks"></a>Tareas y roles de la sección de factura

Una sección de factura le permite organizar los costos en su factura. Puede crear una sección para organizar los costos por departamento, entorno de desarrollo, o según las necesidades de su organización. Asignar a otros usuarios permiso para crear suscripciones de Azure para la sección. Los cargos de uso y las compras de suscripciones a continuación, el programa en la sección de la factura. Para obtener más información, consulte [sección de factura entender](billing-mca-overview.md#understand-invoice-sections).

Las siguientes tablas muestran qué rol debe completar las tareas en el contexto de las secciones de la factura.

### <a name="manage-invoice-section-permissions-and-properties"></a>Administrar permisos de la sección de factura y propiedades

|Tareas|Propietario de las secciones de factura|Colaborador de las secciones de factura|Lector de las secciones de factura|Creador de la suscripción de Azure|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación | |
|---|---|---|---|---|---|---|---|---|
|Ver todos los permisos en la sección de factura|✔|✔|✔|✔|✔|✔|✔| |
|Conceder a otros usuarios permisos para ver y administrar la sección de factura|✔|✘|✘|✘|✘|✘|✘| |
|Ver las propiedades de la sección de factura|✔|✔|✔|✔|✔|✔|✔| |
|Actualizar propiedades de la sección de factura|✔|✔|✘|✘|✘|✘|✘|✘|

### <a name="manage-products-for-invoice-section"></a>Administración de productos para la sección de factura

|Tareas|Propietario de las secciones de factura|Colaborador de las secciones de factura|Lector de las secciones de factura|Creador de la suscripción de Azure|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación
|---|---|---|---|---|---|---|---|
|Ver todos los productos que ha comprado en la sección de factura|✔|✔|✔|✘|✔|✔|✔|
|Administrar la facturación para los productos para la sección de factura como cancelar, desactive la renovación automática etc.|✔|✔|✘|✘|✘|✘|✘|
|Sección de la factura de cambio para los productos|✔|✔|✘|✘|✘|✘|✘|

### <a name="manage-subscriptions-for-invoice-section"></a>Administrar suscripciones para la sección de factura

|Tareas|Propietario de las secciones de factura|Colaborador de las secciones de factura|Lector de las secciones de factura|Creador de la suscripción de Azure|Propietario de la cuenta de facturación|Colaborador de la cuenta de facturación|Lector de la cuenta de facturación
|---|---|---|---|---|---|---|---|
|Ver todas las suscripciones de Azure para la sección de factura|✔|✔|✔|✘|✔|✔|✔|
|Sección de la factura de cambio para las suscripciones|✔|✔|✘|✘|✘|✘|✘|
|Solicitar la propiedad de facturación de suscripciones de los usuarios de otras cuentas de facturación|✔|✔|✘|✘|✘|✘|✘|

## <a name="subscription-billing-roles-and-tasks"></a>Tareas y roles de facturación de la suscripción

La siguiente tabla muestra qué rol debe completar las tareas en el contexto de una suscripción.

|Tareas|Propietario de las secciones de factura|Colaborador de las secciones de factura|Lector de las secciones de factura|Creador de la suscripción de Azure|
|---|---|---|---|---|
|Crear suscripciones de Azure|✔|✔|✘|✔|
|Centro de costo de actualización de la suscripción|✔|✔|✘|✘|
|Sección de factura de cambio de la suscripción|✔|✔|✘|✘|

## <a name="manage-billing-roles-in-the-azure-portal"></a>Administrar roles de facturación en el portal de Azure

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-understand-mca-roles/billing-search-cost-management-billing.png)

3. Seleccione **control de acceso (IAM)** en un ámbito como cuenta de facturación, perfil de facturación o sección de factura, donde desea dar acceso.

4. La página de Access control (IAM) enumera los usuarios y grupos asignados a cada rol para ese ámbito.

   ![Captura de pantalla que muestra la lista de administradores para la cuenta de facturación](./media/billing-understand-mca-roles/billing-list-admins.png)

5. Para dar acceso a un usuario, seleccione **agregar** desde la parte superior de la página. En la lista desplegable de rol, seleccione un rol. Escriba la dirección de correo electrónico del usuario al que desea dar acceso. Seleccione **guardar** para asignar el rol.

   ![Captura de pantalla que muestra cómo agregar un administrador a una cuenta de facturación](./media/billing-understand-mca-roles/billing-add-admin.png)

6. Para quitar el acceso para un usuario, seleccione el usuario con la asignación de roles que desea quitar. Seleccione Quitar.

   ![Captura de pantalla que se muestra cómo eliminar un administrador de una cuenta de facturación](./media/billing-understand-mca-roles/billing-remove-admin.png)

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobar el acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico
Si necesita ayuda, [póngase en contacto con soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener información sobre su cuenta de facturación:

- [Introducción a su cuenta de facturación para el contrato de cliente de Microsoft](billing-mca-overview.md)
- [Crear una suscripción de Azure para su cuenta de facturación para el contrato de cliente de Microsoft](billing-mca-create-subscription.md)

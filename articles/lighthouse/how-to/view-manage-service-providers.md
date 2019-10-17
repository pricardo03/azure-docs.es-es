---
title: Visualización y administración de proveedores de servicios
description: Los clientes pueden usar la página proveedores de servicios en Azure Portal para ver información acerca de los proveedores de servicios, ofertas de proveedores de servicios y recursos delegados.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 0a7d39d95e35e30a16ce11db5b942024b6890438
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286593"
---
# <a name="view-and-manage-service-providers"></a>Visualización y administración de proveedores de servicios

Los clientes pueden usar la página **Proveedores de servicios** de [Azure Portal](https://portal.azure.com) para ver información sobre proveedores de servicios y ofertas de proveedores de servicios, delegar recursos específicos mediante la [administración de recursos delegados de Azure](../concepts/azure-delegated-resource-management.md) y comprar ofertas de proveedores de servicios adicionales. Aunque aquí nos referiremos a los proveedores de servicios y clientes, las empresas que administren varios inquilinos pueden usar el mismo proceso para consolidar su experiencia de administración.

Para acceder a la página **Proveedores de servicios** de Azure Portal, el cliente puede seleccionar **Todos los servicios** y, a continuación, buscar **Proveedores de servicios** y seleccionar esta opción. También pueden encontrarla escribiendo "Proveedores de servicios" en el cuadro de búsqueda situado cerca de la parte superior de Azure Portal.

Tenga en cuenta que la página **Proveedores de servicios** solo muestra información sobre los proveedores de servicios que tienen acceso a las suscripciones o grupos de recursos del cliente mediante la administración de recursos delegados de Azure. Si un cliente trabaja con proveedores de servicios adicionales que no usan la administración de recursos delegados de Azure para acceder a los recursos del cliente, la información sobre esos proveedores de servicios no se muestra aquí.

> [!NOTE]
> Para ver información sobre sus clientes, los proveedores de servicios pueden desplazarse hasta la sección **Mis clientes** de Azure Portal. Para obtener más información, consulte [View and manage customers and delegated resources](view-manage-customers.md) (Ver y administrar clientes y recursos delegados).

## <a name="view-service-provider-details"></a>Ver detalles del proveedor de servicios

Para ver información acerca de los proveedores de servicios con los que trabaja un cliente, pueden seleccionar **Ofertas del proveedor** en el lado izquierdo de la página **Proveedores de servicios**.

Para cada oferta de proveedor de servicios, el cliente verá el nombre del proveedor de servicios y la oferta asociada a él, junto con el nombre que el cliente especificó durante el proceso de incorporación.

En la columna **Delegaciones**, el cliente ve el número de suscripciones o grupos de recursos que se han delegado en el proveedor de servicios de esa oferta. El proveedor de servicios podrá tener acceso a estas suscripciones o grupos de recursos y administrarlos de acuerdo con los niveles de acceso especificados en la oferta.

## <a name="delegate-resources"></a>Recursos delegados

Para que un proveedor de servicios pueda acceder a los recursos de un cliente y administrarlos, se deben delegar. Si un cliente ha aceptado una oferta pero aún no ha delegado los recursos, verá una nota en la parte superior de la sección **Ofertas de proveedor**. Esto permite al cliente saber que deben tomar medidas para que el proveedor de servicios pueda acceder a los recursos del cliente.

Para delegar suscripciones o grupos de recursos:

1. Active la casilla de la fila que contiene el proveedor de servicios, la oferta y el nombre. A continuación, seleccione **Delegar recursos** en la parte superior de la pantalla.
1. En la sección **Detalles de la oferta** de la página **Delegar recursos**, revise los detalles sobre el proveedor de servicios y la oferta. Para revisar las asignaciones de roles de la oferta, seleccione **Hacer clic aquí para ver los detalles de la oferta seleccionada**.
1. En la sección **Delegar**, seleccione **Delegar suscripciones** o **Delegar grupos de recursos**.
1. Elija las suscripciones o los grupos de recursos que quiera delegar para esta oferta y, después, seleccione **Agregar**.
1. Active la casilla situada en la parte inferior de la página para confirmar que quiere conceder a este proveedor de servicios acceso a los recursos que ha seleccionado y, después, seleccione **Delegar**.

## <a name="add-or-remove-service-provider-offers"></a>Agregar o quitar ofertas del proveedor de servicios

Un cliente puede agregar una nueva oferta del proveedor de servicios desde la página **Ofertas del proveedor**. Para ello, debe seleccionar **Agregar oferta**. El proveedor de servicios debe haber publicado una oferta para este cliente. Después, el cliente puede seleccionar esa oferta en la pantalla **Ofertas privadas** y, a continuación, seleccionar **Crear**.

Si el cliente quiere quitar una oferta del proveedor de servicios, puede seleccionar el icono de la papelera en la fila de esa oferta. Después de confirmar la eliminación, ese proveedor de servicios ya no tendrá acceso a los recursos del cliente que anteriormente se delegaron para esa oferta.

## <a name="view-delegations"></a>Ver delegaciones

Las delegaciones representan las asignaciones de roles que conceden permisos al proveedor del servicio para los recursos que un cliente ha delegado. Para ver esta información, seleccione **Delegaciones** en el lado izquierdo de la página **Proveedores de servicios**.

Los filtros de la parte superior de la página le permiten ordenar y agrupar la información de delegación o filtrar por clientes, ofertas o palabras clave específicos.

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de [Azure Lighthouse](../overview.md).
- Descubra cómo los proveedores de servicios pueden [ver y administrar sus clientes](view-manage-customers.md) desde **Mis clientes** en Azure Portal.
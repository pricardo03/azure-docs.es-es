---
title: Cómo se aplican descuentos de reserva a Azure SQL Data Warehouse | Microsoft Docs
description: Obtenga información sobre cómo se aplican descuentos de reserva a Azure SQL Data Warehouse para ayudar a ahorrar dinero.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: 10e19377d31489cd19465fe6171ffb530bd58c28
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60918428"
---
# <a name="how-reservation-discounts-apply-to-azure-sql-data-warehouse"></a>Cómo se aplican descuentos de reserva a Azure SQL Data Warehouse

Después de comprar capacidad reservada de Azure SQL Data Warehouse, el descuento de reserva se aplica automáticamente a almacenes de datos que existen en esa región. Se aplica el descuento de reserva para el uso emitido por el medidor de cDWU SQL Data Warehouse. Las redes y almacenamiento se cobran tarifas de pago por uso.

## <a name="reservation-discount-application"></a>Aplicación del descuento de reserva

El descuento de capacidad reservada de SQL Data Warehouse se aplica a la ejecución de los almacenes de cada hora. Si no tiene un almacén que se implementó durante una hora, se pierde la capacidad reservada para esa hora. No mantienen.

Después de la compra, la reserva de compra se asocia a uso de SQL Data Warehouse emitido mediante la ejecución de los almacenamientos en cualquier momento en el tiempo. Si apaga algunos almacenes, a continuación, reserva automáticamente se aplican descuentos a otros almacenes de búsqueda de coincidencias.

Para los almacenes que no se ejecutan durante una hora completa, la reserva se aplica automáticamente a otras instancias de búsqueda de coincidencias en esa hora.

## <a name="discount-examples"></a>Ejemplos de descuento

Los ejemplos siguientes muestran cómo se aplica el descuento de capacidad reservada de SQL Data Warehouse, dependiendo de las implementaciones.

- **Ejemplo 1**: Compra 5 unidades de capacidad de reserva de cDWU 100. Ejecutar una instancia de SQL Data Warehouse de DW1500c durante una hora. En este caso, se genera el uso de 15 unidades del uso de cDWU 100. El descuento de reserva se aplica a las 5 unidades que ha usado. Se le cobrará con tarifas de pago por uso para las restantes 10 unidades del uso de cDWU 100 que usó.

- **Ejemplo 2**: Compra 5 unidades de capacidad de reserva de cDWU 100. Ejecutar dos instancias de SQL Data Warehouse de DW100c durante una hora. En este caso, se genera dos eventos de uso 1 unidad de uso de cDWU 100. Ambos eventos de uso consiga descuentos de capacidad reservada. El 3 unidades de capacidad de reserva de cDWU 100 restantes se pierde y no se mantienen para un uso futuro.

- **Ejemplo 3**: Comprar 1 unidad de capacidad de reserva de cDWU 100. Ejecutar dos instancias de DW100c SQL Data Warehouse. Cada uno se ejecuta durante 30 minutos. En este caso, ambos eventos de uso consiga descuentos de capacidad reservada. Ningún uso se cobra con las tarifas de pago por uso.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

- Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [¿Qué es Azure Reservations?](billing-save-compute-costs-reservations.md)
- [Ver las transacciones de reserva](billing-view-reservations.md)
- [Obtener las transacciones de reserva y la utilización a través de API](billing-reservation-apis.md)
- [Administrar las reservas de direcciones](billing-manage-reserved-vm-instance.md)

---
title: Entidades de datos
description: Una introducción a las entidades de datos.
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Ricardo.Villalobos
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: a382f9b3ce08bba266311c2cc1d5f868f1bc3143
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64934922"
---
# <a name="data-entities"></a>Entidades de datos

En este artículo se definen las entidades de datos y se proporciona información general acerca de ellas. Incluye información acerca de las funcionalidades de las entidades de datos, los escenarios que admiten, las categorías que se usan para ellas y los métodos para crearlas.

## <a name="overview"></a>Información general

Una entidad de datos es una abstracción de la implementación física de las tablas de bases de datos. Por ejemplo, en las tablas normalizadas, gran parte de los datos de cada cliente pueden almacenarse en una tabla de clientes y el resto pueden estar dispersos por un pequeño conjunto de tablas relacionadas. En este caso, la entidad de datos del concepto de cliente aparece como una vista no normalizada, en la que cada fila contiene todos los datos de la tabla de clientes y las tablas relacionadas. Una entidad de datos encapsula un concepto empresarial en un formato que facilita la integración y desarrollo. La naturaleza abstracta de una entidad de datos puede simplificar el desarrollo y la personalización de las aplicaciones. Posteriormente, la abstracción también aísla el código de la aplicación de la inevitable renovación de las tablas físicas entre versiones.

Resumiendo: una entidad de datos proporciona una abstracción conceptual y una encapsulación (vista no normalizada) de los esquemas de la tabla subyacente para representar los conceptos y las funcionalidades de los datos clave.

## <a name="capabilities"></a>Capacidades

Una entidad de datos tiene las siguientes funcionalidades:

- Reemplaza los conceptos de divergentes y fragmentados de AXD, entidades del marco de importación y exportación de datos (DIXF), y consultas de funciones agregadas con un concepto único.
- Proporciona un sola pila para capturar la lógica de negocios y para habilitar escenarios como la importación y exportación, la integración y la programación.
- Se convierte en el mecanismo principal para exportar e importar paquetes de datos para escenarios de datos de demostración de administración del ciclo de vida de las aplicaciones (ALM).
- Se puede exponer como servicios de OData y, después, usar en escenarios de integración sincrónica de estilo tabular e integraciones de Microsoft Office.

Para más información, consulte [Entidades de datos](https://docs.microsoft.com/dynamics365/operations/dev-itpro/data-entities/data-entities).

---
title: Elección del plan de tarifa adecuado | Microsoft Azure Maps
description: En este artículo, obtendrá información sobre los planes de tarifa que ofrece Microsoft Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 8764e9161f952118ca7ae28343dcd16477cf1eee
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76155764"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Elección del plan de tarifa adecuado de Azure Maps

Azure Maps ofrece dos planes de tarifa, S0 y S1. El propósito de este artículo es ayudarle a elegir el plan de tarifa adecuado para satisfacer sus necesidades. Para ayudarle a elegir el plan de tarifa adecuado, plantéese las dos preguntas siguientes.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>¿Qué funcionalidades geoespaciales tiene previsto usar?
El plan de tarifa S0 es la opción adecuada si las principales API geoespaciales satisfacen los requisitos de su servicio. Si desea funcionalidades más avanzadas para su aplicación, considere la posibilidad de optar por el plan de tarifa S1. Ejemplo de funcionalidades avanzadas: imágenes aéreas e híbridas, obtención del alcance de la ruta y geocodificación por lotes. La tabla **funcionalidades del plan de tarifa** puede ayudarle a elegir el plan de tarifa más adecuado para la aplicación.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>¿Cuántos usuarios simultáneos tiene previsto admitir? 
Los planes de tarifa S0 y S1 manipulan diferentes volúmenes de rendimiento de datos. El plan de tarifa S0 puede manipular hasta **50 consultas por segundo** y el plan de tarifa S1, **más de 50 consultas por segundo**.

### <a name="pricing-tier-capabilities"></a>Funcionalidades de los planes de tarifa

| Capacidad                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Representación de mapas                              | ✓                   | ✓       |
| Imágenes de satélite                       |                     | ✓        |
| Search                                  | ✓                    | ✓        |
| Búsqueda por lotes                            |                     | ✓        |
| Enrutar                                   | ✓                    |✓        |
| Enrutamiento por lotes                            |                    | ✓        |
| Enrutamiento de matriz                          |                     | ✓        |
| Alcance de ruta (isocronas)                |                     | ✓        |
| Tráfico                                |✓                    |✓        |
| Zona horaria                               |✓                    |✓        |
| Geolocalización (versión preliminar)                    |✓                   |✓        |
| Spatial Operations                        |                    |✓        |
| Geovalla                                |                    |✓        |
| Datos de Azure Maps (versión preliminar)                |                     | ✓        |
| Movilidad (versión preliminar)                       |                     | ✓        |
| Meteorología (versión preliminar)                        |✓                    |✓        |

Merece la pena considerar estos puntos de datos adicionales:
* ¿Qué tipo de empresa tiene?
* ¿Es crítica la aplicación?

### <a name="pricing-tier-targeted-customers"></a>Clientes objetivo de los planes de tarifa

Consulte la tabla **Clientes objetivo de los planes de tarifa** para obtener una visión general de los planes de tarifa S0 y S1. Para más información, consulte [Precios de Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/). 

| Plan de tarifa  |     Clientes objetivo                                                                |
|-----------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>El plan de tarifa S0 funciona para las aplicaciones en todas las fases de producción, desde el desarrollo de la prueba de concepto y la primera fase de pruebas hasta la puesta en producción e implementación de la aplicación. Sin embargo, este nivel está diseñado para el desarrollo a pequeña escala, para clientes con pocos usuarios simultáneos o ambos. <p>|
| S1            |    <p>El plan de tarifa S1 está orientado a clientes que necesitan soporte para aplicaciones empresariales a gran escala y críticas para la empresa, o bien grandes volúmenes de usuarios simultáneos. También es para aquellos clientes que requieren servicios geoespaciales avanzados.</p>|

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo ver y cambiar los planes de tarifa:

> [!div class="nextstepaction"] 
> [Administración de un plan de tarifa](how-to-manage-pricing-tier.md)

---
title: Elección del plan de tarifa adecuado de Azure Maps | Microsoft Docs
description: Obtenga información sobre los planes de tarifa que ofrece Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 01/02/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: ac88bc95ac9ffe4c2f32307b0bee427829242add
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56234687"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Elección del plan de tarifa adecuado de Azure Maps

Azure Maps ofrece dos planes de tarifa. El propósito de este artículo es ayudarle a elegir el plan de tarifa adecuado para satisfacer sus necesidades. Para ayudarle a elegir el plan de tarifa adecuado, plantéese las dos preguntas siguientes.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>¿Qué funcionalidades geoespaciales tiene previsto usar?
El plan de tarifa S0 es la opción adecuada si las principales API geoespaciales satisfacen los requisitos de su servicio. Si desea funcionalidades más avanzadas para su aplicación, considere la posibilidad de optar por el plan de tarifa S1. Las funcionalidades de ejemplo son imágenes aéreas e híbridas, obteniendo los servicios Route Range y Batch Geocoding. La tabla **Funcionalidades de los planes de tarifa** que se muestra a continuación ofrece una mejor idea de las necesidades de su aplicación. También le ayuda a elegir un plan de tarifa más adecuado para su aplicación.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>¿Cuántos usuarios simultáneos tiene previsto admitir? 
Los planes de tarifa S0 y S1 manipulan diferentes volúmenes de rendimiento de datos. Antes de elegir un plan de tarifa de Azure Maps, plantéese algunas preguntas. Un ejemplo es "¿cuántos usuarios simultáneos tiene previsto admitir?" El plan de tarifa S0 manipula hasta **50 consultas por segundo**. El plan de tarifa S1 manipula **más de 50 consultas por segundo**.

### <a name="pricing-tier-capabilities"></a>Funcionalidades de los planes de tarifa

| Capacidad                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Search                                  |        ✓           |     ✓    |
| Batch geocoding (versión preliminar)              |                   |     ✓    |
| Polygons from search          |                   |     ✓    |
| Enrutamiento                                 |        ✓           |     ✓    |
| Route range                    |                   |     ✓    |
| Batch routing (versión preliminar)                |                   |     ✓    |
| Matrix routing (versión preliminar)               |                   |     ✓    |
| Representación                                  |        ✓           |     ✓    |
| Imagery plus hybrid imagery    |            |     ✓    |
| Tráfico                                 |        ✓           |     ✓    |
| Zonas horarias                              |        ✓           |     ✓    |
| Geolocation (versión preliminar)                |        ✓           |     ✓    |
| Data (versión preliminar)               |                   |     ✓    |
| Spatial (versión preliminar)               |                   |     ✓    |
| Geofencing (versión preliminar)               |                   |     ✓    |



Merece la pena considerar estos puntos de datos adicionales:
* ¿Qué tipo de empresa tiene?
* ¿Qué importancia tiene la aplicación que se genera?

Consulte la tabla **Clientes objetivo de los planes de tarifa** para obtener una visión general de los planes de tarifa S0 y S1. Para más información, consulte [Precios de Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/). 

### <a name="pricing-tier-targeted-customers"></a>Clientes objetivo de los planes de tarifa

| Plan de tarifa  |     Clientes objetivo                                                                |
|---------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>El plan de tarifa S0 está orientado a los clientes que son pequeñas y medianas empresas. Es el plan de tarifa adecuado para usted si no espera grandes volúmenes de usuarios simultáneos. También es adecuado si las principales API geoespaciales mostradas en la tabla anterior satisfacen los requisitos de su servicio. Este plan se encuentra disponible con carácter general. Funciona para las aplicaciones en todas las fases de producción: desde el desarrollo de la prueba de concepto y la primera fase de pruebas hasta la producción e implementación de la aplicación.<p>|
| S1            |    <p>El plan de tarifa S1 está orientado a clientes que necesitan soporte para aplicaciones empresariales a gran escala y críticas para la empresa, o bien grandes volúmenes de usuarios simultáneos. También es para aquellos clientes que requieren servicios geoespaciales avanzados.</p>|

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo ver y cambiar los planes de tarifa:

> [!div class="nextstepaction"] 
> [Administración de un plan de tarifa](how-to-manage-pricing-tier.md)

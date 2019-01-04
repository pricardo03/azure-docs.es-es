---
title: Elección del plan de tarifa adecuado de Azure Maps | Microsoft Docs
description: Obtenga información sobre los planes de tarifa que ofrece Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 12/05/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: ee277867f449afddeb89c3fd73b5b577a68a4497
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52998378"
---
# <a name="choosing-the-right-pricing-tier-in-azure-maps"></a>Elección del plan de tarifa adecuado de Azure Maps

Azure Maps ofrece dos planes de tarifa. El propósito de este artículo es ayudarle a elegir el plan de tarifa adecuado para satisfacer sus necesidades. Para ayudarle a elegir el plan de tarifa adecuado, plantéese dos preguntas:

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>¿Qué funcionalidades geoespaciales tiene previsto usar?
Si considera que las principales API geoespaciales satisfacen los requisitos de su servicio, el plan de tarifa S0 es la opción adecuada. Si desea contar con funcionalidades más avanzadas para la aplicación, como imágenes de área+híbridas, obtener intervalos de ruta, una geocodificación por lotes, etc., considere la posibilidad de elegir el plan de tarifa S1. La tabla siguiente donde se indican las **funcionalidades de los planes de tarifa** le ayudará a hacerse una idea de las necesidades de la aplicación y a elegir el plan de tarifa más adecuado para ella.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>¿Cuántos usuarios simultáneos tiene previsto admitir? 
Los planes de tarifa S0 y S1 pueden manipular diferentes volúmenes de rendimiento de datos. Antes de elegir un plan de tarifa de Azure Maps, puede plantearse preguntas como: ¿cuántos usuarios simultáneos desea admitir? El plan de tarifa S0 puede manipular hasta **50 consultas por segundo** y el plan de tarifa S1, **más de 50 consultas por segundo**.

### <a name="pricing-tier-capabilities"></a>Funcionalidades de los planes de tarifa

| Capacidad                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Search                                  |        ✓           |     ✓    |
| Enrutamiento                                 |        ✓           |     ✓    |
| Representación                                  |        ✓           |     ✓    |
| Tráfico                                 |        ✓           |     ✓    |
| Zonas horarias                              |        ✓           |     ✓    |
| *Imagery + Hybrid Imagery (versión preliminar)     |        ✓           |     ✓    |
| *Route Range (versión preliminar)                  |        ✓           |     ✓    |
| *IP 2 Location (versión preliminar)                |        ✓           |     ✓    |
| *Polygons from search (versión preliminar)         |        ✓           |     ✓    |
| *Batch Geocoding (versión preliminar)              |        ✓           |     ✓    |
| *Batch Routing (versión preliminar)                |        ✓           |     ✓    |
| *Matrix Routing (versión preliminar)               |        ✓           |     ✓    |

> [!Note]
> No se podrá acceder a estas funcionalidades en el plan de tarifa S0 a partir del 4 de febrero de 2019.

Es conveniente considerar algunos datos adicionales, como el tipo de empresa de la que se trata o la importancia que tiene la aplicación que se va a compilar.

Consulte la tabla **Clientes objetivo de los planes de tarifa** para obtener una visión general de los planes de tarifa S0 y S1. Para obtener más información sobre los precios de Azure Maps, consulte [Precios de Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/). 

### <a name="pricing-tier-targeted-customers"></a>Clientes objetivo de los planes de tarifa

| Plan de tarifa  |        Clientes objetivo                                                                |
|---------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>El plan de tarifa S0 está orientado a los clientes que son pequeñas y medianas empresas. Se trata de un plan de tarifa adecuado para usted si no espera admitir grandes volúmenes de usuarios simultáneos y si las API geoespaciales principales satisfacen los requisitos del servicio como se indica en la tabla siguiente. Este plan se encuentra disponible con carácter general y es aplicable a las aplicaciones en todas las fases de producción, desde el desarrollo de la prueba de concepto y la primera fase de pruebas hasta la producción e implementación de la aplicación.<p>|
| S1            |    <p>El plan de tarifa S1 está orientado a clientes que necesitan soporte para aplicaciones empresariales a gran escala y críticas para la empresa, grandes volúmenes de usuarios simultáneos o que requieren servicios geoespaciales avanzados.</p>|


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo visualizar y cambiar el plan de tarifa, vea:

> [!div class="nextstepaction"]
> [Administración del plan de tarifa](how-to-manage-pricing-tier.md)

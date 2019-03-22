---
title: Especificaciones técnicas del borde del cuadro de datos de Microsoft Azure y compatibilidad | Microsoft Docs
description: Obtenga información sobre las especificaciones técnicas y cumplimiento de normas para el borde del cuadro de datos de Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/12/2019
ms.author: alkohli
ms.openlocfilehash: 8ef35709e90c0a58cc0ff8df1afb6e864adc0a23
ms.sourcegitcommit: 4133f375862fdbdec07b70de047d70c66ac29d50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2019
ms.locfileid: "57994716"
---
# <a name="azure-data-box-edge-technical-specifications-preview"></a>Especificaciones técnicas del borde del cuadro de datos Azure (versión preliminar)

Los componentes de hardware del dispositivo de borde del cuadro de datos de Microsoft Azure cumplen las especificaciones técnicas y los estándares reglamentarios descritos en este artículo. Las especificaciones técnicas describen las unidades de suministro de energía (fuentes de alimentación activas), capacidad de almacenamiento, contenedores y estándares del entorno. 

> [!IMPORTANT]
> Data Box Edge se encuentra en versión preliminar. Antes de solicitar e implementar esta solución, revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

## <a name="power-supply-unit-specifications"></a>Especificaciones de unidad de suministro de energía

El dispositivo de borde del cuadro de datos tiene dos 100-240 v. unidades de alimentación (fuentes de alimentación activas) con ventiladores de alto rendimiento. Las dos fuentes de alimentación activas proporcionan una configuración de alimentación redundante. Si se produce un error en una fuente de alimentación, el dispositivo sigue funcionando con normalidad en la otra fuente de alimentación hasta que se reemplaza el módulo con error. En la tabla siguiente se enumera las especificaciones técnicas de las fuentes de alimentación activas.

| Especificación           | FUENTE DE ALIMENTACIÓN DE 750 W                  |
|-------------------------|----------------------------|
| Potencia de salida máxima    |  750 W                     |
| Frecuencia               | 50/60 Hz                   |
| Selección del intervalo de voltaje | Intervalo automático: 100-240 V CA |
| Conectable en funcionamiento           | Sí                        |

<!--## Power consumption statistics

The following table lists the typical power consumption data (actual values may vary from the published) for the Data Box Edge device.-->

## <a name="storage-specifications"></a>Especificaciones de almacenamiento

Los dispositivos de borde del cuadro de datos tienen 10 X 2.5" SSD de NVMe, cada uno con una capacidad de 1,6 TB. De estas unidades SSD, 2 son discos del sistema operativo y los otros 8 son discos de datos. La capacidad total utilizable para el dispositivo es de aproximadamente 12,5 TB. La tabla siguiente contiene los detalles de la capacidad de almacenamiento del dispositivo.

|     Especificación                          |     Valor             |
|--------------------------------------------|-----------------------|
|    Número de unidades de estado sólido (SSD)     |    8                  |
|    Capacidad de un solo SSD                     |    1,6 TB             |
|    Capacidad total                          |    12,8 TB            |
|    Capacidad total utilizable*                  |    ~ 12,5 TB            |

**Algo de espacio está reservado para uso interno.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Especificaciones de dimensiones y peso del revestimiento

En las tablas siguientes se enumeran las diferentes especificaciones de dimensiones y peso del revestimiento.

### <a name="enclosure-dimensions"></a>Dimensiones del revestimiento

En la tabla siguiente se enumeran las dimensiones del revestimiento en milímetros y pulgadas.

|     Revestimiento     |     Milímetros     |     Pulgadas     |
|-------------------|---------------------|----------------|
|    Alto         |    304.8            |    12          |
|    Ancho          |    660.4            |    26          |
|    profundidad          |    1041.4           |    41          |

### <a name="enclosure-weight"></a>Peso del revestimiento

El paquete del dispositivo pesa 66 libras. y requiere dos personas para manejarlo. El peso del dispositivo depende de la configuración de la caja.

|     Revestimiento                                 |     Peso          |
|-----------------------------------------------|---------------------|
|    Peso total, incluido el empaquetado       |    66 libras.          |
|    Peso del dispositivo                       |    48.3 libras.        |

## <a name="enclosure-environment-specifications"></a>Especificaciones medioambientales del revestimiento

Esta sección enumeran las especificaciones relacionadas con el entorno de alojamiento, como temperatura, humedad y altitud.

### <a name="temperature-and-humidity"></a>Temperatura y humedad

|     Revestimiento         |     Intervalo de temperatura ambiente     |     Humedad ambiente relativa     |     Punto de rocío máximo     |
|-----------------------|--------------------------------------|--------------------------------------|---------------------------|
|    Operativo        |    10° C - 35° C (50° F - 86° F)         |    10% al 80% sin condensación.         |    29 °C (84 °F)            |
|    No operativo    |    -40 ° C a 65 ° C (-40 ° F - 149 ° F)     |    5 a 95% sin condensación.          |    33°C (91°F)            |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Flujo de aire, altitud, golpes, vibraciones, orientación, seguridad y CEM

|     Revestimiento                           |     Especificaciones operativas                                                                                                                                                                                         |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Flujo de aire                              |    El flujo de aire del sistema va de delante atrás. El sistema debe funcionar con una instalación de baja presión y escape trasero. <!--Back pressure created by rack doors and obstacles should not exceed 5 pascals (0.5 mm water gauge).-->    |
|    Máximo Altitud (operativo)        |    3048 metros (10.000 pies) con el funcionamiento de la temperatura máxima determinado por [temperatura clasificación anular las especificaciones de funcionamiento](#operating-temperature-de-rating-specifications).                                                                                |
|    Altitud máximo, en reposo    |    12.000 metros (39,370 pies)                                                                                                                                                                                         |
|    Golpes (operativo)                   |    6 G para 11 milisegundos en 6 orientaciones                                                                                                                                                                         |
|    Golpes (no operativo)               |    71 G 2 milisegundos en 6 orientaciones                                                                                                                                                                           |
|    Vibraciones (operativo)               |    G 0,26<sub>RMS</sub> 5 Hz hasta 350 Hz aleatorio                                                                                                                                                                                     |
|    Vibraciones (no operativo)           |    1,88 G<sub>RMS</sub> 10 Hz a 500 Hz durante 15 minutos (todos los seis lados probados.)                                                                                                                                                  |
|    Orientación y montaje             |    montaje en bastidor de 19"                                                                                                                                                                                        |
|    Seguridad y homologaciones                 |    EN 60950-1:2006 +A1:2010 +A2:2013 +A11:2009 +A12:2011/IEC 60950-1:2005 ed2 +A1:2009 +A2:2013 EN 62311:2008                                                                                                                                                                       |
|    CEM                                  |    FCC A, ICES-003 <br>EN 55032:2012/CISPR 32:2012  <br>EN 55032:2015/CISPR 32:2015  <br>EN 55024:2010 +A1:2015/CISPR 24:2010 +A1:2015  <br>EN 61000-3-2:2014 / CEI 61000-3-2:2014 (D (clase)   <br>EN 61000-3-3:2013/IEC 61000-3-3:2013                                                                                                                                                                                         |
|    Energía             |    Nº de Comisión Reglamento (UE). 617/2013                                                                                                                                                                                        |
|    RoHS           |    EN 50581:2012                                                                                                                                                                                        |


### <a name="operating-temperature-de-rating-specifications"></a>Temperatura clasificación anular las especificaciones de funcionamiento

|     Temperatura operativo anular clasificación     |     Intervalo de temperatura ambiente                                                         |
|--------------------------------------------|------------------------------------------------------------------------------------------|
|    Hasta 35 ° C (95° F)                       |    Temperatura máxima se reduce por 1° C/300 m (de 1° F/547) por encima de los 950 millones (3,117 ft).    |
|    35° C a 40° C (95° F a 104° F)            |    Temperatura máxima se reduce por 1° C/175 m (de 1° F/319) por encima de los 950 millones (3,117 ft).    |
|    40° C a 45° C (104° F a 113° F)           |    Temperatura máxima se reduce por 1° C/125 m (de 1° F/228) por encima de los 950 millones (3,117 ft).    |


## <a name="next-steps"></a>Pasos siguientes

- [Implementación de Azure Data Box Edge](data-box-edge-deploy-prep.md)

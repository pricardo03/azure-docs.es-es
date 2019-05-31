---
title: Notas de la versión de Microsoft Azure StorSimple Virtual Array Update 1.2 | Microsoft Docs
description: Describe los problemas críticos y las soluciones para la matriz Virtual de StorSimple que ejecuta Update 1.2.
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 05/29/2019
ms.author: alkohli
ms.openlocfilehash: ea7e4801dfaad533403c0f927a03735ae409cc52
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420609"
---
# <a name="storsimple-virtual-array-update-12-release-notes"></a>Notas de la versión de StorSimple Virtual Array Update 1.2

En las notas de la versión siguientes se identifican los problemas críticos pendientes y los problemas resueltos de las actualizaciones de la matriz virtual de Microsoft Azure StorSimple.

Notas de la versión se actualizan continuamente. Cuando se detectan problemas críticos que requieren una solución alternativa, se agregan. Antes de implementar la matriz virtual de StorSimple, le recomendamos que lea detenidamente la información que encontrará en las notas de la versión.

La actualización 1.2 se corresponde con la versión de software **10.0.10311.0**.

> [!IMPORTANT]
> - Las actualizaciones causan interrupciones y reinician el dispositivo. Si hay procesos de E/S en curso, el dispositivo incurrirá en tiempo de inactividad. Para obtener instrucciones detalladas sobre los paquetes que se usa para aplicar esta actualización, vaya a [descargar Update 1.2](#download-update-12).
> - La actualización 1.2 está disponible a través de Azure portal solo si el dispositivo ejecuta Update 1.0 o 1.1.

## <a name="whats-new-in-update-12"></a>Novedades de la actualización 1.2

Esta actualización contiene las siguientes correcciones de errores:

- Resistencia mejorada al procesar los archivos eliminados.
- Mejoradas control de excepciones en la ruta de inicio del código lo que reduce los errores en las copias de seguridad, restauración, lecturas en la nube y automatizar la recuperación de espacio.

## <a name="download-update-12"></a>Descargar la actualización 1.2

Para descargar esta actualización, vaya a la [catálogo de Microsoft Update](https://www.catalog.update.microsoft.com/Home.aspx) server y el paquete de descarga el KB4502035. Este paquete contiene los siguientes paquetes:

 - **KB4493446** que contiene las actualizaciones acumulativas de Windows de 2012 R2 hasta abril de 2019. Para obtener más información sobre qué se incluye en este paquete acumulativo de actualizaciones, vaya a [acumulación mensual de seguridad de abril](https://support.microsoft.com/help/4493446/windows-8-1-update-kb4493446).
 - **KB3011067** que es un archivo de paquete independiente de Microsoft Update WindowsTH-KB3011067-x64. Este archivo se utiliza para actualizar el software del dispositivo.

Descargue KB4502035 y siga estas instrucciones para [aplicar la actualización a través de la IU web local](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="issues-fixed-in-update-12"></a>Problemas corregidos en la actualización 1.2

En la tabla siguiente se proporciona un resumen de los problemas corregidos en esta versión.

| No. | Característica | Problema |
| --- | --- | --- |
| 1 |Eliminación| En las versiones anteriores del software, hubo un problema cuando el uso del dispositivo no cambia incluso cuando se eliminan archivos. Este problema se ha corregido en esta versión. Ruta de acceso de código niveles se realizó sea más resistente al procesar los archivos eliminados.|
| 2 |Control de excepciones| En las versiones anteriores del software, hubo un problema tras el reinicio del sistema que puede conducir a errores en las copias de seguridad, restauración, leer desde la nube y automatizar la recuperación de espacio. Esta versión contiene cambios sobre cómo se controlan las excepciones en la ruta de acceso de inicio.|

## <a name="known-issues-in-update-12"></a>Problemas conocidos de la actualización 1.2

No hay nuevos problemas eran notificados en la actualización 1.2. Todos los problemas notificados se realizan a través de las versiones anteriores. Para ver el resumen de problemas conocidos incluidos desde las versiones anteriores, vaya a [problemas conocidos de Update 1.1](storsimple-virtual-array-update-11-release-notes.md#known-issues-in-update-11).

## <a name="next-steps"></a>Pasos siguientes

Descargar KB4502035 y [aplicar la actualización a través de la IU web local](storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="references"></a>Referencias

¿Busca una nota de la versión anterior? Vaya a:
* [Notas de la versión 1.1 Update de la matriz Virtual de StorSimple](storsimple-virtual-array-update-11-release-notes.md)
* [Notas de la versión de StorSimple Virtual Array Update 1.0](storsimple-virtual-array-update-1-release-notes.md)
* [Notas de la versión de StorSimple Virtual Array Update 0.6](storsimple-virtual-array-update-06-release-notes.md)
* [Notas de la versión de StorSimple Virtual Array Update 0.5](storsimple-virtual-array-update-05-release-notes.md)
* [Notas de la versión de StorSimple Virtual Array Update 0.4](storsimple-virtual-array-update-04-release-notes.md)
* [Notas de la versión de StorSimple Virtual Array Update 0.3](storsimple-ova-update-03-release-notes.md)
* [Notas de la versión de la matriz virtual de StorSimple Update 0.1 y 0.2](storsimple-ova-update-01-release-notes.md)
* [Notas de la versión de disponibilidad general de la matriz Virtual de StorSimple](storsimple-ova-pp-release-notes.md)

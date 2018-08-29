---
title: 'Problemas conocidos con el complemento de acústica: Cognitive Services'
description: Podría encontrar los siguientes problemas conocidos al usar la versión preliminar del diseñador para Elementos acústicos de un proyecto.
services: cognitive-services
author: kylestorck
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kylestorck
ms.openlocfilehash: b0dd5e0c365c65fc7b29752f7885acb71bdb3df8
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181429"
---
# <a name="known-issues"></a>Problemas conocidos
Podría encontrar los siguientes problemas conocidos al usar la versión preliminar del diseñador para Elementos acústicos de un proyecto.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Los parámetros acústicos se pierden al cambiar el nombre de una escena

Si cambia el nombre de una escena, todos los parámetros acústicos que pertenecen a dicha escena no se transferirán automáticamente a la escena nueva. Sin embargo, seguirán existiendo en el archivo de recursos antiguo. Busque el archivo **SceneName_AcousticParameters.asset** en el directorio **Editor** junto al archivo de la escena. Cámbiele el nombre para reflejar el nuevo nombre de la escena.

## <a name="the-default-path-for-the-acousticsdata-folder-in-probes-tab-is-an-absolute-path"></a>La ruta de acceso predeterminada para la carpeta AcousticsData en la pestaña Probes (Sondeos) es una ruta absoluta

Debería establecerse por defecto en una ruta de acceso relativa para poder compartir más fácilmente los proyectos entre los colaboradores. Como solución alternativa, cambie la ruta de acceso para que sea relativa al directorio del proyecto.

## <a name="runtime-voxels-are-a-different-size-than-design-time-voxels"></a>Los vóxeles en tiempo de ejecución tienen un tamaño distinto de los vóxeles en tiempo de diseño

Si hace un **cálculo** en la pestaña **Probes** (Sondeos) y ve los vóxeles, haga una elaboración y vea los vóxeles en tiempo de ejecución para la misma escena. Los vóxeles tendrán tamaños distintos. Los vóxeles que se muestran previos a la elaboración son los vóxeles que se usan en la simulación. Los vóxeles que se muestran en tiempo de ejecución se usan para la interpolación entre los puntos de sondeo. Esto puede provocar una incoherencia en la que los portales aparecerán abiertos en tiempo de ejecución, pero no estarán abiertos realmente.

## <a name="uwp-builds-not-working"></a>Las compilaciones de UWP no funcionan

En las versiones más recientes de Unity (2018.2 +), no se completan correctamente las compilaciones de UWP. La fase de ejecución de la compilación se detendrá y se generarán errores del tipo "Unity extensiones are not yet initialized" (Las extensiones de Unity todavía no se inicializan). En [este error de Unity](https://fogbugz.unity3d.com/default.asp?1070491_1rgf14bakv5u779d) se hace un seguimiento de esto.

## <a name="unity-crashes-when-closing-project"></a>Unity se bloquea al cerrar un proyecto

En las versiones más recientes de Unity (2018.2 +), hay un problema conocido en el que Unity se bloqueará cuando se cierra un proyecto. En [este error de Unity](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project) se hace un seguimiento de esto.

## <a name="trouble-deploying-to-android"></a>Problema al implementar en Android
Para usar Elementos acústicos de un proyecto en Android, cambie el destino de compilación a Android. Algunas versiones de Unity tienen un error en la implementación de complementos de audio. Asegúrese de no usar una versión afectada por [este error](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player).

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Recibo un error que me indica que "no se encontró el archivo de metadatos System.Security.dll"

Asegúrese de que la versión en tiempo de ejecución de scripting en la configuración del Reproductor está establecida en **Equivalente a .NET 4.x** y reinicie Unity.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Tengo problemas de autenticación al conectarme a Azure

Compruebe que usó las credenciales correctas para su cuenta de Azure, que la cuenta es compatible con el tipo de nodo solicitado en la elaboración y que el reloj del sistema es exacto.

## <a name="next-steps"></a>Pasos siguientes
* Introducción a la [integración de la acústica en un proyecto de Unity](getting-started.md)


---
title: Problemas conocidos con el complemento Elementos acústicos de un proyecto
titlesuffix: Azure Cognitive Services
description: Podría encontrar los siguientes problemas conocidos al usar la versión preliminar del diseñador para Elementos acústicos de un proyecto.
services: cognitive-services
author: kylestorck
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: resources
ms.date: 03/20/2019
ms.author: kylestorck
ms.openlocfilehash: 50de4d983ed24440d655cf5b9ba3fb5e33d8d7cd
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58309952"
---
# <a name="project-acoustics-known-issues"></a>Problemas conocidos de proyecto acústica
Podría encontrar los siguientes problemas conocidos al usar la versión preliminar del diseñador para Elementos acústicos de un proyecto.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Los parámetros acústicos se pierden al cambiar el nombre de una escena

Si cambia el nombre de una escena, todos los parámetros acústicos que pertenecen a dicha escena automáticamente no se transfieran a la nueva escena. Le siguen existiendo en el antiguo archivo de recursos sin embargo. Busque el archivo **SceneName_AcousticParameters.asset** en el directorio **Editor** junto al archivo de la escena. Cámbiele el nombre para reflejar el nuevo nombre de la escena.

## <a name="unity-crashes-when-closing-project"></a>Unity se bloquea al cerrar un proyecto

En las versiones más recientes de Unity (2018.2 +), hay un problema conocido en el que Unity se bloqueará cuando se cierra un proyecto. En [este error de Unity](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project) se hace un seguimiento de esto.

## <a name="deploying-to-android-from-some-unity-versions"></a>Implementación en Android de algunas versiones de Unity

Algunas versiones de Unity tienen un error en la implementación de complementos de audio para Android. Asegúrese de que no está usando una versión que se ve afectada por [este error](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player).

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Recibo un error que me indica que "no se encontró el archivo de metadatos System.Security.dll"

Asegúrese de que la versión en tiempo de ejecución de scripting en la configuración del Reproductor está establecida en **Equivalente a .NET 4.x** y reinicie Unity.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Tengo problemas de autenticación al conectarme a Azure

Compruebe que usó las credenciales correctas para su cuenta de Azure, que la cuenta es compatible con el tipo de nodo solicitado en la elaboración y que el reloj del sistema es exacto.

## <a name="canceling-a-bake-leaves-the-bake-tab-in-deleting-state"></a>La cancelación de una elaboración deja la pestaña Bake (Elaboración) en el estado "eliminación"
Proyecto acústica limpiará todos los recursos de Azure para un trabajo de cancelación o finalización correcta. Esto puede tardar hasta 5 minutos.

## <a name="next-steps"></a>Pasos siguientes
* Pruebe el [Unity](unity-quickstart.md) o [Unreal](unreal-quickstart.md) contenido de ejemplo


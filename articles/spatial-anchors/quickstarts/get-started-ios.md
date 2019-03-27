---
title: 'Guía de inicio rápido: Creación de una aplicación iOS con Azure Spatial Anchors | Microsoft Docs'
description: En esta guía de inicio rápido aprenderá a compilar una aplicación iOS mediante Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: a5d34b2964d805cdbeced0d4fad3dd889da9f3e8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57900631"
---
# <a name="quickstart-create-an-ios-app-with-azure-spatial-anchors-in-either-swift-or-objective-c"></a>Inicio rápido: Crear una aplicación iOS con Azure Spatial Anchors en Swift u Objective-C

En esta guía de inicio rápido se describe cómo crear una aplicación iOS con [Azure Spatial Anchors](../overview.md) en Swift o en Objective-C. Azure Spatial Anchors es un servicio multiplataforma para desarrolladores que le permite crear experiencias de realidad mixta mediante objetos cuya ubicación persiste en todos los dispositivos a lo largo del tiempo. Cuando haya terminado, tendrá una aplicación iOS de ARKit que puede guardar y recuperar un delimitador espacial.

Aprenderá a:

> [!div class="checklist"]
> * Crear una cuenta de Spatial Anchors
> * Configurar la clave y el identificador de la cuenta de Spatial Anchors
> * Implementar y ejecutar en un dispositivo iOS

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido, asegúrese de que dispone de lo siguiente:

- Una máquina macOS habilitada para el desarrollo que tenga instalados <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode 10+</a> y <a href="https://cocoapods.org" target="_blank">CocoaPods</a>.
- GIT instalado mediante HomeBrew. Escriba el siguiente comando en una sola línea de Terminal: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. A continuación, ejecute `brew install git`.
- Un dispositivo iOS <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">compatible con ARKit</a> habilitado para el desarrollo.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Apertura del proyecto de ejemplo

Use Terminal para realizar las acciones siguientes:

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Instale los pods necesarios mediante CocoaPods:

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

Vaya a `iOS/Swift/`.

```bash
cd ./iOS/Swift/
```

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

Vaya a `iOS/Objective-C/`.

```bash
cd ./iOS/Objective-C/
```

***

Ejecute `pod install --repo-update` para instalar los pods de CocoaPods para el proyecto.

Ahora abra `.xcworkspace` en Xcode.

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

```bash
open ./SampleSwift.xcworkspace
```

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

```bash
open ./SampleObjC.xcworkspace
```

***

## <a name="configure-account-identifier-and-key"></a>Configuración del identificador y la clave de la cuenta

El paso siguiente es configurar la aplicación para usar el identificador de cuenta y la clave de cuenta. Los copió en un editor cuando [configuró el recurso de Spatial Anchors](#create-a-spatial-anchors-resource).

# <a name="swifttabopenproject-swift"></a>[Swift](#tab/openproject-swift)

Abra `iOS/Swift/SampleSwift/ViewController.swift`.

Busque el campo `SpatialAnchorsAccountKey` y reemplace `Set me` por la clave de la cuenta.

Busque el campo `SpatialAnchorsAccountId` y reemplace `Set me` por el identificador de la cuenta.

# <a name="objective-ctabopenproject-objc"></a>[Objective-C](#tab/openproject-objc)

Abra `iOS/Objective-C/SampleObjC/ViewController.m`.

Busque el campo `SpatialAnchorsAccountKey` y reemplace `Set me` por la clave de la cuenta.

Busque el campo `SpatialAnchorsAccountId` y reemplace `Set me` por el identificador de la cuenta.

***

## <a name="deploy-the-app-to-your-ios-device"></a>Implementación de la aplicación en el dispositivo iOS

Conecte el dispositivo iOS al equipo Mac y establezca el **esquema activo** en su dispositivo iOS.

![Selección del dispositivo](./media/get-started-ios/select-device.png)

Seleccione **Build and then run the current scheme** (Compilar y luego ejecutar el esquema actual).

![Implementación y ejecución](./media/get-started-ios/deploy-run.png)

> [!NOTE]
> Si ve un error `library not found for -lPods-SampleObjC`, es probable que haya abierto el archivo `.xcodeproj` en lugar de `.xcworkspace`. Abra `.xcworkspace` e inténtelo de nuevo.

En Xcode, presione **Stop** (Detener) para detener la aplicación.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Tutorial: Uso compartido de Spatial Anchors entre dispositivos](../tutorials/tutorial-share-anchors-across-devices.md)

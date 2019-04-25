---
title: Administración de cuentas y claves
titleSuffix: Language Understanding - Azure Cognitive Services
description: Los dos elementos clave de información de una cuenta de LUIS son la cuenta de usuario y la clave de creación. Se administra la información de inicio de sesión en account.microsoft.com. Administra la clave de creación de la página de configuración del portal de LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: d7d63ad642ab2d3b336e15dcca606077762ceb9d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60195831"
---
# <a name="manage-account-and-authoring-key"></a>Administración de cuenta y clave de creación

Los dos elementos clave de información de una cuenta de LUIS son la cuenta de usuario y la clave de creación. La información de inicio de sesión se administra en [account.microsoft.com](https://account.microsoft.com). La clave de creación se administra desde el [LUIS](luis-reference-regions.md) portal **configuración** página.

## <a name="authoring-key"></a>Clave de creación

Este único y específico de la región de creación clave en el **configuración** página que le permite crear todas las aplicaciones de la [LUIS](luis-reference-regions.md) portal, así como la [creación de API](https://aka.ms/luis-authoring-api). Para su comodidad, la clave de creación puede realizar un número [limitado](luis-boundaries.md) de consultas de punto de conexión al mes.

[![Página de configuración de LUIS](./media/luis-how-to-account-settings/account-settings.png)](./media/luis-how-to-account-settings/account-settings.png#lightbox)

La clave de creación se usa para cualquier aplicación de su propiedad, así como para cualquier aplicación en la que aparezca como colaborador.

## <a name="authoring-key-regions"></a>Regiones de las claves de creación

La clave de creación es específica de la [región de creación](luis-reference-regions.md#publishing-regions). La clave no funciona en una región distinta.

## <a name="reset-authoring-key"></a>Restablecimiento de la clave de creación

Si su clave de creación está en peligro, restablézcala. La clave se restablece en todas las aplicaciones en la [LUIS](luis-reference-regions.md) portal. Si crea sus aplicaciones a través de las API de creación, debe cambiar el valor de `Ocp-Apim-Subscription-Key` por la clave nueva.

## <a name="delete-account"></a>Eliminación de cuenta

Vea [Data storage and removal](luis-concept-data-storage.md#accounts) (Almacenamiento y eliminación de datos) para obtener información acerca de qué datos se eliminan al eliminar la cuenta.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [clave de creación](luis-concept-keys.md#authoring-key).


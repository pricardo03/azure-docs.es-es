---
title: 'Administración de cuentas y claves: LUIS'
titleSuffix: Azure Cognitive Services
description: Los dos elementos clave de información de una cuenta de LUIS son la cuenta de usuario y la clave de creación. La información de inicio de sesión se administra en account.microsoft.com. La clave de creación se administra en la página Settings (Configuración) del portal de LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 11a2040e674a0ee38fbce8b21097f058fe603da2
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881943"
---
# <a name="manage-account-and-authoring-key"></a>Administración de cuenta y clave de creación

Los dos elementos clave de información de una cuenta de LUIS son la cuenta de usuario y la clave de creación. La información de inicio de sesión se administra en [account.microsoft.com](https://account.microsoft.com). La clave de creación se administra en la página **Settings** (Configuración) del portal de [LUIS](luis-reference-regions.md).

## <a name="authoring-key"></a>Clave de creación

Esta clave de creación única y específica de la región, de la página **Settings** (Configuración), le permite crear todas sus aplicaciones desde el portal de [LUIS](luis-reference-regions.md) y las [API de creación](https://go.microsoft.com/fwlink/?linkid=2092087). Para su comodidad, la clave de creación puede realizar un número [limitado](luis-boundaries.md) de consultas de punto de conexión al mes.

[![Página de configuración de LUIS](./media/luis-how-to-account-settings/account-settings.png)](./media/luis-how-to-account-settings/account-settings.png#lightbox)

La clave de creación se usa para cualquier aplicación de su propiedad, así como para cualquier aplicación en la que aparezca como colaborador.

## <a name="authoring-key-regions"></a>Regiones de las claves de creación

La clave de creación es específica de la [región de creación](luis-reference-regions.md#publishing-regions). La clave no funciona en una región distinta.

## <a name="reset-authoring-key"></a>Restablecimiento de la clave de creación

Si su clave de creación está en peligro, restablézcala. La clave se restablece en todas las aplicaciones en el portal de [LUIS](luis-reference-regions.md). Si crea sus aplicaciones a través de las API de creación, debe cambiar el valor de `Ocp-Apim-Subscription-Key` por la clave nueva.

## <a name="delete-account"></a>Eliminación de cuenta

Vea [Data storage and removal](luis-concept-data-storage.md#accounts) (Almacenamiento y eliminación de datos) para obtener información acerca de qué datos se eliminan al eliminar la cuenta.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [clave de creación](luis-concept-keys.md#authoring-key).


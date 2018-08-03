---
title: Administración de la configuración de su cuenta en LUIS | Microsoft Docs
description: Utilice el sitio web de LUIS para administrar la configuración de su cuenta.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/08/2018
ms.author: diberry
ms.openlocfilehash: 73e90e5ae86db2c2c4625762b285f8c86f0e241b
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398052"
---
# <a name="manage-account-and-authoring-key"></a>Administración de cuenta y clave de creación
Los dos elementos clave de información de una cuenta de LUIS son la cuenta de usuario y la clave de creación. La información de inicio de sesión se administra en [account.microsoft.com](https://account.microsoft.com). La clave de creación se administra en la página **Settings** (Configuración) del sitio web de [LUIS](luis-reference-regions.md). 

## <a name="authoring-key"></a>Clave de creación

Esta clave de creación única y específica de la región, de la página **Settings** (Configuración), le permite crear todas sus aplicaciones desde el sitio web de [LUIS](luis-reference-regions.md) y las [API de creación](https://aka.ms/luis-authoring-api). Para su comodidad, la clave de creación puede realizar un número [limitado](luis-boundaries.md) de consultas de punto de conexión al mes. 

![Página de configuración de LUIS](./media/luis-how-to-account-settings/account-settings.png)

La clave de creación se usa para cualquier aplicación de su propiedad, así como para cualquier aplicación en la que aparezca como colaborador.

## <a name="authoring-key-regions"></a>Regiones de las claves de creación
La clave de creación es específica de la [región de creación](luis-reference-regions.md#publishing-regions). La clave no funciona en una región distinta. 

## <a name="reset-authoring-key"></a>Restablecimiento de la clave de creación
Si su clave de creación está en peligro, restablézcala. La clave se restablece en todas las aplicaciones en el sitio web de [LUIS](luis-reference-regions.md). Si crea sus aplicaciones a través de las API de creación, debe cambiar el valor de `Ocp-Apim-Subscription-Key` por la clave nueva. 

## <a name="delete-account"></a>Eliminación de cuenta
Vea [Data storage and removal](luis-concept-data-storage.md#accounts) (Almacenamiento y eliminación de datos) para obtener información acerca de qué datos se eliminan al eliminar la cuenta. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [clave de creación](luis-concept-keys.md#authoring-key). 


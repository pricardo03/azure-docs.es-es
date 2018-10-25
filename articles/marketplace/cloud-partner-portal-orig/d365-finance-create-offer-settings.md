---
title: Cumplimentación del formulario de configuración de oferta | Microsoft Docs
description: Explica los distintos campos que requieren valores en el formulario de configuración de la oferta para una nueva aplicación de Dynamics 365 Business Central.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: a050a8cfdd67f662786fc0b9b7ed5451c88dccb1
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48808442"
---
<a name="how-to-fill-out-the-offer-settings-form"></a>Cumplimentación del formulario de configuración de oferta
=======================================

El formulario de configuración de oferta es un formulario básico en el que se especifica la configuración de la oferta.
Los campos obligatorios se describen a continuación.

### <a name="offer-id"></a>Id. de oferta

`OfferId` es un identificador único de la oferta en un perfil del anunciante.
Este identificador será visible en las direcciones URL de producto. Puede contener solo caracteres alfanuméricos en minúscula o guiones (-). El identificador, que tendrá 50 caracteres como máximo, no puede terminar con un guion. Este campo queda bloqueado en cuanto se lanza una oferta.

Si, por ejemplo, el asociado "Contoso" crea un identificador de oferta denominado "sample-Web App", aparecerá en AppSource como:

&emsp;`https://appsource.microsoft.com/marketplace/apps/contoso.sample-Web App?tab=Overview`


### <a name="publisher-id"></a>Id. de publicador

Esta lista desplegable permite elegir el perfil del publicador en el que desea publicar esta oferta. Este campo queda bloqueado en cuanto se lanza una oferta.


### <a name="name"></a>NOMBRE

Este es el nombre para mostrar de la aplicación u oferta que aparece en Microsoft [AppSource](https://appsource.microsoft.com/). Puede tener un máximo de 50 caracteres.

> [!NOTE]
> El nombre corto debe ser el mismo que el nombre del anunciante especificado en el manifiesto de aplicación.

Haga clic en **Guardar** para guardar su progreso. El paso siguiente sería agregar información técnica para la oferta.

---
title: Cómo rellenar el formulario de información técnica | Microsoft Docs
description: Explica cómo especificar los valores del formulario de información técnica para una nueva aplicación de Dynamics 365 Business Central.
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
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5aa118815146287d00a3bb8ee7d5fce57a6ad9ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60397811"
---
<a name="how-to-fill-out-the-technical-info-form"></a>Cómo rellenar el formulario de información técnica
===========================================

1.  En la sección **Elegir tipo de aplicación**, cargue el archivo del paquete de extensiones (.app) y cualquier otro archivo de este tipo con el que su extensión tenga una dependencia.

    ![Información sobre el paquete de aplicación](./media/d365-financials/image015.png)

-   **Archivo de paquete de extensiones** Obligatorio: el archivo de paquete de extensiones (.app).

-   **Archivo de paquete de dependencias** Obligatorio si la aplicación tiene una dependencia en otra aplicación publicada en AppSource. Se trata del archivo .app de una extensión ya publicada en AppSource, de la cual depende la aplicación actual. 

-   **Archivo de paquete de biblioteca** Obligatorio si la aplicación tiene una dependencia sobre otra aplicación que *no* está publicada en AppSource. Se trata de un archivo .app de una aplicación existente, pero de una que no se ha publicado en AppSource ni lo hará.

-   **Automatización de prueba de la aplicación** Obligatorio: el paquete de pruebas de VS Code que debe crear para las pruebas automatizadas de las extensiones.

1. En la sección de **información adicional para la extensión**, cargue la información correspondiente. Esta información se usa durante la validación.

   ![Información adicional para el formulario de la extensión de aplicación](./media/d365-financials/image016.png)


-   **Dirección URL a la documentación del producto** Obligatoria: dirección URL a la documentación de la extensión.

-   **Escenarios de uso clave** Obligatorio: un documento que muestra información detallada sobre la configuración y el uso de la extensión. Puede encontrar un ejemplo en el artículo de [documentación sobre escenarios de uso](https://docs.microsoft.com/dynamics-nav/compliance/apptest-userscenario/).

-   **Versión de destino** Obligatorio: seleccione la versión en la que se va a implementar la aplicación. Seleccione **actual** si desea implementarla en la versión actual en el mercado. Seleccione **next minor** (siguiente versión secundaria) para realizar la implementación en la siguiente versión secundaria. Seleccione **next major** (siguiente versión principal) para realizar la implementación en la siguiente versión principal.

-   **Requiere SKU Premium** Opcional: seleccione el botón Premium si la aplicación requiere SKU Premium. La administración de servicios y la fabricación solo están disponibles en la opción Premium. Puede encontrar información detallada sobre las distintas opciones del paquete básico en comparación con el premium en el artículo [Changing Which Features are Displayed](https://docs.microsoft.com/dynamics365/financials/ui-experiences) (Cambio de las características que aparecen).

-   **Explicación de los errores de análisis de código** Opcional: un documento que muestra y justifica cualquier código que no cumple los requisitos.

-   **Explicación de funcionalidad principal afectada** Opcional: un documento que enumera y explica cualquier funcionalidad principal que esté limitada por la extensión.

-   **Cuentas de prueba** Opcional: cuentas de usuario para servicios remotos, sitios web, etc que se necesitan para completar totalmente las pruebas de uso.

-   **Excepciones de requisitos de la experiencia de usuario** Opcional: un documento que muestra y justifica cualquier requisito de la experiencia de usuario que no cumple la extensión.

El siguiente paso consiste en agregar los detalles del escaparate para la oferta.

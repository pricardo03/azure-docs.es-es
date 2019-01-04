---
title: 'Oferta de servicios de consultoría de Azure y Dynamics 365: definir la configuración de la oferta | Microsoft Docs'
description: Guía para definir los configuración de una oferta de servicios de consultoría de Azure o Dynamics 365 en Cloud Partner Portal.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: pbutlerm
ms.openlocfilehash: 9cb52aea56e9c1183af9ca76201ab65c62a58f22
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53972178"
---
# <a name="offer-settings-tab"></a>Pestaña Configuración de la oferta

En la pantalla **Nueva oferta**, el primer paso es crear la identidad de la oferta. La identidad de la oferta consta de tres partes: **Id. de oferta**, **Id. de anunciante** y **Nombre**. Cada una de estas partes se explica en las secciones siguientes.

![Creación de una oferta de servicios de consultoría: pestaña Configuración de la oferta](media/consultingoffer-settings-tab.png)

*Id. de oferta*

Este identificador es un nombre único que se crea cuando envía por primera vez una oferta. Debe constar únicamente de caracteres alfanuméricos en minúscula, guiones o caracteres de subrayado. El **identificador de la oferta** se podrá ver en la dirección URL y afecta a los resultados del motor de búsqueda. Por ejemplo, *NombreDeSuEmpresa_ServicioDeEjemplo*

Como se puede ver en el ejemplo, el **identificador de la oferta** se anexa al identificador del anunciante para crear un identificador único. Esto se expone como un vínculo permanente que los motores de búsqueda pueden reservar e indexar.

*Una vez que una oferta está en vigor, su identificador no se puede actualizar*.

*Id. del editor*

Este identificador está relacionado con la cuenta. Si inicia sesión con la cuenta de su organización, el identificador de **anunciante** se mostrará en el menú desplegable.

*Nombre*

Esta cadena es la que se mostrará como el nombre de la oferta en AppSource o Azure Marketplace. El campo de *nombre* está limitado a 50 caracteres.  Es posible que el revisor tenga que deba editar el título para que pueda anexarse el nombre de la oferta con la duración y el tipo de oferta.

El ejemplo siguiente de Edgewater Fullscope muestra cómo se compone el nombre de la oferta. El nombre de la oferta aparece como:

![Creación de una oferta de servicios de consultoría](media/cppsampleconsultingoffer.png)

El nombre de la oferta consta de cuatro partes:

-   **Duración**: se define en la pestaña **Detalles del escaparate** del editor. La duración se puede expresar en horas, días o semanas.
-   **Tipo de servicio**: se define en la pestaña **Detalles del escaparate** del editor. Los tipos de servicio son `Assessment`, `Briefing`, `Implementation`, `Proof of concept` y `Workshop`.
-   **Preposición**: la inserta el revisor.
-   **Nombre**: se define en la página **Configuración de oferta**.

>[!Note]
>El campo de nombre está limitado a 50 caracteres. Existe la posibilidad de que el revisor tenga editar el nombre que envíe para que la duración y el tipo de oferta se anexen al nombre.

En la lista siguiente se proporcionan varios nombres de oferta adecuados:

-   Fundamentos de los servicios profesionales: Sesión informativa de 1 hora
-   Plataforma de migración a la nube: Sesión informativa de 1 hora
-   PowerApps y Microsoft Flow: Taller de 1 día
-   Azure Machine Learning Services: PoC de 3 semanas
-   Solución de venta para tiendas físicas y virtuales: Sesión informativa de 1 hora
-   Traiga sus propios datos: Taller de 1 hora
-   Análisis de la nube: Taller de 3 días
-   Aprendizaje de Power BI: Taller de 3 días
-   Solución de administración de ventas: Implementación de 1 semana
-   Inicio rápido de CRM: Taller de 1 día
-   Dynamics 365 for Sales: Evaluación de 2 días

Después de haber completado la información de la pestaña **Configuración de oferta**, puede guardar su envío. El nombre de la oferta aparece ahora encima del editor y puede encontrarlo de nuevo en Todas las ofertas.

**Pasos siguientes**

Ya puede especificar [los detalles del escaparate y determinar si la publicación se realiza en Azure Marketplace o en AppSource](./cpp-consulting-service-storefront-details.md).

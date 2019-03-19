---
title: 'Oferta de Dynamics 365 for Customer Engagement: Pestaña Información técnica de Azure Marketplace | Microsoft Docs'
description: Especificación de la información técnica para una aplicación de Dynamics 365 for Customer Engagement en AppSource Marketplace.
services: Dynamics 365 for Customer Engagement Offer, Azure, Marketplace, Cloud Partner Portal, AppSource
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
ms.date: 03/05/2019
ms.author: pbutlerm
ms.openlocfilehash: 1dd488c2eb419b5e210a48d7a94f7d0bb423a2b1
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2019
ms.locfileid: "57731484"
---
# <a name="dynamics-365-for-customer-engagement-technical-info-tab"></a>Pestaña Información técnica de Dynamics 365 for Customer Engagement

La pestaña **Información técnica** de la página **Nueva oferta** le permite especificar información detallada acerca de la aplicación Dynamics 365 for Customer Engagement que incluye el paquete CRM y los recursos del logotipo de marketing.  Esta pestaña se divide en cuatro secciones: **Información de la aplicación**, **Paquete CRM**, **Disponibilidad del paquete CRM** y **Artefactos de marketing**. Un asterisco (*) junto al nombre del campo indica que es obligatorio. 


## <a name="application-info-section"></a>Sección Información de la aplicación

En esta sección proporcionará más detalles acerca de la aplicación de Dynamics 365.

![Sección Información de la aplicación de la pestaña Información técnica](./media/dynce-technical-info-tab1.png)

En la siguiente tabla se describen estos campos.

|      Campo                    |    DESCRIPCIÓN                  |
|    ---------                  |  ---------------                |
|   Modelo de licencia básico          |  El modelo de licencia determina cómo se asignan los clientes a la aplicación en el centro de administración de Dynamics 365. Las licencias de **recurso** se basan en instancias, mientras que las de **usuario** se asignan una por cada inquilino.  |
|  Acceso saliente de S2S y acceso al almacenamiento seguro de CRM |  Permite la configuración del acceso al almacenamiento seguro de CRM o del acceso saliente de servidor a servidor (S2S). *Esta característica requiere una atención especial por parte del equipo de Dynamics 365 durante la fase de certificación.* Microsoft se pondrá en contacto con usted para completar los pasos adicionales para admitir esta característica.  |
| Suscripción a los eventos del ciclo de vida del CRM | La integración con los eventos del ciclo de vida de Dynamics 365 requiere que proporcione un servicio dedicado que se registra mediante un acuerdo especial con Microsoft. *Esta característica requiere una atención especial por parte del equipo de Dynamics 365 durante la fase de certificación.* Microsoft se pondrá en contacto con usted para completar los pasos adicionales para admitir esta funcionalidad.  |
| Dirección URL de configuración de la aplicación | Dirección URL de la página web que permite al usuario configurar la aplicación |
| Productos de Dynamics 365 a los que se aplica  | Seleccione los productos de Dynamics 365 a los que se aplica esta oferta. Esta oferta aparecerá en los productos seleccionados en AppSource.  |
| Cambios solo de marketing         | Si establece esta opción en Sí, indicará que solo se han realizado cambios de marketing o descriptivos en la oferta ya existente.  Estos cambios permiten que la oferta pueda omitir las etapas de certificación y aprovisionamiento.  |
|  |  |


## <a name="crm-package-section"></a>Sección Paquete CRM

En esta sección proporcionará más detalles acerca del archivo de paquete de AppSource.  Los equipos de validación y certificación de Dynamics 365 usarán esta información.

![Sección Paquete CRM de la pestaña Información técnica](./media/dynce-technical-info-tab2.png)

En la siguiente tabla se describen estos campos.

|      Campo                    |    DESCRIPCIÓN                  |
|    ---------                  |  ---------------                |
|  Nombre de archivo del paquete     |  Nombre de archivo del paquete (.zip).  Este nombre *no* es público y lo usará internamente el equipo de certificación de Dynamics 365.  |
|  URL                          |  Dirección URL de una cuenta de Azure Storage que contiene el archivo de paquete cargado. Esta dirección URL debe incluir una clave SAS de solo lectura para permitir que nuestro equipo pueda recoger el paquete para su comprobación.  |
| Más de un paquete CRM     | Seleccione Sí SOLO si admite varias versiones de CRM con distintos paquetes.  Cada versión tendrá el correspondiente archivo de paquete que debe crear de manera individual.  |
| Recurso de escenarios y casos de uso   | Permite la carga de un documento de especificación funcional de la aplicación para que lo use el equipo de validación de Dynamics 365.  El formato preferido para esta especificación es la [plantilla de escenario para usuarios E2E](https://isvdocumentation.blob.core.windows.net/d365documentation/Power%20Platform%20E2E%20document.docx).  |
|  |  |


## <a name="crm-package-availability-section"></a>Sección Disponibilidad del paquete CRM

En esta sección puede seleccionar en qué regiones geográficas estará disponible la aplicación para los clientes.  La implementación en las siguientes regiones soberanas *requiere un permiso y una validación especiales* durante el proceso de certificación: [Alemania](https://docs.microsoft.com/azure/germany/), la [nube del gobierno de EE. UU.](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) y TIP.


## <a name="marketing-artifacts-section"></a>Sección de artefactos de marketing

En esta sección deberá cargar un logotipo de la aplicación que se usará para representar al paquete en AppSource Marketplace.  La imagen del logotipo debe tener formato PNG y un tamaño de 255 x 115 píxeles.


## <a name="next-steps"></a>Pasos siguientes

Se recomienda que ofrezca una demostración de la aplicación mediante la [pestaña Versión de prueba](./cpp-testdrive-tab.md)

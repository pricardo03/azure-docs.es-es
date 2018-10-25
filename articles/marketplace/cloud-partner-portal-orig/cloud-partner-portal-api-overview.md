---
title: Referencia de API del Cloud Partner Portal | Microsoft Docs
description: Descripción, requisitos previos para el uso y lista de las operaciones de API del Marketplace.
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
ms.openlocfilehash: 116eb48330381c7560c55ea9535b3c1b7c6a6a70
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48807792"
---
<a name="cloud-partner-portal-api-reference"></a>Referencia de API del Cloud Partner Portal
==================================

Las API de REST de Cloud Partner Portal permiten la recuperación y la manipulación mediante programación de las cargas de trabajo, ofertas y perfiles del publicador. Las API de usan el control de acceso basado en roles (RBAC) para exigir los permisos correctos en el tiempo de procesamiento.

Esta referencia proporciona los detalles técnicos de las API de REST de Cloud Partner Portal. Los ejemplos de carga en este documento son solo para referencia y están sujetos a cambios a medida que se agregan nuevas funcionalidades.


<a name="prerequisites-and-considerations"></a>Requisitos previos y consideraciones
-------------------------------

Antes de usar las API, debe revisar:

- El artículo [API Prerequisites](./cloud-partner-portal-api-prerequisites.md) (Requisitos previos de API) para obtener información sobre cómo agregar una entidad de servicio a su cuenta y obtener el token un acceso de Azure Active Directory (Azure AD) para la autenticación. 
- Las dos estrategias de [Control de simultaneidad](./cloud-partner-portal-api-concurrency-control.md).
disponibles para llamar a estas API.
- [Consideraciones](./cloud-partner-portal-api-considerations.md) de API adicionales, como control de versiones y control de errores.


<a name="common-tasks"></a>Tareas comunes
------------
Esta referencia ofrece información sobre las API adecuadas para realizar las siguientes tareas comunes.


### <a name="offers"></a>Ofertas

-   [Recuperar todas las ofertas](./cloud-partner-portal-api-retrieve-offers.md)
-   [Recuperar una oferta específica](./cloud-partner-portal-api-retrieve-specific-offer.md)
-   [Recuperación del estado de la oferta](./cloud-partner-portal-api-retrieve-offer-status.md)
-   [Creación de una oferta](./cloud-partner-portal-api-creating-offer.md)
-   [Publicación de una oferta](./cloud-partner-portal-api-publish-offer.md)

### <a name="operations"></a>Operaciones

-   [Recuperación de operaciones](./cloud-partner-portal-api-retrieve-operations.md)
-   [Cancelación de operaciones](./cloud-partner-portal-api-cancel-operations.md)

### <a name="publish-an-app"></a>Publicar una aplicación

-   [Publicación](./cloud-partner-portal-api-go-live.md)

### <a name="other-tasks"></a>Otras tareas

-   [Pricing for virtual machine offers](./cloud-partner-portal-api-setting-price.md) (Precios de ofertas de máquinas virtuales)

### <a name="troubleshooting"></a>solución de problemas

-   [Solución de errores de autenticación](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)

---
title: Solución de errores de autenticación comunes | Microsoft Docs
description: Ofrece asistencia con errores de autenticación comunes al usar las API de Cloud Partner Portal.
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
ms.openlocfilehash: 12ed395855b9d870f8f6e3564dc5b3b899b9c6c1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60326485"
---
# <a name="troubleshooting-common-authentication-errors"></a>Solución de errores de autenticación comunes

Este artículo ofrece asistencia con errores de autenticación comunes al usar las API de Cloud Partner Portal.

## <a name="unauthorized-error"></a>Error no autorizado

Si recibe errores `401 unauthorized` sistemáticamente, compruebe que dispone de un token de acceso válido.  Si aún no lo ha hecho, cree una aplicación de Azure Active Directory (Azure AD) básica y una entidad de servicio, tal y como se describe en [Uso del portal para crear una aplicación de Azure Active Directory y una entidad de servicio con acceso a los recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). A continuación, utilice la aplicación o una solicitud HTTP POST simple para comprobar el acceso.  Deberá incluir el identificador de inquilino, el identificador de aplicación, el identificador de objeto y la clave secreta para obtener el token de acceso, tal como se muestra en la siguiente imagen:

![Solución del error 401](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-401-error.jpg)


## <a name="forbidden-error"></a>Error prohibido

Si recibe un error `403 forbidden`, asegúrese de que se ha agregado la entidad de servicio correcta a su cuenta de publicador en Cloud Partner Portal.
Siga los pasos descritos en la página [Requisitos previos](./cloud-partner-portal-api-prerequisites.md) para agregar la entidad de servicio en el portal.

Si se ha agregado la entidad de servicio correcta, compruebe el resto de la información. Preste especial atención al identificador de objeto especificado en el portal. En la página de registro de la aplicación de Azure Active Directory hay dos identificadores de objeto; el que debe usar es el identificador de objeto local. Para encontrar el valor correcto, vaya a la página **Registros de aplicaciones** de su aplicación y haga clic en el nombre de la aplicación en **Aplicación administrada en directorio local**. Esto le llevará a las propiedades locales de la aplicación, donde puede encontrar el identificador de objeto correcto en la página **Propiedades**, tal como se muestra en la siguiente ilustración. Además, asegúrese de usar el identificador de publicador correcto al agregar la entidad de servicio y realizar la llamada de API.

![Solución del error 403](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-403-error.jpg)

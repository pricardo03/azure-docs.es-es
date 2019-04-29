---
title: Configuración de un nombre de dominio personalizado para la instancia de API Management | Microsoft Docs
description: En este tema, se explica cómo configurar un nombre de dominio personalizado para la instancia de Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 12/14/2017
ms.author: apimpm
ms.openlocfilehash: a771b437258046f937b97a9e37ffedbe0a17c1c1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60693607"
---
# <a name="configure-a-custom-domain-name"></a>Configurar un nombre de dominio personalizado 

Cuando se crea una instancia de API Management (APIM), Azure la asigna a un subdominio de azure-api.net (por ejemplo, `apim-service-name.azure-api.net`). Sin embargo, los puntos de conexión de APIM también se pueden exponer utilizando un nombre de dominio propio, como **contoso.com**. En este tutorial, se muestra cómo se asigna un nombre DNS personalizado existente a los puntos de conexión expuestos por una instancia de Azure API Management.

> [!WARNING]
> Los clientes que deseen usar la asignación de certificados para mejorar la seguridad de sus aplicaciones deben usar un nombre de dominio personalizado > y el certificado que administran, no el certificado predeterminado. Los clientes que asignen el certificado predeterminado en su lugar tendrán > una gran dependencia de las propiedades del certificado que no controlen, no siendo esto una práctica recomendada.

## <a name="prerequisites"></a>Requisitos previos

Para seguir los pasos que se describen en este artículo, debe tener:

+ Una suscripción de Azure activa.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Una instancia de APIM. Para más información, consulte [Creación de una instancia de Azure API Management](get-started-create-service-instance.md).
+ Nombre de dominio personalizado que sea de su propiedad. El nombre de dominio personalizado que desee usar debe adquirirse por separado y hospedarse en un servidor DNS. Este tema no contiene instrucciones acerca de cómo hospedar un nombre de dominio personalizado.
+ Debe tener un certificado válido con una clave pública y privada (. PFX). El firmante o el nombre alternativo del firmante (SAN) debe coincidir con el nombre de dominio; de este modo, APIM puede exponer de forma segura direcciones URL a través de SSL.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="use-the-azure-portal-to-set-a-custom-domain-name"></a>Uso de Azure Portal para configurar un nombre de dominio personalizado

1. Acceda a la instancia de APIM de [Azure Portal](https://portal.azure.com/).
1. Seleccione **Dominios personalizados y SSL**.
    
    Hay una serie de puntos de conexión a la que puede asignar un nombre de dominio personalizado. En la actualidad, están disponibles los siguientes: 
   + **Proxy** (el valor predeterminado es `<apim-service-name>.azure-api.net`), 
   + **Portal** (el valor predeterminado es `<apim-service-name>.portal.azure-api.net`),     
   + **Management** (el valor predeterminado es `<apim-service-name>.management.azure-api.net`), 
   + **SCM** (el valor predeterminado es `<apim-service-name>.scm.azure-api.net`).

     >[!NOTE]
     > Puede actualizar algunos o todos los puntos de conexión. Por lo general, los clientes actualizan **Proxy** (esta dirección URL se utiliza para llamara a la API expuesta a través de API Management) y **Portal** (dirección URL del portal del desarrollador). Por lo general, los clientes de APIM utilizan los puntos de conexión **Management** y **SCM** internamente y, por tanto, se les asigna con menor frecuencia un nombre de dominio.

1. Seleccione el punto de conexión que desee actualizar. 
1. En la ventana de la derecha, haga clic en **Personalizar**.

   + En **Nombre de dominio personalizado**, especifique el nombre que desee usar. Por ejemplo, `api.contoso.com`. También se pueden usar nombres de dominio comodín (por ejemplo, *. dominio.com).
   + En el **certificado**, seleccione un certificado de Key Vault. También puede cargar una válida. PFX de archivo y proporcione su **contraseña**, si el certificado está protegido con una contraseña.

     > [!TIP]
     > Si usa Azure Key Vault para administrar el certificado SSL de dominio personalizado, asegúrese de que se inserta el certificado en Key Vault [como un *certificado*](https://docs.microsoft.com/rest/api/keyvault/CreateCertificate/CreateCertificate), no un *secreto*. Si el certificado se establece en autorotate, API Management recogerá la versión más reciente automáticamente.

1. Haga clic en Aplicar.

    >[!NOTE]
    >El proceso de asignación del certificado puede tardar unos 15 minutos o más según el tamaño de la implementación. La SKU de desarrollador tiene un tiempo de inactividad, mientras que las SKU básica y superiores no.

[!INCLUDE [api-management-custom-domain](../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Pasos siguientes

[Actualización y escalado del servicio](upgrade-and-scale.md)

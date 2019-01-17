---
title: 'Oferta de aplicación Dynamics 365 for Customer Engagement: Pestaña Versión de prueba en Azure Marketplace | Microsoft Docs'
description: Configuración de una versión de prueba para una oferta de la aplicación Dynamics 365 for Customer Engagement en AppSource Marketplace.
services: Azure, Marketplace, AppSource, Cloud Partner Portal, Dynamics 365 for Customer Engagement
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
ms.date: 12/25/2018
ms.author: pbutlerm
ms.openlocfilehash: 664a2c6bfc4a73b7d792b71c4b81df54b05fcd74
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54082525"
---
# <a name="dynamics-365-for-customer-engagement-application-test-drive-tab"></a>Pestaña Versión de prueba de la aplicación Dynamics 365 for Customer Engagement

Use la pestaña **Versión de prueba** para crear una experiencia de evaluación para sus clientes.  Proporciona a los clientes una versión de prueba práctica y autodidacta con las principales características y ventajas de la oferta, y una demostración en un escenario de implementación del mundo real.  De las opciones de prueba disponibles, Versión de prueba es la más eficaz a la hora de generar clientes potenciales de alta calidad y lograr un aumento en la conversión de estos.  Para más información, consulte [¿Qué es la versión de prueba?](../../cloud-partner-portal-orig/what-is-test-drive.md)

La experiencia de versión de prueba para aplicaciones de Dynamics 365 se ejecuta automáticamente como una solución hospedada por Microsoft.  Para más información, consulte [Versión de prueba hospedada](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/hosted-test-drive).

La pestaña Versión de prueba tiene tres secciones posibles: **Versión de prueba**, **Detalles** y **Configuración técnica**.  Las dos últimas secciones solo aparecen después de habilitar la funcionalidad Versión de prueba.  Un asterisco (*) junto al nombre del campo indica que es obligatorio. 


## <a name="test-drive-section"></a>Sección Versión de prueba

Para habilitar esta funcionalidad, seleccione **Sí** para **habilitar una versión de prueba**.


## <a name="details-section"></a>Sección Detalles

En la sección **Detalles** proporcionará la información básica de Versión de prueba.   

![Sección Detalles de la versión de prueba](./media/test-drive-tab-details.png)

En la tabla siguiente se describen los campos obligatorios para configurar la versión de prueba de la aplicación de Dynamics 365.

|      Campo                    |    DESCRIPCIÓN                  |
|    ---------                  |  ---------------                |
|      DESCRIPCIÓN              |   Describe lo que puede realizarse en la versión de prueba. Puede usar etiquetas HTML básicas para dar formato a esta descripción. Por ejemplo, &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt; y títulos.  |
|  Manual del usuario                  |   Cargue un manual del usuario que los clientes puedan usar para completar la experiencia de versión de prueba. Este documento debe ser un archivo PDF.              |
|  Vídeo de demostración de la versión de prueba (opcional) |  Puede proporcionar un tutorial en vídeo de la versión de prueba. Un cliente puede ver este vídeo antes de realizar una prueba. Proporcione una dirección URL de YouTube o Vimeo para el vídeo. Si selecciona **+ Agregar vídeo**, se le solicitará que proporcione la siguiente información:<ul><li>NOMBRE</li><li>URL</li><li>Miniatura (formato PNG, 533 x 324 píxeles)</li></ul>  |
|   |   |


## <a name="technical-configuration-section"></a>Sección Configuración técnica

En esta sección, proporcionará detalles técnicos sobre la versión de prueba.

![Sección Detalles de la versión de prueba](./media/test-drive-tab-tech-config.png)

Los campos tienen las siguientes funciones:

|      Campo                    |    DESCRIPCIÓN                  |
|    ---------                  |  ---------------                |
| Tipo de versión de prueba            | Elija **Hospedada por Microsoft (Dynamics 365 for Customer Engagement)**.  |
| Número máximo de versiones de prueba simultáneas    | Número de instancias simultáneas de una versión de prueba activa en cualquier momento determinado de tiempo. Cada usuario consumirá una licencia de Dynamics mientras que esté activa su versión de prueba, por lo que tendrá que garantizar este número de licencias de Dynamics para que estén disponibles para los usuarios de la versión de prueba. Se recomienda un valor entre 3 y 5.  |
| Duración de la versión de prueba (horas)   | Número máximo de horas que estará activa la instancia de la versión de prueba del usuario. Una vez que se supera este tiempo, la instancia se desaprovisionará del inquilino. Se recomienda un valor entre 2 y 24 horas dependiendo de la complejidad de la aplicación. El usuario siempre puede solicitar otra versión de prueba si se agota el tiempo y desea volver a realizar una evaluación.  |
| Dirección URL de la instancia                  | Dirección URL a la que irá inicialmente la versión de prueba. Esta suele ser la dirección URL de la instancia de Dynamics 365 que contiene la aplicación y los datos de ejemplo.  |
| Identificador de inquilino de Azure AD            | El identificador único del inquilino de Azure para la instancia de Dynamics 365. Para recuperar este valor, inicie sesión en Azure Portal y vaya a **Azure Active Directory** >  **Seleccionar propiedades** > **Copiar el identificador de directorio**.  |
| Identificador de aplicación de Azure AD               | Identificador único de la aplicación de Azure AD  |
| Clave de aplicación de Azure AD              | El secreto de la aplicación de Azure AD, por ejemplo: `IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=` |
| Nombre del inquilino de Azure AD          | El nombre del inquilino de Azure para la instancia de Dynamics 365. Use el formato <nombreinquilino.>onmicrosoft.com. Por ejemplo: `testdrive.onmicrosoft.com`  |
| Dirección URL de API web de la instancia          | Dirección URL de API web para la instancia de Dynamics 365. Para recuperar este valor, inicie sesión en la instancia de Microsoft Dynamics 365 y vaya a **Configuración** > **Personalización** > **Recursos de desarrollador** > **API web de la instancia (copie esta dirección URL)**. Valor de ejemplo: `https://testdrive.crm.dynamics.com/api/data/v9.0`  |
| Nombre de rol                     | Nombre del rol de seguridad personalizado de Dynamics 365 que ha creado para la versión de prueba y que se asignará a los usuarios cuando la ejecuten, por ejemplo `testdriveuser`. |
|  |  |

Una vez que haya dado toda la información necesaria, seleccione **Guardar**.


## <a name="next-steps"></a>Pasos siguientes

A continuación, proporcionará información de marketing y ventas en la [pestaña Detalles del escaparate electrónico](./cpp-storefront-details-tab.md).


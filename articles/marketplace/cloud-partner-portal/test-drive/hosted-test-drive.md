---
title: Versión de prueba hospedada | Microsoft Docs
description: Configuración y mantenimiento de una versión de prueba hospedada de Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5057c0c781cb9ec60ecde7dd3f4bf96089b902df
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60559796"
---
# <a name="hosted-test-drive"></a>Versión de prueba hospedada

Una versión de prueba hospedada elimina la complejidad de la configuración haciendo que Microsoft hospede y mantenga el servicio que realiza el aprovisionamiento y desaprovisionamiento de usuarios de la versión de prueba. Este artículo es para aquellos anunciantes que tienen su oferta en AppSource o que van a crear una nueva y desean ofrecer una versión de prueba hospedada que se conecte con una instancia de Dynamics 365 for Customer Engagement, Dynamics 365 for Finance and Operations, o Dynamics 365 Business Central.

## <a name="how-to-publish-a-test-drive"></a>Publicación de una versión de prueba

Vaya a la oferta existente o cree una nueva.

Seleccione la opción Versión de prueba en el menú lateral.

Seleccione \'Sí\' en la opción \'Enable a Test Drive\' (Habilitar una versión de prueba).

Proporcione los siguientes campos en la sección \'Detalles\'.

- **Descripción**: Proporciona una introducción a la versión de prueba. Este texto se mostrará al usuario durante el aprovisionamiento de la versión de prueba. Este campo es compatible con HTML, si desea proporcionar contenido con formato.
- **Manual del usuario**: Cargue un manual del usuario detallada (archivo de tipo .pdf) que ayuda a los usuarios de la versión de prueba a aprender a usar la aplicación.
- **Vídeo de demostración de la unidad de prueba**: Si lo desea cargar un vídeo que presenta la aplicación.

Conceda permiso a AppSource para aprovisionar y desaprovisionar los usuarios de la versión de prueba del inquilino mediante las instrucciones que puede encontrar [aquí](https://github.com/Microsoft/AppSource/blob/patch-1/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md).

En este paso, generará los valores \'Identificador de aplicación de Azure AD\' y \'Clave de aplicación de Azure AD\' que se mencionan a continuación.

Proporcione los siguientes campos en la sección de \'configuración técnica\':

- **Tipo de versión de prueba**: Elija \'Microsoft Hosted (ejemplo de Dynamics 365 for Customer Engagement)' opción. Esto indica que Microsoft hospedará y mantendrá el servicio que realiza el aprovisionamiento y desaprovisionamiento de usuarios de la versión de prueba.
- **Las versiones de prueba simultáneas máximas**: Establezca este campo en el número de usuarios simultáneos que puede tener una unidad de prueba activa un momento determinado de tiempo. Cada usuario consumirá una licencia de Dynamics mientras que esté activa su versión de prueba, por lo que tendrá que garantizar este número de licencias de Dynamics para que estén disponibles para los usuarios de la versión de prueba. Se recomienda un valor entre 3 y 5.
- **Unidad de duración de la prueba (horas)**: Establezca este campo en el número de horas de los usuarios a la para que versión de prueba estará activa. Después de este número de horas, se desaprovisionará al usuario del inquilino. Se recomienda un valor entre 2 y 24 horas dependiendo de la complejidad de la aplicación. El usuario siempre puede solicitar otra versión de prueba si se agota el tiempo y desea volver a acceder a la versión de prueba.
- **Dirección URL de la instancia**: Proporcione una dirección URL que el usuario de la versión de prueba inicialmente se abrirá cuando se inicie la versión de prueba. Esta suele ser la dirección URL de la instancia de Dynamics 365 que contiene la aplicación y los datos de ejemplo. Valor de ejemplo: https:\//testdrive.crm.dynamics.com
- **Id. de inquilino de Azure AD**: Proporcione el identificador del inquilino de Azure para la instancia de Dynamics 365. Para recuperar este valor, inicie sesión en Azure Portal y vaya a \'Azure Active Directory\' -\> Seleccionar propiedades en la hoja del menú -\> Copiar identificador de directorio. Valor de ejemplo: 72f988bf-86f1-41af-91ab-2d7cd0111234
- **Identificador de aplicación de Azure AD**: Id. de la aplicación de Azure AD que creó en el paso 7. \ valor de ejemplo: 53852862-a2ae-4e43-9461-faa49650a096
- **Clave de aplicación de Azure AD**: Secreto de la aplicación de Azure AD que creó en el paso 7. \ valor de ejemplo: IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=
- **Nombre del inquilino de Azure AD**: Proporcione el nombre del inquilino de Azure para la instancia de Dynamics 365. Use el formato \<nombreinquilino.\> onmicrosoft.com. Valor de ejemplo: testdrive.onmicrosoft.com
- **Instancia de la dirección URL de Web API**: Proporcione la URL de la API de Web para la instancia de Dynamics 365. Para recuperar este valor, inicie sesión en la instancia de Microsoft Dynamics 365 y vaya a Configuración -\> Personalización -\> Recursos de desarrollador -\> API web de la instancia (copie esta dirección URL). Valor de ejemplo: https:\//testdrive.crm.dynamics.com/api/data/v9.0 
- **Nombre de la función**: Proporcione el nombre del rol de seguridad de Dynamics 365 personalizado que ha creado para la versión de prueba. Este es el rol que se asignará a los usuarios durante su versión de prueba. Valor de ejemplo: testdriverole

## <a name="next-steps"></a>Pasos siguientes

Cuando esté listo **publicar** su oferta, una vez que haya pasado su aplicación certificación, tendrá un **preview** de la oferta. Inicie una versión de prueba en la interfaz de usuario y compruebe que las versiones de prueba se están ejecutando correctamente. Cuando se sienta cómodo con la oferta de versión preliminar, ahora es el momento de **puesta en marcha!**

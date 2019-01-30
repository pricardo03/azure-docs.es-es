---
title: 'Publicación de ofertas de aplicación de Azure: Azure Marketplace | Microsoft Docs'
description: Describe el proceso y los pasos de publicación de una oferta de aplicación de Azure en Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: pbutlerm
ms.openlocfilehash: ad62b306849b2291c81399cedc1634057e2eec9d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54828101"
---
# <a name="publish-azure-application-offer"></a>Publicación de una oferta de aplicación de Azure

Después de crear una oferta al proporcionar la información de la página **Nueva oferta**, puede publicarla. Seleccione **Publicar** para iniciar el proceso de publicación.

El diagrama siguiente muestra los pasos principales del proceso de publicación de una oferta para "lanzarla".

![Pasos para la publicación de una oferta](./media/offer-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Descripción detallada de los pasos de publicación

En la siguiente tabla se enumera y se describe cada paso de publicación, y se incluye una estimación de tiempo para completar cada paso.  Los tiempos, estimados en días, se definen como los hábiles, fines de semana y festivos excluidos.

|  **Paso de publicación**           | **Hora**    | **Descripción**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Validar requisitos previos         | <15 min    | Se validan la información y la configuración de la oferta.                        |
| Validar la configuración de los ingresos influidos | <15 min  | Se comprueba la atribución de uso de recursos de Azure para la oferta.             |
| Certificación                  | <1 día     | El equipo de certificación de Azure analiza la oferta. Se examina la oferta en busca de virus, malware, cumplimiento de seguridad y problemas de seguridad. La oferta se comprueba para ver si cumple todos los criterios de elegibilidad. Para más información, consulte los [requisitos previos](./cpp-prerequisites.md). Si se detecta un problema, se proporcionan comentarios. |
| Validación de la versión de prueba          | <2 horas   | (Opcional) Si hay una versión de prueba, Microsoft valida que puede implementar y replicar.  |
| Empaquetado y registro de la generación de clientes potenciales | <1 hora  | Los recursos técnicos de la oferta se empaquetan para el uso del cliente y se configuran e implementan los sistemas del cliente potencial. |
|  Aprobación del publicador             |  manual    | Revisión final del publicador y confirmación antes del lanzamiento de la oferta. La oferta está ahora disponible para la versión preliminar.  Puede implementar la oferta en las suscripciones seleccionadas (en los pasos de información de la oferta) para comprobar que cumple todos los requisitos.  Una vez verificada la oferta, seleccione **Transmitir** para que la oferta pueda pasar al paso siguiente. |
| Revisión de Microsoft                | Entre 7 y 14 días | Microsoft revisa holísticamente la aplicación de Azure y le envía por correo electrónico si se detectan problemas.  La duración de este paso depende de la complejidad de la aplicación, y de los problemas detectados y la rapidez con la que responda ante ellos.  |
| En vivo                           | <1 día | La oferta se lanza, se replica en las regiones especificadas y se pone a disposición del público. |
|   |&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|   |
 
Puede supervisar el proceso de publicación en la pestaña **Status** (Estado) de la oferta en Cloud Partner Portal.

![Pestaña de estado de una oferta de aplicación de Azure](./media/offer-status-tab.png)

Cuando termina el proceso de publicación, la oferta se mostrará en la [categoría de aplicaciones de Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/).


## <a name="errors-and-review-feedback"></a>Errores y comentarios de revisión

Además de mostrar el estado de publicación de la oferta, la pestaña **Status** (Estado) también muestra los mensajes de error y los comentarios del paso de **revisión de Microsoft**.  Normalmente, a los problemas de revisión se hace referencia con solicitudes de incorporación de cambios (PR).  Cada solicitud de incorporación de cambios está vinculada a un elemento en línea de Visual Studio Team Services (VSTS, que ahora se llama [Azure DevOps](https://azure.microsoft.com/services/devops/)) con los detalles sobre el problema.  En la siguiente imagen se muestra un ejemplo de una referencia de solicitud de incorporación de cambios de revisión.  Para situaciones más complejas, los equipos de revisión y soporte técnico pueden escribir por correo electrónico. 

![Pestaña Status (Estado) con los comentarios de revisión](./media/status-tab-ms-review.png)

Para que la oferta pueda continuar el proceso de publicación, debe resolver todos los problemas notificados.  En el siguiente diagrama se ilustra cómo se relacionan estos comentarios con la publicación.

![Pasos de publicación con comentarios de VSTS](./media/pub-flow-vsts-access.png)


### <a name="vsts-access"></a>Acceso a VSTS

Para ver los elementos de VSTS a los que se hace referencia en los comentarios de revisión, los publicadores deben tener la autorización pertinente.  De lo contrario, los publicadores nuevos reciben una página de respuesta de tipo `401 - Not Authorized`.  Para solicitar acceso al sistema de VSTS de revisión de ofertas, realice los siguientes pasos:

1. Recopile la siguiente información:
    - El nombre y el identificador del publicador
    - Tipo de oferta (aplicación de Azure), nombre de la oferta e identificador de la SKU
    - Vínculo de la solicitud de incorporación de cambios, por ejemplo: `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/contoso/pullrequest/<number>` Esta dirección URL se puede recuperar del mensaje de notificación o de la dirección de la página de respuesta de tipo 401.
    - Direcciones de correo electrónico de las personas de su organización de publicación a las que desea conceder acceso.  Estas deben incluir las direcciones del propietario que proporcionó al registrarse como publicador en Cloud Partner Portal.
2. Cree una incidencia de soporte técnico.  En la barra de título de Cloud Partner Portal, seleccione el botón **Help** (Ayuda) y elija **Support** (Soporte técnico) en el menú.  El navegador web predeterminado debe iniciar la página de Microsoft con el nuevo incidente de soporte técnico e ir a ella.  (Es posible que deba iniciar sesión).
3. Especifique **Problem type** (Tipo de problema) como **Marketplace Onboarding** (Incorporación a Marketplace) y **Category** (Categoría) como **Access problem** (Problema de acceso), y seleccione **Start request** (Iniciar solicitud).

    ![Categoría de la incidencia de soporte técnico](./media/support-incident1.png)

4. En la página **Step 1 of 2** (Paso 1 de 2), proporcione la información de contacto y seleccione **Continue** (Continuar).
5. En la página **Step 2 of 2** (Paso 2 de 2), especifique un título de incidente (por ejemplo, `Request VSTS access`) y proporcione la información recopilada en el primer paso (arriba).  Lea y acepte el contrato y seleccione **Send** (Enviar).

Si el incidente se creó correctamente, se mostrará una página de confirmación.  Guarde la información de confirmación para consultarla en el futuro.  El servicio de soporte técnico de Microsoft debe responder a la solicitud de acceso en unos días hábiles.


## <a name="next-steps"></a>Pasos siguientes

Una vez publicada una aplicación de Azure, puede [actualizar la oferta existente](./cpp-update-existing-offer.md) para reflejar cambios empresariales o de los requisitos técnicos. 

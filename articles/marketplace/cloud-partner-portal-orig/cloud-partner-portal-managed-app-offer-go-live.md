---
title: Publicación de la oferta de aplicación de Azure en Azure Marketplace | Microsoft Docs
description: .
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
ms.date: 11/15/2018
ms.author: pbutlerm
ms.openlocfilehash: 18a8e6ae8ab3bd4299c6a014f938e73a2a021492
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263601"
---
<a name="make-your-azure-application-offer-live-on-azure-marketplace"></a>Publicación de la oferta de aplicación de Azure en Azure Marketplace 
===========================================================

Ahora que ha rellenado todos los detalles de la oferta, es el momento de publicarla y hacer que esté disponible en Azure Marketplace. La oferta pasa por diferentes pasos para garantizar que el contenido de marketing y la aplicación cumplen con los requisitos de calidad para tener la certificación Azure Certified y publicarse en el sitio web.

![Flujo de publicación](./media/cloud-partner-portal-publish-managed-app/publish_flow.png)

Analicemos este proceso más detalladamente para entender mejor lo que sucede durante cada paso y los elementos de acción del proceso para asegurarse de que su oferta continúe progresando.

<a name="publishing-process"></a>Proceso de publicación 
------------------

Debe hacer clic en \"Publicar\" en la pestaña **Editor** para iniciar el proceso de publicación.

![Secuencia 1 de publicación de la oferta: publicación](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_publish.png)

En la pestaña **Estado**, verá los pasos de publicación y en qué paso se encuentra la oferta.

![Secuencia 2 de publicación de la oferta: flujo de trabajo](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_workflow.png)

En cualquier punto del proceso de publicación, también puede iniciar sesión y hacer clic en la pestaña **Todas las ofertas** para ver el estado más reciente de cualquiera de las ofertas. Puede hacer clic directamente en el estado de su oferta y ver los detalles relativos al punto en que su oferta se encuentra en el proceso de publicación.

> [!WARNING]
> Si la atribución de uso del cliente no está habilitada, obtendrá el siguiente mensaje de error durante el proceso de publicación: "Azure partner customer usage attribution missing in one or more Azure Resource Manager templates. To resolve, add a tracking GUID to the Azure Resource Manager template in mainTemplate.json packages for the following plan: servicenow. For more details, see http://aka.ms/customerusageattribution" (Falta la atribución de uso del cliente para asociado de Azure en una o varias plantillas de Azure Resource Manager. Para resolver, agregue un GUID de seguimiento a la plantilla de Azure Resource Manager en paquetes mainTemplate.json para el siguiente plan: servicenow. Para obtener información, consulte http://aka.ms/customerusageattribution). 

Vamos a analizar cada uno de los pasos de publicación, y explicar lo que sucede en cada uno de ellos y examinar su duración estimada.

### <a name="validate-prerequisites-1-day"></a>Validar requisitos previos (\<1 día) 

Al hacer clic en \"Publicar\", se llevará a cabo una comprobación automatizada para comprobar que ha rellenado todos los campos obligatorios de su oferta. Si no se ha completado alguno de los campos, aparecerá una advertencia junto al campo en cuestión y tendrá que rellenarlo con la información pertinente. A continuación, haga clic de nuevo en \'Publicar\'.

Una vez que haya completado este paso correctamente, aparecerá un elemento emergente solicitando una dirección de correo electrónico. Este será el correo electrónico en el que recibirá las notificaciones del estado de publicación para el resto del proceso de publicación. Una vez que envíe la dirección de correo electrónico, este paso se habrá completado.

![Secuencia 1 de publicación de la oferta: publicación 1](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_publish1.png)

### <a name="certification-5-days"></a>Certificación (\<5 días) 

En este paso se ejecutan varias pruebas para comprobar que el paquete de aplicación de Azure cumple los requisitos de Azure Certified.

Dado que este paso puede tardar varios días, puede cerrar sesión en Cloud Partner Portal. Le enviaremos una notificación por correo electrónico si hay errores. Si todo se completa correctamente, el proceso pasará automáticamente al paso de aprovisionamiento.

### <a name="packaging-and-lead-generation-registration-1-hour"></a>Empaquetado y registro de la generación de clientes potenciales (\<1 hora) 

Durante esta fase, combinaremos el contenido técnico y de marketing en lo que será la página del producto en el sitio web.

### <a name="offer-available-in-preview"></a>Oferta disponible en versión preliminar 

Recibirá un correo electrónico de notificación informando de que su oferta ha completado correctamente los pasos necesarios para tener acceso a su vista previa. En este paso, debe obtener una vista previa de la oferta y asegurarse de que todo se ve correctamente. Compruebe si la VM se implementó correctamente en el entorno de ensayo.

Solamente las suscripciones de la lista blanca pueden realizar esta verificación.\*

### <a name="publisher-sign-out"></a>Cierre de sesión del anunciante 

Cuando haya comprobado que todo está correcto y funciona adecuadamente en la vista previa, está listo para comenzar a trabajar. Haga clic en Publicar en la pestaña Estado y darán comienzo los pasos para publicar su oferta en producción y en el sitio web. Normalmente, pasarán varias horas entre el momento en que hace clic en Publicar y la publicación efectiva de la oferta en el sitio web. Le enviaremos una notificación por correo electrónico una vez que su oferta esté oficialmente publicada en el sitio web.

![Secuencia 6 de publicación de la oferta: publicación](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_goLive.png)

### <a name="microsoft-review"></a>Revisión de Microsoft 

Cuando esté listo para continuar y haya hecho clic en \"Publicar\", se creará un sitio de trabajo en Visual Studio Online, seguido de una revisión de código de Microsoft del contenido del paquete. La revisión de código cubrirá las prácticas y los patrones recomendados para usar al crear plantillas, sugerencias y trucos para optimizar la creación de recursos. Si los comentarios provocan que el anunciante realice cambios en los archivos, el proceso de publicación deberá volver a iniciarse. La publicación actual se cancelará y tendrá que volver a realizarla una vez gestionados los comentarios.

### <a name="live"></a>En vivo

La oferta ya está publicada en Azure Marketplace y Azure Portal. Sus clientes podrán ver e implementar su aplicación administrada de Azure en sus suscripciones a Azure. Puede hacer clic en la pestaña Todas las ofertas y ver el estado de todas las ofertas que figuran en la columna derecha. Puede hacer clic en el estado para ver la situación del proceso de publicación con todo detalle para su oferta.

### <a name="error-handling"></a>Control de errores 

Si detecta un error, recibirá un correo electrónico de notificación que le informará de un error con instrucciones sobre los pasos siguientes. También puede ver errores en cualquier momento durante este proceso haciendo clic en la pestaña Estado. Verá en qué punto del proceso ocurrió el problema junto con un mensaje de error en que se describe qué debe resolverse.

Si se producen errores durante el proceso de publicación, es necesario corregirlos y, a continuación, hacer clic en \'Publicar\' para reiniciar el proceso. Debe empezar al principio de los pasos de publicación, en Validar los requisitos previos, para volver a publicar después de cualquier corrección de errores.

Si tiene problemas para resolver un error, debe abrir una solicitud de soporte técnico para obtener ayuda de nuestros ingenieros de soporte técnico.

### <a name="canceling-the-publishing-request"></a>Cancelación de la solicitud de publicación

Puede iniciar el proceso de publicación y que se presente la necesidad de cancelar su solicitud. Solo puede cancelar una solicitud de publicación una vez que esta alcance el paso de aprobación del publicador. Para cancelar, haga clic en \'Cancelar publicación\'. El estado de publicación volverá al paso 1 y, para volver a publicar, debe hacer clic en Publicar y seguir los pasos del estado.

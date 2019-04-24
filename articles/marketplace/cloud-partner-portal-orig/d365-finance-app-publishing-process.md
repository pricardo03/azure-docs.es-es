---
title: Pasos para la publicación de aplicaciones | Microsoft Docs
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
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 0fc82229e158ed35b97203b11d08841c683c45ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60398058"
---
<a name="app-publishing-steps"></a>Pasos para la publicación de aplicaciones
====================

Para iniciar el proceso de publicación, hará clic en "Publicar" en la pestaña Editor.

![Botón para publicar aplicación de CPP](./media/d365-financials/image014.jpg)


En la pestaña Estado, verá los pasos de publicación, que indican en qué parte del proceso de publicación se encuentra la oferta. En cualquier punto del proceso de publicación, también puede iniciar sesión y hacer clic en la pestaña Todas las ofertas para ver el estado más reciente de cualquiera de las ofertas. Puede hacer clic directamente en el estado de su oferta y ver los detalles relativos al punto en que su oferta se encuentra en el proceso de publicación.

Vamos a analizar cada uno de los pasos de publicación, explicar lo que sucede en cada uno de ellos y examinar su duración estimada.

![Diagrama del proceso de publicación](./media/d365-financials/image017.png)


**Validar requisitos previos**

Al hacer clic en "Publicar", se llevará a cabo una comprobación automatizada para comprobar que ha rellenado todos los campos obligatorios de su oferta. Si no se ha completado alguno de los campos, aparecerá una advertencia junto al campo en cuestión y tendrá que rellenarlo con la información pertinente. A continuación, haga clic de nuevo en "Publicar".

Una vez que haya completado este paso correctamente, aparecerá un elemento emergente solicitando una dirección de correo electrónico, que se usará para enviarle notificaciones sobre publicaciones. Una vez que envíe la dirección de correo electrónico, este paso se habrá completado.


**Validación de aplicaciones automatizada**

En este paso, nuestro servicio de certificación automatizada comprueba las extensiones de aplicación proporcionadas con una oferta que su contenido alinea con los metadatos de la oferta. Asegúrese siempre de que el nombre de la aplicación, la versión, el editor y el id. coincidan con los proporcionados en el manifiesto de extensión denominado `app.json`.


**Validación de la unidad de prueba**

Si ha decidido configurar una unidad de prueba, en esta fase se valida su configuración.


**Registro y validación de administración de clientes potenciales**

Durante esta fase, si ha configurado la característica de generación de clientes potenciales, validaremos que la integración de CRM funcione. Para hacerlo, enviaremos un cliente potencial de prueba al CRM. Verá que se rellena un registro con datos falsos en el CRM o la tabla de Azure una vez completado este paso. Toda la documentación para la generación de clientes potenciales se encuentra aquí.


**Empaquetado de AppSource**

Se comprueban los artefactos de los detalles del escaparate y se genera el paquete de la versión preliminar de AppSource.


**Cierre de sesión de Publisher**

Durante esta fase, se activará el botón **Transmitir**. Ahora también tendrá un vínculo para obtener una vista previa de la oferta (con su valor de hidekey). Cuando esté satisfecho con el aspecto de la vista previa, haga clic en el botón Transmitir.
Tenga en cuenta que esta solicitud no publica la aplicación en App Source, sino que desencadena el proceso de validación interno.


**Validación de aplicaciones de marketing y técnicas**

En este paso realizamos las validaciones técnicas y de marketing en paralelo. Consulte los documentos de referencia [Checklist for Submitting Your App](https://aka.ms/CheckBeforeYouSubmit) (Lista de comprobación para enviar la aplicación) y [Developing Apps for Dynamics 365 for Finance and Operations white paper](https://go.microsoft.com/fwlink/?linkid=841518) (Desarrollar aplicaciones para Dynamics 365 for Finance and Operations) para obtener información sobre los requisitos y las recomendaciones. Durante el proceso de validación:
-  Trabajaremos con usted en relación con cualquier pregunta o problema pendiente.  
- Le proporcionaremos una fecha de publicación de la aplicación y le enviaremos una notificación cuando se publique. 
- Le proporcionaremos comentarios iniciales sobre la validación técnica y de marketing en un periodo de entre 5 y 7 días laborables.

Normalmente, estos pasos pueden tardar más de una semana y no es necesario que tenga la sesión iniciada continuamente en Cloud Partner Portal.


**Publicar la aplicación con el servicio**

La oferta está pasando por un procesamiento final. La aplicación ha superado la validación de marketing y técnica, pero ahora debe pasar un procesamiento final para estar lista para AppSource.


**Publicación**

La oferta ya se ha publicado en AppSource y los clientes podrán verla e implementarla en sus suscripciones a Microsoft Dynamics 365 Business Central. Recibirá un correo electrónico nuestro que le notificará que la aplicación se ha publicado en AppSource. En cualquier momento, puede hacer clic en la pestaña Todas las ofertas y ver el estado de todas las ofertas que figuran en la columna derecha. Puede hacer clic en el estado para ver la situación del proceso de publicación con todo detalle para su oferta.


<a name="error-handling"></a>Control de errores
--------------

Durante el proceso de publicación, puede encontrar algún error. De ser así, recibirá un correo electrónico de notificación que le informa de que se produjo un error con instrucciones sobre los pasos siguientes. También puede ver errores en cualquier momento durante este proceso haciendo clic en la pestaña Estado. Verá en qué punto del proceso ocurrió el problema junto con un mensaje de error en que se describe qué debe resolverse.

Si se producen errores durante el proceso de publicación, debe corregirlos y, a continuación, hacer clic en **Publicar** para reiniciar el proceso. Debe empezar al principio de los pasos de publicación, en **Validar los requisitos previos**, para volver a publicar después de cualquier corrección de errores.

Si tiene problemas para resolver un error, debe abrir una solicitud de soporte técnico para obtener ayuda de nuestros ingenieros de soporte técnico.


<a name="canceling-the-publishing-request"></a>Cancelación de la solicitud de publicación
--------------------------------

Puede iniciar el proceso de publicación y que se presente la necesidad de cancelar su solicitud. Solo puede cancelar una solicitud de publicación una vez que esta alcance el paso de aprobación del publicador. Para cancelar, haga clic en **Cancelar publicación**. El estado de publicación volverá al paso 1 y, para volver a publicar, debe hacer clic en Publicar y seguir los pasos del estado.

![Botón Cancelar publicación](./media/d365-financials/image013.png)

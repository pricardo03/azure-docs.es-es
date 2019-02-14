---
title: 'Publicación de una oferta de aplicación de Power BI: Azure Marketplace | Microsoft Docs'
description: Publique una oferta de aplicación de Power BI en Microsoft AppSource Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
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
ms.date: 01/31/2019
ms.author: pbutlerm
ms.openlocfilehash: 2b3783060cf5502076ce3bc98cf07f005366a9e2
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665716"
---
# <a name="publish-power-bi-app-offer"></a>Publicación de una oferta de aplicación de Power BI

El último paso, después de haber definido la oferta en el portal y creado los recursos técnicos asociados, consiste en enviar la oferta para la publicación.  Para iniciar este proceso, haga clic en el botón **Publicar** en el menú vertical de la ventana **Nueva oferta**.  Para más información, consulte [Publicación de ofertas de Azure Marketplace y AppSource](../manage-offers/cpp-publish-offer.md).


## <a name="publishing-steps"></a>Pasos para la publicación

En el diagrama siguiente se describen los pasos principales del proceso de publicación para el "lanzamiento".

![Pasos del proceso de publicación para la aplicación de Power BI](./media/publishing-process-steps.png)

En la tabla siguiente se describen estos pasos y se proporciona una estimación del tiempo máximo para su finalización:

|   Paso de publicación            |   Hora     |   DESCRIPCIÓN                                                                  |
| --------------------         |------------| ----------------                                                               |
| Validar requisitos previos       | 15 minutos     | Se validan la información y la configuración de la oferta.                            |
| Certificación                | 1-7 días   | El equipo de certificación de Power BI analiza su oferta. Ejecutamos la aplicación de Power BI a través de una prueba de verificación manual al instalar la aplicación a través de la dirección URL de instalación proporcionada. Las validaciones principales se realizan como parte del proceso de certificación de la aplicación; consulte a continuación.         |
| Packaging                    | \< 1 hora  | Los recursos técnicos de la oferta se empaquetan para uso del cliente.                        |
| Registro de la generación de clientes potenciales | \< 1 hora  | Los sistemas de clientes potenciales se configuran e implementan.                                      |
| Aprobación del publicador            | \-         | Revisión final del publicador y confirmación antes del lanzamiento de la oferta. Ahora también tendrá un vínculo para obtener una vista previa de la oferta. Cuando esté satisfecho con el aspecto de la vista previa, haga clic en el botón **Publicar** en la pestaña **Estado**. Esta acción envía una solicitud al equipo de incorporación para que muestre su aplicación en AppSource.    |
| En vivo                         | \< 3 horas | La oferta ya se ha publicado ("en vivo") en AppSource, y los clientes podrán ver e implementar la aplicación en sus suscripciones a Power BI. También recibirá un correo electrónico de confirmación. En cualquier momento, puede hacer clic en la pestaña **Todas las ofertas** y ver el estado de todas las ofertas que figuran en la columna derecha. Puede hacer clic en la pestaña **Estado** para ver la situación del proceso de publicación con todo detalle para su oferta. |
|   |   |

Espere hasta ocho días a que el proceso se complete. Después de completar estos pasos de publicación, la oferta de aplicación de Power BI se mostrará en la sección de aplicaciones de Power BI de [AppSource](https://appsource.microsoft.com/marketplace/apps?product=power-bi%20).


### <a name="app-certification-process"></a>Proceso de certificación de aplicaciones

El equipo de incorporación de Microsoft usa el siguiente proceso para validar el envío de la oferta de Power BI:

1. Se revisan los vínculos de ayuda y los documentos legales.
2. Se valida la información de contacto de soporte técnico.
3. Se usa la dirección URL del instalador para verificar la correcta instalación. 
4. Se analiza la aplicación en busca de malware y otro contenido malintencionado. 
5. Se verifica que el contenido que se muestra coincida con la descripción de la aplicación.
6. Se comprueba que las operaciones relacionadas con la aplicación funcionan según lo previsto en Power BI: abrir informes y paneles con datos de ejemplo, conectarse a orígenes de datos personalizados, actualización, etc.

El equipo de certificación proporciona comentarios si encuentra algún problema.  Para más información sobre los requisitos de la aplicación de Power BI, consulte la [documentación de las aplicaciones de Power BI](https://go.microsoft.com/fwlink/?linkid=2028636).


## <a name="next-steps"></a>Pasos siguientes

Le recomendamos supervisar periódicamente la aplicación en [AppSource Marketplace](https://appsource.microsoft.com).  Además, debería usar periódicamente la característica [Seller Insights](../../cloud-partner-portal-orig/si-getting-started.md) de [Cloud Partner Portal](https://cloudpartner.azure.com/#insights) para ofrecer perspectivas sobre los clientes y el uso de Marketplace.  También puede realizar ciertas [actualizaciones en la oferta](./cpp-update-existing-offer.md).

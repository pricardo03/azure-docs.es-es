---
title: 'Publicación de una oferta de aplicación de Power BI: Azure Marketplace | Microsoft Docs'
description: Publicar una oferta de aplicación de Power BI en el marketplace de Microsoft AppSource.
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
ms.openlocfilehash: 0e82041dab1932ed7c129b598740b2441aa59315
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56822759"
---
# <a name="publish-a-power-bi-app-offer"></a>Publicar una oferta de aplicación de Power BI

Para enviar la oferta para la publicación es el último paso, después de que ha creado los recursos técnicos asociados y define una oferta en Cloud Partner Portal. Para iniciar este proceso, en el panel izquierdo de la **nueva oferta** ventana, seleccione **publicar**. Para más información, consulte [Publicación de ofertas de Azure Marketplace y AppSource](../manage-offers/cpp-publish-offer.md).


## <a name="publishing-steps"></a>Pasos para la publicación

Estos son los pasos principales del proceso de publicación:

![Ofrecen pasos del proceso de publicación para la aplicación de Power BI](./media/publishing-process-steps.png)

Esta tabla describe cada paso y proporciona su tiempo de finalización estimado:

|   Paso de publicación            |   Hora     |   DESCRIPCIÓN                                                                  |
| --------------------         |------------| ----------------                                                               |
| Validar requisitos previos       | 15 minutos     | Se validan la información y la configuración de la oferta.                            |
| Certificación                | 1-7 días   | El equipo de certificación de Power BI analiza su oferta. El equipo ejecuta la aplicación de Power BI a través de una prueba de comprobación manual mediante la instalación de la aplicación a través de la dirección URL de instalación proporcionado. Validaciones principales se realizan como parte del proceso de certificación de aplicaciones (descrito más adelante en este documento).         |
| Packaging                    | \< 1 hora  | Recursos técnicos de la oferta se empaquetan para uso del cliente.                        |
| Registro de la generación de oportunidades | \< 1 hora  | Los sistemas de clientes potenciales se configuran e implementan.                                      |
| Aprobación del publicador            | \-         | Completar una revisión final y la confirmación antes de la oferta esté activa. También ahora tendrá un vínculo para obtener una vista previa de la oferta. Una vez que esté satisfecho con el aspecto de la versión preliminar, seleccione **Go Live** en el **estado** ficha. Esto envía una solicitud para el equipo de incorporación para mostrar su aplicación en AppSource.    |
| En vivo                         | \< 3 horas | La oferta pública aparece ahora ("vivo") en AppSource, y los clientes pueden ver la aplicación e implementarlo en sus suscripciones de Power BI. También recibirá un correo electrónico de confirmación. En la columna derecha en la **todas las ofertas** ficha, puede ver el estado de todas las ofertas. En el **estado** ficha, puede ver el estado del flujo de publicación detallados para su oferta. |
|   |   |

Permitir hasta ocho días para completar el proceso. Después de completar estos pasos de publicación, la oferta de aplicación de Power BI aparecerán en el [AppSource](https://appsource.microsoft.com/marketplace/apps?product=power-bi%20) sección aplicaciones de Power BI.


### <a name="app-certification-process"></a>Proceso de certificación de aplicaciones

El equipo de incorporación de Microsoft usa este proceso para validar el envío de la oferta de aplicación de Power BI:

1. Revise los vínculos de ayuda y documentos legales.
2. Validar la información de contacto de soporte técnico.
3. Use la dirección URL del instalador para comprobar la correcta instalación.
4. Examinar la aplicación de malware y otro contenido malintencionado.
5. Compruebe que el contenido mostrado coincide con la descripción de la aplicación.
6. Compruebe que las operaciones relacionadas con la aplicación funcionen según lo previsto en Power BI. El equipo abre paneles e informes con datos de ejemplo, se conecta a orígenes de datos personalizados, actualiza los datos y así sucesivamente.

El equipo de certificación proporciona comentarios si encuentra algún problema.  Para obtener más información sobre los requisitos de aplicaciones de Power BI, consulte el [documentación de la aplicación de Power BI](https://go.microsoft.com/fwlink/?linkid=2028636).


## <a name="next-steps"></a>Pasos siguientes

Le recomendamos que supervise regularmente la aplicación en el [AppSource marketplace](https://appsource.microsoft.com).  También se debe utilizar el [Seller Insights](../../cloud-partner-portal-orig/si-getting-started.md) característica de la [Cloud Partner Portal](https://cloudpartner.azure.com/#insights) para obtener información acerca de los clientes de marketplace y el uso de la aplicación. Por último, puede [actualizar su oferta](./cpp-update-existing-offer.md).

---
title: Supervisión de Azure Data Share (versión preliminar)
description: Supervisión de Azure Data Share (versión preliminar)
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: fbc6847dd86b7687d477b2bae0deab1389dc8491
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827062"
---
# <a name="monitor-azure-data-share-preview"></a>Supervisión de Azure Data Share (versión preliminar) 

En este artículo se explica cómo puede supervisar los recursos compartidos de datos mediante Azure Data Share (versión preliminar). Como proveedor de datos, puede supervisar diversos aspectos de las relaciones del uso compartido de datos. Aspectos como saber si los consumidores de datos han aceptado la invitación al recurso compartido de datos, si han creado una suscripción al mismo o si han empezado a utilizar los datos, están todos disponibles para su supervisión. 

Como consumidor de datos, puede supervisar las instantáneas que se han desencadenado en la suscripción de Azure. 

## <a name="monitor-invitation-status"></a>Supervisión del estado de la invitación

Vea el estado de las invitaciones a los recursos compartidos de datos. Para ello vaya a Sent shares (Recursos compartidos enviados) -> Invitaciones. 

![Estado de la invitación](./media/invitation-status.png "Invitation status") 

La invitación puede tener uno de los tres estados siguientes:

* Pendiente: el destinatario del recurso compartido de datos no ha aceptado todavía la invitación.
* Aceptada: el destinatario del recurso compartido de datos ha aceptado la invitación.
* Rechazada: el destinatario del recurso compartido de datos ha rechazado la invitación.

> [!IMPORTANT]
> Eliminar una invitación una vez aceptada no equivale a revocar el acceso. Si desea impedir que se copien las instantáneas futuras en la cuenta de almacenamiento de los consumidores de datos, debe revocar el acceso mediante la pestaña *Share subscriptions* (Suscripciones a recursos compartidos). 

## <a name="monitor-share-subscriptions"></a>Supervisión de suscripciones a recursos compartidos

Vea el estado de las suscripciones a recursos compartidos de datos. Para ello vaya a Sent shares (Recursos compartidos enviados) -> Share Subscriptions (Suscripciones a recursos compartidos). Esto le proporcionará detalles sobre las suscripciones activas que han creado los consumidores de datos después de aceptar la invitación. Puede detener las actualizaciones futuras para los consumidores de datos seleccionando la suscripción a recurso compartido y seleccionando *Revocar*. 

## <a name="snapshot-history"></a>Historial de instantáneas 

En la pestaña de historial, es posible ver las instantáneas que se han copiado en el inquilino del consumidor de los datos. Es posible supervisar la frecuencia y duración de cada intervalo de instantánea. 

![Historial de instantáneas](./media/sent-shares.png "Snapshot history") 

Para ver más información sobre la ejecución de cada instantánea, haga clic en la fecha de inicio de la ejecución. 

De forma predeterminada, se muestra un máximo de 30 días de historial de instantáneas. Si tiene que ver más de 30 días de historial, vaya a Supervisión-> Configuración de diagnóstico y seleccione **Agregar configuración de diagnóstico**. Se le pedirá que seleccione una cuenta de almacenamiento en la que guardar estos registros. 

![Historial de instantáneas](./media/diagnostic-settings.png "Configuración de diagnóstico") 

## <a name="next-steps"></a>Pasos siguientes 

Más información sobre [terminología de Azure Data Share](terminology.md)
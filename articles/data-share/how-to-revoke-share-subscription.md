---
title: Revocación de una suscripción a un recurso compartido en la versión preliminar de Azure Data Share
description: Revocación de una suscripción a un recurso compartido
author: madams0013
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: jasonh
ms.openlocfilehash: dc14bb121d18179091aa5f6f2854b391e72632f1
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70258624"
---
# <a name="how-to-revoke-a-consumers-share-subscription-in-azure-data-share-preview"></a>Revocación de una suscripción a un recurso compartido de un consumidor en la versión preliminar de Azure Data Share

En este artículo se explica cómo revocar una suscripción a un recurso compartido de uno o varios de los consumidores mediante la versión preliminar de Azure Data Share. Esto evita que un consumidor desencadene más instantáneas. Si el consumidor todavía no ha desencadenado una instantánea, nunca recibirá los datos una vez que se haya revocado la suscripción a un recurso compartido. Si anteriormente ha desencadenado una instantánea, los datos más recientes que tenga permanecerán en su cuenta.

## <a name="navigate-to-a-sent-data-share"></a>Desplazamiento a un recurso compartido de datos enviado

En la versión preliminar de Azure Data Share, vaya a su recurso compartido enviado y seleccione la pestaña **Share Subscriptions** (Suscripciones a recursos compartidos).

![Revocación de una suscripción a un recurso compartido](./media/how-to/how-to-revoke-share-subscription/revoke-share-subscription.png) 

Active las casillas situadas junto a los destinatarios cuyas suscripciones a recursos compartidos desea eliminar y haga clic en **Revocar**. El consumidor ya no recibirá actualizaciones para sus datos.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre como [exportar los recursos compartidos de datos](how-to-monitor.md).
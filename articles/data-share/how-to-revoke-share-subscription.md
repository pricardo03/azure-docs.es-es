---
title: Revocación de una suscripción a un recurso compartido en la versión preliminar de Azure Data Share
description: Revocación de una suscripción a un recurso compartido
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 08a48202c26df1c24216572b1a52ac45506c6229
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71326532"
---
# <a name="how-to-revoke-a-consumers-share-subscription-in-azure-data-share-preview"></a>Revocación de una suscripción a un recurso compartido de un consumidor en la versión preliminar de Azure Data Share

En este artículo se explica cómo revocar una suscripción a un recurso compartido de uno o varios de los consumidores mediante la versión preliminar de Azure Data Share. Esto evita que un consumidor desencadene más instantáneas. Si el consumidor todavía no ha desencadenado una instantánea, nunca recibirá los datos una vez que se haya revocado la suscripción a un recurso compartido. Si anteriormente ha desencadenado una instantánea, los datos más recientes que tenga permanecerán en su cuenta.

## <a name="navigate-to-a-sent-data-share"></a>Desplazamiento a un recurso compartido de datos enviado

En la versión preliminar de Azure Data Share, vaya a su recurso compartido enviado y seleccione la pestaña **Share Subscriptions** (Suscripciones a recursos compartidos).

![Revocación de una suscripción a un recurso compartido](./media/how-to/how-to-revoke-share-subscription/revoke-share-subscription.png) 

Active las casillas situadas junto a los destinatarios cuyas suscripciones a recursos compartidos desea eliminar y haga clic en **Revocar**. El consumidor ya no recibirá actualizaciones para sus datos.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre como [exportar los recursos compartidos de datos](how-to-monitor.md).
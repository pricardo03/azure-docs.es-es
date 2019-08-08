---
title: Terminología de Azure Data Share, versión preliminar
description: Terminología de Azure Data Share, versión preliminar
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 01025acfa2fd939db8134a1e61c7902034a1d2c3
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68424219"
---
# <a name="azure-data-share-preview-concepts"></a>Conceptos de Azure Data Share (versión preliminar) 

La versión preliminar de Azure Data Share presenta nueva terminología relacionada con el uso compartido de datos. En este artículo se explican algunos términos frecuentemente utilizados que puede ver en este servicio. 

## <a name="data-provider"></a>Proveedor de datos

Un proveedor de datos es la organización que comparte datos con sus consumidores. Normalmente, el proveedor de datos puede ser un propietario o un administrador provisional de los mismos. Los proveedores de datos comparten datos de varios tipos. Entre los ejemplos de datos que un proveedor de datos puede compartir se incluyen los datos sin procesar como, por ejemplo, los datos de puntos de venta o de series temporales. Un proveedor de datos también puede compartir datos procesados previamente que ya contienen análisis y conclusiones. 

## <a name="data-consumer"></a>Consumidor de datos 

Un consumidor de datos es la organización que recibe datos de un proveedor. Puede que el consumidor desee unir los datos compartidos con los suyos propios para extraer conclusiones. En algunos casos, el consumidor puede recibir datos que ya se han procesado. 

## <a name="data-share"></a>Recurso compartido de datos

Un recurso compartido de datos es un grupo de conjuntos de datos que se comparten como una sola entidad. Los conjuntos de datos pueden proceder de varios orígenes de datos de Azure que son compatibles con Azure Data Share. Actualmente, Azure Data Share es compatible con Azure Blob Storage y Azure Data Lake Store. 

## <a name="share-subscription"></a>Suscripción a un recurso compartido 

Se crea una suscripción a un recurso compartido cuando un consumidor de datos acepta una invitación de recurso compartido de datos de un proveedor. Los proveedores de datos pueden ver las suscripciones activas a recursos compartidos de datos. Para ello, deben ir a **Sent Shares** (Recursos compartidos enviados) en la cuenta de Azure Data Share y seleccionar **Share Subscriptions** (Suscripciones a recursos compartidos).

Si un consumidor de datos quiere comprobar si tienen una suscripción activa a un recurso compartido, debe ir a **Received Shares** (Recursos compartidos recibidos) y ver el estado de dichos recursos. 

## <a name="snapshot"></a>Instantánea

Un consumidor de datos puede crear una instantánea cuando acepta una invitación a un recurso compartido de datos. Cuando se acepta una invitación se desencadena una instantánea completa de los datos compartidos. La instantánea es una copia de los datos en el momento en que el consumidor generó la instantánea. 

Hay dos tipos de instantáneas: completas e incrementales. Una instantánea completa contiene todos los datos del recurso compartido de datos. Una instantánea incremental contiene todos los datos que se han actualizado y agregado desde que se desencadenó la última instantánea. 

## <a name="snapshot-settings-in-azure-data-share"></a>Configuración de instantáneas en Azure Data Share
 
Un proveedor de datos puede habilitar un valor de instantánea para un recurso compartido de datos. Este valor permite a los consumidores de datos recibir las actualizaciones incrementales cuando se producen. Se debe habilitar esta opción si el proveedor de datos desea que los consumidores reciban actualizaciones de los datos que se han compartido. 

Si el proveedor de datos habilita este valor, se puede seleccionar un intervalo de periodicidad. El intervalo de periodicidad puede ser cada hora o diariamente. 

Un consumidor de datos tiene la opción de participar en esta programación de instantáneas para recibir las actualizaciones incrementales, que incluyen todos los datos que han cambiado desde que se generó por primera vez una nueva instantánea. 

## <a name="invitation"></a>Invitación

Un proveedor de datos puede invitar a varios destinatarios a sus recursos compartidos de datos. Para ello deben agregar los destinatarios al recurso compartido de datos. Las invitaciones también se pueden agregar una vez creado un recurso compartido de datos. 

Un proveedor de datos puede eliminar una invitación después de haberla enviado. Tenga en cuenta que si un proveedor de datos elimina una invitación después de que esta se haya aceptado, el consumidor de datos puede seguir teniendo una suscripción activa al recurso compartido. Si el proveedor de datos elimina una invitación y esta todavía no se había aceptado, el consumidor ya no podrá aceptarla. 

## <a name="recipient"></a>Recipient

Un destinatario es alguien que recibe una invitación a un recurso compartido de datos. Normalmente, el proveedor de datos agrega destinatarios al recurso compartido de datos que crea. Una vez que el destinatario acepta la invitación, se convierte en un consumidor de datos.  

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).


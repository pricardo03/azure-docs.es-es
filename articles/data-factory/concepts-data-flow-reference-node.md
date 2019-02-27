---
title: Nodo de referencia la asignación de Data Flow de Azure Data Factory
description: Data Flow de Data Factory agregará un nodo de referencia para combinaciones, búsquedas y uniones.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 2e318210d96822b13f65eadeef79798b1b4595d1
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325904"
---
# <a name="mapping-data-flow-reference-node"></a>Nodo de referencia de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Nodo de referencia](media/data-flow/referencenode.png "nodo de referencia")

Un nodo de referencia se agrega automáticamente al lienzo para indicar que el nodo conectado hace referencia a otro nodo existente en el lienzo. Un nodo de referencia podría ser un puntero o una referencia a otra transformación de flujo de datos.

Por ejemplo:  Cuando combina o une más de un flujo de datos, el lienzo de Data Flow puede agregar un nodo de referencia que refleje el nombre y la configuración del flujo entrante no principal.

Recuerde que el nodo de referencia no se puede mover o eliminar. Sin embargo, puede hacer clic en el nodo para modificar la configuración de transformación de origen.

Las reglas de interfaz de usuario que se aplican cuando Data Flow agrega el nodo de referencia se basan en el espacio disponible y en el espaciado vertical entre filas.

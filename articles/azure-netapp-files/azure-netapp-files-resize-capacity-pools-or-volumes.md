---
title: Cambiar el tamaño de un grupo de capacidad o de un volumen de Azure NetApp Files  | Microsoft Docs
description: Describe cómo cambiar el tamaño de un grupo de capacidad o de un volumen.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to-article
ms.date: 04/03/2018
ms.author: b-juche
ms.openlocfilehash: 72da85fb7296d02bc6d5f7fcd3279953c312c920
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39012013"
---
# <a name="resize-a-capacity-pool-or-a-volume"></a>Cambiar el tamaño de un grupo de capacidad o de un volumen
Puede cambiar el tamaño de un grupo de capacidad o de un volumen según sea necesario. 

## <a name="resize-the-capacity-pool"></a>Cambiar el tamaño del grupo de capacidad 

Puede cambiar el tamaño del grupo de capacidad en incrementos o decrementos de 4 TiB. Al cambiar el tamaño del grupo de capacidad, cambia la capacidad adquirida de Azure NetApp Files.

1. En la hoja de administración de cuenta de NetApp, haga clic en el grupo de capacidad cuyo tamaño desee cambiar. 
2. Haga clic con el botón derecho en el nombre del grupo de capacidad o haga clic en el icono "…" al final de la fila del grupo de capacidad para mostrar el menú contextual. 
3. Use las opciones del menú contextual para cambiar el tamaño del grupo de capacidad o eliminarlo.

## <a name="resize-a-volume"></a>Cambiar el tamaño de un volumen

Puede cambiar el tamaño de un volumen según sea necesario. El consumo de la capacidad de un volumen se descuenta de la capacidad aprovisionada de su grupo.

1. En la hoja de administración de cuenta de NetApp, haga clic en **Volúmenes**. 
2. Haga clic con el botón derecho en el nombre del volumen cuyo tamaño desee cambiar o haga clic en el icono "…" al final de la fila del volumen para mostrar el menú contextual.
3. Use las opciones del menú contextual para cambiar el tamaño del volumen o eliminarlo.


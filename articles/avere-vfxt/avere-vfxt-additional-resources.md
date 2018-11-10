---
title: Vínculos adicionales sobre Avere vFXT para Azure
description: Vínculos para obtener información adicional sobre Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: afba39d1af700650cfbf7226dff36729a76a0bda
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669792"
---
# <a name="additional-documentation"></a>Documentación adicional

En este artículo encontrará vínculos que le permitirán obtener documentación adicional sobre la interfaz de administración del panel de control de Avere y otros temas relacionados. 

## <a name="avere-cluster-documentation"></a>Documentación del clúster de Avere

Puede encontrar documentación adicional sobre el clúster de Avere en el sitio web de <http://library.averesystems.com/>. Gracias a estos documentos podrá comprender las capacidades del clúster y sabrá cómo configurar sus ajustes. 

* La [guía de creación de clústeres de FXT](<http://library.averesystems.com/#fxt_cluster>) está diseñada para crear clústeres compuestos por nodos de hardware físicos, pero también podrá encontrar cierta información que también es relevante para los clústeres de vFXT. En particular, los nuevos administradores de clústeres de vFXT pueden obtener información valiosa si leen estas secciones:
  * En [Customizing Support and Monitoring Settings](<http://library.averesystems.com/create_cluster/4_8/html/config_support.html#config-support>) (Personalización de la configuración de soporte y supervisión) se explica cómo personalizar la configuración de la carga de soporte técnico y habilitar la supervisión remota. 
  * En [Configuring VServers and Global Namespace](<http://library.averesystems.com/create_cluster/4_8/html/config_vserver.html#config-vserver>) (Configuración de VServers y el espacio de nombres global) encontrará información sobre cómo crear un espacio de nombres orientado al cliente.
  * En [Configuring DNS for the Avere cluster](<http://library.averesystems.com/create_cluster/4_8/html/config_network.html#dns-overview>) (Configuración DNS para el clúster de Avere) se explica cómo configurar el DNS de round robin.
  * En [Adding Backend Storage](<http://library.averesystems.com/create_cluster/4_8/html/config_core_filer.html#add-core-filer>) (Agregar el almacenamiento de back-end) se documenta el modo de agregar archivadores centrales.

* En [Cluster Configuration Guide](<http://library.averesystems.com/#operations>) (Guía de configuración del clúster) encontrará una referencia completa acerca de la configuración y las opciones de un clúster de Avere. Un clúster de vFXT usa un subconjunto de estas opciones, pero se puede aplicar la mayoría de páginas de esta configuración.

* En [Dashboard Guide](<http://library.averesystems.com/#operations>) (Guía del panel) se explica cómo usar las características de supervisión de clústeres del panel de control de Avere.

## <a name="vfxt-creation-and-management-documentation"></a>Documentación de creación y administración de vFXT

Se proporciona una guía completa para usar vfxt.py, para crear el clúster en la nube y para la utilidad de administración en <https://github.com/AvereSystems/vFXT.py/blob/master/docs/README.md>.  

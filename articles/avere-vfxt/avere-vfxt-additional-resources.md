---
title: Vínculos adicionales sobre Avere vFXT para Azure
description: Vínculos para obtener información adicional sobre Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: v-erkell
ms.openlocfilehash: 2efbe7ddc39b8bde76ee4a135f3f44af0864a374
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515594"
---
# <a name="additional-documentation"></a>Documentación adicional

En este artículo encontrará vínculos que le permitirán obtener documentación adicional sobre la interfaz de administración del panel de control de Avere y otros temas relacionados. 

## <a name="avere-cluster-documentation"></a>Documentación del clúster de Avere

Puede encontrar documentación adicional sobre el clúster de Avere en el sitio web de <https://azure.github.io/Avere/>. Gracias a estos documentos podrá comprender las capacidades del clúster y sabrá cómo configurar sus ajustes. 

* La [guía de creación de clústeres de FXT](<https://azure.github.io/Avere/#fxt_cluster>) está diseñada para crear clústeres compuestos por nodos de hardware físicos, pero también podrá encontrar cierta información que también es relevante para los clústeres de vFXT. En particular, los nuevos administradores de clústeres de vFXT pueden obtener información valiosa si leen estas secciones:
  * En [Customizing Support and Monitoring Settings](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_support.html#config-support>) (Personalización de la configuración de soporte y supervisión) se explica cómo personalizar la configuración de la carga de soporte técnico y habilitar la supervisión remota. 
  * En [Configuring VServers and Global Namespace](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_vserver.html#config-vserver>) (Configuración de VServers y el espacio de nombres global) encontrará información sobre cómo crear un espacio de nombres orientado al cliente.
  * En [Configuring DNS for the Avere cluster](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_network.html#dns-overview>) (Configuración DNS para el clúster de Avere) se explica cómo configurar el DNS de round robin.
  * En [Adding Backend Storage](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_core_filer.html#add-core-filer>) (Agregar el almacenamiento de back-end) se documenta el modo de agregar archivadores centrales.

* En [Cluster Configuration Guide](<https://azure.github.io/Avere/#operations>) (Guía de configuración del clúster) encontrará una referencia completa acerca de la configuración y las opciones de un clúster de Avere. Un clúster de vFXT usa un subconjunto de estas opciones, pero se puede aplicar la mayoría de páginas de esta configuración.

* En [Dashboard Guide](<https://azure.github.io/Avere/#operations>) (Guía del panel) se explica cómo usar las características de supervisión de clústeres del panel de control de Avere.

## <a name="vfxt-creation-and-management-documentation"></a>Documentación de creación y administración de vFXT

En GitHub se ofrece una guía completa del uso de vfxt.py, la creación de clústeres en la nube y la utilidad de administración: [Cloud Cluster Management with vfxt.py](https://github.com/Azure/AvereSDK/blob/master/docs/README.md) (Administración de clústeres en la nube con vfxt.py).  

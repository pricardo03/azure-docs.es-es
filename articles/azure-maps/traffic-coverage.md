---
title: Cobertura de tráfico en Azure Maps | Microsoft Docs
description: Información sobre la cobertura de tráfico en Azure Maps
author: kgremban
ms.author: kgremban
ms.date: 11/28/2017
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: a6fc1a8f050c3b1f546b4d0150f14b2cbebc31d0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599714"
---
# <a name="azure-maps-traffic-coverage"></a>Cobertura de tráfico en Azure Maps

Azure Maps proporciona información de tráfico enriquecida en forma de **flujo** e **incidentes** de tráfico. Estos datos se pueden visualizar en mapas o se pueden utilizar para generar rutas más inteligentes que se apoyan en las condiciones de conducción reales. 

Sin embargo, Maps no tiene el mismo nivel de información y precisión en todas las regiones. La tabla siguiente proporciona información acerca de qué tipo de información de tráfico puede solicitar en cada región: 

|Region  |Incidentes  |Flujo  |
|---------|:---------:|:---------:|
|Argentina      |         |✓         |
|Australia     |✓         |✓        |
|Austria     |✓         |✓         |
|Bahréin     |         |✓         |
|Bélgica     |✓         |✓         |
|Brasil     |✓         |✓         |
|Bulgaria     |✓         |✓         |
|Canadá     |✓         |✓         |
|Chile     |✓         |✓         |
|Colombia      |         |✓         |
|Croacia     |         |✓         |
|República Checa     |✓         |✓         |
|Dinamarca     |✓         |✓         |
|Egipto     |         |✓         |
|Estonia     |         | ✓        |
|Finlandia     |✓         |✓         |
|+Islas Åland      |✓         |✓         |
|Francia     |✓         |✓         |
|+Mónaco     |✓         |✓         |
|Alemania     |✓         |✓         |
|Grecia     |✓         |✓         |
|RAE de Hong Kong     |✓         |✓         |
|Hungría     |✓         |✓         |
|Islandia     |         |✓         |
|Indonesia     |✓         |✓         |
|Irlanda (República de)     |✓         |✓         |
|Israel     |         |✓         |
|Italia     |✓         |✓        |
|+San Marino     |✓         |✓         |
|Kuwait     |✓         |✓         |
|Letonia     |         |✓         |
|Lesoto     |✓         |✓         |
|Lituania     |         |✓         |
|Luxemburgo     |✓         |✓         |
|RAE de Macao     |         |✓         |
|Malasia     |✓         |✓         |
|Malta     |✓         |✓         |
|México     |✓         |✓         |
|Marruecos     |         |✓         |
|Países Bajos     |✓         |✓         |
|Nueva Zelanda     |✓         |✓         |
|Noruega     |✓         |✓         |
|Omán     |         |✓         |
|Polonia     |✓         |✓         |
|Portugal     |✓         |✓         |
|+Azores y Madeira     |         |✓         |
|Qatar     |         |✓         |
|Rumania     |         |✓         |
|Federación Rusa     |✓         |✓         |
|Arabia Saudí     |✓         |✓         |
|Singapur     |✓         |✓         |
|Eslovaquia     |✓         |✓         |
|Eslovenia     |✓         |✓         |
|Sudáfrica     |✓         |✓         |
|España     |✓         |✓         |
|+Andorra     |✓         |✓         |
|+Islas Baleares     |✓         |✓         |
|+Islas Canarias     |✓         |✓         |
|+Gibraltar     |✓         |✓         |
|Suecia     |✓         |✓         |
|Suiza     |✓         |✓        |
|+Liechtenstein      |✓         |✓         |
|Taiwán     |✓         |✓        |
|Tailandia     |✓         |✓        |
|Turquía     |✓         |✓         |
|Ucrania     |✓         |✓         |
|Emiratos Árabes Unidos     |✓         |✓         |
|Reino Unido     |✓         |✓         |
|(Guernsey y Jersey)     |✓         |✓         |
|Isla de Man     |✓         |✓         |
|Estados Unidos     |✓         |✓        |
|+Puerto Rico     |✓         |✓         |

Para más información acerca de los datos de tráfico de Azure Maps, consulte las páginas de referencia de [Tráfico](https://docs.microsoft.com/rest/api/maps/traffic).
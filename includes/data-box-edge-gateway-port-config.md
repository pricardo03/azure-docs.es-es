---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 11/06/2018
ms.author: alkohli
ms.openlocfilehash: 67de9042af11a2b17c4b65f8225ecd0580b95466
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263480"
---
| N.º de puerto| Dentro o fuera | Ámbito de puerto| Obligatorio|   Notas |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|Fuera|WAN |Sin |El puerto de salida se usa para obtener acceso a Internet para así recuperar las actualizaciones. <br>El usuario puede configurar el proxy web de salida. |
| TCP 443 (HTTPS)|Fuera|WAN|SÍ|El puerto de salida se usa para tener acceso a los datos en la nube.<br>El usuario puede configurar el proxy web de salida.|   
| UDP 53 (DNS)|Fuera|WAN|En algunos casos.<br>Vea las notas|Este puerto es necesario solo si está utilizando un servidor DNS basado en Internet.<br>Se recomienda usar un servidor DNS local. |
| UDP 123 (NTP)|Fuera|WAN|En algunos casos.<br>Vea las notas|Este puerto es necesario solo si está utilizando un servidor NTP basado en Internet.  |
| UDP 67 (DHCP)|Fuera|WAN|En algunos casos.<br>Vea las notas|Este puerto solo es necesario si se utiliza un servidor DHCP.  |
| TCP 80 (HTTP)|En|LAN|SÍ|Este es el puerto de entrada de la interfaz de usuario local del dispositivo para la administración local. <br>Si obtiene acceso a la interfaz de usuario local mediante HTTP, se le redireccionará automáticamente a HTTPS.  |
| TCP 443 (HTTPS)|En|LAN|SÍ|Este es el puerto de entrada de la interfaz de usuario local del dispositivo para la administración local. |
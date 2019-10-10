---
title: 'Recomendaciones de ancho de banda para sesiones remotas: Azure'
description: Recomendaciones de ancho de banda disponible para sesiones remotas.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: helohr
ms.openlocfilehash: 8352858b9fd6059257b5ba7637822f85c56eb070
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802624"
---
# <a name="bandwidth-recommendations-for-remote-sessions"></a>Recomendaciones de ancho de banda para sesiones remotas

Al usar una sesión remota de Windows, el ancho de banda disponible de la red afecta en gran medida a la calidad de la experiencia. Las distintas aplicaciones y resoluciones de pantalla requieren configuraciones de red diferentes, por lo que es importante asegurarse de que la red está configurada para satisfacer sus necesidades.

>[!NOTE]
>Las siguientes recomendaciones se aplican a las redes con una pérdida inferior al 0,1 %.

## <a name="applications"></a>APLICACIONES

En la tabla siguiente se enumeran los anchos de banda mínimos recomendados para que la experiencia de usuario sea fluida. 

|Carga de trabajo        |Aplicaciones de ejemplo                                                                                           |Ancho de banda recomendado|
|----------------|--------------------------------------------------------------------------------------------------------------|---------------------|
|Trabajador de producción     |Microsoft Word, Outlook, Excel y Adobe Reader                                                                  |1,5&nbsp;Mbps        |
|Trabajador de oficina   |Microsoft Word, Outlook, Excel, Adobe Reader, PowerPoint y Photo Viewer                                        |3&nbsp;Mbps          |
|Trabajador del conocimiento|Microsoft Word, Outlook, Excel, Adobe Reader, PowerPoint, Photo Viewer y Java                                  |5&nbsp;Mbps          |
|Trabajador avanzado    |Microsoft Word, Outlook, Excel, Adobe Reader, PowerPoint, Photo Viewer, Java, CAD/CAM, ilustración/publicación|15&nbsp;Mbps         |

>[!NOTE]
>Estas recomendaciones se aplican independientemente del número de usuarios que se encuentren en la sesión.

Tenga en cuenta que el esfuerzo que pone en la red depende tanto de la velocidad de fotogramas de salida de la carga de trabajo de la aplicación como de la resolución de pantalla. Si se aumenta la velocidad de fotogramas o la resolución de la pantalla, también aumentará el requisito de ancho de banda. Por ejemplo, una carga de trabajo ligera con una pantalla de alta resolución requiere más ancho de banda disponible que una carga de trabajo ligera con una resolución normal o baja.

Otros escenarios pueden cambiar sus requisitos de ancho de banda en función de cómo los use, por ejemplo:

- Conferencia de voz o videoconferencia
- Comunicación en tiempo real
- Vídeo 4K en streaming

Asegúrese de que realiza una prueba de carga de estos escenarios en la implementación mediante herramientas de simulación como Login VSI. Para conocer mejor los requisitos de la red, varíe el tamaño de carga, realice pruebas de esfuerzo y pruebe escenarios comunes de usuario en sesiones remotas. 

## <a name="display-resolutions"></a>Resoluciones de pantalla

Las distintas resoluciones de pantalla requieren diferentes anchos de banda disponibles. En la tabla siguiente se enumeran los anchos de banda que se recomiendan para una experiencia de usuario fluida a resoluciones de pantalla habituales con una velocidad de pantalla de 30 fotogramas por segundo (fps). Estas recomendaciones se aplican a escenarios con uno y varios usuarios. Tenga en cuenta que los escenarios que implican una velocidad de fotogramas inferior a 30 fps, como la lectura de texto estático, requieren menos ancho de banda disponible. 

|Resoluciones de pantalla habituales a 30 fps    |Ancho de banda recomendado|
|-----------------------------------------|---------------------|
|Aproximadamente 1024 × 768 px                      |1,5 Mbps             |
|Aproximadamente 1280 × 720 px                      |3 Mbps               |
|Aproximadamente 1920 × 1080 px                     |5 Mbps               |
|Aproximadamente 3840 × 2160 px (4K)                |15 Mbps              |

>[!NOTE]
>Estas recomendaciones se aplican independientemente del número de usuarios que se encuentren en la sesión.

## <a name="additional-resources"></a>Recursos adicionales

La región de Azure en la que se encuentra puede afectar a la experiencia del usuario tanto como las condiciones de la red. Para más información, consulte [Estimador de experiencia de Windows Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/assessment/).

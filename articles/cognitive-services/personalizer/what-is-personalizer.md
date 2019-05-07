---
title: ¿Qué es Personalizer?
titleSuffix: Azure Cognitive Services
description: Azure Personalizer es un servicio de API basado en la nube que permite elegir la mejor experiencia, mostrársela a los usuarios y aprender de su comportamiento en tiempo real.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: c969029bcc0412267507efe81549ec6f8b2988ce
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025481"
---
# <a name="what-is-personalizer"></a>¿Qué es Personalizer?

Azure Personalizer es un servicio de API basado en la nube que permite elegir la mejor experiencia, mostrársela a los usuarios y aprender de su comportamiento en tiempo real.

* Proporcione información acerca de los usuarios y del contenido, y reciba la principal acción que va a mostrar a los usuarios. 
* Para utilizar Personalizer no es preciso limpiar y etiquetar los datos.
* Proporcione los comentarios a Personalizer cuando le resulte más cómodo. 
* Vea análisis en tiempo real. 
* Utilice Personalizer como parte de un mayor esfuerzo de ciencia de datos para validar los experimentos existentes.

## <a name="how-does-personalizer-work"></a>Funcionamiento de Personalizer

Personalizer utiliza modelos de aprendizaje automático para detectar qué acción se debe colocar la primera en un contexto. La aplicación cliente proporciona una lista de posibles acciones, con información sobre ellas, así como información acerca del contexto, que puede incluir información sobre el usuario, dispositivo, etc. Personalizer determina la acción que se va a realizar. Una vez que la aplicación cliente use la acción elegida, proporciona comentarios a Personalizer en forma de puntuación de recompensa. Cuando se haya completado el bucle de comentarios, Personalizer actualiza automáticamente el modelo propio que ha utilizado para usarlo en el futuro.

## <a name="how-do-i-use-the-personalizer"></a>¿Cómo se usa Personalizer?

![Uso de Personalizer para elegir el vídeo que se va a mostrar a un usuario](media/what-is-personalizer/personalizer-example-highlevel.png)

1. Elija en su aplicación lo que desea personalizar.
1. Creación y configuración del servicio Personalization en Azure Portal
1. Use el SDK para llamar a Personalizer con información (_características_) acerca de los usuarios y el contenido (_acciones_). Para usar Personalizer no es preciso proporcionar datos limpios y con etiqueta. 
1. En la aplicación cliente, muestre al usuario la acción que ha seleccionado Personalizer.
1. Use el SDK para proporcionar comentarios a Personalizer que indiquen si el usuario seleccionó la acción de Personalizer. Se trata de una _puntuación de recompensa_ y normalmente oscila entre -1 y 1.
1. Vea el análisis en Azure Portal y evalúe el funcionamiento del sistema y cómo ayudan los datos a la personalización.

## <a name="where-can-i-use-personalizer"></a>¿Dónde se puede utilizar Personalizer?

Por ejemplo, una aplicación cliente puede agregar Personalizer para:

* Personalizar qué artículo se resalta en un sitio web de noticias.    
* Optimizar la colocación de anuncios en un sitio web.
* Mostrar un "elemento recomendado" personalizado en un sitio web de compras.
* Sugerir elementos de la interfaz de usuario, como filtros, para aplicarlos a una foto concreta.
* Elegir la respuesta de un bot de chat para clarificar la intención del usuario o sugerir una acción.
* Dar prioridad a las sugerencias relativas a lo debe hacer un usuario en el siguiente paso de un proceso empresarial.

## <a name="personalization-for-developers"></a>Personalization para desarrolladores

El servicio Personalizer tiene dos API:

* Envíe información (_características_) acerca de los usuarios y el contenido (_acciones_) para realizar la personalización. Personalizer responde con la acción principal.
* Envíe comentarios a Personalizer acerca del funcionamiento de la clasificación como número, normalmente entre 0 y 1 (en la sección anterior era entre -1 y 1). 

![Secuencia básica de eventos para Personalization](media/what-is-personalizer/personalization-intro.png)

## <a name="next-steps"></a>Pasos siguientes

[Inicio rápido: Creación un bucle de comentarios en C#](csharp-quickstart-commandline-feedback-loop.md)

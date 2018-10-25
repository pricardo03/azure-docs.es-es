---
title: ¿Qué es Translator Text API?
titlesuffix: Azure Cognitive Services
description: Integre Translator Text API en aplicaciones, sitios web, herramientas u otras soluciones para proporcionar experiencias de usuario en varios idiomas.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: overview
ms.date: 05/10/2018
ms.author: erhopf
ms.openlocfilehash: 6c89ff41531b130843eb288b98ffe7def1d8915e
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645077"
---
# <a name="what-is-translator-text-api"></a>¿Qué es Translator Text API?

Translator Text API puede integrarse perfectamente en aplicaciones, sitios web, herramientas u otras soluciones para proporcionar experiencias de usuario en [más de 60 idiomas](languages.md). Se puede usar en cualquier plataforma de hardware y con cualquier sistema operativo para realizar la traducción de un idioma de texto a texto.

Translator Text API forma parte de la colección de [API de Azure Cognitive Services](https://docs.microsoft.com/azure/#pivot=products&panel=ai), una colección de algoritmos de aprendizaje automático y de inteligencia artificial en la nube, que se pueden consumir fácilmente en los proyectos de desarrollo.

## <a name="about-microsoft-translator"></a>Acerca de Microsoft Translator

Microsoft Translator es un servicio de traducción automática basado en la nube. En el centro de este servicio están Translator Text API y [Translator Speech API](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-translation), que dan servicio a diversos productos y servicios de Microsoft y que utilizan miles de empresas en todo el mundo en sus aplicaciones y flujos de trabajo para que su contenido llegue a una audiencia mundial.

La traducción de voz también está disponible mediante la [versión preliminar del servicio Voz de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/speech-service/), que combina los componentes Translator Speech API, Bing Speech API y Custom Speech Service (versión preliminar), ya existentes, en un servicio unificado y totalmente personalizable.  

Obtenga más información sobre el [servicio Microsoft Translator](https://www.microsoft.com/en-us/translator/home.aspx)

## <a name="language-customization"></a>Personalización de lenguaje

Custom Translator, una extensión del servicio Microsoft Translator, se puede usar en combinación con Translator Text API para ayudar a personalizar el sistema de traducción neuronal y mejorar la traducción para su terminología y estilo específicos.

Con Custom Translator puede crear sistemas de traducción que administran la terminología usada en su propio negocio o sector. Luego, los sistemas de traducción personalizados se pueden integrar fácilmente en las aplicaciones, flujos de trabajo y sitios web ya existentes, en varios tipos de dispositivos, mediante el componente normal Microsoft Translator Text API, por medio del parámetro de categoría.

Más información sobre la [personalización de idioma](customization.md)

## <a name="microsoft-translator-neural-machine-translation"></a>Traducción automática neuronal de Microsoft Translator

La traducción automática neuronal (NMT) es el nuevo estándar para las traducciones automáticas de alta calidad basadas en inteligencia artificial. Este estándar reemplaza la traducción automática estadística (SMT) que alcanzó un nivel estable a mediados de 2010.

NMT proporciona mejores traducciones que SMT, no solo a la hora de puntuar la calidad de la traducción bruta, sino también porque suenan más fluidas y humanas. El motivo principal de esta fluidez es que NMT usa el contexto completo de una frase para traducir las palabras. SMT solo tomaba el contexto inmediato de unas cuantas palabras antes y después de cada palabra.

Los modelos NMT se sitúan en el centro de la API y no son visibles para los usuarios finales. La única diferencia perceptible es una mejor calidad de la traducción, en especial en los idiomas chino, japonés y árabe.

Más información sobre [el funcionamiento de NMT](https://www.microsoft.com/en-us/translator/mt.aspx#nnt)

## <a name="next-steps"></a>Pasos siguientes

- Lea acerca de los [detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/).

- [Regístrese](translator-text-how-to-signup.md) para obtener una clave de acceso.

- [Inicio rápido](quickstarts/csharp.md) es un tutorial escrito en C# de las llamadas a API REST. Aprenda a traducir texto de un idioma a otro con un código mínimo.

- [Documentación de referencia de API](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference) proporciona la documentación técnica de las API.

## <a name="see-also"></a>Otras referencias

- [Página de documentación de Cognitive Services](https://docs.microsoft.com/azure/#pivot=products&panel=ai)
- [Página de producto de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)
- [Información de precios y soluciones](https://www.microsoft.com/en-us/translator/default.aspx)

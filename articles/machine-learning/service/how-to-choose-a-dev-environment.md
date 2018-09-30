---
title: Entornos de desarrollo para Azure Machine Learning | Microsoft Docs
description: Información general de los entornos de desarrollo que puede usar con el servicio Azure Machine Learning. Python 3 es el único requisito para el entorno de desarrollo, pero también recomendamos utilizar entornos Conda. Para el desarrollo de herramientas, recomendamos los cuadernos Jupyter Notebook, Azure Notebooks y los editores de código o entornos de desarrollo integrado.
services: machine-learning
author: rastala
ms.author: roastala
manager: cgronlun
ms.service: machine-learning
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 9/24/2018
ms.openlocfilehash: 260e209bcf00396ee545851684038578c4fd148a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971049"
---
# <a name="development-environment-for-azure-machine-learning"></a>Entorno de desarrollo para Azure Machine Learning 

Aprenda más sobre los entornos de desarrollo que puede usar con el servicio Azure Machine Learning. 

El servicio Azure Machine Learning es de independiente de plataforma y no requiere un entorno de desarrollo específico. Requiere __Python 3__ y es recomendable utilizar __Conda__ para crear entornos aislados. __Si ya tiene un entorno de desarrollo que cumple con esos requisitos__, puede omitir este documento e ir al documento [Configuración de un entorno de desarrollo](how-to-configure-environment.md).

El resto de este documento aborda los entornos de desarrollo que hemos recomendado:

* __Jupyter Notebooks__
* __Azure Notebooks__
* __Entornos de desarrollo integrado (IDE) y editores de código__
* __Data Science Virtual Machine__

La comparación entre estos entornos es difícil, ya que tanto los cuadernos como los entornos de desarrollo integrado pueden ampliarse. Por ejemplo, algunos entornos de desarrollo integrado pueden usarse como clientes en cuadernos de Jupyter Notebook. Por lo general, los __cuadernos__ están diseñados para una __experimentación interactiva__ y para la __visualización__. Los __entornos de desarrollo integrado y los editores de código__ ofrecen herramientas para __mejorar la calidad del código__ e __integrarse con sistemas externos__ como control de versiones.

> [!TIP]
> Usar un entorno no impide el uso del otro. Los cuadernos y los entornos de desarrollo integrado son solo herramientas y se pueden combinar cuando se necesite. Por ejemplo, puede empezar a experimentar en un cuaderno y luego exportar a un script Python para editar y depurarlo en un entorno de desarrollo integrado.
>
> Esta es la razón por la que Data Science Virtual Machine ofrece tanto cuadernos de Jupyter Notebook como varios entornos de desarrollo integrado de Python populares.

## <a name="jupyter-notebooks"></a>Jupyter Notebooks

Los cuadernos de Jupyter Notebook forman parte del [proyecto Jupyter](https://jupyter.org/). Se centran en proporcionar una experiencia de codificación interactiva en la que se crean documentos que mezclan código activo con texto narrativo y gráficos. Puede instalar cuadernos de Jupyter Notebook en una variedad de plataformas.

Tener su propia instalación de Jupyter Notebook le permite instalar y configurar el entorno según sea necesario. Sin embargo, es responsable de mantener el sistema.

## <a name="azure-notebooks"></a>Azure Notebooks

[Azure Notebooks](https://notebooks.azure.com) (versión preliminar) es un entorno de cuadernos en la nube de Azure. Está basado en Jupyter, y proporciona un entorno preinstalado con bibliotecas populares. El SDK de Azure Machine Learning ya está instalado en Azure Notebooks, para que pueda empezar a experimentar prácticamente sin ninguna configuración.

Azure Notebooks también simplifica el proceso de compartir los cuadernos. Puede compartir una dirección URL a los cuadernos, hacer que la biblioteca sea pública o compartirla en redes sociales desde la interfaz de Azure Notebooks.

El inconveniente de Azure Notebooks es que el usuario no tiene control completo sobre el entorno y es posible que no pueda instalar el software personalizado que necesita. También es un entorno compartido, por lo que los cuadernos pueden funcionar más lentamente que en una instalación dedicada de Jupyter Notebook.

## <a name="ides-and-code-editors"></a>Entornos de desarrollo integrado y editores de código

Hay muchos entornos de desarrollo integrado y editores de código que funcionan con Azure Machine Learning. El único requisito de software es el SDK de Azure Machine Learning, que puede instalarse mediante la utilidad `pip`.

Se recomienda [Visual Studio Code](https://code.visualstudio.com/), ya que proporciona herramientas para trabajar tanto con el lenguaje Python como con Azure Machine Learning. Está disponible para plataformas Linux, macOS y Windows.

## <a name="data-science-virtual-machine"></a>Máquina virtual de ciencia de datos

Data Science Virtual Machine (DSVM) es una combinación de los entornos anteriores. Es una máquina virtual en la plataforma Azure que tiene cuadernos de Jupyter Notebook, Visual Studio Code y el SDK de Azure Machine Learning preinstalado. Crear la máquina virtual es más complejo que Azure Notebooks, pero menos compleja que la configuración de una máquina desde el principio. Dado que el software necesario está preinstalado en la imagen de la máquina virtual, puede empezar a experimentar con Azure Machine Learning rápidamente una vez que se haya creado la máquina virtual.

DSVM le permite seleccionar los recursos de proceso que necesita, como CPU, GPU y memoria. También está instala previamente con otros editores como PyCharm, así como con software de aprendizaje automático popular como TensorFlow, Keras y PyTorch. Si no está instalado el software que necesita, puede instalarlo.

Para más información sobre lo que está preinstalado, consulte el documento [¿Qué es Data Science Virtual Machine?](../data-science-virtual-machine/overview.md)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido sobre los entornos de desarrollo, aprenda [cómo configurar un entorno de desarrollo](how-to-configure-environment.md).


---
title: Tipos de clase de ejemplo en Azure Lab Services | Microsoft Docs
description: Proporciona algunos tipos de clases para los que puede configurar laboratorios mediante Azure Lab Services.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: 0e8b203240b4b2d6b67534ab52a7cd4ccf5df571
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2019
ms.locfileid: "71976597"
---
# <a name="class-types-overview---azure-lab-services"></a>Información general sobre tipos de clase: Azure Lab Services
Azure Lab Services le permite configurar rápidamente un entorno de laboratorio educativo en la nube. En los artículos de esta sección se proporcionan instrucciones sobre cómo configurar varios tipos de laboratorios de clase mediante Azure Lab Services.

## <a name="deep-learning-in-natural-language-processing"></a>Aprendizaje profundo en el procesamiento del lenguaje natural
Puede configurar un laboratorio centrado en el aprendizaje profundo en el procesamiento de lenguaje natural (NLP) mediante Azure Lab Services. El procesamiento de lenguaje natural (NLP) es una forma de inteligencia artificial (AI) que permite a los equipos utilizar la traducción, el reconocimiento de voz y otras capacidades de comprensión de lenguajes. Los alumnos que sigan una clase de NLP obtienen una máquina virtual Linux para aprender a aplicar algoritmos de red neuronal para desarrollar modelos de aprendizaje profundo que se utilizan para analizar el lenguaje humano escrito. 

Para más información sobre cómo configurar este tipo de laboratorio, consulte [Configuración de un laboratorio centrado en el aprendizaje profundo del procesamiento de lenguaje natural con Azure Lab Services](class-type-deep-learning-natural-processing.md).

## <a name="shell-scripting-on-linux"></a>Generación de scripts en shell en Linux
Puede configurar un laboratorio para enseñar el scripting de shell en Linux. El scripting es una parte útil de la administración del sistema que permite a los administradores evitar tareas repetitivas. En este escenario de ejemplo, la clase cubre scripts bash tradicionales y scripts mejorados. Los scripts mejorados son scripts que combinan comandos bash y Ruby. Este enfoque permite a Ruby pasar datos y comandos bash para interactuar con el shell. 

Los alumnos que sigan estas clases de scripting obtienen una máquina virtual Linux para conocer los aspectos básicos de Linux y también familiarizarse con el scripting de shell de bash. La máquina virtual Linux incluye el acceso al escritorio remoto habilitado y con los editores de texto [gedit](https://help.gnome.org/users/gedit/stable/) y [Visual Studio Code](https://code.visualstudio.com/) instalados.

Para más información sobre cómo configurar este tipo de laboratorio, consulte [Scripting de shell en Linux](class-type-shell-scripting-linux.md).

## <a name="ethical-hacking"></a>Piratería ética 
Puede configurar un laboratorio para una clase que se centre en la parte forense de la piratería ética. Las pruebas de penetración son una práctica que usa la comunidad de piratería ética y que se producen cuando alguien intenta obtener acceso al sistema o a la red para demostrar las vulnerabilidades que un atacante malintencionado puede explotar. 

En una clase de piratería ética, los alumnos pueden aprender técnicas modernas para defender los puntos vulnerables. Cada alumno obtiene una máquina virtual host de Windows Server que tiene dos máquinas virtuales anidadas: una máquina virtual con una imagen de **Metaspoiltable** y otra con una imagen de [Kali Linux](https://www.kali.org/). La máquina virtual Metasploitable se usa para la explotación, mientras que la máquina virtual Kali proporciona acceso a las herramientas necesarias para ejecutar tareas forenses.

Para más información sobre cómo configurar este tipo de laboratorio, consulte [Configuración de un laboratorio para impartir una clase acerca de la piratería ética](class-type-ethical-hacking.md).

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes: 

- [Configuración de un laboratorio centrado en el aprendizaje profundo en el procesamiento de lenguaje natural mediante Azure Lab Services](class-type-deep-learning-natural-processing.md)
- [Generación de scripts de shell en Linux](class-type-shell-scripting-linux.md)
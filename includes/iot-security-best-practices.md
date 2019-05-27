---
title: archivo de inclusión
description: archivo de inclusión
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 08/07/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 2138eed9975abe804442c476d19b5b7229685362
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146633"
---
# <a name="security-best-practices-for-internet-of-things-iot"></a>Procedimientos recomendados de seguridad para Internet de las cosas (IoT)

La protección de una infraestructura de Internet de las cosas precisa de una estrategia de seguridad rigurosa y detallada. La eficacia de esta estrategia se basa en la protección de los datos en la nube, la protección de la integridad de los datos en su tránsito por la red pública de Internet y el aprovisionamiento de dispositivos de forma segura. Cada capa genera mayor garantía de seguridad en toda la infraestructura.

## <a name="secure-an-iot-infrastructure"></a>Protección de una infraestructura de IoT

Esta estrategia de seguridad detallada puede desarrollarse y ejecutarse con la participación activa de diversas personas implicadas en la fabricación, el desarrollo y la implementación de infraestructura y dispositivos IoT. A continuación verá una descripción de alto nivel de estos participantes.

* **Integrador/fabricante de hardware de IoT**: Normalmente, estos actores son los fabricantes de hardware de IoT implementado, integradores que ensamblan hardware de diversos fabricantes o proveedores que proporcionan hardware para una implementación de IoT que fabrican o integran otros proveedores.

* **Desarrollador de soluciones de IoT**: El desarrollo de una solución de IoT se suele realizar mediante un programador de soluciones. Este desarrollador puede formar parte de un equipo interno o ejercer como integrador de sistemas especializado en esta actividad. El desarrollador de soluciones de IoT puede desarrollar diversos componentes de la solución de IoT desde cero, integrar varios componentes de serie o de código abierto o adoptar aceleradores de soluciones mediante adaptaciones menores.

* **Implementador de soluciones de IoT**: Una vez que se desarrolla una solución de IoT, debe implementarse en el campo. Este proceso implica la implementación de hardware, la interconexión de dispositivos y la implementación de soluciones en dispositivos de hardware o en la nube.

* **Operador de soluciones de IoT**: Después de implementa la solución de IoT, requiere operaciones a largo plazo, supervisión, actualizaciones y mantenimiento. De estas tareas puede encargarse un equipo interno formado por especialistas en tecnología de la información, equipos de operaciones de hardware y mantenimiento y especialistas en dominios que supervisan que la infraestructura IoT funciona de manera correcta globalmente.

En las secciones que siguen se proporcionan los procedimientos recomendadas para cada uno de los implicados a fin de desarrollar, implementar y poner en funcionamiento una infraestructura de IoT segura.

## <a name="iot-hardware-manufacturerintegrator"></a>Integrador/fabricante de hardware IoT

Estos son los procedimientos recomendados para los fabricantes de hardware de IoT e integradores de hardware.

* **Definir el ámbito de hardware a unos requisitos mínimos**: El diseño de hardware debe incluir las características mínimas necesarias para el funcionamiento del hardware y nada más. Un ejemplo es la inclusión de puertos USB únicamente si es necesario para el funcionamiento del dispositivo. Estas características adicionales abren el dispositivo para vectores de ataques no deseados, los cuales deben evitarse.

* **Asegúrese de hardware a prueba de manipulaciones**: Mecanismos integrados para detectar la alteración física, como la apertura de la cubierta del dispositivo o quitar una parte del dispositivo. Estas señales de alteración pueden formar parte de la transmisión de datos que se carga en la nube y que podría alertar a los operadores sobre estos eventos.

* **Basarse en hardware seguro**: Si COGS lo permite, cree características de seguridad, como el almacenamiento seguro y cifrado, o las funcionalidades de arranque basadas en el módulo de plataforma segura (TPM). Estas características crean dispositivos más seguros y ayudan a proteger la infraestructura de IoT general.

* **Proteger las actualizaciones**: Actualizaciones de firmware durante la vigencia del dispositivo son inevitables. La creación de dispositivos con rutas de acceso seguras para las actualizaciones y la garantía criptográfica de la versión del firmware permitirá la protección del dispositivo durante y después de las actualizaciones.

## <a name="iot-solution-developer"></a>Desarrollador de soluciones de IoT

Estos son los procedimientos recomendados para los desarrolladores de soluciones de IoT:

* **Siga la metodología de desarrollo de software seguro**: Desarrollo de software seguro requiere suelo pensar bien la seguridad, desde la concepción del proyecto hasta su implementación, prueba e implementación. Esta metodología influye en la elección de plataformas, idiomas y herramientas. El Ciclo de vida de desarrollo de seguridad de Microsoft proporciona un enfoque paso a paso para la creación de software seguro.

* **Elegir software de código abierto con cuidado**: Software de código abierto proporciona la oportunidad de desarrollar soluciones con rapidez. Al elegir software de código abierto, tenga en cuenta el nivel de actividad de la comunidad para cada componente de código abierto. Una comunidad activa garantiza la compatibilidad con el software; se detectarán problemas, a los que se hará frente. La otra opción es que no se admita un proyecto de software de código abierto complejo e inactivo, y así es probable es que no se detecten problemas.

* **Integrar con cuidado**: Existen muchos defectos de seguridad de software en el límite de bibliotecas y API. Es posible que la funcionalidad que puede no requerirse para la implementación actual siga estando disponible a través de un nivel de API. Para garantizar la seguridad general, asegúrese de comprobar que no existen errores de seguridad en todas las interfaces de los componentes que se están integrando.

## <a name="iot-solution-deployer"></a>Implementador de soluciones de IoT

A continuación se muestran los procedimientos recomendados para los implementadores de soluciones de IoT:

* **Implementar hardware de forma segura**: las implementaciones de IoT pueden requerir la implementación de hardware en ubicaciones no seguras; por ejemplo, en espacios públicos o configuraciones regionales no supervisadas. En este tipo de situaciones, asegúrese de que la implementación de hardware sea lo más resistente posible a las manipulaciones. Si hay puertos USB o de otro tipo disponibles en el hardware, asegúrese de que estos se tratan de forma segura. Muchos vectores de ataques pueden usarlos como punto de entrada.

* **Mantener seguras las claves de autenticación**: Durante la implementación, cada dispositivo requiere identificadores de dispositivo y claves de autenticación asociadas generadas por el servicio en la nube. Mantenga seguras estas claves físicamente incluso después de la implementación. Un dispositivo malintencionado puede usar cualquier clave en peligro para su enmascaramiento como dispositivo existente.

## <a name="iot-solution-operator"></a>Operador de soluciones de IoT

Estos son los procedimientos recomendados para los operadores de soluciones de IoT:

* **Mantener actualizado el sistema**: Asegúrese de que todos los controladores de dispositivos y sistemas operativos de dispositivos están actualizados con las versiones más recientes. Si activa las actualizaciones automáticas en Windows 10 (IoT u otras SKU), Microsoft mantiene el equipo actualizado, ofreciendo a los dispositivos de IoT un sistema operativo seguro. En el caso de otros sistemas operativos (como Linux), el hecho de mantenerlos actualizados garantiza también su protección contra ataques malintencionados.

* **Proteger contra la actividad malintencionada**: Si el sistema operativo lo permite, instale las últimas funcionalidades antivirus y antimalware en cada sistema operativo del dispositivo. Esta práctica puede ayudar a mitigar las amenazas más externas. Puede proteger los sistemas operativos más modernos contra amenazas realizando los pasos adecuados.

* **Auditar con frecuencia**: Auditar infraestructura IoT para problemas relacionados con la seguridad es clave al responder a incidentes de seguridad. La mayoría de los sistemas operativos proporciona un registro de eventos integrado que se debe revisar con frecuencia para asegurarse de que no se haya producido ninguna infracción de seguridad. La información de la auditoría se puede enviar como transmisión independiente de telemetría al servicio en la nube, donde puede analizarse.

* **Proteger físicamente la infraestructura IoT**: Los peores ataques de seguridad contra la infraestructura IoT se inician mediante el acceso físico a los dispositivos. La protección contra el uso malintencionado de puertos USB y otros accesos físicos es una importante práctica de seguridad. Una clave para descubrir las infracciones que pudieran haberse producido es registrar el acceso físico, como el uso del puerto USB. Una vez más, Windows 10 (IoT y el resto de SKU) habilita el registro detallado de estos eventos.

* **Proteger las credenciales en la nube**: Credenciales de autenticación en la nube usadas para configurar y manejar una implementación de IoT posiblemente son la manera más fácil de obtener acceso y poner en peligro un sistema IoT. Proteja las credenciales cambiando la contraseña con frecuencia y no usándolas en equipos públicos.

Las funcionalidades de los distintos dispositivos IoT varían. Algunos dispositivos pueden ser equipos que ejecutan sistemas operativos de escritorio comunes, y otros pueden estar ejecutando sistemas operativos muy ligeros. Los procedimientos recomendados de seguridad descritos anteriormente pueden ser aplicables a estos dispositivos en diverso grado. En caso de proporcionarse, deben seguirse las prácticas recomendadas de seguridad e implementación facilitadas por el fabricante de estos dispositivos.

Puede que algunos dispositivos heredados y limitados no se hayan diseñado de forma específica para la implementación de IoT. Estos dispositivos pueden carecer de la funcionalidad para cifrar datos, conectarse a Internet o proporcionar auditoría avanzada. En estos casos, una puerta de enlace de campo moderna y segura puede agregar datos desde dispositivos heredados y proporcionar la seguridad necesaria para la conexión a Internet de estos dispositivos. Las puertas de enlace de campo proporcionan una autenticación segura, una negociación de sesiones cifradas, la recepción de comandos desde la nube y muchas otras características de seguridad.
---
title: Marco de seguridad en Azure IoT Edge | Microsoft Docs
description: Obtenga información sobre la seguridad, la autenticación y los estándares de autorización que se usaron para desarrollar Azure IoT Edge y que se deben considerar al diseñar la solución
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 02/25/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 8aadddbc9ae13a87f89db4d7e7189ea7aa8aeef5
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56883511"
---
# <a name="security-standards-for-azure-iot-edge"></a>Estándares de seguridad para Azure IoT Edge

Azure IoT Edge está diseñado para tratar los escenarios de riesgo que son inherentes al mover los datos y análisis en el perímetro inteligente. Los estándares de seguridad de IoT Edge proporcionan flexibilidad para diferentes perfiles de riesgo y escenarios de implementación y seguir ofreciendo la protección que se espera de todos los servicios de Azure. 

Azure IoT Edge se ejecuta en varias marcas y modelos de hardware, es compatible con varios sistemas operativos y se puede aplicar a diversos escenarios de implementación. La evaluación del riesgo de un escenario de implementación depende de muchas consideraciones, entre las que se incluyen la propiedad de la solución, la geografía de implementación, la confidencialidad de datos, la privacidad, el mercado vertical de aplicación y los requisitos normativos. En lugar de ofrecer soluciones concretas para escenarios específicos, IoT Edge es un marco de seguridad extensible basado en principios bien fundamentados y diseñados para la escala. 
 
En este artículo se ofrece información general sobre el marco de seguridad de IoT Edge. Para más información, consulte la entrada de blog [Securing the intelligent edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/) (Protección del perímetro inteligente).

## <a name="standards"></a>Estándares

Los estándares promueven la facilidad de examen y la de implementación, que constituyen el sello distintivo de la seguridad. Una solución de seguridad debe prestarse al examen bajo evaluación para crear relaciones de confianza y no debe ser un obstáculo para la implementación. El diseño del marco para proteger a Azure IoT Edge se basa en protocolos de seguridad probados en el sector y probados en el tiempo para aprovechar el conocimiento y la reutilización. 

## <a name="authentication"></a>Authentication

Al implementar una solución de IoT, debe saber que sólo confianza actores, dispositivos y módulos tienen acceso a la solución. Este conocimiento ofrece la seguridad de los participantes. Azure IoT Edge alcanza este conocimiento a través de la autenticación. Autenticación basada en certificado es el mecanismo principal para la autenticación para la plataforma de Azure IoT Edge. Este mecanismo procede de un conjunto de normas que rigen la infraestructura de clave pública (PKiX) de Internet Engineering Task Force (IETF).     

Todos los dispositivos, módulos y actores que interactúan con el dispositivo de Azure IoT Edge, ya sea físicamente o a través de una conexión de red, deben tener las identidades de certificado únicas. Sin embargo, no todos los escenarios o componentes pueden prestarse a la autenticación basada en certificados. En tales escenarios, la extensibilidad del marco de seguridad ofrece alternativas seguras. 

## <a name="authorization"></a>Autorización

El principio de privilegios mínimos afirma que los usuarios y componentes de un sistema deben tener acceso solo al conjunto mínimo de recursos y datos necesarios para realizar sus funciones. Los dispositivos, módulos y actores solo deberían acceder a los recursos y datos dentro del ámbito de su permiso y únicamente cuando sea factible desde el punto de vista de la arquitectura. Algunos permisos se pueden configurables con suficientes privilegios y otros se aplican en su arquitectura.  Por ejemplo, un módulo puede autorizarse mediante una configuración con privilegios para que inicie una conexión a Azure IoT Hub. Sin embargo, no hay ningún motivo por el que un módulo en un dispositivo de Azure IoT Edge deba acceder al gemelo de un módulo en otro dispositivo de Azure IoT Edge.

Otros esquemas de autorización incluyen derechos de firma de certificado y control de acceso basado en roles (RBAC). 

## <a name="attestation"></a>Atestación

La atestación garantiza la integridad de los componentes de software.  Es importante para la detección y prevención del malware.  El marco de seguridad de Azure IoT Edge clasifica la atestación en tres categorías principales:

* Atestación estática
* Atestación en tiempo de ejecución
* Atestación de software

### <a name="static-attestation"></a>Atestación estática

La atestación estática comprueba la integridad de todos los componentes de software de un dispositivo, incluido el sistema operativo, todos los entornos en tiempo de ejecución y la información de configuración en el arranque del dispositivo. A menudo se conoce como arranque seguro. El marco de seguridad para los dispositivos de Azure IoT Edge se extiende a los fabricantes e incorpora funcionalidades seguras de hardware para garantizar procesos de atestación estática. Estos procesos incluyen procesos de arranque seguro y de actualización de firmware.  Trabajar en estrecha colaboración con los proveedores de silicio elimina las capas de firmware superfluas, con lo que se minimiza la superficie expuesta a amenazas. 

### <a name="runtime-attestation"></a>Atestación en tiempo de ejecución

Cuando un sistema ha completado un proceso de arranque seguro y está en funcionamiento, los sistemas bien diseñados podrían detectar intentos de insertar malware y tomarán las contramedidas apropiadas. Los ataques de malware puede tener como destino el sistema puertos e interfaces para tener acceso al sistema. O también, si actores malintencionados acceden físicamente a un dispositivo, pueden alterar el propio dispositivo o usar ataques de canal lateral para obtener acceso. Atestación estática no podrá detectar tal contenido malintencionado, que puede ser en forma de malware o cambios de configuración no autorizados, ya que se inserta después del proceso de arranque. Las contramedidas ofrecidas o exigidas por el hardware del dispositivo ayudan a evitar tales amenazas.  El marco de seguridad para Azure IoT Edge pide explícitamente extensiones para combatir amenazas en tiempo de ejecución.  

### <a name="software-attestation"></a>Atestación de software

Deben aceptar todos los sistemas en buen estado, incluidos los sistemas perimetrales inteligentes, actualizaciones y revisiones.  La seguridad es importante para los procesos de actualización; de lo contrario, pueden ser vectores potenciales de amenazas.  El marco de seguridad para Azure IoT Edge requiere actualizaciones a través de paquetes medidos y firmados para asegurar la integridad de y autenticar el origen de los paquetes.  Este estándar se aplica a todos los sistemas operativos y componentes de software de la aplicación. 

## <a name="hardware-root-of-trust"></a>Raíz de confianza del hardware

Para muchos dispositivos de inteligencia perimetral, especialmente aquellos en situaciones en las que actores malintencionados potenciales tienen acceso físico al dispositivo, la seguridad del hardware del dispositivo es la última defensa de protección. Un hardware resistente a alteraciones es fundamental en tales implementaciones. Azure IoT Edge anima a los proveedores de hardware de silicio seguros a ofrecer distintos tipos de raíz de confianza del hardware para dar cabida a varios perfiles de riesgo y escenarios de implementación. La confianza en el hardware puede proceder de estándares comunes de protocolos de seguridad como Módulo de plataforma segura (ISO/IEC 11889) y el motor Device Identifier Composition Engine (DICE) de Trusted Computing Group. Las tecnologías de enclave seguro como TrustZones y Software Guard Extensions (SGX) también proporcionan confianza de hardware. 

## <a name="certification"></a>Certificación

Para ayudar a los clientes a tomar decisiones informadas cuando adquieren dispositivos de Azure IoT Edge para su implementación, el marco de Azure IoT Edge incluye requisitos de certificación.  La base de estos requisitos son las certificaciones relativas a las notificaciones y certificaciones de seguridad relativas a la validación de la implementación de seguridad.  Por ejemplo, una certificación de la notificación de seguridad podría informar que el dispositivo de IoT Edge utiliza hardware seguro conocido por resistir ataques de arranque. Una certificación de validación podría informar que el hardware seguro se ha implementado correctamente para ofrecer este valor en el dispositivo.  De acuerdo con el principio de simplicidad, el marco intenta mantener la carga de certificación al mínimo.   

## <a name="extensibility"></a>Extensibilidad

Con la tecnología IoT que impulsa diferentes tipos de transformaciones de negocio, es lógico que la seguridad evolucione a la par que los escenarios emergentes.  El marco de seguridad de Azure IoT Edge comienza con una base sólida en la que se construye la extensibilidad en diferentes dimensiones para incluir: 

* Servicios de seguridad de primera mano, como el servicio Device Provisioning para Azure IoT Hub.
* Servicios de terceros, como servicios de seguridad administrados para diferentes aplicaciones verticales (como servicios industriales o sanitarios) o enfocados a la tecnología (como la supervisión de seguridad de redes de malla o servicios de atestación de hardware de silicio) a través de una rica red de asociados.
* Los sistemas heredados deben incluir la retroadaptación con estrategias de seguridad alternativas, como el uso de tecnología segura distinta de los certificados para la autenticación y administración de identidades.
* Hardware seguro para la adopción sin fisuras de tecnologías de hardware seguro emergentes y contribuciones de asociados de silicio.

Al final, el mayor éxito en asegurar el perímetro inteligente resulta de las contribuciones de colaboración de una comunidad abierta impulsada por el interés común de proteger IoT.  Estas contribuciones podrían consistir en servicios o tecnologías seguros.  El marco de seguridad de Azure IoT Edge ofrece una base sólida para la seguridad que es extensible a la máxima cobertura para ofrecer el mismo nivel de confianza e integridad en el perímetro inteligente que con la nube de Azure.  

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo Azure IoT Edge va a [proteger el perímetro inteligente](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

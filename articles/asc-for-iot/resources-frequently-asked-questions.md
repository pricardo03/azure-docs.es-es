---
title: Preguntas para ASC para la versión preliminar de IoT más frecuentes | Microsoft Docs
description: Encuentre respuestas a más preguntas más frecuentes sobre ASC para el servicio y las características de IoT.
services: ASCforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 97fda6c2-1ecb-491f-b48d-41788bd7e0d3
ms.service: ASCforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 8940d629d2f4827ee724c813a40394378dde5a4c
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58575154"
---
# <a name="asc-for-iot-frequently-asked-questions"></a>Preguntas más frecuentes de ASC para IoT  

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se proporciona una lista de las preguntas más frecuentes y respuestas acerca de cómo ASC para IoT. 

## <a name="does-azure-provide-support-for-iot-security"></a>¿Azure ofrece compatibilidad para seguridad de IoT?

Azure proporciona una vista integrada para supervisar y administrar la seguridad de IoT como parte de la solución general de seguridad a través de Azure Security Center. Si es un desarrollador de aplicaciones, puede usar la vista de centro de IoT para administrar la seguridad de la aplicación de IoT.

## <a name="what-is-azures-unique-value-proposition-for-iot-security"></a>¿Qué es la propuesta de valor único de Azure para la seguridad de IoT?

ASC para IoT permite a las empresas a ampliar su vista de la seguridad cibernética existente a toda la solución de IoT. Azure proporciona una vista integral de su solución empresarial, lo que le permite realizar acciones relacionadas con la empresa y las decisiones basándose en la posición de seguridad empresarial y los datos recopilan. Combina la seguridad mediante Azure IoT, Azure IoT Edge, Azure Sphere, Central de Azure y Azure Security Center le permiten crear la solución que desee con la seguridad que necesita.

## <a name="who-is-asc-for-iot-made-for"></a>¿Quién es ASC para prever IoT? 

ASC para IoT está integrado en la seguridad de Azure IoT Hub y proporciona las operaciones de seguridad de administración para la solución empresarial de un día para otro. ASC para IoT también se integra en las capacidades de Azure Security Center y proporcionar una vista integrada para supervisar y administrar la seguridad de IoT como parte de su solución de seguridad global.

## <a name="how-does-asc-for-iot-compare-to-the-competition"></a>¿Cómo se compara con la competencia ASC para IoT?

Mientras que otras soluciones proporcionan un conjunto de funcionalidades que permiten a los clientes crear sus propias soluciones, ASC para IoT proporciona una solución de seguridad de IoT-to-end única que proporciona una vista amplia a través de la seguridad de todos los recursos de Azure relacionados. Azure permite una implementación rápida e integración completa con los módulos gemelos de IoT Hub para facilitar la integración con herramientas de administración de dispositivos existentes.

## <a name="do-i-have-to-be-an-azure-security-center-asc-customer"></a>¿Tiene que ser un cliente de Azure Security Center (ASC)?

No, pero se recomienda. Sin ASC, ASC para IoT recibe datos limitados recursos conectados y proporciona un análisis limitado de la superficie expuesta a ataques potenciales, amenazas y ataques potenciales. 

## <a name="do-i-have-to-be-an-azure-iot-customer"></a>¿Tiene que ser un cliente de IoT de Azure?

Sí. ASC para IoT se basa en la infraestructura y conectividad de IoT de Azure.

## <a name="do-i-have-to-install-an-agent"></a>¿Es necesario instalar a un agente?

Instalación del agente en los dispositivos de IoT no es obligatorio para habilitar la ASC Microsoft para IoT. Puede elegir entre las tres opciones siguientes, obtener funcionalidades de supervisión y administración según la selección de diferentes niveles de seguridad:

1. Instale el ASC para el agente de seguridad de IoT con o sin modificaciones. Esta opción proporciona el máximo nivel de sobre la seguridad mejorada de acceso y el comportamiento del dispositivo.

2. Cree a su propio agente e implemente Microsoft ASC para esquema de mensaje de seguridad de IoT. Esta opción habilita el uso de Microsoft ASC para herramientas de análisis de IoT sobre el agente de seguridad del dispositivo.

3. Ninguna instalación del agente de seguridad de los dispositivos de IoT. Esta opción habilita la supervisión comunicación de IoT Hub, con capacidades de administración y supervisión de seguridad reducida. 

## <a name="what-does-the-asc-for-iot-agent-do"></a>¿Qué hace el ASC para el agente de IoT?

ASC para el agente de IoT proporciona cobertura de amenazas en el nivel de dispositivo para la configuración del dispositivo, comportamiento y acceso (mediante el examen de la configuración), procesos y conectividad. La ASC para el agente de seguridad de IoT no examina los datos relacionados con el negocio o actividad.

## <a name="where-i-can-get-the-asc-for-iot-security-agent"></a>¿Dónde puedo obtener la ASC para el agente de seguridad de IoT?

Los procesos de ASC para el agente de seguridad de IoT es código abierto y está disponible en GitHub en 32 bits y las versiones de Windows y Linux de 64 bits: https://github.com/Azure/Azure-Security-IoT-Preview

## <a name="where-does-the-asc-for-iot-agent-get-installed"></a>¿Donde instalado obtener los procesos de ASC para el agente de IoT? 

Encontrará información detallada sobre instalación y agente de implementación en GitHub: https://github.com/Azure/Azure-Security-IoT-Preview

## <a name="what-are-the-dependencies-and-prerequisites-of-the-agent"></a>¿Cuáles son las dependencias y requisitos previos del agente?

ASC para IoT admite una amplia variedad de plataformas. Consulte [plataformas de dispositivos compatibles](how-to-deploy-agent.md) para comprobar la compatibilidad para los dispositivos específicos. 

## <a name="which-data-is-collected-by-the-agent"></a>¿El agente recopila los datos?

El agente recopila conectividad, acceso, configuración del firewall, lista de procesos y línea base del SO.

## <a name="how-much-data-will-the-agent-generate"></a>¿Cuántos datos se generará el agente?

Generación de datos del agente está controlada por dispositivo, aplicación, tipo de conectividad y configuración del agente de cliente. Debido a la gran variabilidad entre dispositivos y las soluciones de IoT, se recomienda implementar primero el agente en un laboratorio o configuración de pruebas para observar, aprender y establecer la configuración específica que se adapte a sus necesidades, mientras mide la cantidad de datos generados. Después de iniciar el servicio, los procesos de ASC para el agente de IoT proporcionan recomendaciones operativas para optimizar el rendimiento del agente que le ayudarán en el proceso de configuración y personalización.

## <a name="how-can-i-control-my-billing"></a>¿Cómo puedo controlar mi facturación?

ASC para IoT proporciona a agente configurable exámenes, los búferes de datos y la capacidad para crear alertas personalizadas que aumentar o reducen la cantidad de datos generados por el agente.

## <a name="do-agent-messages-use-up-quota-from-iot-hub"></a>¿Se usa mensajes del agente de cuota de IoT Hub?

Sí. Los datos del agente transmitido se contabilizarán en su cuota de IoT Hub. 

## <a name="what-next-ive-installed-an-agent-and-dont-see-any-activities-or-logs"></a>¿Qué más? Ha instalado a un agente y no verán ninguna actividad o registros

1. Compruebe el [tipo de agente se ajusta a la plataforma de SO designada de su dispositivo](how-to-deploy-agent.md)

1. Confirme la [agente se está ejecutando en el dispositivo](how-to-agent-configuration.md).

2. Compruebe el [servicio se ha habilitado correctamente](quickstart-onboard-iot-hub.md) a **seguridad** en IoT Hub. 

3. Compruebe que el dispositivo está [configurado en IoT Hub con la ASC para el módulo de IoT](quickstart-create-security-twin.md).  

Si las actividades o los registros no están aún disponibles, póngase en contacto con su ASC para IoT asociado para obtener ayuda adicional.

## <a name="what-happens-when-the-internet-connection-stops-working"></a>¿Qué ocurre cuando la conexión a internet deja de funcionar?

El agente continúa ejecutándose y almacenar los datos siempre y cuando el dispositivo se está ejecutando. Datos se almacenan en la caché de mensajes de seguridad según la configuración de tamaño. Cuando el dispositivo vuelve a tener conectividad, los mensajes de seguridad reanudación el envío. 

## <a name="if-the-device-is-restarted-will-the-security-agent-self-recover"></a>¿Si el dispositivo se reinicia, el agente de seguridad automática recuperará?

El agente de seguridad está diseñado para volver a ejecutar automáticamente con cada reinicio del dispositivo.

## <a name="can-the-agent-affect-the-performance-of-the-device-or-other-installed-software"></a>¿El agente puede afectar al rendimiento del dispositivo o de otro software instalado?

El agente consume recursos de la máquina como cualquier otra aplicación o proceso y no debe interrumpir la actividad normal de los dispositivos. Consumo de recursos en el dispositivo que se ejecuta el agente está emparejado con su programa de instalación y configuración. Se recomienda probar la configuración de agente en un entorno independiente, junto con interoperabilidad con las otras aplicaciones de IoT y funcionalidad, antes de intentar realizar la implementación en un entorno de producción.

## <a name="im-making-some-maintenance-on-the-device-can-i-turn-off-the-agent"></a>Estoy realizando algún mantenimiento en el dispositivo. ¿Puedo desactivar el agente?

El agente no puede desactivarse.

## <a name="is-there-a-way-to-test-if-the-agent-is-working-correctly"></a>¿Hay una manera de probar si el agente está funcionando correctamente? 

Si el agente se detiene la comunicación o se produce un error al enviar mensajes de seguridad, un **dispositivo es silencioso** alerta se genera.

## <a name="can-i-create-my-own-alerts"></a>¿Puedo crear mis propias alertas?

Sí. Puede establecer una alerta personalizada en el conjunto predeterminado de comportamientos, como la dirección IP y puertos abiertos. Consulte [crear alertas personalizadas](quickstart-create-custom-alerts.md) para obtener más información sobre alertas personalizadas y cómo realizarlas. 

## <a name="where-can-i-see-logs-can-i-customize-logs"></a>¿Dónde puedo ver los registros? ¿Puedo personalizar los registros?

- Ver las alertas y las recomendaciones mediante el área de trabajo de Log Analytics conectado. Configurar el tamaño de almacenamiento y la duración en el área de trabajo.

- Datos sin procesar desde el agente de seguridad también pueden almacenarse en su cuenta de Log Analytics. Considere la posibilidad de tamaño, la duración, los requisitos de almacenamiento y los costos asociados antes de cambiar la configuración de esta opción. 

## <a name="why-should-i-add-asc-for-iot-to-the-module-identity-what-is-it-used-for"></a>¿Por qué debo agregar ASC para IoT a la identidad de módulo? ¿Para qué se usa?

Los procesos de ASC para el módulo de IoT se usan para la administración y configuración del agente.


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de cómo empezar a trabajar con procesos de ASC para IoT, consulte los artículos siguientes:


- Leer los procesos de ASC para IoT [información general](overview.md)
- Compruebe el [requisitos previos de servicio](service-prerequisites.md)
- Más información acerca de cómo [empezar a trabajar](getting-started.md)
- Comprender [ASC para las alertas de seguridad de IoT](concept-security-alerts.md)


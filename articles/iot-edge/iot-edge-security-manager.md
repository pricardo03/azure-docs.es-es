---
title: Administrador de seguridad de Azure IoT Edge | Microsoft Docs
description: Administra la posición de seguridad del dispositivo IoT Edge y la integridad de los servicios de seguridad.
services: iot-edge
keywords: seguridad, elemento, enclave, IoT Edge
author: eustacea
manager: timlt
ms.author: eustacea
ms.date: 07/30/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4594685291396b8b80e62abe57be109f0abbd81d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126350"
---
# <a name="azure-iot-edge-security-manager"></a>Administrador de seguridad de IoT Edge

El Administrador de seguridad de Azure IoT Edge es un núcleo de seguridad bien delimitado para proteger el dispositivo IoT Edge y todos sus componentes mediante la abstracción de hardware de silicio seguro. Es crucial para fortalecer la seguridad y proporciona el punto de integración de la tecnología para los fabricantes de dispositivos originales (OEM).

![Administrador de seguridad de IoT Edge](media/edge-security-manager/iot-edge-security-manager.png)

El objeto del administrador de seguridad de IoT Edge es defender la integridad del dispositivo IoT Edge y todas las operaciones de software inherentes.  Para ello, transiciona la confianza desde la raíz de confianza de hardware subyacente (si está disponible) para arrancar de manera segura en el entorno de ejecución de IoT Edge y continuar supervisando la integridad de sus operaciones.  En esencia, el administrador de seguridad de IoT Edge consta del software que funciona junto con el hardware de silicio seguro donde esté disponible y habilitado para ayudar a entregar las garantías de seguridad más altas posible.  

Las responsabilidades del administrador de seguridad de IoT Edge incluyen (entre otras):

* Arranque protegido y medido del dispositivo Azure IoT Edge.
* Aprovisionamiento de identidad del dispositivo y transición de confianza donde corresponda.
* Hospedaje y protección de los componentes del dispositivo de servicios en la nube como servicio Device Provisioning.
* Aprovisionamiento seguro de los módulos de IoT Edge con la identidad única del módulo.
* Equipo selector de la raíz de confianza de hardware de dispositivo a través de servicios notariales.
* Supervisión de la integridad de las operaciones de IoT Edge en el entorno de ejecución.

El administrador de seguridad de IoT Edge consta de tres componentes principales:

* Demonio de seguridad de IoT Edge.
* Capa de abstracción de plataforma del módulo de seguridad de hardware (HSM PAL).
* HSM o raíz de confianza de silicio de hardware opcional o altamente recomendado.

## <a name="the-iot-edge-security-daemon"></a>El demonio de seguridad de IoT Edge

El demonio de seguridad de IoT Edge es el software responsable de las operaciones lógicas del administrador de seguridad de IoT Edge.  Consta de una porción importante de la base informática de confianza del dispositivo IoT Edge. 

### <a name="design-principles"></a>Principios de diseño

Para ejecutar sus responsabilidades de manera adecuada y práctica, el demonio de seguridad de IoT Edge apunta a cumplir con los principios básicos siguientes:

#### <a name="maximize-operational-integrity"></a>Maximización de la integridad de las operaciones

El demonio de seguridad de IoT Edge está diseñado para funcionar con la mayor integridad posible dentro de la funcionalidad de defensa de cualquier hardware de raíz de confianza determinado. Con una integración correcta, el hardware de raíz de confianza mide y supervisa el demonio de seguridad de manera estática y en el entorno de ejecución para resistir manipulaciones.

El acceso físico siempre es una amenaza para los dispositivos IoT en general.  Por tanto, la raíz de confianza de hardware juega un papel importante en la defensa de la integridad del demonio de seguridad de IoT Edge.  La raíz de confianza de hardware tiene dos variantes:

* elementos seguros para la protección de información confidencial, como secretos y claves criptográficas.
* enclaves seguros para la protección de secretos como claves y cargas de trabajo confidenciales, como la medición y facturación.

La existencia de estos dos modelos para el uso de la raíz de confianza de hardware da lugar a dos tipos de entornos de ejecución:

* El entorno de ejecución estándar o enriquecido (REE) que se basan en el uso de elementos seguros para proteger información confidencial.
* El entorno de ejecución de confianza (TEE) que se basan en el uso de la tecnología de enclave seguro para proteger información confidencial y ofrecer protección para la ejecución de software.

Para dispositivos que usan los enclaves seguros como raíz de confianza de hardware, se espera que la lógica confidencial dentro del demonio de seguridad de IoT Edge resida dentro del enclave.  Las partes no confidenciales del demonio de seguridad pueden residir fuera del entorno de ejecución de confianza.  En cualquier caso, se espera de los original design manufacturers (ODM) y los fabricantes de equipos originales (OEM) para ampliar la confianza desde su HSM para medir y defender la integridad del demonio de seguridad de Edge en el arranque y en el entorno de ejecución.

#### <a name="minimize-bloat-and-churn"></a>Minimización del sobredimensionamiento y la renovación

Otro principio básico para el demonio de seguridad de Edge es minimizar la renovación.  Para obtener el máximo nivel de confianza, el demonio de seguridad de IoT Edge puede acoplarse estrechamente con la raíz de confianza de hardware de dispositivo si está disponible y funcionar como código nativo.  Es común que estos tipos de realizaciones actualicen el software de demonio a través de las rutas de actualización segura de la raíz de confianza de hardware (en lugar de los mecanismos de actualización que proporciona el SO), algo que puede ser complicado en función del escenario de hardware e implementación específico.  Aunque la renovación de seguridad es una recomendación firme para los dispositivos IoT, resulta evidente que los requisitos excesivos de actualización o las cargas de actualizaciones de gran tamaño pueden expandir de muchas maneras la superficie expuesta a amenazas.  Algunos ejemplos son la omisión de las actualizaciones para maximizar la disponibilidad operativa o hardware de raíz de confianza demasiado limitado para procesar grandes cargas de actualizaciones.  Por tanto, el diseño del demonio de seguridad de IoT Edge es conciso para que la superficie y, por tanto, base informática de confianza no crezcan y para minimizar los requisitos de actualizaciones.

### <a name="architecture-of-iot-edge-security-daemon"></a>Arquitectura del demonio de seguridad de IoT Edge

![Demonio de seguridad de Azure IoT Edge](media/edge-security-manager/iot-edge-security-daemon.png)

El demonio de seguridad de IoT Edge e-stá diseñado para aprovechar las ventajas de cualquier tecnología de raíz de confianza de hardware disponible para fortalecer la seguridad.  Por tanto, se diseñó para permitir la operación de división global entre un entorno de ejecución estándar o enriquecido (REE) y un entorno de ejecución de confianza (TEE) para aprovechar las tecnologías de hardware que ofrecen los entornos de ejecución de confianza (TEE).  Esenciales para la arquitectura del demonio de seguridad de IoT Edge son las interfaces específicas de rol para habilitar la interacción de componentes importantes de Edge para garantizar la integridad del dispositivo IoT Edge y sus operaciones.

#### <a name="cloud-interface"></a>Interfaz de la nube

La interfaz de la nube permite que el demonio de seguridad de IoT Edge tenga acceso a los servicios en la nube como cortesía de la nube para la seguridad de los dispositivos, como la renovación de seguridad.  Por ejemplo, el demonio de seguridad de IoT Edge actualmente usa esta interfaz para tener acceso a Azure IoT Hub [el servicio de aprovisionamiento de dispositivos (DPS)](https://docs.microsoft.com/azure/iot-dps/) para la administración del ciclo de vida de la identidad.  

#### <a name="management-api"></a>API de administración

El demonio de seguridad de IoT Edge ofrece una API de administración, que el agente de Edge llama en el momento de crear, iniciar, detener o eliminar un módulo de Edge. El demonio de seguridad de IoT Edge almacena "registros" para todos los módulos activos. Estos registros asignan la identidad de un módulo a alguna propiedades del módulo. Algunos ejemplos de estas propiedades son el identificador de proceso (pid) del proceso que se ejecuta en el contenedor o el hash de contenido del contenedor de Docker.

La API de carga de trabajo (que se describe a continuación) usa estas propiedades para atestar que el autor de llamada está autorizado para hacer una acción.

La API de administración es una API con privilegios, a la que solo se puede llamar desde el agente de IoT Edge.  Como el demonio de seguridad de IoT Edge arranca e inicia al agente de IoT Edge, puede crear un registro implícito del agente de IoT Edge, una vez que se ha atestado que el agente de IoT Edge que no se ha manipulado. El mismo proceso de atestación que usa la API de carga de trabajo se usa para restringir el acceso a la API de administración solo para el agente de IoT Edge.

#### <a name="container-api"></a>API de contenedor

El demonio de seguridad de IoT Edge ofrece la interfaz de contenedor para interactuar con el sistema del contenedor en uso, como Moby y Docker, para la creación de instancias de módulo.

#### <a name="workload-api"></a>API de carga de trabajo

La API de carga de trabajo es un API del demonio de seguridad de IoT Edge accesible para todos los módulos, incluido el agente de IoT Edge. Proporciona una prueba de identidad, en forma de un certificado X509 o token firmado liberado de HSM, y la agrupación de confianza correspondiente a un módulo. La agrupación de confianza contiene certificados de entidad de certificación para todos los demás servidores en que deben confiar los módulos.

El demonio de seguridad de IoT Edge usa un proceso de atestación para proteger esta API. Cuando un módulo llama a esta API, el demonio de seguridad de IoT Edge intenta encontrar un registro para la identidad. Si eso se completa correctamente, usa las propiedades del registro para medir el módulo. Si el resultado del proceso de medición coincide con el registro, se genera un nuevo certificado X509 o token firmado liberado de HSM, y los certificados de entidad de certificación correspondientes (agrupación de confianza) se devuelven al módulo.  El módulo usa este certificado para conectarse a IoT Hub, otros módulos, o iniciar un servidor. Cuando el certificado o un token firmado se acerca a la expiración, es responsabilidad del módulo solicitar un certificado nuevo.  Se incorporan consideraciones adicionales en el diseño para que el proceso de renovación de identidades sea lo más sencillo posible.

### <a name="integration-and-maintenance"></a>Integración y mantenimiento

Microsoft mantiene el código base principal para el [demonio de seguridad de IoT Edge en GitHub](https://github.com/Azure/iotedge/tree/master/edgelet).

#### <a name="installation-and-updates"></a>Instalación y actualizaciones

Como se trata de un proceso nativo, la instalación y las actualizaciones del demonio de seguridad de IoT Edge se administra a través del sistema de administración de paquetes del sistema operativo.  Sin embargo, se espera que los dispositivos IoT Edge con raíz de confianza de hardware proporcionen una protección adicional a la integridad del demonio a través de la administración de su ciclo de vida mediante los sistemas de administración de actualizaciones y el arranque seguro de los dispositivos.  Se deja al criterio de los fabricantes de dispositivos la exploración de estas vías según las funcionalidades de sus respectivos dispositivos.

#### <a name="versioning"></a>Control de versiones

El entorno de ejecución de IoT Edge hace el seguimiento de la versión del demonio de seguridad de IoT Edge y la notifica. La versión se notifica como el atributo *runtime.platform.version* de la propiedad notificada del módulo del agente de Edge.

### <a name="hardware-security-module-platform-abstraction-layer-hsm-pal"></a>Capa de abstracción de plataforma del módulo de seguridad de hardware (HSM PAL)

HSM PAL abstrae todo el hardware de raíz de confianza para aislar al desarrollador o usuario de IoT Edge de sus complejidades.  Se compone de una combinación de la interfaz de programación de aplicaciones (API) y procedimientos de comunicaciones entre dominios, por ejemplo, la comunicación entre un entorno de ejecución estándar y un enclave seguro.  La implementación real de HSM PAL depende del hardware seguro específico que esté en uso.  Su existencia permite el uso de prácticamente cualquier hardware de silicio seguro en todo el ecosistema de IoT.

## <a name="secure-silicon-root-of-trust-hardware"></a>Hardware de raíz de confianza de silicio seguro

El silicio seguro es necesario para delimitar la confianza dentro del hardware del dispositivo IoT Edge.  El silicio seguro viene en una gran variedad de formas que incluyen Módulo de plataforma segura (TPM), Elemento seguro integrado (eSM), ARM TrustZone, Intel SGX y tecnologías de silicio seguro.  Se recomienda usar la raíz de confianza de silicio seguro en dispositivos, dadas las amenazas asociadas con la accesibilidad física de los dispositivos IoT.

## <a name="iot-edge-security-manager-integration-and-maintenance"></a>Mantenimiento e integración del administrador de seguridad de IoT Edge

Un objetivo primordial del administrador de seguridad de IoT Edge es identificar y aislar los componentes que se encargan de defender la seguridad y la integridad de la plataforma de Azure IoT Edge para el sistema de protección personalizado.  Por tanto, se espera que terceros (como fabricantes de dispositivos) usen las características de seguridad personalizadas disponibles con su hardware de dispositivo.  Consulte la sección Pasos siguientes que aparece a continuación con vínculos a ejemplos sobre cómo proteger el administrador de seguridad de Azure con el Módulo de plataforma segura (TPM) en plataformas Linux y Windows.  Estos ejemplos usan módulos TPM virtuales o software, pero se aplican directamente al uso de dispositivos TPM discretos.  

## <a name="next-steps"></a>Pasos siguientes

Lea el blog sobre [Securing the intelligent edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/) (Protección del perímetro inteligente).

Cree y aprovisione un dispositivo Edge con un [TPM virtual en una máquina virtual con Linux](how-to-auto-provision-simulated-device-linux.md).

Creación y aprovisionamiento de un [dispositivo TPM Edge simulado en Windows](how-to-auto-provision-simulated-device-windows.md).

<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/

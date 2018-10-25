---
title: Certificación del módulo de IoT Edge | Microsoft Docs
description: Realice la certificación de un módulo de IoT Edge para Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: c37ed908b61ca54957affed3f81526353bc3f53b
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389571"
---
# <a name="the-iot-edge-module-certification-process"></a>Proceso de certificación del módulo de IoT Edge

En este artículo se describen los pasos y requisitos necesarios para certificar un módulo de IoT Edge para publicarlo en Azure Marketplace. 

>[!Note]
>Tenga en cuenta que este es un documento temporal. La plataforma de Marketplace para el módulo de IoT Edge se está compilando en paralelo y este artículo será reemplazado con documentación pública.

## <a name="understanding-an-iot-edge-module"></a>Descripción de un módulo de IoT Edge

Terminología del módulo:

-   Una **imagen de módulo** es un paquete que contiene el software que define un módulo.

-   Una **instancia de módulo** es la unidad específica del cálculo que ejecuta la imagen de módulo en un dispositivo IoT Edge. La instancia de módulo se inicia mediante la instancia de IoT Edge en tiempo de ejecución.

Los módulos también pueden incluir el módulo SDK de IoT, que utiliza la siguiente terminología:

-   Una **identidad de módulo** es un fragmento de información (incluidas las credenciales de seguridad) almacenada en IoT Hub y que está asociada a cada instancia del módulo.

-   Un **módulo gemelo** es un documento JSON almacenado en IoT Hub, que contiene información de los estados de una instancia de módulo, incluidos los metadatos, las configuraciones y las condiciones.

-   Los **SDK** se utilizan para desarrollar módulos personalizados en varios lenguajes, como: C\#, C, Python, Java y Node.JS.

## <a name="the-onboarding-process-for-an-iot-edge-module"></a>Proceso de incorporación de un módulo de IoT Edge

En la siguiente captura de pantalla se muestra el proceso para que un módulo de IoT Edge se publique en Azure Marketplace.

![Proceso de certificación](./media/cloud-partner-portal-iot-edge-module-certification-process/onboarding-process.png)

### <a name="onboarding-step-details"></a>Detalles del paso de incorporación

-   **Paso 1**: los asociados envían sus ofertas con el tipo de oferta **Módulo de IoT Edge** en la herramienta Cloud Partner Portal que ha creado el equipo de Azure Marketplace. Debe ser un asociado oficial de Microsoft para acceder a la herramienta Cloud Partner Portal. Para obtener más información, consulte la [guía del editor](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

-   **Paso 2**: Marketplace ejecuta automáticamente revisiones antivirus y antimalware. El equipo de IoT puede ejecutar sus pruebas de ingesta automatizadas y personalizadas durante esta fase.

-   **Paso 3**: el módulo es público. Se enumera en Marketplace y el contenedor puede extraerse de forma anónima desde una dirección URL. Por ejemplo, *marketplace.azurecr.io/module-identifier*.

## <a name="iot-edge-module-certification-criteria"></a>Criterios de certificación del módulo de IoT Edge

A continuación, se detallan los principales requisitos que se deben cumplir para que un módulo de IoT Edge se certifique y se publique en Azure Marketplace.

>[!Note]
>Es posible que estos requisitos cambien. Recibirá una notificación con antelación y tendrá tiempo para actualizar sus contenedores para que puedan cumplir los requisitos actualizados.

### <a name="technical-requirements"></a>Requisitos técnicos

**Módulo de IoT Edge**

-   Actualmente, solo los contenedores compatibles con Docker se admiten como módulos de IoT Edge. El módulo debe ejecutarse en [Moby](https://mobyproject.org/). 

    >[!Note]
    >Es probable que los contenedores de Docker funcionen con Moby, ya que Moby es el proyecto de código abierto subyacente de Docker.

-   El asociado debe proporcionar la configuración predeterminada siguiente: 

    -   Una **etiqueta** predeterminada (que no puede estar vacía).

    -   Un valor predeterminado **createOptions** (que puede estar vacío).

    -   Si usa el SDK del módulo de IoT, un **módulo gemelo** predeterminado (que puede estar vacío).

    -   Si usa el SDK del módulo de IoT, unas **rutas** predeterminadas (que pueden estar vacías).

**Compatibilidad con plataformas**

-   Solo las plataformas que están **generalmente disponibles** en IoT Edge Tier 1 (según lo registrado en el [soporte técnico de Azure IoT Edge ](https://docs.microsoft.com/azure/iot-edge/support)) deben ser compatibles. Por ejemplo, esto normalmente significa admitir las siguientes combinaciones de arquitectura y sistemas operativos:

    -   Ubuntu Server 18.04 para x64

    -   Ubuntu Server 16.04 para x64

    -   Raspbian-stretch para arm32 (armhf)

**Dimensionamiento del dispositivo**

-   Cualquier dispositivo con las dimensiones equivalentes (CPU, RAM, Storage, GPU, etc.) de una Raspberry Pi o superior puede ser un dispositivo de IoT Edge. Si un módulo solo funciona dentro de los límites de las dimensiones específicas, esos límites deben especificarse en la descripción del módulo.

**Configuración predeterminada**

-   Un módulo debe comenzar con los parámetros predeterminados integrados en cada plataforma compatible (sistema operativo y arquitectura).

-   Las opciones de configuración deben estar claramente documentadas (etiquetas, variables de entorno, módulos gemelos, rutas). Como parte de la lista, los asociados pueden definir una descripción para su módulo que admita el formato HTML básico o apunte a una página web externa.

**Control de versiones**

-   Los clientes deben poder elegir si quieren actualizar automáticamente un módulo procedente de Marketplace o si prefieren usar una versión exacta que ya hayan probado. Los módulos de Marketplace deben seguir el mismo patrón de versión que el tiempo de ejecución de IoT Edge. Por ejemplo: 

    -   Las etiquetas en secuencia, como "1.0", se pueden actualizar.

    -   Las etiquetas de versiones secundarias, como "1.3.24", no se pueden actualizar.

**Telemetría**

-   Los módulos que usan el SDK del módulo de IoT deben establecer el identificador de módulo único que les asignó Marketplace para fines de telemetría. Gracias a ello, Azure Marketplace podrá identificar la cantidad de instancias de módulos que se están ejecutando. Este identificador único es el nombre del contenedor que asignó Marketplace en el momento de la ingestión. Use los métodos de los siguientes SDK para establecer este identificador:
    - [C\#](https://hub.docker.com/_/mysql/)
    - [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
    - [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
    - [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._product_info?view=azure-java-stable)

-   En cuanto a los módulos que no usan el SDK del módulo de IoT, puede encontrar detalles menos precisos en Cloud Partner Portal. Por ejemplo, el número de descargas.

**Seguridad**

-   Los contenedores deben obtener un acceso con privilegios mínimos al host. Recuerde que los contenedores "con privilegios" deben ser excepcionales.

-   Los asociados deben mantener su módulo protegido como parte del servicio de soporte técnico que ofrecen.

**Actualizaciones**

-   Los asociados deben mantener su módulo actualizado y en funcionamiento. Si hubiera cambios importantes de última hora en el entorno de ejecución de IoT Edge, recibirán las notificaciones adecuadas de antemano.

**SDK del módulo de IoT**

-   Incluir el SDK del módulo de IoT no es un requisito previo para realizar la certificación.
    Sin embargo, si decide incluir el SDK del módulo de IoT, puede proporcionar una experiencia de usuario aún mejor. Por ejemplo, para admitir el enrutamiento, el envío de mensajes a la nube, etc. El SDK del módulo de IoT es necesario para obtener datos de telemetría sobre la cantidad de instancias de módulos en ejecución.

**Requisito subjetivo**

-   Debe cumplir con al menos un caso de uso real de IoT Edge. Por ejemplo, un contenedor de WordPress no debe estar incorporado a menos que un cliente esté dispuesto a usarlo desde IoT Edge.

**Requisitos legales (de la directiva de AMP)**

-   El módulo debe cumplir con los contratos y directivas de Microsoft Azure Marketplace. Para obtener más información, consulte el Contrato del editor y la [Directiva de participación](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) adjuntos.

-   Además de los contratos y las directivas de Azure Marketplace, puede haber otros requisitos legales adicionales específicos para un tipo de oferta del módulo de IoT Edge. Este apartado está actualmente bajo revisión.

-   El módulo debe tener unos *términos de uso* y una *directiva de privacidad*.

-   También debe tener un aviso de terceros si usa elementos de terceros.

**Requisitos de soporte técnico (de la directiva de AMP)**

-   Los asociados son responsables de ofrecer soporte técnico para sus módulos de IoT Edge. Asimismo, se asegurarán de que las opciones de soporte técnico proporcionadas en la descripción del módulo de IoT Edge permanezcan disponibles y que al menos una de esas opciones de soporte técnico esté disponible en todo momento. (Consulte la Sección 4 del Contrato del editor de AMP para obtener más detalles).

**Categorización**

-   Todos los módulos de IoT Edge aparecerán en la categoría **Internet de las cosas \> Módulo de IoT Edge**. Es responsabilidad del asociado elegir la mejor subcategoría para su producto.

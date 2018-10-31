---
title: 'Implementación de la solución Supervisión remota de Java: Azure | Microsoft Docs'
description: En este tutorial, se muestra cómo aprovisionar el acelerador de la solución Supervisión remota mediante la CLI.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/12/2018
ms.topic: conceptual
ms.openlocfilehash: ddb0b5b1a0847200caa7d8d04ecdc9dab4c41d14
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956704"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>Implementación del acelerador de la solución Supervisión remota mediante la CLI

En este tutorial, se muestra cómo aprovisionar el acelerador de la solución Supervisión remota. La solución se implementa mediante la CLI. También puede implementar la solución con la interfaz de usuario basada en web de azureiotsuite.com. Para obtener más información sobre esta opción, consulte [Implementación del acelerador de la solución Supervisión remota](quickstart-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Requisitos previos

Para implementar el acelerador de la solución Supervisión remota, necesita una suscripción de Azure activa.

En caso de no tener ninguna, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](http://azure.microsoft.com/pricing/free-trial/).

Para ejecutar la CLI, necesita tener instalado [Node.js](https://nodejs.org/) en su máquina local.

## <a name="install-the-cli"></a>Instalación de la CLI de Azure

Para instalar la CLI, ejecute el siguiente comando en el entorno de la línea de comandos:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Inicio de sesión en la CLI

Para poder implementar el acelerador de la solución, debe iniciar sesión en su suscripción de Azure mediante la CLI de la manera siguiente:

```cmd/sh
pcs login
```

Siga las instrucciones que aparecen en pantalla para completar el proceso de inicio de sesión.

## <a name="deployment-options"></a>Opciones de implementación

Al implementar el acelerador de la solución, hay varias opciones que permiten configurar el proceso de implementación:

| Opción | Valores | DESCRIPCIÓN |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | Una implementación _básica_ está destinada a pruebas y demostraciones. En este tipo de implementación, todos los microservicios se implementan en una única máquina virtual. Una implementación _estándar_ está destinada a producción. En este tipo de implementación, los microservicios se implementan en varias máquinas virtuales. Una implementación _local_ configura un contenedor de Docker para que ejecute los microservicios en la máquina local y usa los servicios de Azure, como Storage y Cosmos DB, en la nube. |
| Tiempo de ejecución | `dotnet`, `java` | Selecciona la implementación del lenguaje de los microservicios. |

Para obtener información sobre cómo usar la implementación local, consulte el artículo sobre la [ejecución local de la solución Supervisión remota](iot-accelerators-remote-monitoring-deploy-local.md).

## <a name="basic-vs-standard-deployments"></a>Implementación básica frente a estándar

### <a name="basic"></a>Básica
La implementación básica está orientada a presentar la solución. Para reducir el costo de esta demostración, todos los microservicios se implementan en una única máquina virtual; esto no se considera una arquitectura lista para entornos de producción.

La opción de implementación estándar debe usarse cuando esté listo para personalizar una arquitectura para entornos de producción, creada para fines de escalabilidad y extensibilidad.

Al crear una solución básica, se aprovisionarán los servicios de Azure siguientes en su suscripción de Azure al costo de: 

| Recuento | Recurso                       | Escriba         | Se usa para |
|-------|--------------------------------|--------------|----------|
| 1     | [Máquina virtual con Linux](https://azure.microsoft.com/services/virtual-machines/) | Estándar D1 v2  | Hospedaje de microservicios |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1: nivel Estándar | Comunicación y administración de dispositivos |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Estándar        | Almacenamiento de datos de configuración, reglas, alarmas y otro tipo de almacenamiento en frío |  
| 1     | [Cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Estándar        | Almacenar puntos de comprobación de máquina virtual y streaming |
| 1     | [Aplicación web](https://azure.microsoft.com/services/app-service/web/)        |                 | Hospedaje de aplicación web front-end |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Administración de identidades y seguridad de usuarios |
| 1     | [Azure Maps](https://azure.microsoft.com/services/azure-maps/)        | Estándar                | Visualización de ubicaciones de recursos |
| 1     | [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)        |   3 unidades              | Habilitación de análisis en tiempo real |
| 1     | [Servicio de Azure Device Provisioning](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Aprovisionamiento de dispositivos a escala |
| 1     | [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/)        |   S1: 1 unidad              | Almacenamiento para datos de mensajes y permite el análisis detallado de telemetría |



### <a name="standard"></a>Estándar
La implementación estándar es una implementación lista para producción que un desarrollador puede personalizar y ampliar para satisfacer sus necesidades. La opción de implementación estándar debe usarse cuando esté listo para personalizar una arquitectura para entornos de producción, creada para fines de escalabilidad y extensibilidad. Los microservicios de aplicación se crean como contenedores de Docker y se implementan con Azure Kubernetes Service (AKS). El orquestador es responsable de la implementación, el escalado y la administración de la aplicación.


Al crear una solución estándar, se aprovisionarán los servicios de Azure siguientes en su suscripción de Azure al costo de:

| Recuento | Recurso                                     | SKU / Tamaño      | Se usa para |
|-------|----------------------------------------------|-----------------|----------|
| 1     | [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service)| Use un servicio de orquestación de contenedores de Kubernetes totalmente administrados, con un valor predeterminado de tres agentes.|
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                     | S2: nivel Estándar | Comando, control y administración de dispositivos |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Estándar        | Almacenar datos de configuración y telemetría de dispositivos como reglas, alarmas y mensajes |
| 5     | [Cuentas de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Estándar        | 4 para el almacenamiento de máquina virtual y 1 para los puntos de comprobación de streaming |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Estándar     | Application Gateway frente a SSL |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Administración de identidades y seguridad de usuarios |
| 1     | [Azure Maps](https://azure.microsoft.com/services/azure-maps/)        | Estándar                | Visualización de ubicaciones de recursos |
| 1     | [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)        |   3 unidades              | Habilitación de análisis en tiempo real |
| 1     | [Servicio de Azure Device Provisioning](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Aprovisionamiento de dispositivos a escala |
| 1     | [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/)        |   S1: 1 unidad              | Almacenamiento para datos de mensajes y permite el análisis detallado de telemetría |

> Puede encontrar más información sobre los precios de estos servicios [aquí](https://azure.microsoft.com/pricing). Las cantidades de uso y los detalles de facturación de la suscripción se pueden encontrar en [Azure Portal](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>Implementación del acelerador de solución

### <a name="example-deploy-net-version"></a>Ejemplo: Implementación de la versión de .NET

En el ejemplo siguiente, se muestra cómo implementar la versión de .NET básica del acelerador de la solución Supervisión remota:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Ejemplo: Implementación de la versión de Java

En el ejemplo siguiente, se muestra cómo implementar la versión de Java estándar del acelerador de la solución Supervisión remota:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Opciones del comando pcs

Al ejecutar el comando `pcs` para implementar una solución, se le pide:

- Un nombre para la solución. Este nombre debe ser único.
- La suscripción de Azure que se va a usar.
- Una ubicación.
- Las credenciales de las máquinas virtuales que hospedan los microservicios. Puede usar estas credenciales para tener acceso a las máquinas virtuales de cara a la solución de problemas.

Cuando el comando `pcs` finaliza, muestra la dirección URL de la nueva implementación del acelerador de la solución. El comando `pcs` también crea un archivo `{deployment-name}-output.json` con información adicional, como el nombre de la instancia de IoT Hub que se aprovisionó para usted.

Para más información sobre los parámetros de la línea de comandos, ejecute:

```cmd/sh
pcs -h
```

Para más información sobre la CLI, consulte [Uso de la CLI](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Configuración del acelerador de la solución
> * Implementación del acelerador de solución
> * Inicio de sesión en el acelerador de la solución

Ahora que ha implementado la solución Supervisión remota, el paso siguiente es [explorar las funcionalidades del panel de soluciones](./quickstart-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->

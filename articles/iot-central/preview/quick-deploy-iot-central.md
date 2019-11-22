---
title: Creación de una aplicación de Azure IoT Central | Microsoft Docs
description: Cree una nueva aplicación de Azure IoT Central. Cree una aplicación de evaluación o de pago por uso mediante una plantilla de aplicación.
author: lmasieri
ms.author: lmasieri
ms.date: 10/24/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: c639cb059d773042b7f45160dea18bfc2130cae9
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896283"
---
# <a name="create-an-azure-iot-central-application-preview-features"></a>Creación de una aplicación de Azure IoT Central (características en versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

En esta guía de inicio rápido se muestra cómo crear una aplicación de Azure IoT Central que contiene características de vista previa como IoT Plug and Play.

> [!WARNING]
> Las funcionalidades de IoT Plug and Play en Azure IoT Central se encuentran actualmente en versión preliminar pública. No use ninguna aplicación de IoT Central preparada para IoT Plug and Play para cargas de trabajo de producción. Para entornos de producción, use una aplicación de IoT Central creada con una plantilla de aplicación actual y disponible con carácter general.

## <a name="create-an-application"></a>Creación de una aplicación

Navegue al sitio de [Compilación Azure IoT Central](https://aka.ms/iotcentral). Después, inicie sesión con una cuenta Microsoft personal, profesional o educativa.

Puede crear una nueva aplicación desde la lista de plantillas de IoT Central relevantes del sector para ayudarle a empezar a trabajar rápidamente, o desde cero con la plantilla de **Aplicaciones personalizada**.

![Página de creación de una aplicación de Azure IoT Central](media/quick-deploy-iot-central/iotcentralcreate-templates-pnp.png)

Para crear una nueva aplicación de Azure IoT Central:

1. Para crear una nueva aplicación de Azure IoT Central a partir de una *Plantilla del sector*, seleccione una plantilla de aplicación en la lista de plantillas disponibles en una de las industrias. También puede empezar desde cero seleccionando *Aplicación personalizada*.
1. Azure IoT Central sugiere automáticamente un **nombre de aplicación** en función de la plantilla de aplicación que ha seleccionado. Puede usar este nombre o escribir su propio nombre descriptivo de la aplicación.
1. Azure IoT Central también genera un único prefijo de **dirección URL de aplicación**, según el nombre de la aplicación. Esta dirección URL se usa para tener acceso a la aplicación. Cambiar si lo desea este prefijo de dirección URL por algo más fácil de recordar.

    ![Página de creación de una aplicación de Azure IoT Central](media/quick-deploy-iot-central/iotcentralcreate-industry-pnp.png)

    > [!NOTE]
    > Si está usando la plantilla de aplicación personalizada, verá un campo desplegable de **Plantilla de aplicación**. Desde aquí puede cambiar entre las plantillas de vista previa y disponible con carácter general. También puede ver otras plantillas que se han puesto a disposición de su organización.

1. Elija si desea crear esta aplicación con una evaluación gratuita de 7 días o use una suscripción de Pago por uso.
    - Las aplicaciones de **prueba** son gratis durante siete días y admiten hasta cinco dispositivos. Se pueden convertir a pago por uso en cualquier momento antes. Si crea una Aplicación de prueba, debe escribir su información de contacto y elegir si desea recibir información y sugerencias de Microsoft.
    - Las aplicaciones de **Pago por uso** se cobran por dispositivo, con los dos primeros dispositivos gratis. Más información sobre los [precios de IoT Central](https://aka.ms/iotcentral-pricing). Si crea una aplicación de Pago por uso, tendrá que seleccionar su *Directorio*, *Suscripción de Azure* y *Región*:
        - El *Directorio* es el Azure Active Directory (AAD) en el que creará la aplicación. Un Azure AD contiene identidades de usuario, credenciales y otra información de la organización. Si no tiene un inquilino de Azure AD, se crea uno automáticamente al crearse una suscripción de Azure.
        - Una *suscripción de Azure* permite crear instancias de los servicios de Azure. IoT Central aprovisiona los recursos de su suscripción. Si no tiene una suscripción de Azure, puede crear una en [la página de suscripción a Azure](https://aka.ms/createazuresubscription). Después de crear la suscripción de Azure, vuelva a la página **Crear aplicación**. La nueva suscripción aparecerá en la lista desplegable **Suscripción de Azure**.
        - *Región* es la ubicación física en la que se almacenarán los datos de los dispositivos. Normalmente, debe elegir la región más cercana a los dispositivos para obtener un rendimiento óptimo y garantizar el cumplimiento de la soberanía de los datos. Una vez que elija una región, no podrá mover la aplicación a otra región más adelante.

        > [!NOTE]
        > Durante la versión preliminar pública, las únicas regiones disponibles para **aplicaciones de vista previa** son **Norte de Europa** y **Centro de EE. UU.** .

1. Revise los Términos y Condiciones y seleccione **Crear** en la parte inferior de la página.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado una aplicación de IoT Central. Este es el siguiente paso que le sugerimos:

> [!div class="nextstepaction"]
> [Agregar un dispositivo simulado a la aplicación IoT Central](./quick-create-pnp-device.md)

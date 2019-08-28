---
title: Administración de una aplicación de Azure IoT Central | Microsoft Docs
description: Como administrador, aprenderá a administrar su aplicación de Azure IoT Central cambiando el nombre de la aplicación, la dirección URL, cargando la imagen y copiando y eliminando una aplicación.
author: viv-liu
ms.author: viviali
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: bdb13ef0d4f05802e4ab0f383c1bf3d82d9ba632
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879360"
---
# <a name="manage-your-iot-central-application"></a>Administración de la aplicación de IoT Central

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

En este artículo se describe cómo, como administrador, puede administrar la aplicación cambiando el nombre de la aplicación y la dirección URL, cargando la imagen, así como aprender a copiar y eliminar una aplicación en la aplicación de Azure IoT Central.

Para tener acceso a la sección **Administración** y poder usarla, debe disponer del rol **Administrador** para la aplicación Azure IoT Central. Si crea una aplicación Azure IoT Central, se le asigna automáticamente el rol **Administrator** para esa aplicación. 

## <a name="change-application-name-and-url"></a>Cambio del nombre y la dirección URL de la aplicación

En la página **Configuración de la aplicación** puede cambiar el nombre y la dirección URL de la aplicación, después, seleccione **Guardar**.

![Página Configuración de la aplicación](media/howto-administer-pnp/image0-a.png)

Si el administrador crea un tema personalizado para la aplicación, esta página incluye una opción para ocultar el **nombre de la aplicación** en la interfaz de usuario. Esto resulta útil si el logotipo de la aplicación en el tema personalizado incluye el nombre de la aplicación. Para más información, consulte [Personalización de la interfaz de usuario de Azure IoT Central](./howto-customize-ui-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

> [!Note]
> Si cambia la dirección URL, la dirección URL anterior la puede tomar otro cliente de Azure IoT Central. En ese caso, usted ya no podrá usarla. Cuando cambie la dirección URL, la dirección antigua ya no funcionará y tendrá que notificar a los usuarios la nueva URL que deben usar.

## <a name="prepare-and-upload-image"></a>Preparación y carga de imágenes

Para cambiar la imagen de la aplicación, consulte [Preparación y carga de imágenes a una aplicación de Azure IoT Central](howto-prepare-images.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="copy-an-application"></a>Copia de una aplicación

Puede crear una copia de cualquier aplicación, menos las instancias de dispositivo, el historial de datos del dispositivo y los datos de usuario. La copia es una aplicación de pago por uso por la que se le cobrará. No se puede crear una aplicación de prueba de esta manera.

Seleccione **Copiar**. En el cuadro de diálogo, escriba los detalles de la nueva aplicación de pago por uso. Después, seleccione **Copiar** para confirmar que quiere continuar. Más información sobre los campos de este formulario en la guía de inicio rápido sobre la [creación de una aplicación](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

![Página Configuración de la aplicación](media/howto-administer-pnp/appcopy2.png)

Una vez que la operación de copia de la aplicación se realiza correctamente, puede navegar a la nueva aplicación mediante el vínculo.

![Página Configuración de la aplicación](media/howto-administer-pnp/appcopy3a.png)

Al copiar una aplicación, también se copia la definición de las reglas y la acción de correo electrónico. Algunas acciones, como Flow, Logic Apps, etc., están asociadas a reglas específicas mediante el identificador de regla. Cuando una regla se copia en otra aplicación, obtiene su propio identificador de regla. En este caso, los usuarios tendrán que crear una nueva acción y, a continuación, asociar la nueva regla a ella. En general, es una buena idea comprobar las reglas y las acciones para garantizar que están actualizadas en la nueva aplicación.

> [!WARNING]
> Si un panel incluye iconos que muestran información acerca de dispositivos específicos, estos iconos muestran el mensaje **No se encontró el recurso solicitado** en la nueva aplicación. Debe volver a configurar estos iconos para mostrar información acerca de los dispositivos en la nueva aplicación.

## <a name="delete-an-application"></a>Eliminar una aplicación

Use el botón **Eliminar** para quitar permanentemente la aplicación de IoT Central. Con esta acción se eliminan de manera permanente todos los datos asociados a la aplicación.

> [!Note]
> Para eliminar una aplicación, también debe tener permisos para eliminar recursos en la suscripción de Azure que haya elegido al crear la aplicación. Para obtener más información, vea el artículo sobre el [uso del control de acceso basado en rol para administrar el acceso a los recursos de la suscripción de Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).


## <a name="manage-programatically"></a>Mediante programación

Hay paquetes de SDK de Azure Resource Manager SDK de IoT Central disponibles para Node, Python, C#, Ruby, Java y Go. Puede usar estos paquetes para crear, enumerar, actualizar o eliminar aplicaciones de IoT Central. Los paquetes incluyen aplicaciones auxiliares para administrar la autenticación y el control de errores.

Encontrará ejemplos de cómo usar los SDK de Azure Resource Manager en [https://github.com/emgarten/iotcentral-arm-sdk-examples](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Para obtener más información, consulte los siguientes repositorios y paquetes de GitHub:

| Idioma | Repositorio | Paquete |
| ---------| ---------- | ------- |
| Nodo | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Go | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Pasos siguientes
 
Ahora que ha aprendido a administrar la aplicación de Azure IoT Central, el siguiente paso sugerido es aprender a [administrar usuarios y roles](howto-manage-users-roles-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) en Azure IoT Central.
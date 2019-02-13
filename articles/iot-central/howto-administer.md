---
title: Administración de una aplicación de Azure IoT Central | Microsoft Docs
description: Como administrador, ¿cómo administra su aplicación de Azure IoT Central?
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: feaedff2cb9f8eae30fd17c497b4a2f6de490b07
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55822602"
---
# <a name="administer-your-iot-central-application"></a>Administración de la aplicación de IoT Central

Después de crear una aplicación de IoT Central, puede ir a la sección de **Administración** para:

- Administrar configuración de la aplicación
- Administrar usuarios
- Administrar roles
- Ver la factura
- Convertir la cuenta de evaluación gratuita en una de pago por uso
- Exportar datos
- Administrar la conexión del dispositivo
- Usar tokens de acceso

Para tener acceso a la sección **Administración** y poder usarla, debe disponer del rol **Administrador** para la aplicación Azure IoT Central. Si crea una aplicación Azure IoT Central, se le asigna automáticamente el rol **Administrator** para esa aplicación. En la sección [Administración de usuarios](#manage-users) de este artículo se explica en profundidad cómo asignar el rol **Administrador** a otros usuarios.

## <a name="manage-application-settings"></a>Administrar configuración de la aplicación

### <a name="change-application-name-and-url"></a>Cambio del nombre y la dirección URL de la aplicación
En la página **Configuración de la aplicación** puede cambiar el nombre y la dirección URL de la aplicación, después, seleccione **Guardar**.

![Página Configuración de la aplicación](media/howto-administer/image0-a.png)

> [!Note]
> Si cambia la dirección URL, la dirección URL anterior la puede tomar otro cliente de Azure IoT Central. En ese caso, usted ya no podrá usarla. Cuando cambie la dirección URL, la dirección antigua ya no funcionará y tendrá que notificar a los usuarios la nueva URL que deben usar.

### <a name="prepare-and-upload-image"></a>Preparación y carga de imágenes
Para cambiar la imagen de la aplicación, consulte [Preparación y carga de imágenes a una aplicación de Azure IoT Central](howto-prepare-images.md).

### <a name="copy-an-application"></a>Copia de una aplicación
Puede crear una copia de cualquier aplicación, menos las instancias de dispositivo, el historial de datos del dispositivo y los datos de usuario. La copia será una aplicación de pago por uso por la que se le cobrará. No se puede crear una aplicación de prueba de esta manera.

Haga clic en el botón **Copiar**. En el cuadro de diálogo, escriba los detalles de la nueva aplicación de pago por uso. Después, haga clic en el botón **Copiar** para confirmar que quiere continuar. Más información sobre los campos de este formulario en la guía de inicio rápido sobre la [creación de una aplicación](quick-deploy-iot-central.md).

![Página Configuración de la aplicación](media/howto-administer/appCopy2.png)

Una vez finalizada correctamente la operación de copia de la aplicación, puede ir a la nueva aplicación que ha creado al copiar la aplicación mediante el vínculo que aparece.

![Página Configuración de la aplicación](media/howto-administer/appCopy3.png)

> [!Note]
> Al copiar una aplicación, también se copia la definición de las reglas y las acciones. Aun así, puesto que los usuarios que tienen acceso a la aplicación original no se copian en la aplicación copiada, tendrá que agregar manualmente los usuarios a las acciones del tipo correo electrónico, para las que los usuarios son un requisito previo. En general, es una buena idea comprobar las reglas y las acciones para garantizar que están actualizadas en la nueva aplicación.

### <a name="delete-an-application"></a>Eliminar una aplicación

> [!Note]
> Para eliminar una aplicación, también debe tener permisos para eliminar recursos en la suscripción de Azure que haya elegido al crear la aplicación. Para obtener más información, vea el artículo sobre el [uso del control de acceso basado en rol para administrar el acceso a los recursos de la suscripción de Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure).

Use el botón **Eliminar** para quitar permanentemente la aplicación de IoT Central. Al hacerlo se eliminan todos los datos asociados con la aplicación.

## <a name="manage-users"></a>Administrar usuarios

### <a name="add-users"></a>Agregar usuarios

Cada usuario debe tener una cuenta de usuario antes de poder iniciar sesión y acceder a la aplicación Azure IoT Central. Se admiten las cuentas de Microsoft (MSA) y las de Azure Active Directory (Azure AD) en Azure IoT Central. Actualmente no se admiten los grupos de Azure Active Directory en Azure IoT Central.

Para más información, consulte la [ayuda de la cuenta de Microsoft](https://support.microsoft.com/products/microsoft-account?category=manage-account) y la [Guía de inicio rápido: Adición de nuevos usuarios a Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).

1. Para agregar un usuario a una aplicación de IoT Central, vaya a la página **Usuarios** de la sección **Administración**.

    ![Lista de usuarios](media/howto-administer/image1.png)

1. Para agregar un usuario, en la página **Usuarios**, seleccione **+ Agregar usuario**.

1. Seleccione un rol para el usuario en el menú desplegable **Rol**. Más información sobre los roles en la sección [Administración de roles](#manage-roles) de este artículo.

    ![Selección de roles](media/howto-administer/image3.png)

    > [!NOTE]
    >  Para agregar usuarios de forma masiva, indique los identificadores de usuario de todos los usuarios que quiera agregar, separados por punto y coma. Seleccione un rol en el menú desplegable **Rol**. Después, seleccione **Guardar**.

### <a name="edit-the-roles-that-are-assigned-to-users"></a>Modificar los roles asignados a usuarios

Los roles no se pueden cambiar una vez asignados. Para cambiar el rol asignado a un usuario, elimine el usuario y agréguelo de nuevo con un rol diferente.

### <a name="delete-users"></a>Eliminación de usuarios

Para eliminar usuarios, seleccione una o más casillas en la página **Usuarios**. A continuación, seleccione **Eliminar**.

## <a name="manage-roles"></a>Administrar roles

Los roles le permiten controlar qué usuarios de la organización pueden realizar varias tareas en IoT Central. Hay tres roles que se pueden asignar a los usuarios de la aplicación. 

### <a name="administrator"></a>Administrador

Los usuarios con el rol **Administrador** tienen acceso a toda la funcionalidad de la aplicación.

El usuario que crea una aplicación se asigna automáticamente al rol **Administrador**. Siempre debe haber al menos un usuario en el rol **Administrador**.

### <a name="application-builder"></a>Generador de aplicaciones

Los usuarios con el rol **Application Builder** (Generador de aplicaciones) pueden hacerlo todo en una aplicación, excepto administrarla. Esto significa que los generadores de pueden crear, editar y eliminar plantillas de dispositivo y dispositivos, administrar conjuntos de dispositivos y ejecutar análisis y trabajos. Los generadores no tienen acceso a la sección **Administración** de la aplicación.

### <a name="application-operator"></a>Operador de aplicaciones

Los usuarios con el rol **Application Operator** (Operador de la aplicación) no puede realizar cambios en las plantillas de dispositivo ni administrar la aplicación. Esto significa que los operadores pueden agregar y eliminar dispositivos, administrar conjuntos de dispositivos y ejecutar análisis y trabajos. Los operadores no tienen acceso a las páginas **Application Builder** (Generador de aplicaciones) ni **Administración**.


## <a name="view-your-bill"></a>Ver la factura

Para ver la factura, vaya a la página **Facturación** en la sección **Administración**. La página de facturación de Azure se abre en una nueva pestaña, en la que puede ver la factura de cada una de las aplicaciones de Azure IoT Central.

### <a name="convert-your-trial-to-pay-as-you-go"></a>Convertir la cuenta de evaluación gratuita en una de pago por uso

Puede convertir la aplicación de prueba en una de pago por uso. Estas son las diferencias entre estos tipos de aplicaciones.

- Las aplicaciones de **versión de evaluación** son gratuitas durante siete días y luego caducan. Se pueden convertir a pago por uso en cualquier momento antes.
- Las aplicaciones de **pago por uso** se cobran por dispositivo y los cinco primeros dispositivos son gratuitos.

Obtenga más información sobre los precios en la [página de precios de Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).
    
Para completar el proceso de autoservicio, siga estos pasos:

1. Vaya a la página **Facturación** de la sección **Administración**. 

    ![Evaluación gratuita](media/howto-administer/freetrialbilling.png)

1. Haga clic en **Convert to Pay-As-You-Go** (Convertir a pago por uso). 

    ![Conversión de la evaluación gratuita](media/howto-administer/convert.png)

1. Seleccione la instancia de Azure Active Directory correspondiente y la suscripción de Azure que se usará con la aplicación de pago por uso.

1. Después de hacer clic en **Convertir**, la aplicación pasará a ser de pago por uso y se comenzará la facturación.

## <a name="export-data"></a>Exportar datos

Puede habilitar la **Exportación continua de datos** para exportar los datos de medidas, dispositivos y plantillas de dispositivo a la cuenta de Azure Blob Storage. Más información sobre la exportación de datos.

## <a name="manage-device-connection"></a>Administrar la conexión del dispositivo

Conecte dispositivos a escala en la aplicación mediante estos certificados y claves. Más información sobre la conexión de dispositivos.

## <a name="use-access-tokens"></a>Usar tokens de acceso

Genere tokens de acceso para usarlos en las herramientas para desarrolladores. Actualmente hay una herramienta para desarrolladores disponible, el explorador de IoT Central para la supervisión de los mensajes de dispositivo, y los cambios en las propiedades y la configuración. Más información sobre el explorador de IoT Central. 

## <a name="use-the-azure-sdks-for-control-plane-operations"></a>Usar Azure SDK para operaciones de plano de control

Hay paquetes de SDK de Azure Resource Manager SDK de IoT Central disponibles para Node, Python, C#, Ruby, Java y Go. Estas bibliotecas admiten operaciones de plano de control para IoT Central, que permiten crear, enumerar, actualizar o eliminar aplicaciones de IoT Central. También proporcionan aplicaciones auxiliares para tratar con la autenticación y el control de errores específico de cada idioma. 

Encontrará ejemplos de cómo usar los SDK de Azure Resource Manager en [https://github.com/emgarten/iotcentral-arm-sdk-examples](https://github.com/emgarten/iotcentral-arm-sdk-examples).

Para obtener más información, eche un vistazo a estos paquetes en GitHub.

| Idioma | Repositorio | Paquete |
| ---------| ---------- | ------- |
| Nodo | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Go | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a administrar una aplicación de Azure IoT Central, este es el paso siguiente sugerido:

> [!div class="nextstepaction"]
> [Configuración de la plantilla de dispositivo](howto-set-up-template.md)

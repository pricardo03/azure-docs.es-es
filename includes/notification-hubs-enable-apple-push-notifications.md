---
title: archivo de inclusión
description: archivo de inclusión
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 08/28/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 3e4549a21ec32f1a2c1c869c3b2e0bd8c2e4204e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446569"
---
## <a name="generate-the-certificate-signing-request-file"></a>Generación del archivo de solicitud de firma de certificado

El servicio Apple Push Notification Service (APNS) utiliza certificados para autenticar las notificaciones push. Siga estas instrucciones para crear el certificado de inserción necesario para enviar y recibir notificaciones. Para obtener más información sobre estos conceptos, consulte la documentación oficial de [Apple Push Notification Service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) (Servicio de notificaciones push de Apple).

Genere el archivo de solicitud de firma de certificado (CSR) que utiliza Apple para generar un certificado push firmado.

1. En el Mac, ejecute la herramienta Acceso a llaves. Se puede activar desde la carpeta **Utilities** (Utilidades) o la carpeta **Other** (Otros) en el panel de inicio.

1. Seleccione **Keychain Access** (Acceso con cadena de claves), expanda **Certificate Assistant** (Asistente para certificados) y, después, seleccione **Request a Certificate from a Certificate Authority** (Solicitar un certificado a una entidad de certificación).

    ![Use Acceso a llaves para solicitar un nuevo certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

1. Seleccione una **dirección de correo electrónico de usuario**, escriba un valor en **Common Name** (Nombre común), asegúrese de que especifica **Saved to disk** (Guardar en disco) y, finalmente, seleccione **Continue** (Continuar). Deje en blanco **CA Email Address** (Dirección de correo de la entidad de certificación), ya que no es obligatorio.

    ![Información obligatoria del certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

1. Escriba un nombre para el archivo CSR en **Save As** (Guardar como), seleccione la ubicación en **Where** (Dónde) y, a continuación, seleccione **Save** (Guardar).

    ![Elección de un nombre de archivo para el certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Esta acción guarda el archivo CSR en la ubicación seleccionada. La ubicación predeterminada es **Escritorio**. Recuerde la ubicación seleccionada para este archivo.

A continuación, se registrará la aplicación en Apple, se habilitarán las notificaciones push y se cargará el archivo CSR exportado para crear un certificado de inserción.

## <a name="register-your-app-for-push-notifications"></a>Registro de la aplicación para notificaciones push

Para insertar notificaciones en una aplicación iOS, es preciso no solo registrar la aplicación en Apple, sino también las notificaciones push.  

1. Si todavía no ha registrado la aplicación, vaya al [portal de aprovisionamiento de iOS](https://go.microsoft.com/fwlink/p/?LinkId=272456) en el centro para desarrolladores de Apple. Después, inicie sesión con su identificador de Apple, seleccione **Identifiers** (Identificadores), seleccione **App IDs** (Id. de aplicaciones) y, por último, seleccione **+** para registrar una nueva aplicación.

    ![Página de identificadores de aplicaciones del portal de aprovisionamiento de iOS](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

1. Actualice los tres campos siguientes de la nueva aplicación y, después, seleccione **Continue** (Continuar):

   * **Nombre**: escriba un nombre descriptivo para la aplicación en el cuadro **Name** (Nombre) de la sección **App ID Description** (Descripción del identificador de la aplicación).

   * **Bundle Identifier** (Identificador de agrupación): en la sección **Explicit App ID** (Identificador de aplicación explícita), escriba un **identificador de paquete** con el formato `<Organization Identifier>.<Product Name>`, tal como se menciona en [App Distribution Guide](https://help.apple.com/xcode/mac/current/#/dev91fe7130a) (Guía de distribución de aplicaciones). El *identificador de organización* y el *nombre del producto* deben coincidir con el identificador de la organización y con el nombre del producto que use al crear el proyecto Xcode. En la captura de pantalla que aparece a continuación, el valor de *NotificationHubs* se usa como identificador de una organización, mientras que el valor *GetStarted* se usa como el nombre del producto. Asegúrese de que el valor del **identificador de paquete** coincide con el valor del proyecto Xcode, para que Xcode utilice el perfil de publicación correcto.

   * **Notificaciones push**: Seleccione la opción **Notificaciones push** en la sección **App Services**.

     ![Formulario para registrar un nuevo identificador de aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

     De esta forma, se genera el identificador de la aplicación y se solicita que confirme la información. Seleccione **Register** (Registrar) para confirmar el nuevo identificador de la aplicación.

     Después de seleccionar **Register** (Registrar), verá la pantalla **Registration complete** (Registro completo), tal como se muestra en la siguiente imagen. Seleccione **Listo**.

     ![Registro de identificador de aplicación completo que muestra los derechos](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)

1. En el centro para desarrolladores, en **App IDs** (Id. de aplicaciones), busque el identificador de la aplicación que ha creado y seleccione su fila.

    ![Lista de identificadores de aplicaciones](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)

    Seleccione el identificador de la aplicación para mostrar los detalles de la aplicación y, después, seleccione el botón **Edit** (Editar) situado en la parte inferior.

    ![Editar página de identificadores de aplicaciones](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

1. Desplácese a la parte inferior de la pantalla y seleccione el botón **Create Certificate** (Crear certificado) en la sección **Development Push SSL Certificate** (Certificado SSL de inserción de desarrollo).

    ![Crear certificado para el botón de identificador de aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Se mostrará ahora el asistente **Add iOS Certificate** (Agregar certificado de iOS).

    > [!NOTE]
    > Este tutorial usa un certificado de desarrollo. Se usa el mismo proceso cuando se registra un certificado de producción. Asegúrese de usa el mismo tipo de certificado cuando envíe notificaciones.

1. Seleccione **Choose File** (Elegir archivo), vaya a la ubicación donde guardó el archivo CSR de la primera tarea y, después, seleccione **Generate** (Generar).

    ![Página de carga de archivos CSR de certificados generados](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

1. Cuando el portal crea el certificado, seleccione el botón **Download** (Descargar) y, a continuación, seleccione **Done** (Listo).

    ![Página de descarga de certificados generados](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    El certificado se descarga y se guarda en el equipo en la carpeta de **descargas**.

    ![Busque el archivo de certificado en la carpeta de descargas](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > De manera predeterminada, el certificado de desarrollo descargado se denomina **aps_development.cer**.

1. Seleccione el certificado push **aps_development.cer** descargado.

    Esta operación instala el certificado nuevo en la cadena de llaves, tal como se muestra en la siguiente imagen:

    ![Lista de certificados de Acceso a llaves en la que aparece el nuevo certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Aunque el nombre del certificado puede ser diferente, llevará el prefijo **Apple Development iOS Push Services** (Servicios push de desarrollo de Apple iOS).

1. En Acceso a llaves, haga clic en el nuevo certificado de inserción que creó, en la categoría **Certificados** . Haga clic en **Export** (Exportar), asigne un nombre al archivo, seleccione el formato **.p12** y, después, haga clic en **Save** (Guardar).

    ![Exportación del certificado en formato p12](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Anote el nombre de archivo y la ubicación del certificado p12 exportado. Se usan para habilitar la autenticación con APNs.

    > [!NOTE]
    > En este tutorial se crea un archivo denominado **QuickStart.p12**. El nombre del archivo y la ubicación pueden ser diferentes.

## <a name="create-a-provisioning-profile-for-the-app"></a>Creación de un perfil de aprovisionamiento para la aplicación

1. En el [portal de aprovisionamiento de iOS](https://go.microsoft.com/fwlink/p/?LinkId=272456), seleccione **Provisioning Profiles** (Perfiles de aprovisionamiento), luego **All** (Todo) y, finalmente, seleccione **+** para crear un perfil. A continuación, verá el asistente para **agregar el perfil de aprovisionamiento de iOS**.

    ![Lista de perfiles de aprovisionamiento](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

1. Seleccione **iOS App Development** (Desarrollo de aplicaciones de iOS) bajo **Development** (Desarrollo) como tipo de perfil de aprovisionamiento y, a continuación, seleccione **Continue** (Continuar).

1. Una vez hecho esto, seleccione el identificador de la aplicación que acaba de crear en la lista desplegable **App ID** (Id. de la aplicación) y, a continuación, seleccione **Continuar**.

    ![Seleccionar el identificador de la aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

1. En la ventana **Select certificates** (Seleccionar certificados), seleccione el certificado de desarrollo habitual que se usó para la firma de código y, a continuación, seleccione **Continue** (Continue). Este certificado no es el certificado push que creó.

    ![Seleccionar el certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)

1. Después, seleccione en los dispositivos que va a usar para la prueba y seleccione **Continue** (Continuar).

    ![Seleccionar los dispositivos](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)

1. Para terminar, seleccione un nombre para el perfil en **Profile Name** (Nombre de perfil) y seleccione **Generate** (Generar).

    ![Elegir el nombre del perfil de aprovisionamiento](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

1. Cuando se crea el nuevo perfil de aprovisionamiento, elija descargarlo e instalarlo en la máquina de desarrollo de Xcode. A continuación, seleccione **Done** (Listo).

    ![Descargar el perfil de aprovisionamiento](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)

## <a name="create-a-notification-hub"></a>Creación de un centro de notificaciones

En esta sección, va a crear un centro de notificaciones y configurar la autenticación con APNs mediante el certificado push .p12 que creó anteriormente. Si desea usar un centro de notificaciones ya creado, puede omitir los pasos hasta el paso 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](notification-hubs-portal-create-new-hub.md)]

## <a name="configure-your-notification-hub-with-apns-information"></a>Configuración de un centro de notificaciones con información de APNs

1. En **Notification Services** (Servicios de notificación), seleccione **Apple (APNS)** .

1. Seleccione **Certificado**.

1. Seleccione el icono del archivo.

1. Seleccione el archivo .p12 que exportó antes.

1. Especifique la contraseña correcta.

1. Seleccione el modo **Espacio aislado**. Use el modo **Producción** solo si desea enviar notificaciones push a los usuarios que compraron la aplicación en la tienda.

    ![Configuración del certificado de APNs en Azure Portal](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-apple-config-cert.png)

Ahora ya ha configurado el centro de notificaciones con APNs. Tiene también la cadena de conexión para registrar la aplicación y enviar notificaciones push.

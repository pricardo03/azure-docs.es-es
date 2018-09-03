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
ms.openlocfilehash: bf73f64cc1e1789b770d7fae5f62e0ec27425263
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42915182"
---
## <a name="generate-the-certificate-signing-request-file"></a>Generación del archivo de solicitud de firma de certificado

El Servicio de notificaciones push de Apple (APNS) usa certificados para autenticar las notificaciones push. Siga estas instrucciones para crear el certificado de inserción necesario para enviar y recibir notificaciones. Para obtener más información sobre estos conceptos, consulte la documentación oficial de [Apple Push Notification Service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) (Servicio de notificaciones push de Apple).

Genere el archivo de solicitud de firma de certificado (CSR) que Apple usa para generar un certificado push firmado.

1. En el Mac, ejecute la herramienta **Acceso a llaves**. Se puede activar desde la carpeta **Utilidades** o la carpeta **Otros** del panel de inicio.
2. Haga clic en **Keychain Access** (Acceso a llaves), expanda **Certificate Assistant**, (Asistente para certificados) y, a continuación, haga clic en **Request a Certificate from a Certificate Authority...** (Solicitar un certificado de una entidad de certificación...).

    ![Use Acceso a llaves para solicitar un nuevo certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Seleccione una **dirección de correo electrónico de usuario** y un nombre común en **Common Name**, asegúrese de que **Saved to disk** (Guardar en disco) está seleccionado y, a continuación, haga clic en **Continue** (Continuar). Deje el campo **CA Email Address** (Dirección de correo de la entidad de certificación) en blanco, ya que no es obligatorio.

    ![Información obligatoria del certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Escriba un nombre para el archivo de solicitud de firma de certificado (CSR) en **Save As** (Guardar como), seleccione la ubicación en **Where** (Dónde) y, a continuación, haga clic en **Save** (Guardar).

    ![Elegir un nombre de archivo para el certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    De esta forma, el archivo CSR se guarda en la ubicación seleccionada. La ubicación predeterminada es el escritorio. Recuerde la ubicación seleccionada para este archivo.

A continuación, se registrará la aplicación en Apple, se habilitarán las notificaciones push y se cargará el archivo CSR exportado para crear un certificado de inserción.

## <a name="register-your-app-for-push-notifications"></a>Registro de la aplicación para notificaciones push

Para poder enviar notificaciones push a una aplicación iOS, debe registrar su aplicación con Apple y también registrar las notificaciones push.  

1. Si aún no ha registrado la aplicación, vaya al [portal de aprovisionamiento de iOS](http://go.microsoft.com/fwlink/p/?LinkId=272456) del Centro para desarrolladores de Apple, inicie sesión con su identificador de Apple, haga clic en **Identifiers** (Identificadores) y, a continuación, en **App IDs** (Identificadores de aplicación). Para finalizar, haga clic en el signo **+** para registrar una nueva aplicación.

    ![Página de identificadores de aplicaciones del portal de aprovisionamiento de iOS](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Actualice los tres campos siguientes para la nueva aplicación y, a continuación, haga clic en **Continue**(Continuar):

    * **Name** (Nombre): escriba un nombre descriptivo para la aplicación en el campo **Name** (Nombre) de la sección **App ID Description** (Descripción del identificador de la aplicación).
    * **Bundle Identifier** (Identificador de paquete): en la sección **Explicit App ID** (Identificador de aplicación explícita), escriba un **identificador de paquete** con el formato `<Organization Identifier>.<Product Name>`, tal como se menciona en la [Guía de distribución de aplicaciones](https://help.apple.com/xcode/mac/current/#/dev91fe7130a). El *identificador de organización* y el *nombre del producto* que use deben coincidir con el identificador de la organización y con el nombre del producto que use al crear el proyecto XCode. En la captura de pantalla que aparece a continuación, *NotificationHubs* se usa como identificador de una organización y *GetStarted* se usa como el nombre del producto. Asegúrese de que estos valores coinciden con los valores que utilizará en el proyecto XCode, ya que así podrá usar el perfil de publicación correcto con XCode.
    * **Notificaciones push**: seleccione la opción **Notificaciones push** en la sección **App Services**.

    ![Formulario para registrar un nuevo identificador de aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

    De esta forma, se genera el identificador de la aplicación y se solicita que confirme la información. Haga clic en **Register** (Registrar) para confirmar el nuevo identificador de aplicación.

    Cuando haga clic en **Register** (Registrar), verá la pantalla **Registration complete** (Registro completado), tal como se muestra en la siguiente imagen. Haga clic en **Done**(Listo).

    ![Registro de identificador de aplicación completo que muestra los derechos](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)

3. En el Centro para desarrolladores, en la sección de identificadores de aplicaciones, busque el identificador de la aplicación que acaba de crear y haga clic en su fila.

    ![Lista de identificadores de aplicaciones](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)

    Al hacer clic en la fila del identificador de la aplicación se muestran los detalles de la misma. Haga clic en el botón **Edit** (Editar) en la parte inferior.

    ![Editar página de identificadores de aplicaciones](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

4. Desplácese a la parte inferior de la pantalla y haga clic en el botón **Create Certificate...** (Crear certificado...) en la sección **Development Push SSL Certificate** (Certificado SSL de inserción de desarrollo).

    ![Crear certificado para el botón de identificador de aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Se mostrará el asistente **Add iOS Certificate** (Agregar certificado de iOS).

    > [!NOTE]
    > Este tutorial usa un certificado de desarrollo. Se usa el mismo proceso cuando se registra un certificado de producción. Asegúrese de usa el mismo tipo de certificado cuando envíe notificaciones.

5. Haga clic en **Choose File** (Elegir archivo), diríjase a la ubicación en la que guardó el archivo CSR que creó en la primera tarea y, a continuación, haga clic en **Generate** (Generar).

    ![Página de carga de archivos CSR de certificados generados](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

6. Una vez que el portal haya creado el certificado, haga clic en el botón **Download** (Descargar) y en **Done** (Listo).

    ![Página de descarga de certificados generados](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    De esta forma, se descarga el certificado y se guarda en el equipo en la carpeta de **Descargas**.

    ![Busque el archivo de certificado en la carpeta de descargas](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > De manera predeterminada, el certificado de desarrollo del archivo descargado se llama **aps_development.cer**.

7. Haga doble clic en el certificado de inserción **aps_development.cer** descargado.

    Esta operación instala el certificado nuevo en la cadena de llaves, tal como se muestra en la siguiente imagen:

    ![Lista de certificados de Acceso a llaves en la que aparece el nuevo certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > El nombre del certificado puede ser diferente, pero tendrá el prefijo de **Apple Development iOS Push Services (Servicios inserción de desarrollo de Apple iOS)**.

8. En Acceso a llaves, haga clic en el nuevo certificado de inserción que creó, en la categoría **Certificados** . Haga clic en **Exportar**, asigne un nombre al archivo, seleccione el formato **.p12** y, luego, haga clic en **Guardar**.

    ![Exportación del certificado en formato p12](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Anote el nombre de archivo y la ubicación del certificado p12 exportado. Se usará para habilitar la autenticación con APNS.

    > [!NOTE]
    > Con este tutorial se crea un archivo QuickStart.p12. El nombre del archivo y la ubicación pueden ser diferentes.

## <a name="create-a-provisioning-profile-for-the-app"></a>Creación de un perfil de aprovisionamiento para la aplicación

1. Vuelva al [Portal de aprovisionamiento de iOS](http://go.microsoft.com/fwlink/p/?LinkId=272456), seleccione **Provisioning Profiles** (Perfiles de aprovisionamiento), seleccione **All** (Todo) y, a continuación, haga clic en el botón **+** (signo más) para crear un nuevo perfil. A continuación, verá el asistente para **agregar el perfil de aprovisionamiento de iOS**:

    ![Lista de perfiles de aprovisionamiento](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. Seleccione **iOS App Development** (Desarrollo de aplicaciones de iOS) bajo **Development** (Desarrollo) como tipo de perfil de aprovisionamiento y, a continuación, haga clic en **Continue** (Continuar).

3. Una vez hecho esto, seleccione el id. de aplicación que acaba de crear desde la lista desplegable **Identificador de aplicación** y, a continuación, haga clic en **Continuar**.

    ![Seleccionar el identificador de la aplicación](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

4. En la pantalla **Seleccionar certificados**, seleccione el certificado de desarrollo usual que se usó para firma de código y, a continuación, haga clic en **Continuar**. Este certificado no es el certificado push que creó.

    ![Seleccionar el certificado](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)

5. A continuación, seleccione en **Devices** los dispositivos que usará para la prueba y haga clic en **Continue** (Continuar).

    ![Seleccionar los dispositivos](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)

6. Para terminar, seleccione un nombre para el perfil en **Nombre de perfil** y haga clic en **Generar**.

    ![Elegir el nombre del perfil de aprovisionamiento](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

7. Cuando se crea el nuevo perfil de aprovisionamiento, haga clic aquí para descargarlo e instalarlo en el equipo de desarrollo de Xcode. A continuación, haga clic en **Hecho**.

    ![Descargar el perfil de aprovisionamiento](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)

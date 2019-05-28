---
title: Envío de notificaciones push a aplicaciones Kindle mediante Azure Notification Hubs | Microsoft Docs
description: En este tutorial, obtenga información acerca de cómo usar Azure Notification Hubs para enviar notificaciones push a una aplicación Kindle.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 346fc8e5-294b-4e4f-9f27-7a82d9626e93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-kindle
ms.devlang: Java
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/29/2019
ms.author: jowargo
ms.openlocfilehash: edd0e12460e07cfd2990cc43a9056ed06b84fb1d
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926701"
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Introducción a Notification Hubs para aplicaciones Kindle

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Este tutorial muestra cómo puede usar Azure Notification Hubs para enviar notificaciones push a una aplicación Kindle. Cree una aplicación Kindle vacía que reciba notificaciones push mediante Amazon Device Messaging (ADM).

En este tutorial, se crea o actualiza código para realizar las tareas siguientes:

> [!div class="checklist"]
> * Incorporación de una aplicación nueva al portal para desarrolladores
> * Creación de una clave de API
> * Creación y configuración de un centro de notificaciones
> * Configuración de la aplicación
> * Creación del controlador de mensajes de ADM
> * Incorporación de clave de API a la aplicación
> * Ejecución de la aplicación
> * Envío de una notificación de prueba

## <a name="prerequisites"></a>Requisitos previos

- [Android Studio](https://developer.android.com/studio/?gclid=CjwKCAjwwZrmBRA7EiwA4iMzBPZ9YYmR0pbb5LtjnWhWCxe8PWrmjmeaR6ad5ksCr_j2mmkVj_-o6hoCAqwQAvD_BwE).
- Siga los pasos que se indican en [Configuración de su entorno de desarrollo](https://developer.amazon.com/docs/fire-tablets/ft-set-up-your-development-environment.html) para configurar el entorno de desarrollo para Kindle.

## <a name="add-a-new-app-to-the-developer-portal"></a>Incorporación de una aplicación nueva al portal para desarrolladores

1. Inicie sesión en el [portal de desarrolladores de Amazon](https://developer.amazon.com/apps-and-games/console/apps/list.html).
2. Seleccione **Agregar nueva aplicación** y seleccione **Android**.  

    ![Botón Agregar nueva aplicación](./media/notification-hubs-kindle-get-started/add-new-app-button.png)
1. En la página **New App Submission** (Enviar nueva aplicación), siga estos pasos para obtener la **clave de aplicación**:
    1. Escriba un nombre para **App title** (Nombre de la aplicación).
    2. Seleccione cualquier **categoría** (por ejemplo, educación)
    4. Escriba una dirección de correo electrónico para el campo **Customer support email address** (Dirección de correo electrónico de soporte al cliente). 
    5. Seleccione **Guardar**.

        ![Página New App Submission (Enviar nueva aplicación)](./media/notification-hubs-kindle-get-started/new-app-submission-page.png) 
2.  En la parte superior, cambie a la pestaña **Mobile Ads** (Anuncios móviles) y realice los siguientes pasos: 
    1. Especifique si la aplicación está dirigida principalmente a menores de 13 años. Para este tutorial, seleccione **No**.
    2. Seleccione **Submit** (Enviar). 

        ![Página Mobile Ads (Anuncios móviles)](./media/notification-hubs-kindle-get-started/mobile-ads-page.png)
    3. Copie la **clave de aplicación** desde la página **Mobile Ads** (Anuncios móviles). 

        ![Clave de la aplicación](./media/notification-hubs-kindle-get-started/application-key.png)
3.  Seleccione el menú **Apps & Services** (Aplicaciones y servicios) de la parte superior y la aplicación en la lista. 

    ![Selección de la aplicación de la lista](./media/notification-hubs-kindle-get-started/all-apps-select.png)
4. Cambie a la pestaña **Device Messaging** (Mensajes de dispositivo) y siga estos pasos: 
    1. Seleccione **Create a New Security Profile** (Crear perfil de seguridad).
    2. Escriba un **nombre** para el perfil de seguridad. 
    3. Escriba una **descripción** para el perfil de seguridad. 
    4. Seleccione **Guardar**. 
    5. Seleccione **View Security Profile** (Ver perfil de seguridad) en la página de resultados. 
5. Ahora, en la página **Security Profile** (Perfil de seguridad), realice los siguientes pasos: 
    1. Cambie a la pestaña **Web Settings** (Configuración web) y copie los valores de **Client ID** (Id. de cliente) y **Client Secret** (Secreto de cliente) para usarlos después. 

        ![Obtención del identificador y el secreto de cliente](./media/notification-hubs-kindle-get-started/client-id-secret.png) 
    2. Cambie a la página **Android/Kindle Settings** (Configuración de Android/Kindle) y manténgala abierta. Deberá escribir estos valores en la sección siguiente. 

## <a name="create-an-api-key"></a>Creación de una clave de API
1. Abra un símbolo del sistema con privilegios de administrador.
2. Vaya a la carpeta del SDK de Android.
3. Escriba el comando siguiente:

    ```shell
    keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
    ```
4. Para la contraseña de **keystore**, escriba **android**.
5. Copie las huellas digitales de **MD5** y **SHA256**. 
6. De nuevo en el portal para desarrolladores, en la pestaña **Android/Kindle Settings** (Configuración de Android/Kindle), realice los siguientes pasos: 
    1. Escriba un **nombre para la clave de API**. 
    2. Escriba el **nombre del paquete** para la aplicación (por ejemplo, **com.fabrikam.mykindleapp**) y el valor de **MD5**.
        
        >[!IMPORTANT]
        > Al crear una aplicación en Android Studio, use el mismo nombre de paquete especificado aquí. 
    1. Pegue la **firma MD5** que copió anteriormente. 
    2. Pegue la **firma SHA256** que copió anteriormente.  
    3. Seleccione **Generate New Key** (Generar nueva clave).

        ![Configuración de Android/Kindle: generar la clave](./media/notification-hubs-kindle-get-started/android-kindle-settings.png)
    4. Ahora, seleccione **Show** (Mostrar) en la lista para ver la clave de API. 

        ![Configuración de Android/Kindle: mostrar la clave de API](./media/notification-hubs-kindle-get-started/show-api-key-button.png) 
    5. En la ventana de **detalles de la clave de API**, copie la clave de API y guárdela en algún lugar. Seleccione **X** en la esquina superior derecha para cerrar la ventana. 


## <a name="create-and-configure-a-notification-hub"></a>Creación y configuración de un centro de notificaciones

1. Siga los pasos del artículo [Create an Azure notification hub in the Azure portal](create-notification-hub-portal.md) (Creación de un centro de notificaciones de Azure en Azure Portal) para crear un centro de notificaciones. 
2. Seleccione **Amazon (ADM)** en el menú **Settings** (Configuración).
3. Pegue los valores de **Client ID** (Id. de cliente) y **Client Secret** (Secreto de cliente) que obtuvo anteriormente. 
4. Seleccione **Guardar** en la barra de herramientas. 

    ![Configuración de los valores de ADM para un centro de notificaciones](./media/notification-hubs-kindle-get-started/configure-notification-hub.png)
5. Seleccione **Access Policies** (Directivas de acceso) en el menú de la izquierda y el botón de **copia** de la cadena de conexión para directiva **DefaultListenSharedAccessSignature**. Guárdela en algún lugar. Lo usará más adelante en el código fuente. 

    ![Centro de notificaciones: escuchar la cadena de conexión](./media/notification-hubs-kindle-get-started/event-hub-listen-connection-string.png)    

## <a name="set-up-your-application"></a>Configuración de la aplicación

1. Inicie Android Studio. 
2. Seleccione sucesivamente **File** (Archivo), **New** (Nuevo) y **New Project** (Nuevo proyecto). 
3. En la ventana **Choose your project** (Elija su proyecto), seleccione **Empty Activity** (Actividad vacía) y **Next** (Siguiente) en la pestaña **Phone and Tablet** (Teléfono y tableta). 
4. En la ventana **Configure your project** (Configurar el proyecto), siga estos pasos:
    1. Escriba un **nombre para la aplicación**. Puede coincidir con el nombre de la aplicación que creó en el portal para desarrolladores de Amazon. 
    2. Escriba un **nombre para el paquete**. 
        
        >[!IMPORTANT]
        >El nombre del paquete debe coincidir con el que especificó en el portal para desarrolladores de Amazon.
    3. Revise y actualice los valores restantes según corresponda. 
    4. Seleccione **Finalizar**. 

        ![Configuración del proyecto de Android](./media/notification-hubs-kindle-get-started/new-android-studio-project.png)
5. Descargue la biblioteca de [SDK para desarrolladores de Amazon para Android](https://developer.amazon.com/sdk-download) en el disco duro. Extraiga el archivo ZIP del SDK.
6. En Android Studio, cambie la estructura de carpetas de **Android** a **Project** (Proyecto) si no está establecido en **Project** (Proyecto). 

    ![Android Studio: cambiar a la estructura del proyecto](./media/notification-hubs-kindle-get-started/android-studio-project-view.png)
7. Expanda la **aplicación** para ver la carpeta **libs** en la vista de árbol.     
8. En una ventana del explorador de archivos, vaya a la carpeta donde descargó los SDK de Android para Amazon.
9. Presione **CTRL**, y arrastre y coloque el archivo **amazon-device-messaging-1.0.1.jar** en el nodo **lib** en la vista de árbol. 

    ![Android Studio: agregar Amazon Device Messaging JAR](./media/notification-hubs-kindle-get-started/drag-drop-amazon-device-messaging-jar.png)
9. En la ventana de **copia**, seleccione **OK** (Aceptar). Si ve la ventana **Move** (Mover) en lugar de la de **copia**, ciérrela e intente la operación de arrastrar y colocar con el botón **CTRL** presionado. 

    ![Android Studio: copiar JAR](./media/notification-hubs-kindle-get-started/copy-jar-window.png)
10. Agregue la siguiente instrucción al archivo **build.gradle de la aplicación** de la sección de **dependencias**: `implementation files('libs/amazon-device-messaging-1.0.1.jar')`. 

    ![Android Studio: agregar ADM a build.gradle de la aplicación](./media/notification-hubs-kindle-get-started/adm-build-gradle.png)
11. En el archivo `Build.Gradle` de la **aplicación**, agregue las siguientes líneas en la sección de **dependencias**: 

    ```gradle
    implementation 'com.microsoft.azure:notification-hubs-android-sdk:0.6@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```
12. Agregue el siguiente repositorio **después** de la sección de **dependencias**:

    ```gradle
    repositories {
        maven {
            url "https://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```
13. En el editor para el archivo **build.gradle** para la **aplicación**, seleccione **Sync now** (Sincronizar ahora) en la barra de herramientas. 

    ![Android Studio: sincronizar build.gradle de la aplicación](./media/notification-hubs-kindle-get-started/gradle-sync-now.png)
14. Cambie a la estructura de Android anterior en la vista de árbol.  Agregue el espacio de nombres de Amazon al elemento del manifiesto raíz:

    ```xml
    xmlns:amazon="http://schemas.amazon.com/apk/res/android"
    ```
   
    ![Espacio de nombres de Amazon en el manifiesto](./media/notification-hubs-kindle-get-started/amazon-namespace-manifest.png)
2. Agregue permisos como el primer elemento del manifiesto. Reemplace **[NOMBRE DEL PAQUETE]** por el paquete usado para crear la aplicación.

    ```xml
    <permission
        android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
        android:protectionLevel="signature" />

    <uses-permission android:name="android.permission.INTERNET"/>

    <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

    <!-- This permission allows your app access to receive push notifications
    from ADM. -->
    <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

    <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    ```
3. Inserte el siguiente elemento como el primer elemento secundario de la aplicación. Reemplace **[NOMBRE DEL PAQUETE]** por el nombre del paquete usado para crear la aplicación. La clase MyADMMessageHandler se creará en el paso siguiente. 

    ```xml
        <amazon:enable-feature
            android:name="com.amazon.device.messaging"
            android:required="true"/>
        <service
            android:name="[YOUR PACKAGE NAME].MyADMMessageHandler"
            android:exported="false" />
        <receiver
            android:name="[YOUR PACKAGE NAME].MyADMMessageHandler$Receiver"
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
                <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
                <!-- Replace the name in the category tag with your app's package name. -->
                <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>
    ```

## <a name="create-your-adm-message-handler"></a>Creación del controlador de mensajes de ADM

1. Agregue una clase nueva al paquete `com.fabrikam.mykindleapp` en el proyecto que herede de `com.amazon.device.messaging.ADMMessageHandlerBase` y dele el nombre `MyADMMessageHandler`, como se muestra en la siguiente imagen:

    ![Creación de la clase MyADMMessageHandler](./media/notification-hubs-kindle-get-started/create-adm-message-handler.png)

2. Agregue las siguientes instrucciones `import` a la clase `MyADMMessageHandler`:

    ```java
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.support.v4.app.NotificationCompat;
    import android.util.Log;
    import com.amazon.device.messaging.ADMMessageReceiver;
    import com.microsoft.windowsazure.messaging.NotificationHub;
    ```

3. Agregue el código siguiente a la clase que ha creado. Recuerde `[HUB NAME]` y `[LISTEN CONNECTION STRING]` con el nombre del centro de notificaciones y escuche la cadena de conexión: 

    ```java
    public static final int NOTIFICATION_ID = 1;
    private NotificationManager mNotificationManager;
    NotificationCompat.Builder builder;
    private static NotificationHub hub;

    public static NotificationHub getNotificationHub(Context context) {
        Log.v("com.wa.hellokindlefire", "getNotificationHub");
        if (hub == null) {
            hub = new NotificationHub("[HUB NAME]", "[HUB NAMESPACE CONNECTION STRING]", context);
        }
        return hub;
    }

    public MyADMMessageHandler() {
        super("MyADMMessageHandler");
    }

    @Override
    protected void onMessage(Intent intent) {
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
    }

    @Override
    protected void onRegistrationError(String s) {

    }

    @Override
    protected void onRegistered(String s) {
        try {
            getNotificationHub(getApplicationContext()).register(s);
        } catch (Exception e) {
            Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
        }
    }

    @Override
    protected void onUnregistered(String s) {
        try {
            getNotificationHub(getApplicationContext()).unregister();
        } catch (Exception e) {
            Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
        }
    }

    public static class Receiver extends ADMMessageReceiver
    {
        public Receiver()
        {
            super(MyADMMessageHandler.class);
        }
    }

    private void sendNotification(String msg) {
        Context ctx = getApplicationContext();

        mNotificationManager = (NotificationManager)
                ctx.getSystemService(Context.NOTIFICATION_SERVICE);

        PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);


        NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                        .setSmallIcon(R.mipmap.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle()
                                .bigText(msg))
                        .setContentText(msg);

        mBuilder.setContentIntent(contentIntent);
        mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
    }
    ```

## <a name="add-your-api-key-to-your-app"></a>Incorporación de clave de API a la aplicación
1. Siga estos pasos para agregar una carpeta de recursos al proyecto. 
    1. Cambie a la vista **Project** (Proyecto). 
    2. Haga clic con el botón derecho en la **aplicación**.
    3. Seleccione **Nuevo**.
    4. Seleccione **Folder** (Carpeta). 
    5. A continuación, seleccione **Assets Folder** (Carpeta de recursos). 

        ![Incorporación de la carpeta de recursos](./media/notification-hubs-kindle-get-started/add-assets-folder-menu.png)    
    6. En la página **Configure Component** (Configurar componente), siga estos pasos:
        1. Seleccione Aceptar para **Change folder location** (Cambiar ubicación de carpeta).
        2. Confirme que la carpeta esté establecida en: `src/main/assets`.
        3. Seleccione **Finalizar**. 
        
            ![Configuración de la carpeta de recursos](./media/notification-hubs-kindle-get-started/configure-asset-folder.png)
2. Agregue un archivo denominado **api_key.txt** a la carpeta **assets** (recursos). En la vista de árbol, expanda la **aplicación**, **src**, **main** (principal) y haga clic con el botón derecho en **assets** (recursos); apunte a **New** (Nuevo) y seleccione **File** (Archivo). Escriba **api_key.txt** como nombre de archivo. 3. 
5. Copie la clave de API generada en el portal para desarrolladores de Amazon en el archivo api_key.txt. 
6. Compile el proyecto. 

## <a name="run-the-app"></a>Ejecución de la aplicación
1. En el dispositivo Kindle, desplácese desde la parte superior y haga clic en **Settings** (Configuración) y luego en **My account** (Mi cuenta), y regístrese con una cuenta de Amazon válida.
2. Ejecute la aplicación en un dispositivo Kindle desde Android Studio. 

> [!NOTE]
> Si se produce un error, compruebe el tiempo del emulador (o del dispositivo). El valor de tiempo debe ser preciso. Para cambiar el tiempo del emulador de Kindle, puede ejecutar el comando siguiente desde el directorio de herramientas de la plataforma del SDK de Android:

```shell
adb shell  date -s "yyyymmdd.hhmmss"
```

## <a name="send-a-notification-message"></a>Envío de un mensaje de notificación

Para enviar un mensaje con .NET:

```csharp
static void Main(string[] args)
{
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

    hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
}
```

Como código de ejemplo, consulte [este ejemplo en GitHub](https://github.com/Azure/azure-notificationhubs-dotnet/blob/master/Samples/SendPushSample/SendPushSample/Program.cs).

![][7]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se envían las notificaciones de difusión a todos los dispositivos Kindle registrados con el back-end. Para aprender a enviar notificaciones push a dispositivos Kindle específicos, pase al siguiente tutorial:  El tutorial siguiente muestra cómo enviar notificaciones push a dispositivos Android concretos, pero puede usar la misma lógica para enviar notificaciones push a dispositivos Kindle concretos.

> [!div class="nextstepaction"]
>[Envío de notificaciones push a dispositivos específicos](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- URLs. -->
[Amazon developer portal]: https://developer.amazon.com/home.html
[download the SDK]: https://developer.amazon.com/public/resources/development-tools/sdk
[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png

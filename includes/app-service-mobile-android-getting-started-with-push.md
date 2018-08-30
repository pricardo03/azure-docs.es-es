---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 654bc3745768fccea41d7c3991142bf7183b54be
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42809700"
---
1. En su proyecto de **aplicación**, abra el archivo `AndroidManifest.xml`. Agregue el siguiente código después de la etiqueta de apertura `application` :

    ```xml
    <service android:name=".ToDoMessagingService">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT"/>
        </intent-filter>
    </service>
    <service android:name=".ToDoInstanceIdService">
        <intent-filter>
            <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
        </intent-filter>
    </service>
    ```

2. Abra el archivo `ToDoActivity.java` y realice los siguientes cambios:

    - Agregue la instrucción import:

        ```java
        import com.google.firebase.iid.FirebaseInstanceId;
        ```

    - Cambie la definición de `MobileServiceClient` de **privado** a **público estático**; ahora tendrá el siguiente aspecto:

        ```java
        private static MobileServiceClient mClient;
        ```

    - Agregue el método `registerPush`:

        ```java
        public static void registerPush() {
            final String token = FirebaseInstanceId.getInstance().getToken();
            if (token != null) {
                new AsyncTask<Void, Void, Void>() {
                    protected Void doInBackground(Void... params) {
                        mClient.getPush().register(token);
                        return null;
                    }
                }.execute();
            }
        }
        ```

    - Actualice el método **onCreate** de la clase `ToDoActivity`. Asegúrese de agregar este código después de crear una instancia de `MobileServiceClient`.

        ```java
        registerPush();
        ```

3. Agregue una nueva clase para controlar las notificaciones. En el Explorador de proyectos, abra los nodos **app** > **java** > **your-project-namespace** y haga clic con el botón derecho en el nodo del nombre del paquete. Haga clic en **Nuevo** y en **Clase Java**. En Nombre escriba `ToDoMessagingService` y, después, haga clic en Aceptar. A continuación, reemplace la declaración de clase por:

    ```java
    import android.app.Notification;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;

    import com.google.firebase.messaging.FirebaseMessagingService;
    import com.google.firebase.messaging.RemoteMessage;

    public class ToDoMessagingService extends FirebaseMessagingService {

        private static final int NOTIFICATION_ID = 1;

        @Override
        public void onMessageReceived(RemoteMessage remoteMessage) {
            String message = remoteMessage.getData().get("message");
            if (message != null) {
                sendNotification("Notification Hub Demo", message);
            }
        }

        private void sendNotification(String title, String messageBody) {
            PendingIntent contentIntent = PendingIntent.getActivity(this, 0, new Intent(this, ToDoActivity.class), 0);
            Notification.Builder notificationBuilder = new Notification.Builder(this)
                    .setSmallIcon(R.drawable.ic_launcher)
                    .setContentTitle(title)
                    .setContentText(messageBody)
                    .setContentIntent(contentIntent);
            NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
            if (notificationManager != null) {
                notificationManager.notify(NOTIFICATION_ID, notificationBuilder.build());
            }
        }
    }
    ```

4. Agregue otra clase para controlar las actualizaciones del token. Cree la clase Java `ToDoInstanceIdService` y reemplace la declaración de clase por:

    ```java
    import com.google.firebase.iid.FirebaseInstanceIdService;

    public class ToDoInstanceIdService extends FirebaseInstanceIdService {

        @Override
        public void onTokenRefresh() {
            ToDoActivity.registerPush();
        }
    }
    ```

Ahora su aplicación está actualizada para que sea compatible con las notificaciones push.

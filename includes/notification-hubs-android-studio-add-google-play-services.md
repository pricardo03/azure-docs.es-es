---
title: archivo de inclusión
description: archivo de inclusión
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 02/05/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: fb27386881e89cd9056d0efccb7d3c301867bd83
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66156817"
---
1. En **Android Studio**, seleccione **Tools** (Herramientas) en el menú y seleccione **SDK Manager**. 
2. Seleccione la versión de destino de Android SDK que se utiliza en el proyecto y seleccione **Show Package Details** (Mostrar detalles de paquete). 

    ![Android SDK Manager: selección de la versión de destino](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Seleccione **Google APIs**, si no están ya instaladas.

    ![Android SDK Manager: Google APIs seleccionado](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. Cambie a la pestaña **SDK Tools**. Si no ha instalado todavía el servicio Google Play, seleccione **Google Play Services**, tal como se muestra en la imagen siguiente. A continuación, haga clic en **Apply** (Aplicar) para instalarlo. Tome nota de la ruta de acceso del SDK para usarla en el paso siguiente.

    ![Android SDK Manager: Google Play Services seleccionado](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. Si ve el cuadro de diálogo **Confirm Change** (Confirmar cambio), seleccione **OK** (Aceptar). El instalador de componentes se instala los componentes solicitados. Seleccione **Finish** (Finalizar) una vez instalados los componentes.
4. Seleccione **OK** (Aceptar) para cerrar el cuadro de diálogo **Settings for New Projects** (Configuración para nuevos proyectos).  
5. Abra el archivo `build.gradle` en el directorio **app** y agregue esta línea en `dependencies`. 

    ```gradle
    implementation 'com.google.android.gms:play-services-gcm:16.0.0'
    ```
5. Seleccione el icono **Sync Now** (Sincronizar ahora) en la barra de herramientas.

    ![Sincronización con Gradle](./media/notification-hubs-android-studio-add-google-play-services/gradle-sync.png)
1. Abra **AndroidManifest.xml** y agregue esta etiqueta a la etiqueta *application* .

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```

---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 5518036d34a493558458673202e52e4559e49573
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843473"
---
## <a name="set-up-your-project"></a>Configurar su proyecto

¿Quiere descargar este proyecto de Android Studio de ejemplo en su lugar? [Descargue un proyecto](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) y vaya al [paso de configuración](#register-your-application) para configurar el código de ejemplo antes de ejecutarlo.

### <a name="create-a-new-project"></a>Creación de un nuevo proyecto 
1.  Abra Android Studio y seleccione **File** > **New** > **New Project** (Archivo > Nuevo > Nuevo proyecto).
2.  Dé un nombre a la aplicación y seleccione **Next** (Siguiente).
3.  Asegúrese de seleccionar **API 21 o posterior (Android 5.0)** y haga clic en **Next** (Siguiente).
4.  Deje **Empty Activity** (Actividad vacía) tal como está, seleccione **Next** (Siguiente) y después **Finish** (Finalizar).


### <a name="add-msal-to-your-project"></a>Adición de MSAL al proyecto
1.  En Android Studio, seleccione **Gradle Scripts** > **build.gradle (Module: app)**.
2.  En **Dependencies** (Dependencias), pegue el código siguiente:

    ```gradle  
    compile ('com.microsoft.identity.client:msal:0.1.+') {
        exclude group: 'com.android.support', module: 'appcompat-v7'
    }
    compile 'com.android.volley:volley:1.0.0'
    ```

<!--start-collapse-->
### <a name="about-this-package"></a>Acerca de este paquete

El paquete del código anterior instala la biblioteca de autenticación de Microsoft. MSAL controla todas las operaciones de token incluida la adquisición, el almacenamiento en caché, la actualización y la eliminación.  Los tokens son necesarios para acceder a las API protegidas por la plataforma de identidad de Microsoft.
<!--end-collapse-->

## <a name="create-the-apps-ui"></a>Creación de la interfaz de usuario de la aplicación

1. Vaya a **res** > **layout** y abra **activity_main.xml**. 
2. Cambie el diseño de la actividad de `android.support.constraint.ConstraintLayout` u otro a `LinearLayout`.
3. Agregue la propiedad `android:orientation="vertical"` al nodo `LinearLayout`.
4. Pegue el código siguiente en el nodo `LinearLayout`, reemplazando el contenido actual:

    ```xml
    <TextView
        android:text="Welcome, "
        android:textColor="#3f3f3f"
        android:textSize="50px"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10dp"
        android:layout_marginTop="15dp"
        android:id="@+id/welcome"
        android:visibility="invisible"/>

    <Button
        android:id="@+id/callGraph"
        android:text="Call Microsoft Graph"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="200dp"
        android:textAllCaps="false" />

    <TextView
        android:text="Getting Graph Data..."
        android:textColor="#3f3f3f"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:id="@+id/graphData"
        android:visibility="invisible"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dip"
        android:layout_weight="1"
        android:gravity="center|bottom"
        android:orientation="vertical" >

        <Button
            android:text="Sign Out"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="15dp"
            android:textColor="#FFFFFF"
            android:background="#00a1f1"
            android:textAllCaps="false"
            android:id="@+id/clearCache"
            android:visibility="invisible" />
    </LinearLayout>
    ```

---
title: Uso del control de mapa de Android para Azure Maps | Microsoft Docs
description: Use el control de mapa de Android para Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 57cc585d621c71872a4b7658c74f581c8998b245
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2019
ms.locfileid: "56341086"
---
# <a name="how-to-use-azure-maps-android-sdk"></a>Uso de Android SDK para Azure Maps

Android SDK para Azure Maps es una biblioteca de mapas vectoriales para Android. En este artículo se ofrece orientación sobre el proceso para instalar Android SDK para Azure Maps, cargar un mapa y colocar un marcador en él.

## <a name="prerequisites-to-get-started"></a>Requisitos previos para empezar a trabajar

### <a name="create-an-azure-maps-account"></a>Crear una cuenta de Azure Maps 

Para seguir los pasos de esta guía, primero necesita consultar el artículo sobre cómo [administrar la cuenta y las claves](how-to-manage-account-keys.md) para crear y administrar la suscripción de la cuenta con el plan de tarifa S1.

### <a name="download-android-studio"></a>Descargar Android Studio

Puede descargar [Android Studio](https://developer.android.com/studio/) gratis desde Google. Para instalar Android SDK para Azure Maps, primero debe descargar Android Studio y crear un proyecto con una actividad vacía.

## <a name="create-a-project-in-android-studio"></a>Creación de un proyecto en Android Studio

Deberá crear un proyecto con una actividad vacía. Siga los pasos descritos a continuación para crear un proyecto en Android Studio:

1. En *Choose your project* (Elegir proyecto), marque "Phone and Tablet" (Teléfono y tableta) como el factor de forma en el que se ejecutará la aplicación.
2. Haga clic en *Empty Activity* (Actividad vacía) en el factor de forma y haga clic en **Next** (Siguiente).
3. En *Configure your project* (Configurar proyecto), seleccione `API 21: Android 5.0.0 (Lollipop)` como el SDK mínimo. Se trata de la versión mínima compatible con Android SDK para Azure Maps.
4. Acepte los valores predeterminados `Activity Name` y `Layout Name` y haga clic en **Finish** (Finalizar).

Consulte la [documentación de Android Studio](https://developer.android.com/studio/intro/) a fin de obtener más ayuda para la instalación de Android Studio y la creación de un proyecto.

![Crear un proyecto](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Configuración de un dispositivo virtual

Android Studio permite configurar un dispositivo virtual Android en el equipo. Lo que puede ayudar a probar la aplicación mientras la desarrolla. Para configurar un dispositivo virtual, haga clic en el icono del administrador de dispositivos virtuales Android (AVD) en la parte superior derecha de la pantalla del proyecto. A continuación, haga clic en el botón **Crear un dispositivo virtual**. También puede abrir el administrador en **Herramientas > Android > AVD Manager** en la barra de herramientas. En la categoría **Phones** (Teléfonos), seleccione **Nexus 5X** y haga clic en **Next** (Siguiente).

Obtenga más información sobre cómo configurar un AVD en la [documentación de Android Studio](https://developer.android.com/studio/run/managing-avds).

![Android Emulator](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-azure-maps-android-sdk"></a>Instalación de Android SDK para Azure Maps

Antes de avanzar hacia la creación de la aplicación, siga estos pasos para instalar Android SDK para Azure Maps. 

1. Agregue lo siguiente al bloque de repositorios de **todos los proyectos** del archivo **build.gradle**.

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Actualice **app/build.gradle** y agregue lo siguiente:

    1. Agregue lo siguiente al bloque de Android:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    2. Actualice el bloque de dependencias y agregue lo siguiente:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.1"
        ```

3. Configure los permisos con la adición de lo siguiente a **AndroidManifest.xml**:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest>
        ...
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        ...
    </manifest>
    ```

4. Edite **res > layout > activity_main.xml**, para que su aspecto sea similar al del XML siguiente:
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:mapcontrol_cameraTargetLat="47.64"
            app:mapcontrol_cameraTargetLng="-122.33"
            app:mapcontrol_cameraZoom="12"
            />

    </FrameLayout>
    ```

5. Edite **MainActivity.java** para crear una clase de actividad de la vista de mapa. Una vez realizada la edición, el aspecto debe ser similar al de la clase siguiente:

    ```java
    package com.example.myapplication;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {
        
        static {
            AzureMaps.setSubscriptionKey("{subscription-key}");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);

        }

        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }

        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }

        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }

        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }

        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }

    }

    ```

## <a name="import-classes"></a>Importación de clases

Después de completar los pasos anteriores, probablemente obtendrá advertencias de Android Studio en alguna parte del texto en el código. Para solucionar estas advertencias, importe las clases a las que se hace referencia en `MainActivity.java`.

Puede importar automáticamente estas clases presionando `Alt`+`Enter`(`Option`+`Return` en Mac). 

Haga clic en el botón **Run 'App'** (Ejecutar "aplicación"), o en `Control`+`R` si se trata de un Mac, para crear la aplicación.

![Haga clic en Ejecutar.](./media/how-to-use-android-map-control-library/run-app.png)

Android Studio tardará unos segundos en crear la aplicación. Una vez finalizada la compilación, puede probar la aplicación en el dispositivo Android emulado. Verá un mapa como el siguiente.

![Mapa Android](./media/how-to-use-android-map-control-library/android-map.png)

## <a name="add-a-marker-to-the-map"></a>Agregar un marcador al mapa

Para agregar un marcador al mapa, agregue la función `mapView.getMapAsync()` a `MainActivity.java`. El archivo `MainActivity.java` final debería presentar un aspecto similar al siguiente:

```java
package com.example.myapplication;

import android.app.Activity;
import android.os.Bundle;
import com.mapbox.geojson.Feature;
import com.mapbox.geojson.Point;
import com.microsoft.azure.maps.mapcontrol.AzureMaps;
import com.microsoft.azure.maps.mapcontrol.MapControl;
import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
import com.microsoft.azure.maps.mapcontrol.source.DataSource;
import static com.microsoft.azure.maps.mapcontrol.options.SymbolLayerOptions.iconImage;
public class MainActivity extends AppCompatActivity {
    
    static{
            AzureMaps.setSubscriptionKey("{subscription-key}");
        }

    MapControl mapControl;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mapControl = findViewById(R.id.mapcontrol);

        mapControl.onCreate(savedInstanceState);

        mapControl.getMapAsync(map -> {
            DataSource dataSource = new DataSource();
            dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

            SymbolLayer symbolLayer = new SymbolLayer(dataSource);
            symbolLayer.setOptions(iconImage("my-icon"));

            map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            map.sources.add(dataSource);
            map.layers.add(symbolLayer);
        });
    }

    @Override
    public void onStart() {
        super.onStart();
        mapControl.onStart();
    }

    @Override
    public void onResume() {
        super.onResume();
        mapControl.onResume();
    }

    @Override
    public void onPause() {
        super.onPause();
        mapControl.onPause();
    }

    @Override
    public void onStop() {
        super.onStop();
        mapControl.onStop();
    }

    @Override
    public void onLowMemory() {
        super.onLowMemory();
        mapControl.onLowMemory();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        mapControl.onDestroy();
    }

    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        mapControl.onSaveInstanceState(outState);
    }
}
```

Vuelva a ejecutar la aplicación y, a continuación, debería ver un marcador en el mapa similar al siguiente.

![Marcador en el mapa Android](./media/how-to-use-android-map-control-library/android-map-pin.png)
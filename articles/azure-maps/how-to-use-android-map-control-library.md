---
title: Cómo usar el control de mapa de Android en Azure Maps | Microsoft Docs
description: El control de mapa de Android en Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 15706addbe6b7f6310223978130158c792a47c89
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60770370"
---
# <a name="how-to-use-the-azure-maps-android-sdk"></a>Cómo usar el SDK de Android de Azure Maps

El SDK de Android de Azure Maps es una biblioteca de mapa vectoriales para Android. En este artículo le guiará a través de los procesos de instalación del SDK de Android de Azure Maps, cargar un mapa y colocar un pin en el mapa.

## <a name="prerequisites"></a>Requisitos previos

### <a name="create-an-azure-maps-account"></a>Crear una cuenta de Azure Maps

Para completar los procedimientos descritos en este artículo, primero deberá [crear una cuenta de Azure Maps](how-to-manage-account-keys.md) en el plan de tarifa S1.

### <a name="download-android-studio"></a>Descargar Android Studio

Deberá descargar Android Studio y cree un proyecto con una actividad vacía antes de instalar el SDK de Android de Azure Maps. También puede [descargar Android Studio](https://developer.android.com/studio/) gratuitamente de Google. 

## <a name="create-a-project-in-android-studio"></a>Creación de un proyecto en Android Studio

En primer lugar, deberá crear un nuevo proyecto con una actividad vacía. Siga estos pasos para crear un proyecto de Android Studio:

1. En **elija el proyecto**, seleccione **teléfonos y tabletas**. La aplicación se ejecutará en este factor de forma.
2. En el **teléfonos y tabletas** ficha, seleccione **actividad vacía**y, a continuación, seleccione **siguiente**.
3. En **Configure your project** (Configurar proyecto), seleccione `API 21: Android 5.0.0 (Lollipop)` como el SDK mínimo. Se trata de la versión más antigua compatible con el SDK de Android de Azure Maps.
4. Acepte el valor predeterminado `Activity Name` y `Layout Name` y seleccione **finalizar**.

Consulte la [documentación de Android Studio](https://developer.android.com/studio/intro/) para obtener más ayuda con la instalación de Android Studio y crear un nuevo proyecto.

![Crear un proyecto](./media/how-to-use-android-map-control-library/form-factor-android.png)

## <a name="set-up-a-virtual-device"></a>Configuración de un dispositivo virtual

Android Studio permite configurar un dispositivo virtual Android en el equipo. Si lo hace, puede ayudarle a probar la aplicación durante el desarrollo. Para configurar un dispositivo virtual, seleccione el icono de dispositivo Virtual Android (AVD) Manager en la esquina superior derecha de la pantalla de proyecto y, a continuación, seleccione **crear un dispositivo Virtual**. También puede obtener para el administrador AVD seleccionando **herramientas** > **Android** > **AVD Manager** desde la barra de herramientas. En el **teléfonos** categoría, seleccione **Nexus 5 X**y, a continuación, seleccione **siguiente**.

Puede aprender más acerca de cómo configurar un AVD en el [documentación de Android Studio](https://developer.android.com/studio/run/managing-avds).

![Android Emulator](./media/how-to-use-android-map-control-library/android-emulator.png)

## <a name="install-the-azure-maps-android-sdk"></a>Instalar el SDK de Android de Azure Maps

El siguiente paso en la creación de la aplicación es instalar el SDK de Android de Azure Maps. Siga estos pasos para instalar el SDK:

1. Agregue el código siguiente a la **todos los proyectos**, **repositorios** bloquear su **build.gradle** archivo.

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Actualización de su **App/Build.gradle** y agregue el código siguiente en él:

    1. Agregue el código siguiente al bloque de Android:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    2. Actualice el bloque de dependencias y agregue el código siguiente en él:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.1"
        ```

3. Configurar los permisos agregando el siguiente código XML para su **AndroidManifest.xml** archivo:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest>
        ...
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        ...
    </manifest>
    ```

4. Editar **res** > **diseño** > **activity_main.xml** para que quede así este XML:
    
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

5. Edite **MainActivity.java** para crear una clase de actividad de la vista de mapa. Después de la modificación, debe parecerse a esta clase:

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

## <a name="import-classes"></a>Importar clases

Después de completar los pasos anteriores, probablemente recibirá advertencias de Android Studio acerca de la parte del código. Para solucionar estas advertencias, importar las clases que se hace referenciadas en `MainActivity.java`.

Puede importar automáticamente estas clases seleccionando ALT+ENTRAR (opción + retorno de un equipo Mac).

Seleccione el botón de ejecución, como se muestra en el siguiente gráfico (o presione Control + R en un equipo Mac), para compilar la aplicación.

![Haga clic en Ejecutar.](./media/how-to-use-android-map-control-library/run-app.png)

Android Studio tardará unos segundos para compilar la aplicación. Una vez completada la compilación, puede probar la aplicación en el dispositivo emulado Android. Debería ver un mapa como este:

![Mapa Android](./media/how-to-use-android-map-control-library/android-map.png)

## <a name="add-a-marker-to-the-map"></a>Agregar un marcador al mapa

Para agregar un marcador al mapa, agregar el `mapView.getMapAsync()` función `MainActivity.java`. El último `MainActivity.java` código debe ser similar al siguiente:

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

Ejecute la aplicación de nuevo. Debería ver un marcador en el mapa, como se muestra aquí:

![Marcador en el mapa Android](./media/how-to-use-android-map-control-library/android-map-pin.png)
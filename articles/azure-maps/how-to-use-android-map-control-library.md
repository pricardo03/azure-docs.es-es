---
title: Introducción al control de mapa de Android en Azure Maps | Microsoft Docs
description: El control de mapa de Android en Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: e655b442ba9290d4b4525108521f2d1a0c766b48
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64869816"
---
# <a name="getting-started-with-azure-maps-android-sdk"></a>Introducción al SDK de Android de Azure Maps

El SDK de Android de Azure Maps es una biblioteca de mapa vectoriales para Android. En este artículo le guiará a través de los procesos de instalación del SDK de Android de Azure Maps y carga de un mapa.

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

1. Abra el nivel superior **build.gradle** archivo y agregue el código siguiente a la **todos los proyectos**, **repositorios** bloquear sección:

    ```
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Actualización de su **App/Build.gradle** y agregue el código siguiente en él:
    
    1. Asegúrese de que el proyecto **minSdkVersion** es API 21 o superior.

    2. Agregue el código siguiente a la sección de Android:

        ```
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```
    3. Actualice el bloque de dependencias y agregar una nueva línea de dependencia de implementación para el SDK de Android más reciente de Azure Maps:

        ```
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```

    > [!Note]
    > El SDK de Android de Azure Maps regularmente se está actualizado y mejorado. Puede ver el [Introducción al control de mapa Android](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) documentación para obtener el número de versión de implementación más reciente de Azure Maps. Además, puede establecer el número de versión de "0.2" a "0 +" para que apunte siempre a la versión más reciente.

3. Editar **res** > **diseño** > **activity_main.xml** y reemplácelo con lo siguiente:
    
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
            />
    </FrameLayout>
    ```

4. En el **MainActivity.java** archivo deberá:
    
    * Agregue las importaciones del SDK de Azure Maps
    * establecer la información de autenticación de Azure Maps
    * obtener la instancia del control de mapa en el **onCreate** (método)

    Establecer la información de autenticación en la clase AzureMaps globalmente mediante los métodos setSubscriptionKey o setAadProperties hace que no tendrá que agregar la información de autenticación en cada vista. El control de mapa contiene sus propios métodos de ciclo de vida para la administración OpenGL del ciclo de vida de Android, que debe llamarse directamente desde la actividad contenedora. En el orden de la aplicación correctamente, llame a métodos de ciclo de vida del control de mapa, debe reemplazar los siguientes métodos de ciclo de vida de la actividad que contiene el control de mapa y llamar al método de control de mapa correspondiente. 

    Editar el **MainActivity.java** archivo como sigue:
    
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
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
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

<center>

![Mapa de Android](./media/how-to-use-android-map-control-library/android-map.png)</center>

## <a name="next-steps"></a>Pasos siguientes

Para agregar cosas a la asignación, consulte:

> [!div class="nextstepaction"]
> [Agregar una capa de símbolo a un mapa de Android](https://review.docs.microsoft.com/azure/azure-maps/how-to-add-symbol-to-android-map)

> [!div class="nextstepaction"]
> [Agregar formas a un mapa de Android](https://docs.microsoft.com/azure/azure-maps/how-to-add-shapes-to-android-map)

> [!div class="nextstepaction"]
> [Cambiar estilos de mapa en las asignaciones de Android](https://docs.microsoft.com/azure/azure-maps/set-android-map-styles)



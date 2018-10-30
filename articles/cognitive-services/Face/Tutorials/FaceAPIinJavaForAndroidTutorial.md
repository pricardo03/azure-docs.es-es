---
title: 'Tutorial: Detección y enmarcado de las caras de una imagen con Android SDK'
titleSuffix: Azure Cognitive Services
description: En este tutorial, va a crear una aplicación Android sencilla que use Face API para detectar y enmarcar caras en una imagen.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 07/12/2018
ms.author: pafarley
ms.openlocfilehash: 99b2734745df722f45443b5347ae6dd054c8aa31
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957044"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Tutorial: Creación de una aplicación Android para detectar y enmarcar caras en una imagen

En este tutorial, va a crear una aplicación de Android simple que utiliza la biblioteca de clases Java del servicio Face para detectar caras en una imagen. La aplicación muestra una imagen seleccionada con cada cara detectada enmarcada por un rectángulo. El código de ejemplo completo está disponible en GitHub en [Detección y enmarcado de caras de una imagen en Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample).

![Captura de pantalla de Android de una foto con las caras enmarcadas por un rectángulo rojo](../Images/android_getstarted2.1.PNG)

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
> - Creación de una aplicación Android
> - Instalar la biblioteca cliente del servicio Face
> - Usar la biblioteca cliente para detectar caras en una imagen
> - Dibujar un marco alrededor de cada cara detectada

## <a name="prerequisites"></a>Requisitos previos

- Necesita una clave de suscripción para ejecutar el ejemplo. Puede obtener las claves de la suscripción de evaluación gratuita en la página de [Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api).
- [Android Studio](https://developer.android.com/studio/) con el SDK 22 mínimo (requerido por la biblioteca cliente de Face).
- La biblioteca cliente de Face [com.microsoft.projectoxford:face:1.4.3](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.projectoxford%22) de Maven. No es necesario descargar el paquete. A continuación, se proporcionan instrucciones de instalación.

## <a name="create-the-project"></a>Creación del proyecto

Cree el proyecto de aplicación de Android mediante estos pasos:

1. Abra Android Studio. En este tutorial se usa Android Studio 3.1.
1. Seleccione **Start a new Android Studio project** (Iniciar un nuevo proyecto de Android Studio).
1. En la pantalla **Create Android Project** (Crear un proyecto de Android), puede modificar los campos predeterminados, si es necesario, y después haga clic en **Next** (Siguiente).
1. En la pantalla **Target Android Devices** (Dispositivos Android de destino), utilice el selector de lista desplegable para elegir **API 22** o superior y, después, haga clic en **Next** (Siguiente).
1. Seleccione **Empty Activity** (Actividad vacía) y, después, haga clic en **Next** (Siguiente).
1. Desactive la opción **Backwards Compatibility** (Compatibilidad con versiones anteriores) y haga clic en **Finish** (Finalizar).

## <a name="create-the-ui-for-selecting-and-displaying-the-image"></a>Creación de la interfaz de usuario para seleccionar y mostrar la imagen

Abra *activity_main.xml*; debería ver el editor de diseño. Seleccione la pestaña **Text** (Texto) y sustituya el contenido por el código siguiente.

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="fill_parent"
        android:id="@+id/imageView1"
        android:layout_above="@+id/button1"
        android:contentDescription="Image with faces to analyze"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Browse for face image"
        android:id="@+id/button1"
        android:layout_alignParentBottom="true"/>
</RelativeLayout>
```

Abra *MainActivity.java* y reemplace todo excepto la primera instrucción `package` por el código siguiente.

El código establece un controlador de eventos en `Button` que inicia una nueva actividad para permitir al usuario seleccionar una imagen. Una vez seleccionada, esta se muestra en `ImageView`.

```java
import java.io.*;
import android.app.*;
import android.content.*;
import android.net.*;
import android.os.*;
import android.view.*;
import android.graphics.*;
import android.widget.*;
import android.provider.*;

public class MainActivity extends Activity {
    private final int PICK_IMAGE = 1;
    private ProgressDialog detectionProgressDialog;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            Button button1 = (Button)findViewById(R.id.button1);
            button1.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
                intent.setType("image/*");
                startActivityForResult(Intent.createChooser(
                        intent, "Select Picture"), PICK_IMAGE);
            }
        });

        detectionProgressDialog = new ProgressDialog(this);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK &&
                data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(
                        getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);

                // Uncomment
                //detectAndFrame(bitmap);
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }
}
```

La aplicación ya puede buscar una fotografía y mostrarla en la ventana similar a la imagen siguiente.

![Captura de pantalla de Android de una foto con caras](../Images/android_getstarted1.1.PNG)

## <a name="configure-the-face-client-library"></a>Configuración de la biblioteca cliente de Face

Face API es una API en la nube que puede llamar mediante solicitudes HTTPS. En este tutorial se usa la biblioteca cliente de Face, que encapsula estas solicitudes web, para simplificar el trabajo.

En el panel **Project** (Proyecto), utilice el selector de lista desplegable para seleccionar **Android**. Expanda **Gradle Scripts** (Scripts de Gradle) y abra *build.gradle (Module: app)*.

Agregue una dependencia para la biblioteca cliente de Face, `com.microsoft.projectoxford:face:1.4.3`, tal como se muestra en la siguiente captura de pantalla y, después, haga clic en **Sync Now** (Sincronizar ahora).

![Captura de pantalla de Android Studio del archivo build.gradle de aplicación](../Images/face-tut-java-gradle.png)

Abra **MainActivity.java** y anexe las siguientes directivas de importación:

```java
import com.microsoft.projectoxford.face.*;
import com.microsoft.projectoxford.face.contract.*;
```

## <a name="add-the-face-client-library-code"></a>Adición del código de biblioteca cliente de Face

Inserte el código siguiente en la clase `MainActivity`, sobre el método `onCreate`:

```java
private final String apiEndpoint = "<API endpoint>";
private final String subscriptionKey = "<Subscription Key>";

private final FaceServiceClient faceServiceClient =
        new FaceServiceRestClient(apiEndpoint, subscriptionKey);
```

Reemplace `<API endpoint>` por el punto de conexión de API que se asignó a la clave. Las claves de suscripción de la evaluación gratuita se generan en la región **westcentralus**. Por lo que si usa una clave de suscripción de evaluación gratuita, la instrucción sería:

```java
apiEndpoint = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0";
```

Reemplace `<Subscription Key>` por la clave de suscripción. Por ejemplo: 

```java
subscriptionKey = "0123456789abcdef0123456789ABCDEF"
```

En el panel **Project** (Proyecto), expanda **app**, a continuación, **manifests** y abra *AndroidManifest.xml*.

Inserte el siguiente elemento como elemento secundario directo del elemento `manifest`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Compile el proyecto para buscar errores. Ahora está listo para llamar al servicio Face.

## <a name="upload-an-image-to-detect-faces"></a>Carga de una imagen para detectar caras

La manera más sencilla de detectar caras es llamar al método `FaceServiceClient.detect`. Este método encapsula el método de [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API y devuelve una matriz de `Face`.

Cada `Face` devuelta contiene un rectángulo para indicar su ubicación, junto con una serie de atributos de cara opcionales. En este ejemplo, solo se necesitan las ubicaciones de la cara.

Si se produce un error, un `AlertDialog` muestra el motivo subyacente.

Inserte los siguientes métodos a la clase `MainActivity`.

```java
// Detect faces by uploading a face image.
// Frame faces after detection.
private void detectAndFrame(final Bitmap imageBitmap) {
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
    imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
    ByteArrayInputStream inputStream =
            new ByteArrayInputStream(outputStream.toByteArray());

    AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                String exceptionMessage = "";

                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0],
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null          // returnFaceAttributes:
                                /* new FaceServiceClient.FaceAttributeType[] {
                                    FaceServiceClient.FaceAttributeType.Age,
                                    FaceServiceClient.FaceAttributeType.Gender }
                                */
                        );
                        if (result == null){
                            publishProgress(
                                    "Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(String.format(
                                "Detection Finished. %d face(s) detected",
                                result.length));
                        return result;
                    } catch (Exception e) {
                        exceptionMessage = String.format(
                                "Detection failed: %s", e.getMessage());
                        return null;
                    }
                }

                @Override
                protected void onPreExecute() {
                    //TODO: show progress dialog
                }
                @Override
                protected void onProgressUpdate(String... progress) {
                    //TODO: update progress
                }
                @Override
                protected void onPostExecute(Face[] result) {
                    //TODO: update face frames
                }
            };

    detectTask.execute(inputStream);
}

private void showError(String message) {
    new AlertDialog.Builder(this)
    .setTitle("Error")
    .setMessage(message)
    .setPositiveButton("OK", new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int id) {
        }})
    .create().show();
}
```

## <a name="frame-faces-in-the-image"></a>Enmarcado de caras en la imagen

Inserte el siguiente método auxiliar a la clase `MainActivity`. Este método dibuja un rectángulo alrededor de cada cara detectada.

```java
private static Bitmap drawFaceRectanglesOnBitmap(
        Bitmap originalBitmap, Face[] faces) {
    Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
    Canvas canvas = new Canvas(bitmap);
    Paint paint = new Paint();
    paint.setAntiAlias(true);
    paint.setStyle(Paint.Style.STROKE);
    paint.setColor(Color.RED);
    paint.setStrokeWidth(10);
    if (faces != null) {
        for (Face face : faces) {
            FaceRectangle faceRectangle = face.faceRectangle;
            canvas.drawRect(
                    faceRectangle.left,
                    faceRectangle.top,
                    faceRectangle.left + faceRectangle.width,
                    faceRectangle.top + faceRectangle.height,
                    paint);
        }
    }
    return bitmap;
}
```

Complete los métodos `AsyncTask`, indicados por los comentarios `TODO`, en el método `detectAndFrame`. Si se ejecuta correctamente, se muestra la imagen seleccionada con las caras enmarcadas en `ImageView`.

```java
@Override
protected void onPreExecute() {
    detectionProgressDialog.show();
}
@Override
protected void onProgressUpdate(String... progress) {
    detectionProgressDialog.setMessage(progress[0]);
}
@Override
protected void onPostExecute(Face[] result) {
    detectionProgressDialog.dismiss();
    if(!exceptionMessage.equals("")){
        showError(exceptionMessage);
    }
    if (result == null) return;
    ImageView imageView = findViewById(R.id.imageView1);
    imageView.setImageBitmap(
            drawFaceRectanglesOnBitmap(imageBitmap, result));
    imageBitmap.recycle();
}
```

Por último, en el método `onActivityResult`, quite la llamada al método `detectAndFrame`, tal y como se muestra a continuación.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if (requestCode == PICK_IMAGE && resultCode == RESULT_OK &&
                data != null && data.getData() != null) {
        Uri uri = data.getData();
        try {
            Bitmap bitmap = MediaStore.Images.Media.getBitmap(
                    getContentResolver(), uri);
            ImageView imageView = findViewById(R.id.imageView1);
            imageView.setImageBitmap(bitmap);

            // Uncomment
            detectAndFrame(bitmap);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## <a name="run-the-app"></a>Ejecución de la aplicación

Ejecute esta aplicación y busque una imagen con una cara. Espere unos segundos para permitir que el servicio Face responda. Después, obtendrá un resultado similar al de la imagen siguiente:

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido el proceso básico de usar el servicio Face y ha creado una aplicación para mostrar caras enmarcadas en una imagen.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre cómo detectar y usar los puntos de referencia de la cara.

> [!div class="nextstepaction"]
> [Detección de caras en una imagen](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)

Explore las Face API usadas para detectar caras y sus atributos como la postura, sexo, edad, postura de la cabeza, vello facial y gafas.

> [!div class="nextstepaction"]
> [Referencia de Face API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
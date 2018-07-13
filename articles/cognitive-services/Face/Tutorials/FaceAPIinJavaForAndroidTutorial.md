---
title: Tutorial de la API de Face API Java para Android | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Cree una aplicación de Android simple que use Face API de Cognitive Services para detectar y enmarcar caras en una imagen.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 5164a261d482d0cca3842a973d2109b17999bd25
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382550"
---
# <a name="getting-started-with-face-api-in-java-for-android-tutorial"></a>Tutorial de introducción a Face API en Java para Android

En este tutorial, aprenderá a crear y desarrollar una aplicación de Android simple que invoque Face API para detectar caras en una imagen. La aplicación muestra el resultado mediante la encuadre de las caras que detecta.

![GettingStartedAndroid](../Images/android_getstarted2.1.PNG)

## <a name="preparation"></a> Preparación

Para usar el tutorial, necesitará los siguientes requisitos previos:

- Android Studio y SDK instalados
- Dispositivo Android (opcional para las pruebas).

## <a name="step1"></a>Paso 1: Suscribirse a Face API y obtener la clave de suscripción

Antes de usar Face API, debe suscribirse a Face API en el portal de Microsoft Cognitive Services. Consulte [Suscripciones](https://azure.microsoft.com/try/cognitive-services/). En este tutorial, puede usarse tanto la clave principal como la secundaria.

## <a name="step2"></a>Paso 2: Crear el marco de trabajo de la aplicación

En este paso creará un proyecto de aplicación de Android para implementar la interfaz de usuario básica para recoger y mostrar una imagen. Solo tiene que seguir las instrucciones que se describen a continuación: 

1. Abra Android Studio.
2. En el menú Archivo, haga clic en **Nuevo proyecto...**
3. Asigne a la aplicación el nombre **MyFirstApp** y haga clic en Siguiente. 

    ![GettingStartAndroidNewProject](../Images/AndroidNewProject.png)

4. Elija la plataforma de destino según sea necesario y, a continuación, haga clic en Siguiente. 

    ![GettingStartAndroidNewProject2](../Images/AndroidNewProject2.png)

5. Seleccione **Actividad básica** y, a continuación, haga clic en Siguiente.
6. Asigne un nombre a la actividad como se indica a continuación y, después, haga clic en Finalizar. 

    ![GettingStartAndroidNewProject4](../Images/AndroidNewProject4.png)

7. Abra **activity_main.xml** y debería ver el editor de diseño de esta actividad.
8. Consulte el archivo de origen del texto y, a continuación, edite el diseño de la actividad de la manera siguiente:

    ```xml
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
        android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">
     
        <ImageView
            android:layout_width="match_parent"
            android:layout_height="fill_parent"
            android:id="@+id/imageView1"
            android:layout_above="@+id/button1" />
    
        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Browse"
            android:id="@+id/button1"
            android:layout_alignParentBottom="true" />
    </RelativeLayout>
    ```  

9. Abra **MainActivity.java** e inserte las siguientes directivas de importación al principio del archivo:

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
    ```
      
    En segundo lugar, modifique la clase de la manera siguiente:  
    
    ```java
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
                Intent gallIntent = new Intent(Intent.ACTION_GET_CONTENT);
                gallIntent.setType("image/*");
                startActivityForResult(Intent.createChooser(gallIntent, "Select Picture"), PICK_IMAGE);
            }
        });
         
        detectionProgressDialog = new ProgressDialog(this);
    }
    
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
                } catch (IOException e) {
                e.printStackTrace();
                }
        }
    }
    ```

La aplicación ya puede buscar una fotografía de la galería y mostrarla en la ventana similar a la imagen siguiente:

![GettingStartAndroidUI](../Images/android_getstarted1.1.PNG)

## <a name="step3"></a>Paso 3: Configurar la biblioteca cliente de Face API

Face API es una API en la nube que puede invocar mediante solicitudes HTTPS. Para usar de forma más conveniente Face API en las aplicaciones de la plataforma .NET, también se proporciona una biblioteca cliente para encapsular solicitudes web. En este ejemplo, usamos la biblioteca cliente para simplificar nuestro trabajo. 

Siga las instrucciones que aparecen a continuación para configurar la biblioteca cliente: 

1. Busque el archivo **build.gradle** de nivel superior del proyecto desde el panel de proyectos que se muestra en el ejemplo. Tenga en cuenta que existen otros archivos **build.gradle** en el árbol de proyectos, así como que debe abrir el archivo **build.gradle** de nivel superior en primer lugar.
2. Agregue **mavenCentral()** al repositorio de proyectos. También puede utilizar jcenter(), que es el repositorio predeterminado de Android Studio, ya que jcenter() es un supraconjunto de mavenCentral().  

```
    allprojects {
        repositories {
            ...
            mavenCentral()
        }
    }
```

3. Abra el archivo **build.gradle** en el proyecto "aplicación".
4. Agregue una dependencia para la biblioteca de cliente almacenada en Maven Central Repository:

```
    dependencies {  
        ...  
        implementation 'com.microsoft.projectoxford:face:1.4.3'  
    }
```

5. Abra **MainActivity.java** en el proyecto "aplicación" e inserte las siguientes directivas de importación: 
    
    ```java
    import com.microsoft.projectoxford.face.*;  
    import com.microsoft.projectoxford.face.contract.*;  
    ```
    
   A continuación, inserte el código siguiente en la clase:

    ```java
    private FaceServiceClient faceServiceClient = new FaceServiceRestClient("your API endpoint", "<Subscription Key>");
    ```

   Reemplace el primer parámetro indicado anteriormente por el punto de conexión de API que se asignó a la clave en el paso 1. Por ejemplo: 
   
        https://eastus2.api.cognitive.microsoft.com/face/v1.0
   
   Reemplace el segundo parámetro por la clave de suscripción que obtuvo en el paso 1.
   
6. Abra el archivo **AndroidManifest.xml** en el proyecto "aplicación". Inserte el siguiente elemento como elemento secundario de **manifest**:  

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />  
    ```

7. Ya está listo para llamar a Face API desde la aplicación. 

## <a name="step4"></a>Paso 4: Cargar imágenes para detectar caras

La manera más sencilla de detectar caras es mediante una llamada a la API [Face – Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) mediante la carga del archivo de imagen directamente. Al usar la biblioteca cliente, se puede hacer mediante el método asincrónico **DetectAsync** de la clase **FaceServiceClient**. Cada cara devuelta contiene un rectángulo para indicar su ubicación, junto con una serie de atributos de cara opcionales. En este ejemplo, solo es necesario recuperar la ubicación de la cara. Aquí es necesario insertar un método en la clase **MainActivity** para la detección de caras: 

```java

    // Detect faces by uploading face images
    // Frame faces after detection
    
    private void detectAndFrame(final Bitmap imageBitmap)
    {
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
        ByteArrayInputStream inputStream = 
            new ByteArrayInputStream(outputStream.toByteArray());
        AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0], 
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null           // returnFaceAttributes: a string like "age, gender"
                /* If you want value of FaceAttributes, try adding 4th argument like below.
                            new FaceServiceClient.FaceAttributeType[] {
                    FaceServiceClient.FaceAttributeType.Age,
                    FaceServiceClient.FaceAttributeType.Gender }
                */              
                        );
                        if (result == null)
                        {
                            publishProgress("Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(
                                String.format("Detection Finished. %d face(s) detected",
                                        result.length));
                        return result;
                    } catch (Exception e) {
                        publishProgress("Detection failed");
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
```

## <a name="step5"></a>Paso 5: Marcar caras en la imagen

En este último paso, se combinan todos los pasos anteriores y se marcan las caras detectadas con marcos en la imagen. En primer lugar, abra **MainActivity.java** e inserte un método auxiliar para dibujar rectángulos: 

```java
    private static Bitmap drawFaceRectanglesOnBitmap(Bitmap originalBitmap, Face[] faces) {
        Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
        Canvas canvas = new Canvas(bitmap);
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setColor(Color.RED);
        int stokeWidth = 2;
        paint.setStrokeWidth(stokeWidth);
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

Ahora finalice las partes TODO del método **detectAndFrame** para enmarcar las caras e informar del estado.

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
        if (result == null) return;
        ImageView imageView = (ImageView)findViewById(R.id.imageView1);
        imageView.setImageBitmap(drawFaceRectanglesOnBitmap(imageBitmap, result));
        imageBitmap.recycle();
    }
```
 
Por último, agregue una llamada al método **detectAndFrame** desde el método **onActivityResult**, tal como se indica a continuación. 

```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
     
                // This is the new addition.
                // detectAndFrame(bitmap);
     
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

Ejecute esta aplicación y busque una imagen que contenga una cara. Espere unos segundos para permitir que la API en la nube responda. Después, obtendrá un resultado similar al de la imagen siguiente: 

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a> Resumen

En este tutorial, aprendió el proceso básico para el uso de Face API y creó una aplicación para mostrar las marcas de cara en las imágenes. Para obtener más información sobre Face API, consulte los procedimientos y la [referencia de la API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236). 

## <a name="related"></a> Tutoriales relacionados

- [Tutorial de introducción a Face API en CSharp](FaceAPIinCSharpTutorial.md)
- [Tutorial de introducción a Face API en Python](FaceAPIinPythonTutorial.md)

---
title: 'Tutorial: Creación de una aplicación de vinculación de entidad con C#'
titlesuffix: Azure Cognitive Services
description: Analice texto y vincule entidades con nombre a las entradas correspondientes de Knowledge Base mediante Entity Linking API.
services: cognitive-services
author: DavidLiCIG
manager: nitinme
ms.service: cognitive-services
ms.subservice: entity-linking-intelligence
ms.topic: tutorial
ms.date: 07/06/2016
ms.author: davl
ROBOTS: NOINDEX
ms.openlocfilehash: 4b283103920230a0d2aae98c83f75fb03679a675
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706807"
---
# <a name="tutorial-build-an-entity-linking-app-with-c"></a>Tutorial: Creación de una aplicación de Entity Linking con C#

Entity Linking de Microsoft es una herramienta de procesamiento de lenguaje natural que sirve para analizar texto y vincular entidades con nombre a las entradas correspondientes de Knowledge Base. 

En este tutorial se explora Entity Linking utilizando la biblioteca cliente de Entity Linking como paquete NuGet. 

### <a name="Prerequisites">Requisitos previos</a>

- Visual Studio 2015
- Una clave de Microsoft Cognitive Services API
- Obtener la biblioteca cliente y ejemplo
- Paquete NuGet de Microsoft Entity Linking

Puede descargar la biblioteca cliente de Entity Linking Intelligence Service API a través del [SDK](https://www.github.com/microsoft/cognitive-entitylinking-windows). Debe extraer el archivo ZIP descargado en una carpeta de su elección, muchos usuarios eligen la carpeta de Visual Studio 2015.

### <a name="step-1-subscribe-entity-linking-intelligence-service-and-get-your-own-key">Paso 1: Suscripción a Entity Linking Intelligence Service y obtención de la clave</a>
Antes de usar Entity Linking Intelligence Service, debe suscribirse para obtener una clave de API. Consulte [Mis suscripciones](https://www.microsoft.com/cognitive-services/en-us/sign-up). En este tutorial, puede usarse tanto la clave principal como la secundaria.

### <a name="step-2-create-a-new-project-in-visual-studio"> Paso 2: Creación de un proyecto en Visual Studio</a>

Empecemos creando un proyecto nuevo en Visual Studio. En primer lugar, inicie Visual Studio 2015 desde el menú Inicio. A continuación, cree un proyecto nuevo seleccionando **Instalado → Plantillas → Visual C# → Windows Universal → Aplicación vacía** para la plantilla de proyecto:

 ![Crear una aplicación universal](./Images/CreateUWP.png)

### <a name="step-3-add-the-entity-linking-nuget-package-to-your-project">Paso 3: Incorporación del paquete NuGet de Entity Linking al proyecto</a>

Entity Linking de Cognitive Services se lanzó como paquete NuGet.org y debe instalarse antes de utilizarse.
Para agregar el paquete al proyecto, vaya a la pestaña **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet**.

Primero, en la ventana **Administrador de paquetes NuGet**, seleccione NuGet.org como **Origen del paquete** en la esquina superior derecha. Seleccione **Examinar** en la esquina superior izquierda y, en el cuadro de búsqueda, escriba "ProjectOxford.EntityLinking". Seleccione el paquete NuGet **Microsoft.ProjectOxford.EntityLinking** y haga clic en **Instalar**.

A continuación, busque Newtonsoft.Json e instálelo. Si se le solicita que revise los cambios, haga clic en **Aceptar**. Si aparecen los términos de la licencia EntityLinking, haga clic en **Acepto**.

Entity Linking se instala como parte de la aplicación. Para confirmarlo, compruebe que la referencia ** Microsoft.ProjectOxford.EntityLinking** está presente como parte del proyecto en el Explorador de soluciones.

 ![Biblioteca NuGet incluida en el proyecto](./Images/NugetLibraryInProject.png)
 
### <a name="step-4-add-an-input-and-output-text-block-to-your-apps-xaml">Paso 4: Incorporación de un bloque de texto de entrada y salida al XAML de la aplicación</a>
Vaya a **MainPage.xaml** en el **Explorador de soluciones** y haga doble clic en el archivo, que se abrirá en una ventana nueva. Para mayor comodidad, puede hacer doble clic en el botón **XAML** de la pestaña **Diseñador**, esto ocultará el **Diseñador visual** y usará todo el espacio para la visualización del código.

 ![Biblioteca NuGet incluida en el proyecto](./Images/UWPMainPage.png)
 
Como servicio de texto, la mejor manera de visualizar la funcionalidad es creando un bloque de texto de entrada y salida. Para ello, agregue el XAML siguiente en la **Cuadrícula**. Este código agrega tres componentes: un cuadro de texto de entrada, un bloque de texto de salida y un botón de inicio.
 
 ```XAML
 <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <TextBox x:Name="inputBox" Grid.Row="0" TextWrapping="Wrap" Text="Enter a paragraph" Margin="10" AcceptsReturn="True" />
    <TextBlock x:Name="outputBlock" Grid.Row="1" TextWrapping="Wrap" Text="Result will be here" Margin="10" />
    <Button x:Name="button" Grid.Row="2" Content="Get Result" />
</Grid>
 ```
 
### <a name="step-5-proceed-to-add-entity-linking-intelligence-service">Paso 5: Incorporación de Entity Linking Intelligence Service</a>
 
Se ha creado la interfaz de usuario. Antes de usar Entity Linking Service, es necesario agregar el controlador button-Click. Abra **MainPage.xaml** del **Explorador de soluciones**. Agregue un controlador button_Click al final del botón.
 
 ```XAML
 <Button x:Name="button" Grid.Row="2" Content="Get Result" Click="button_Click" />
 ```
 
Es necesario implementar un controlador button-Click en el código. Abra **MainPage.xaml.cs** en el **Explorador de soluciones** para implementar button-Click. EntityLinkingServiceClient es un contenedor para recuperar las respuestas de Entity Linking. El argumento del constructor de EntityLinkingServiceClient es la clave de suscripción de Cognitive Services. Pegue la clave de suscripción que obtuvo en el **paso 1** para llamar a Entity Linking Service. 

A continuación, se muestra un código de ejemplo que agrega "wikipediaId" a la respuesta mediante Entity Linking Service. 
 
 ```csharp
 private async void button_Click(object sender, RoutedEventArgs e)
{
    var text = this.inputBox.Text;
    var client = new EntityLinkingServiceClient("Your subscription key","https://api.labs.cognitive.microsoft.com");
    var linkResponse = await client.LinkAsync(text);
    var result = string.Join(", ", linkResponse.Select(i => i.WikipediaID).ToList());
    this.outputBlock.Text = result;
}
 ```
 
Ahora está listo para ejecutar la primera aplicación de Entity Linking de proceso de lenguaje natural. Presione la **tecla F5** para compilar e iniciar la aplicación. Pegue los fragmentos de código o párrafos en el cuadro de entrada. Presione el botón "Get Result" y observe las entidades identificadas en el bloque de salida.
 
 ![Resultado del ejemplo de UWP](./Images/DemoCodeResult.png)
 
### <a name="summary">Resumen</a>
 
En este tutorial ha aprendido a crear una aplicación para utilizar la biblioteca cliente de Entity Linking Intelligence Service con solo unas pocas líneas de código C# y XAML. 


---
title: Aplicación híbrida en la nube/local (.NET) de Azure WCF Relay | Microsoft Docs
description: Aprenda a exponer un servicio WCF local a una aplicación web en la nube con Azure Relay
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 9ed02f7c-ebfb-4f39-9c97-b7dc15bcb4c1
ms.service: service-bus-relay
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: 2972d04d1617b755bb6c2ff60d9922accdd09f2a
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614844"
---
# <a name="expose-an-on-premises-wcf-service-to-a-web-application-in-the-cloud-by-using-azure-relay"></a>Exposición de un servicio WCF local a una aplicación web en la nube con Azure Relay 
En este artículo se muestra cómo compilar una aplicación de nube híbrida con Microsoft Azure y Visual Studio. Va a crear una aplicación que utiliza varios recursos de Azure funcionando en la nube.

* Crear o adaptar un servicio web existente para su consumo por una solución web.
* Utilizar la retransmisión de WCF de Azure Service Bus para compartir datos entre una aplicación de Azure y un servicio web hospedado en otra parte.

En este tutorial, realice los siguientes pasos:

> [!div class="checklist"]
> * Revise el escenario.
> * Cree un espacio de nombres.
> * Creación de un servidor local
> * Cree una aplicación ASP .NET
> * Ejecute localmente la aplicación.
> * Implemente la aplicación web en Azure
> * Ejecute la aplicación en Azure

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, debe cumplir los siguientes requisitos previos:

- Una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- [Visual Studio 2015 o posterior](http://www.visualstudio.com). En los ejemplos de este tutorial se usa Visual Studio 2017.
- SDK de Azure para .NET. Instálelo desde el [página de descargas de SDK](https://azure.microsoft.com/downloads/).

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a>Cómo ayuda Relay de Azure con soluciones híbridas
Las soluciones de negocio por lo general están compuestas por una combinación de código personalizado escrito para afrontar los nuevos y exclusivos requisitos empresariales, así como la funcionalidad existente proporcionada por soluciones y sistemas que ya están instalados.

Los arquitectos de soluciones están comenzando a utilizar la nube para abordar con más facilidad los requisitos de escala y reducir los costes operativos. De esta manera, se dan cuenta de que los activos de servicio existentes que les gustaría aprovechar como base de sus soluciones se encuentran dentro del firewall corporativo y no resulta sencillo para la solución en la nube el acceso a ellos. Muchos de los servicios internos no están construidos ni hospedados de una forma que se puedan exponer fácilmente en los servidores perimetrales de la red corporativa.

[Azure Relay](https://azure.microsoft.com/services/service-bus/) está diseñada para el caso de uso en que se toman los servicios web de Windows Communication Foundation (WCF) existentes y se permite el acceso seguro a los mismos a las soluciones que residen fuera del perímetro corporativo, sin necesidad de realizar cambios molestos en la infraestructura de la red corporativa. Dichos servicios de retransmisión aún se hospedan en el entorno existente, pero delegan la escucha de sesiones y solicitudes de entrada en el servicio de relé hospedado en la nube. Azure Relay también protege dichos servicios del acceso no autorizado mediante el uso de la autenticación de [Firma de acceso compartido (SAS)](../service-bus-messaging/service-bus-sas.md).

## <a name="review-the-scenario"></a>Revisión del escenario
En este tutorial, va a crear un sitio web de ASP.NET que le permitirá ver una lista de productos en la página del inventario de productos.

![Escenario][0]

En este tutorial se asume que la información de los productos se encuentra en un sistema local y se usa Relay de Azure para obtener acceso a dicho sistema. Esto lo simula un servicio web que se ejecuta en una aplicación de consola simple y el respaldo lo proporciona un conjunto de productos en memoria. Dicha aplicación de consola se puede ejecutar en el equipo propio y el rol web se puede implementar en Azure. Al hacerlo, observará que el rol web que se ejecuta en el centro de datos de Azure llamará a su equipo, aunque casi seguro que este residirá al menos detrás de un firewall y una capa de traducción de direcciones de red (NAT).

## <a name="set-up-the-development-environment"></a>Configuración del entorno de desarrollo

Antes de comenzar a desarrollar aplicaciones de Azure, obtenga las herramientas y configure el entorno de desarrollo:

1. Instale el SDK de Azure para .NET desde la [página de descargas](https://azure.microsoft.com/downloads/) del SDK.
2. En la columna **.NET**, haga clic en la versión de [Visual Studio](https://www.visualstudio.com) que está usando. En los pasos de este tutorial se usa Visual Studio 2017.
3. Cuando se le solicite ejecutar o guardar el programa de instalación, haga clic en **Ejecutar**.
4. En el **instalador de plataforma web**, haga clic en **Instalar** y continúe con la instalación.
5. Cuando la instalación se complete, dispondrá de todo lo necesario para iniciar el desarrollo de la aplicación. El SDK incluye las herramientas que le permiten desarrollar fácilmente aplicaciones Azure en Visual Studio.

## <a name="create-a-namespace"></a>Creación de un espacio de nombres
El primer paso consiste en crear un espacio de nombres y obtener una clave de [firma de acceso compartido (SAS)](../service-bus-messaging/service-bus-sas.md). Un espacio de nombres proporciona un límite de aplicación para cada aplicación que se expone a través del servicio de retransmisión. El sistema genera una clave SAS automáticamente cuando se crea un espacio de nombres de servicio. La combinación del espacio de nombres de servicio y la clave SAS proporciona las credenciales para que Azure autentique el acceso a una aplicación.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Creación de un servidor local
En primer lugar, cree un sistema de catálogo de productos local ficticio.  Este proyecto es una aplicación de consola de Visual Studio y emplea el [paquete NuGet de Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) para incluir las bibliotecas y los valores de configuración de Service Bus.

### <a name="create-the-project"></a>Creación del proyecto
1. Inicie Microsoft Visual Studio con privilegios de administrador. Para hacerlo, haga clic con el botón derecho en el icono del programa Visual Studio y, luego, haga clic en **Ejecutar como administrador**.
2. En Visual Studio, en el menú **Archivo**, haga clic en **Nuevo** y, a continuación, en **Proyecto**.
3. En **Plantillas instaladas**, en **Visual C#**, haga clic en **Aplicación de consola (.NET Framework)**. En el cuadro **Nombre**, escriba el nombre **ProductsServer**:

   ![Cuadro de diálogo Nuevo proyecto][11]
4. Haga clic en **Aceptar** para crear el proyecto **ProductsServer**.
5. Si ya ha instalado el administrador del paquete NuGet para Visual Studio, vaya al paso siguiente. De lo contrario, visite [NuGet][NuGet] y haga clic en [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) (Instalar NuGet). Siga las indicaciones para instalar el administrador del paquete NuGet y, a continuación, reinicie Visual Studio.
6. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **ProductsServer** y luego haga clic en **Administrar paquetes NuGet**.
7. Haga clic en la pestaña **Examinar** y, después, busque **WindowsAzure.ServiceBus**. Seleccione el paquete **WindowsAzure.ServiceBus**.
8. Haga clic en **Instalar**y acepte las condiciones de uso.

   ![Selección de un paquete NuGet][13]

   Ahora se hace referencia a los ensamblados del cliente requeridos.
8. Agregue una clase nueva para el contrato de su producto. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **ProductsServer**, a continuación en **Agregar** y, por último, en **Clase**.
9. En el cuadro **Name**, escriba el nombre **ProductsContract.cs**. A continuación, haga clic en **Agregar**.
10. En **ProductsContract.cs**, sustituya la definición del espacio de nombres por el siguiente código, que define el contrato del servicio.

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();

        }

        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```
11. En Program.cs, sustituya la definición del espacio de nombres por el siguiente código, que agrega el servicio de perfil y su host:

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement the IProducts interface.
        class ProductsService : IProducts
        {

            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };

            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```
12. En el Explorador de soluciones, haga doble clic en el archivo **App.config** para abrirlo en el editor de Visual Studio. Al final del elemento `<system.ServiceModel>`, pero aún dentro de `<system.ServiceModel>`, agregue el siguiente código XML, asegúrese de reemplazar *yourServiceNamespace* por el nombre de su espacio de nombres y *yourKey* por la clave SAS que recuperó anteriormente en el portal:

    ```xml
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
    El error producido por "transportClientEndpointBehavior" es simplemente una advertencia y no es un problema de bloqueo en este ejemplo.
    
13. Aún en App.config, en el elemento `<appSettings>`, reemplace el valor de la cadena de conexión por la cadena de conexión que obtuvo antes en el portal.

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
14. Presione **Ctrl+Mayús+B**, o bien, en el menú **Compilar**, haga clic en **Compilar solución** para compilar la aplicación y comprobar la exactitud del trabajo hasta el momento.

## <a name="create-an-aspnet-application"></a>Creación de una aplicación ASP.NET

En esta sección va a crear una aplicación de ASP.NET simple que mostrará los datos recuperados del servicio de producto.

### <a name="create-the-project"></a>Creación del proyecto

1. Asegúrese de que se está ejecutando Visual Studio con privilegios de administrador.
2. En Visual Studio, en el menú **Archivo**, haga clic en **Nuevo** y, a continuación, en **Proyecto**.
3. En **Plantillas instaladas**, en **Visual C#**, haga clic en **Aplicación web ASP.NET (.NET Framework)**. Asigne al proyecto el nombre **ProductsPortal**. A continuación, haga clic en **Aceptar**.

   ![Cuadro de diálogo Nuevo proyecto][15]

4. En la lista de **plantillas ASP.NET** del cuadro de diálogo **Nueva aplicación web ASP.NET**, haga clic en **MVC**.

   ![Selección de la aplicación web ASP.NET][16]

6. Haga clic en el botón **Cambiar autenticación**. En el cuadro de diálogo **Cambiar autenticación**, asegúrese de que esté seleccionada la opción **Sin autenticación** y luego haga clic en **Aceptar**. En este tutorial, va a implementar una aplicación que no necesita un inicio de sesión de usuario.

    ![Especificación de la autenticación][18]

7. De vuelta en el cuadro de diálogo **Nueva aplicación web ASP.NET**, haga clic en **Aceptar** para crear la aplicación de MVC.
8. Ahora debe configurar recursos de Azure para una aplicación web nueva. Siga los pasos de la [sección Publicación en Azure de este artículo](../app-service/app-service-web-get-started-dotnet-framework.md#launch-the-publish-wizard). A continuación, vuelva a este tutorial y continúe con el paso siguiente.
10. En el Explorador de soluciones, haga clic con el botón derecho en **Modelos** y, luego, en **Agregar** y, por último, en **Clase**. En el cuadro **Name**, escriba el nombre **Product.cs**. A continuación, haga clic en **Agregar**.

    ![Creación del modelo de producto][17]

### <a name="modify-the-web-application"></a>Modificación de la aplicación web

1. En el archivo Product.cs en Visual Studio, sustituya la definición del espacio de nombres existente con el código siguiente:

   ```csharp
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
       public class Product
       {
           public string Id { get; set; }
           public string Name { get; set; }
           public string Quantity { get; set; }
       }
    }
    ```
2. En el Explorador de soluciones, expanda la carpeta **Controladores** y luego haga doble clic en el archivo **HomeController.cs** para abrirlo en Visual Studio.
3. En el archivo **HomeController.cs**, sustituya la definición de espacio de nombres existente con el código siguiente:

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```
4. En el Explorador de soluciones, expanda la carpeta Views\Shared y luego haga doble clic en **_Layout.cshtml** para abrirlo en el Editor de Visual Studio.
5. Cambie todas las repeticiones de **Mi aplicación ASP.NET** por **Productos de Northwind Traders**.
6. Quite los vínculos **Página principal**, **Acerca de** y **Contacto**. En el siguiente ejemplo, elimine el código resaltado.

    ![Eliminación de los elementos de lista generados][41]

7. En el Explorador de soluciones, expanda la carpeta Views\Home y luego haga doble clic en **Index.cshtml** para abrirlo en el Editor de Visual Studio. Reemplace todo el contenido del archivo con el código siguiente:

   ```html
   @model IEnumerable<ProductsWeb.Models.Product>

   @{
            ViewBag.Title = "Index";
   }

   <h2>Prod Inventory</h2>

   <table>
             <tr>
                 <th>
                     @Html.DisplayNameFor(model => model.Name)
                 </th>
                 <th></th>
                 <th>
                     @Html.DisplayNameFor(model => model.Quantity)
                 </th>
             </tr>

   @foreach (var item in Model) {
             <tr>
                 <td>
                     @Html.DisplayFor(modelItem => item.Name)
                 </td>
                 <td>
                     @Html.DisplayFor(modelItem => item.Quantity)
                 </td>
             </tr>
   }

   </table>
   ```
8. Para comprobar la precisión del trabajo realizado hasta el momento, puede presionar **Ctrl+Mayús+B** para compilar el proyecto.

### <a name="run-the-app-locally"></a>Ejecución de la aplicación de forma local

Ejecute la aplicación para comprobar que funciona.

1. Asegúrese de que **ProductsPortal** es el proyecto activo. Haga clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones y seleccione **Establecer como proyecto de inicio**.
2. En Visual Studio, presione **F5**.
3. La aplicación debería aparecer ejecutándose en un explorador.

   ![Aplicación web][21]

## <a name="put-the-pieces-together"></a>Combinación de todos los componentes

El siguiente paso es conectar el servidor de productos local con la aplicación ASP.NET.

1. Si no está abierto, vuelva a abrir en Visual Studio el proyecto **ProductsPortal** que ha creado en la sección [Creación de una aplicación de ASP.NET](#create-an-aspnet-application).
2. Agregue el paquete NuGet a las referencias del proyecto de forma similar al paso de la sección "Creación de un servidor local". En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **ProductsPortal** y luego haga clic en **Administrar paquetes NuGet**.
3. Busque **WindowsAzure.ServiceBus** y seleccione el elemento **WindowsAzure.ServiceBus**. Después finalice la instalación y cierre este cuadro de diálogo.
4. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **ProductsPortal** y, a continuación, haga clic en **Agregar** y, finalmente, en **Elemento existente**.
5. Desplácese al archivo **ProductsContract.cs** desde el proyecto de consola **ProductsServer**. Haga clic para resaltar ProductsContract.cs. Haga clic en la flecha abajo situada junto a **Agregar** y, a continuación, haga clic en **Agregar como vínculo**.

   ![Incorporación como vínculo][24]

6. Abra ahora el archivo **HomeController.cs** en el editor de Visual Studio y reemplace la definición de espacio de nombres con el siguiente código: no olvide reemplazar *yourServiceNamespace* con el espacio de nombres de servicio, y *yourKey* con su clave de SAS. Esto permite que el cliente llame al servicio local y que se devuelva el resultado de la llamada.

   ```csharp
   namespace ProductsWeb.Controllers
   {
       using System.Linq;
       using System.ServiceModel;
       using System.Web.Mvc;
       using Microsoft.ServiceBus;
       using Models;
       using ProductsServer;

       public class HomeController : Controller
       {
           // Declare the channel factory.
           static ChannelFactory<IProductsChannel> channelFactory;

           static HomeController()
           {
               // Create shared access signature token credentials for authentication.
               channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                   "sb://yourServiceNamespace.servicebus.windows.net/products");
               channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                   TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                       "RootManageSharedAccessKey", "yourKey") });
           }

           public ActionResult Index()
           {
               using (IProductsChannel channel = channelFactory.CreateChannel())
               {
                   // Return a view of the products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```
7. En el Explorador de soluciones, haga clic con el botón derecho en la solución **ProductsPortal** (asegúrese de que hace clic en la solución, no en el proyecto). Haga clic en **Agregar** y, a continuación, en **Proyecto existente**.
8. Desplácese al proyecto **ProductsServer** y haga doble clic en la solución **ProductsServer.csproj** para agregarla.
9. **ProductsServer** debe estar en ejecución para mostrar los datos en **ProductsPortal**. En el Explorador de soluciones, haga clic con el botón derecho en la solución **ProductsPortal** y, a continuación, haga clic en **Propiedades**. Se muestra el cuadro de diálogo **Páginas de propiedades**.
10. A la izquierda, haga clic en **Proyecto de inicio**. A la derecha, haga clic en **Proyectos de inicio múltiples**. Asegúrese de que aparezcan **ProductsServer** y **ProductsPortal** en ese orden y que la acción **Iniciar** esté establecida para ambos.

      ![Proyectos de inicio múltiples][25]

11. Aún en el cuadro de diálogo **Propiedades**, haga clic en **Dependencias del proyecto** a la izquierda.
12. En la lista **Proyectos**, haga clic en **ProductsServer**. Asegúrese de que la opción **ProductsPortal** no esté seleccionada.
13. En la lista **Proyectos**, haga clic en **ProductsPortal**. Asegúrese de que **ProductsServer** esté seleccionado.

    ![Dependencias del proyecto][26]

14. En el cuadro de diálogo **Páginas de propiedades**, haga clic en **Aceptar**.

## <a name="run-the-project-locally"></a>Ejecución del proyecto de forma local

Para probar la aplicación localmente, en Visual Studio, presione **F5**. El servidor local (**ProductsServer**) se debe iniciar primero y luego la aplicación **ProductsPortal** se debe iniciar en una ventana del explorador. Esta vez verá que el inventario de productos muestra los datos recuperados del sistema local del servicio de productos.

![Aplicación web][10]

Presione **Actualizar** en la página **ProductsPortal**. Cada vez que actualice la página, verá que la aplicación de servidor muestra un mensaje cuando se llama a `GetProducts()` desde **ProductsServer**.

Cierre ambas aplicaciones antes de continuar con el paso siguiente.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Implementación del proyecto ProductsPortal en una aplicación web de Azure

El siguiente paso consiste en volver a publicar el front-end **ProductsPortal** de la aplicación web de Azure. Haga lo siguiente:

1. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **ProductsPortal** y haga clic en **Publicar**. A continuación, haga clic en **Publicar** en la página **Publicar**.

  > [!NOTE]
  > Puede que vea un mensaje de error en la ventana del explorador cuando el proyecto web **ProductsPortal** se inicie automáticamente después de la implementación. Esto es normal y se produce porque la aplicación **ProductsServer** no se está ejecutando todavía.
>
>

2. Copie la dirección URL de la aplicación web implementada, la necesitará en el paso siguiente. También puede obtener esta dirección URL de la ventana de actividad de Azure App Service en Visual Studio:

  ![Dirección URL de la aplicación implementada][9]

3. Cierre la ventana del explorador para detener la aplicación en ejecución.

### <a name="set-productsportal-as-web-app"></a>Establecimiento de ProductsPortal como aplicación web

Antes de ejecutar la aplicación en la nube, debe asegurarse de que **ProductsPortal** se inicie desde dentro de Visual Studio como una aplicación web.

1. En Visual Studio, haga clic con el botón derecho en el proyecto **ProductsPortal** y luego haga clic en **Propiedades**.
2. En la columna izquierda, haga clic en **Web**.
3. En la sección **Acción de inicio**, haga clic en el botón **Dirección URL de inicio** y, en el cuadro de texto, escriba la dirección URL de la aplicación web anteriormente implementada, por ejemplo `http://productsportal1234567890.azurewebsites.net/`.

    ![Dirección URL de inicio][27]

4. En el menú **Archivo** de Visual Studio, haga clic en **Guardar todo**.
5. En el menú Compilar de Visual Studio, haga clic en **Recompilar solución**.

## <a name="run-the-application"></a>Ejecución de la aplicación

1. Presione F5 para compilar y ejecutar la aplicación. El servidor local (la aplicación de consola **ProductsServer** ) se debe iniciar primero y, a continuación, la aplicación **ProductsPortal** debe iniciarse en una ventana del explorador, como se muestra en la captura de pantalla siguiente. Observe de nuevo que el inventario de productos enumera los datos recuperados del sistema local del servicio de producto y muestra esos datos en la aplicación web. Compruebe la dirección URL para asegurarse de que **ProductsPortal** se ejecuta en la nube como una aplicación web de Azure.

   ![Ejecute la aplicación web en Azure][1]

   > [!IMPORTANT]
   > La aplicación de consola **ProductsServer** se debe estar ejecutando y debe poder suministrar los datos a la aplicación **ProductsPortal**. Si el explorador muestra un error, espere unos segundos a que **ProductsServer** se cargue y muestre el siguiente mensaje. A continuación, presione **Actualizar** en el explorador.
   >
   >

   ![Salida del servidor][37]
2. En el explorador, presione **Actualizar** en la página **ProductsPortal**. Cada vez que actualice la página, verá que la aplicación de servidor muestra un mensaje cuando se llama a `GetProducts()` desde **ProductsServer**.

    ![Salida actualizada][38]

## <a name="next-steps"></a>Pasos siguientes
Avance al siguiente tutorial: 

> [!div class="nextstepaction"]
>[Exposición de un servicio WCF local en un cliente WCF fuera de la red](service-bus-relay-tutorial.md)

[0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
[1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[NuGet]: http://nuget.org

[11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
[13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
[15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
[16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
[17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
[18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
[9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
[10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

[21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
[24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
[25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
[26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
[27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png

[36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
[38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
[41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
[43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png

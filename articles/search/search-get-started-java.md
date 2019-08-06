---
title: 'Inicio rápido: Creación de un índice de Azure Search en Java'
description: Se explica cómo crear un índice, cargar datos y ejecutar consultas mediante Java y las API REST de Azure Search.
services: search
author: jj09
manager: jlembicz
ms.service: search
ms.topic: conceptual
ms.date: 08/26/2018
ms.author: jjed
ms.custom: seodec2018, seo-java-july2019
ms.openlocfilehash: 7172cd01ca881ec3027854444107b0744b65feb3
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489782"
---
# <a name="quickstart-create-an-azure-search-index-in-java"></a>Inicio rápido: Creación de un índice de Azure Search en Java
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Aprenda a crear una aplicación de búsqueda de Java personalizada que utiliza Azure Search para la experiencia de búsqueda. Este tutorial usa la [API de REST del servicio Azure Search](https://msdn.microsoft.com/library/dn798935.aspx) para construir los objetos y las operaciones que se utilizan en este ejercicio.

Para ejecutar este ejemplo, debe tener un servicio Azure Search, al que puede suscribirse en [Azure Portal](https://portal.azure.com). Para obtener instrucciones detalladas, consulte [Creación de un servicio Azure Search en Azure Portal](search-create-service-portal.md).

Hemos usado el siguiente software para compilar y probar este ejemplo:

* [IDE de Eclipse para desarrolladores de Java EE](https://www.eclipse.org/downloads/packages/release/photon/r/eclipse-ide-java-ee-developers). Asegúrese de descargar la versión EE. Uno de los pasos de comprobación requiere una característica que solo se encuentra en esta edición.
* [JDK 8u181](https://aka.ms/azure-jdks)
* [Apache Tomcat 8.5.33](https://tomcat.apache.org/download-80.cgi#8.5.33)

## <a name="about-the-data"></a>Acerca de los datos
Esta aplicación de ejemplo usa los datos del [Servicio geológico de Estados Unidos (USGS)](https://geonames.usgs.gov/domestic/download_data.htm), filtrados por el estado de Rhode Island para reducir el tamaño del conjunto de datos. Vamos a usar estos datos para crear una aplicación de búsqueda que devuelva edificios de referencia como hospitales y escuelas, además de características geológicas como ríos, lagos y montes.

En esta aplicación, el programa **SearchServlet.java** compila y carga el índice utilizando un constructo de [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) y obtiene el conjunto de datos filtrado de USGS desde una base de datos de Azure SQL. En el código del programa se proporcionan credenciales predefinidas e información de conexión al origen de datos en línea. En términos de acceso a datos, no es necesario realizar ninguna otra configuración.

> [!NOTE]
> Se aplicó un filtro a este conjunto de datos para no sobrepasar el límite de 10.000 documentos del nivel de precios gratuito. Si utiliza el nivel estándar, este límite no se aplica y puede modificar el código para utilizar un conjunto de datos más grande. Para obtener más información acerca de la capacidad de cada nivel de precios, consulte [Límites y restricciones](search-limits-quotas-capacity.md).
> 
> 

## <a name="about-the-program-files"></a>Acerca de los archivos del programa
La lista siguiente describe los archivos que son relevantes para este ejemplo.

* Search.jsp: proporciona la interfaz de usuario
* SearchServlet.java: proporciona métodos (similar a un controlador de MVC)
* SearchServiceClient.java: controla las solicitudes HTTP
* SearchServiceHelper.java: clase auxiliar que proporciona métodos estáticos
* Document.java: proporciona el modelo de datos
* config.properties: establece la dirección URL del servicio Search y `api-key`
* pom.xml: una dependencia de Maven

<a id="sub-2"></a>

## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Búsqueda del nombre del servicio y `api-key` del servicio Azure Search
Todas las llamadas de API REST en Azure Search requieren que proporcione la dirección URL del servicio y una `api-key`. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En la barra de acceso rápido, haga clic en **Servicio de búsqueda** para enumerar todos los servicios de Azure Search aprovisionados para la suscripción.
3. Seleccione el servicio que desea utilizar.
4. En el panel del servicio verá mosaicos con información esencial, así como el icono de llave para tener acceso a las claves de administrador.
   
      ![Captura de pantalla que muestra cómo obtener acceso a las claves de administración desde el panel de servicios][3]
5. Copie la dirección URL del servicio y una clave de administración. Las necesitará más adelante para agregarlas al archivo **config.properties** .

## <a name="download-the-sample-files"></a>Descarga de los archivos de ejemplo
1. Vaya a [search-java-indexer-demo](https://github.com/Azure-Samples/search-java-indexer-demo) en GitHub.
2. Haga clic en **Download ZIP**, guarde el archivo .zip en el disco y extraiga todos los archivos que contiene. Es recomendable que extraiga los archivos en su área de trabajo de Java para que le resulte más fácil encontrar el proyecto más adelante.
3. Los archivos de ejemplo son de solo lectura. Haga clic con el botón secundario en las propiedades de la carpeta y borre el atributo de sólo lectura.

Todas las modificaciones y las instrucciones de ejecución subsiguientes se realizarán en los archivos de esta carpeta.  

## <a name="import-project"></a>Importación del proyecto
1. En Eclipse, elija **Archivo** > **Importar** > **General** > **Proyectos existentes al área de trabajo**.
   
    ![Captura de pantalla que muestra cómo importar un proyecto existente][4]
2. En **Select root directory**(Seleccionar directorio raíz), vaya a la carpeta que contiene los archivos de ejemplo. Seleccione la carpeta que contiene la carpeta .project. El proyecto debe aparecer en la lista **Projects** (Proyectos) como elemento seleccionado.
   
    ![Captura de pantalla que muestra la lista de proyectos en la ventana Importar proyectos][12]
3. Haga clic en **Finalizar**
4. Utilice **Project Explorer** (Explorador de proyectos) para ver y editar los archivos. Si aún no está abierto, haga clic en **Ventana** > **Mostrar vista** > **Explorador de proyectos** o use el método abreviado para abrirlo.

## <a name="configure-the-service-url-and-api-key"></a>Configuración de la dirección URL del servicio y `api-key`
1. En el **Explorador de proyectos**, haga doble clic en **config.properties** para editar la configuración que contiene el nombre del servidor y `api-key`.
2. Consulte los pasos anteriores de este artículo, donde encontró la dirección URL del servicio y `api-key` en [Azure Portal](https://portal.azure.com), para obtener los valores que ahora va a especificar en **config.properties**.
3. En **config.properties**, reemplace "API Key" por la `api-key` del servicio. A continuación, el nombre de servicio (el primer componente de la dirección URL https://servicename.search.windows.net) reemplaza "nombre de servicio" en el mismo archivo.
   
    ![Captura de pantalla que muestra cómo reemplazar la clave de API][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Configuración de los entornos de proyecto, compilación y tiempo de ejecución
1. En Eclipse, en Project Explorer, haga clic con el botón secundario en el proyecto > **Propiedades** > **Facetas del proyecto**.
2. Seleccione **Dynamic Web Module**, **Java** y **JavaScript**.
   
    ![Captura de pantalla que muestra cómo seleccionar las facetas de proyecto para su proyecto][6]
3. Haga clic en **Aplicar**.
4. Seleccione **Ventana** > **Preferencias** > **Servidor** > **Entornos en tiempo de ejecución** > **Agregar...** .
5. Expanda Apache y seleccione la versión del servidor Apache Tomcat que ha instalado anteriormente. En nuestro sistema, se instala la versión 8.
   
    ![Captura de pantalla que muestra la ubicación de la ventana Runtime Environment (Entorno de tiempo de ejecución) en la que puede seleccionar la versión de Apache Tomcat][7]
6. En la página siguiente, especifique el directorio de instalación de Tomcat. En un equipo Windows, probablemente será C:\Program Files\Apache Software Foundation\Tomcat *versión*.
7. Haga clic en **Finalizar**
8. Seleccione **Ventana** > **Preferencias** > **Java** > **JRE instalados** > **Agregar**.
9. En **Add JRE** (Agregar JRE), seleccione **Standard VM** (VM estándar).
10. Haga clic en **Next**.
11. En JRE Definition (Definición de JRE), en JRE home (Directorio de JRE), haga clic en **Directory**(Directorio).
12. Vaya a **Archivos de programa** > **Java** y seleccione el JDK instalado anteriormente. Es importante seleccionar el JDK como JRE.
13. En Installed JREs, elija el **JDK**. Su configuración debería ser similar a la siguiente captura de pantalla.
    
    ![Captura de pantalla que muestra cómo seleccionar JDK como JRE instalado][9]
14. Opcionalmente, seleccione **Ventana** > **Explorador web** > **Internet Explorer** para abrir la aplicación en una ventana del explorador externo. Utilizar un explorador externo proporciona una mejor experiencia de aplicación web.
    
    ![Captura de pantalla que muestra cómo seleccionar Internet Explorer como una ventana de exploración externa][8]

Ahora ha completado las tareas de configuración. A continuación, podrá compilar y ejecutar el proyecto.

## <a name="build-the-project"></a>Compilación del proyecto
1. En el Explorador de proyectos, haga clic con el botón secundario en el nombre del proyecto y elija **Ejecutar como** > **Compilación de Maven...** para configurar el proyecto.
   
    ![Captura de pantalla que muestra cómo elegir la compilación de Maven en la ventana Explorador de proyectos][10]
2. En Edit Configuration (Editar configuración), en la sección Goals (Objetivos), escriba "clean install" y, a continuación, haga clic en **Run**(Ejecutar).

Los mensajes de estado se envían a la ventana de consola. Debería ver BUILD SUCCESS, lo que indica que el proyecto se ha compilado sin errores.

## <a name="run-the-app"></a>Ejecución de la aplicación
En este último paso, ejecutará la aplicación en un entorno de tiempo de ejecución del servidor local.

Si todavía no ha especificado un entorno de tiempo de ejecución del servidor en Eclipse, deberá hacerlo en primer lugar.

1. En Project Explorer, expanda **WebContent**(Contenido web).
2. Haga clic con el botón secundario en **Search.jsp** > **Ejecutar como** > **Ejecutar en el servidor**. Seleccione el servidor Apache Tomcat y, a continuación, haga clic en **Run**(Ejecutar).

> [!TIP]
> Si ha usado un área de trabajo distinta de la predeterminada para almacenar el proyecto, tendrá que modificar **Configuración de ejecución** para que apunte a la ubicación del proyecto y evitar un error de inicio del servidor. En el Explorador de proyectos, haga clic en **Search.jsp** > **Ejecutar como** > **Configuraciones de ejecución**. Seleccione el servidor Apache Tomcat. Haga clic en **Arguments**(Argumentos). Haga clic en **Workspace** (Área de trabajo) o en **File System** (Sistema de archivos) para definir la carpeta que contiene el proyecto.
> 
> 

Al ejecutar la aplicación, aparecerá una ventana del explorador que ofrece un cuadro de búsqueda para introducir términos.

Aguarde un minuto antes de hacer clic en **Search** (Buscar) para que el servicio tenga tiempo de crear y cargar el índice. Si obtiene un error HTTP 404, sólo necesitará esperar un poco más antes de intentarlo.

## <a name="search-on-usgs-data"></a>Buscar en los datos de USGS
El conjunto de datos de USGS incluye los registros que son relevantes para el estado de Rhode Island. Si hace clic en **Search** en un cuadro de búsqueda vacío, obtendrá las 50 primeras entradas (es el valor predeterminado).

Escriba un término de búsqueda para que el motor de búsqueda tenga con qué trabajar. Pruebe a escribir un nombre regional. "Roger Williams" fue el primer gobernador de Rhode Island. Hay numerosos parques, edificios y escuelas que llevan su nombre.

![Captura de pantalla que muestra cómo buscar en los datos de USGS][11]

También puede probar con alguno de estos términos:

* Pawtucket
* Pembroke
* goose +cape

## <a name="next-steps"></a>Pasos siguientes
Este es el primer tutorial de Azure Search basada en Java y en el conjunto de datos de USGS. Con el tiempo, ampliaremos este tutorial para mostrar otras características de búsqueda que podría querer usar en sus soluciones personalizadas.

Si ya Tiene alguna experiencia con Azure Search, puede utilizar este ejemplo como punto de partida para experimentación adicional, por ejemplo, aumentando la [página de búsqueda](search-pagination-page-layout.md), o implementando la [navegación por facetas](search-faceted-navigation.md). También puede mejorar la página de resultados de búsqueda si agrega recuentos y procesamiento por lotes de documentos para que los usuarios puedan navegar por las páginas de resultados.

¿Es la primera vez que usa Azure Search? Le recomendamos que pruebe otros tutoriales para comprender mejor lo que puede crear. Visite nuestra [página de documentación](https://azure.microsoft.com/documentation/services/search/) para encontrar más recursos. 

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png

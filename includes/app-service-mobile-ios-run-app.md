1. En el Mac, vaya a [Azure Portal]. Haga clic en **Todos los servicios** > **App Services** > el back-end que acaba de crear. En la configuración de la aplicación móvil, elija su lenguaje preferido:

    - Objective-C &ndash; **Inicio rápido** > **iOS (Objective-C)**
    - Swift &ndash; **Inicio rápido** > **iOS (Swift)**

    En **3. Configurar la aplicación cliente**, haga clic en **Descargar**. Esto descarga un proyecto de Xcode completo configurado previamente para conectarse a su back-end. Abra el proyecto con Xcode.

1. Presione el botón **Ejecutar** para compilar el proyecto e iniciar la aplicación en el simulador de iOS.

1. En la aplicación, escriba un texto significativo, como *Complete el tutorial* y, a continuación, haga clic en el icono de signo de suma (**+**). Esto envía una solicitud POST al back-end de Azure implementado anteriormente. El back-end inserta datos de la solicitud que está en la tabla TodoItem SQL y devuelve información acerca de los elementos recién almacenados a la aplicación móvil. La aplicación móvil muestra estos datos en la lista.

   ![Aplicación de inicio rápido que se ejecuta en iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure Portal]: https://portal.azure.com/

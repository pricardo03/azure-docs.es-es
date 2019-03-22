---
title: 'Conectarse a aplicaciones de 3270 en IBM mainframes con Azure: Azure Logic Apps'
description: Integrar y automatizar 3270 controlado por la pantalla de aplicaciones con Azure mediante el conector de Azure Logic Apps y IBM 3270
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: estfan, valthom
ms.topic: article
ms.date: 03/06/2019
tags: connectors
ms.openlocfilehash: 7388dc0c61dad9c31da0c178febcee4c8481bc50
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313199"
---
# <a name="integrate-3270-screen-driven-apps-on-ibm-mainframes-with-azure-by-using-azure-logic-apps-and-ibm-3270-connector"></a>Integrar 3270 aplicaciones controladas por la pantalla de IBM mainframes con Azure mediante el conector de Azure Logic Apps y IBM 3270

> [!NOTE]
> Este conector está en [ *versión preliminar pública*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Con Azure Logic Apps y el conector de IBM 3270, puede acceder y ejecutar aplicaciones de gran sistema IBM que normalmente la unidad navegando por 3270 pantallas del emulador. De este modo, puede integrar sus aplicaciones de IBM mainframe con Azure, Microsoft y otras aplicaciones, servicios y sistemas mediante la creación de flujos de trabajo automatizados con Azure Logic Apps. El conector se comunica con IBM mainframes con el protocolo TN3270 y está disponible en todas las regiones de Azure Logic Apps, excepto Azure Government y Azure China 21Vianet. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

En este artículo se describe estos aspectos para usar el conector de 3270: 

* ¿Por qué usar el conector de IBM 3270 en Azure Logic Apps y cómo ejecuta el conector a aplicaciones controladas por pantalla 3270

* Los requisitos previos y el programa de instalación para usar el conector de 3270

* Los pasos para agregar 3270 acciones del conector a la aplicación lógica

## <a name="why-use-this-connector"></a>¿Por qué usar este conector?

Para obtener acceso a las aplicaciones de IBM mainframes, normalmente se utiliza un emulador de terminal 3270, a menudo denominado "pantalla verde". Este método es una manera comprobada, pero tiene limitaciones. Aunque ayuda a Host Integration Server (HIS) que trabaja directamente con estas aplicaciones, a veces, la separación de la pantalla y la lógica empresarial no es posible que sea posible. O bien, quizá ya no tiene información para el funcionan de las aplicaciones host.

Para ampliar estos escenarios, el conector IBM 3270 en Azure Logic Apps funciona con la herramienta de diseño 3270, que se usa para el registro o "capturar", las pantallas de host utilizadas para una tarea específica, definir el flujo de navegación para esa tarea a través de la aplicación de gran sistema y definir el métodos con parámetros de entrada y salida de esa tarea. La herramienta de diseño convierte esa información en los metadatos que utiliza el conector de 3270 al llamar a una acción que representa esa tarea desde la aplicación lógica.

Después de generar el archivo de metadatos de la herramienta de diseño, agregue ese archivo a una cuenta de integración de Azure. De este modo, la aplicación lógica puede tener acceso a los metadatos de la aplicación al agregar una acción del conector 3270. El conector lee el archivo de metadatos de la cuenta de integración, controla la navegación a través de las 3270 pantallas y presenta dinámicamente los parámetros para la acción del conector 3270. A continuación, puede proporcionar datos a la aplicación host y el conector devuelve los resultados a la aplicación lógica. De este modo, puede integrar las aplicaciones heredadas con Azure, Microsoft y otras aplicaciones, servicios y sistemas compatibles con Azure Logic Apps.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Se recomienda: Un [entorno del servicio de integración (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) 

  Puede seleccionar este entorno como la ubicación para crear y ejecutar la aplicación lógica. Una instancia de ISE proporciona acceso desde la aplicación lógica a recursos protegidos dentro de redes virtuales de Azure.

* La aplicación lógica a usar para automatizar y ejecutar la aplicación controlada por la pantalla de 3270

  El conector IBM 3270 no tiene desencadenadores, así que use otro desencadenador para iniciar la aplicación lógica, como el **periodicidad** desencadenador. A continuación, puede agregar 3270 acciones del conector. Para empezar a trabajar, [crear una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). 
  Si usa una instancia de ISE, seleccione esa ISE como ubicación de la aplicación lógica.

* [Descargue e instale la herramienta de diseño 3270](https://aka.ms/3270-design-tool-download).
Es el único requisito previo [Microsoft .NET Framework 4.6.1](https://aka.ms/net-framework-download).

  Esta herramienta le ayuda a registrar las pantallas, las rutas de navegación, métodos y parámetros para las tareas en la aplicación que se agrega y se ejecuta como 3270 acciones del conector. La herramienta genera un archivo de Host Integration Designer XML (HIDX) que proporciona los metadatos necesarios para el conector que se usará para la aplicación de gran sistema de conducción.
  
  Después de descargar e instalar esta herramienta, siga estos pasos para conectarse a su host:

  1. Abra la herramienta de diseño 3270. Desde el **sesión** menú, seleccione **sesiones Host**.
  
  1. Proporcionar el host de TN3270 información del servidor.

* Un [cuenta de integración](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), que es el lugar donde almacenar el archivo HIDX como un mapa para la aplicación lógica pueda acceder a las definiciones de metadatos y el método en ese archivo. 

  Asegúrese de que la cuenta de integración está vinculada a la aplicación lógica que está usando. También, si usa una instancia de ISE, asegúrese de que la ubicación de la cuenta de integración es la mismo ISE que usa la aplicación lógica.

* Acceso al servidor TN3270 que hospeda la aplicación del sistema central

<a name="define-app-metadata"></a>

## <a name="create-metadata-overview"></a>Crear información general sobre metadatos

En una aplicación controlada por pantalla 3270, las pantallas y los campos de datos son únicos para sus escenarios, por lo que el conector de 3270 necesita esta información acerca de la aplicación, que puede proporcionar como metadatos. Estos metadatos describen la información que ayuda a la aplicación lógica a identificar y reconocer las pantallas, se describe cómo navegar entre pantallas, dónde los datos de entrada y dónde esperar los resultados. Para especificar y generar estos metadatos, utilice la herramienta de diseño 3270, que le guiará a través de particulares *modos*, o fases, tal como se describe más adelante con más detalle:

* **Capturar**: En este modo, registre las pantallas para completar una tarea específica con la aplicación de gran sistema, por ejemplo, obteniendo un saldo bancario.

* **Navegación**: En este modo, especifique el plan o la ruta de acceso para saber cómo navegar por las pantallas de la aplicación del sistema central para una tarea específica.

* **Métodos**: En este modo, se define el método, por ejemplo, `GetBalance`, que describe la ruta de navegación de pantalla. También elegir los campos de cada pantalla que se convierten en la entrada del método y los parámetros de salida.

### <a name="unsupported-elements"></a>Elementos no admitidos

La herramienta de diseño no admite estos elementos:

* Mapas de soporte técnico de asignación básica de IBM (BMS) parcial: Si importa un mapa BMS, la herramienta de diseño pasa por alto las definiciones parciales de pantalla.
* Parámetros: de entrada/salida No se puede definir los parámetros In/Out.
* Procesamiento de menú: No se admite durante la versión preliminar
* Procesamiento de la matriz: No se admite durante la versión preliminar

<a name="capture-screens"></a>

## <a name="capture-screens"></a>Captura de pantallas

En este modo, se marca un elemento en cada pantalla 3270 que identifica de forma que la pantalla. Por ejemplo, podría especificar una línea de texto o un conjunto más complejo de condiciones, como un texto específico y un campo vacío. Puede registrar estas pantallas a través de una conexión dinámica con el servidor host, o puede importar esta información desde un mapa de soporte técnico de asignación básica (BMS) de IBM. La conexión dinámica usa un emulador TN3270 para conectarse al host. Cada acción del conector debe asignarse a una sola tarea que se inicia con la conexión a la sesión y termina con la desconexión de la sesión.

1. Si no lo ha hecho ya, abra la herramienta de diseño 3270. En la barra de herramientas, elija **capturar** para que pueda especificar el modo de captura.

1. Para iniciar la grabación, presione la tecla F5, o desde el **grabación** menú, seleccione **Iniciar grabación**. 

1. Desde el **sesión** menú, seleccione **Connect**.

1. En el **capturar** panel, a partir de la primera pantalla de la aplicación, paso a través de la aplicación para la tarea concreta que se está grabando.

1. Después de finalizar la tarea, cierre la sesión de la aplicación como lo haría normalmente.

1. Desde el **sesión** menú, seleccione **desconexión**.

1. Para detener la grabación, presione la tecla MAYÚS + F5 claves, o desde el **grabación** menú, seleccione **Detener grabación**.

   Después de capturar las pantallas para una tarea, la herramienta del diseñador muestra las miniaturas que representan las pantallas. Algunas notas acerca de estas miniaturas:

   * Se incluye en sus pantallas capturadas, tiene una pantalla que se denomina "Empty".

     Cuando primero se conectará a [CICS](https://www.ibm.com/it-infrastructure/z/cics), debe enviar la clave "Borrar" antes de que puede escribir el nombre de la transacción que desea ejecutar. La pantalla donde enviar la clave "Borrar" no tiene ninguna *atributos reconocimiento*, por ejemplo, un título de la pantalla, que puede agregar mediante el editor de reconocimiento de la pantalla. Para representar esta pantalla, las miniaturas incluye una pantalla denominada "Vacía". Más adelante puede utilizar esta pantalla para representar la pantalla donde escribir el nombre de transacción.

   * De forma predeterminada, el nombre de una pantalla capturada utiliza la primera palabra en la pantalla. Si ese nombre ya existe, la herramienta de diseño anexa el nombre con un carácter de subrayado y un número, por ejemplo, "WBGB" y "WBGB_1".

1. Para dar un nombre más significativo a una pantalla capturada, siga estos pasos:

   1. En el **Host pantallas** panel, seleccione la pantalla que desea cambiar el nombre.

   1. En el mismo panel, en la parte inferior en el mismo panel, busque el **nombre de pantalla** propiedad.

   1. Cambie el nombre de la pantalla actual a un nombre más descriptivo.

1. Ahora, especifique los campos para la identificación de cada pantalla.

   Con la secuencia de 3270 datos pantallas no tienen identificadores de forma predeterminada, por lo que deberá seleccionar texto único en cada pantalla. Para escenarios complejos, puede especificar varias condiciones, por ejemplo, texto único y un campo con una condición específica.

Cuando termine de seleccionar los campos de reconocimiento, pasar al modo siguiente.

### <a name="conditions-for-identifying-repeated-screens"></a>Condiciones para identificar las pantallas repetidas

Para que el conector navegar y diferenciar entre las pantallas, normalmente encontrar texto únicos en una pantalla que puede usar como identificador entre las pantallas capturadas. Para pantallas repetidas, puede que necesite más métodos de identificación. Por ejemplo, suponga que tiene dos pantallas que tienen el mismo aspecto excepto en una pantalla devuelve un valor válido, mientras que la otra pantalla devuelve un mensaje de error.

En la herramienta de diseño, puede agregar *atributos reconocimiento*, por ejemplo, un título de pantalla como "Obtener el saldo de cuenta", mediante el editor de reconocimiento de la pantalla. Si tiene una ruta de acceso bifurcado y ambas ramas devuelven la misma pantalla, pero con resultados diferentes, necesita otros atributos de reconocimiento. En tiempo de ejecución, el conector utiliza estos atributos para determinar la rama actual y la bifurcación. Estas son las condiciones que se puede usar:

* Valor específico: Este valor coincide con la cadena especificada en la ubicación especificada.
* NO es un valor específico: Este valor no coincide con la cadena especificada en la ubicación especificada.
* Vacío: Este campo está vacío.
* NO está vacío: Este campo no está vacío.

Para obtener más información, consulte el [plan de navegación de ejemplo](#example-plan) más adelante en este tema.

<a name="define-navigation"></a>

## <a name="define-navigation-plans"></a>Definir planes de navegación

En este modo, definir el flujo o los pasos para navegar por las pantallas de la aplicación del sistema central para una tarea específica. Por ejemplo, en ocasiones, podría tener más de una ruta de acceso que puede tomar la aplicación donde produce el resultado correcto, en una ruta de acceso mientras la otra ruta de acceso produce un error. Para cada pantalla, especifique las pulsaciones de teclas necesarias para desplazarse a la siguiente pantalla, como `CICSPROD <enter>`.

> [!TIP]
> Si desea automatizar varias tareas que usan el mismo conectar y desconexión las pantallas, la herramienta de diseño proporciona tipos especiales de plan de conectar y desconectar. Al definir estos planes, puede agregarlos al principio y al final de su plan de navegación.

### <a name="guidelines-for-plan-definitions"></a>Directrices para las definiciones de plan

* Incluir todas las pantallas, empezando por la conexión y termina con la desconexión.

* Puede crear un plan independiente o usar los planes de conectar y desconectar, que le permiten volver a usar una serie de pantallas común a todas las transacciones.

  * La última pantalla en el plan de conexión debe ser la misma pantalla como la primera pantalla en el plan de navegación.

  * La primera pantalla en el plan de desconexión debe ser la misma pantalla de la última pantalla en el plan de navegación.

* Sus pantallas capturadas puedan contener muchas pantallas repetidas, así que seleccione y usar solo una instancia de cualquiera de las pantallas repetida en el plan. Estos son algunos ejemplos de pantallas repetidas:

  * El inicio de sesión en la pantalla, por ejemplo, el **MSG 10** pantalla
  * La pantalla de bienvenida para CICS
  * El "Clear" o **vacía** pantalla

<a name="create-plans"></a>

### <a name="create-plans"></a>Creación de planes

1. En la barra de herramientas de la herramienta de diseño 3270, elija **navegación** para que pueda especificar el modo de navegación.

1. Para iniciar el plan, en la **navegación** panel, elija **nuevo Plan**.

1. En **elegir nuevo nombre del Plan**, escriba un nombre para el plan. Desde el **tipo** lista, seleccione el tipo de plan:

   | Tipo de plan | DESCRIPCIÓN |
   |-----------|-------------|
   | **Proceso** | Para los planes combinados o independiente |
   | **Conexión** | Para los planes de Connect |
   | **Desconectar** | Para los planes de desconexión |
   |||

1. Desde el **Host pantallas** panel, arrastre las miniaturas capturadas al plan de navegación exponen en la **navegación** panel.

   Para representar la pantalla en blanco donde escribir el nombre de transacción, use la pantalla "Vacía".

1. Organice las pantallas en el orden en que se describe la tarea que se está definiendo.

1. Para definir la ruta de flujo entre las pantallas, incluidas las bifurcaciones y combinaciones, en la barra de herramientas de la herramienta de diseño, elija **flujo**.

1. Elija la primera pantalla en el flujo. Arrastrar y dibujar una conexión a la pantalla siguiente en el flujo.

1. Para cada pantalla, proporcione los valores para el **clave de Ayuda** propiedad (identificador de atención) y para el **texto fijo** propiedad, que mueve el flujo a la pantalla siguiente.

   Podría tener sólo la clave de ayuda, o la clave de ayuda y texto fijo.

Después de finalizar el plan de navegación, puede [definir métodos en el siguiente modo](#define-method).

<a name="example-plan"></a>

### <a name="example"></a>Ejemplo

En este ejemplo, suponga que ejecuta una transacción CICS denominada "WBGB" que tenga estos pasos: 

* En la primera pantalla, escriba un nombre y una cuenta.
* En la segunda pantalla, obtener el saldo de cuenta.
* Salga a la pantalla "Vacía".
* Cerrar la sesión de CICS a la pantalla "MSG-10".

Supongamos también que repita estos pasos, pero escriba datos incorrectos, por lo que puede capturar la pantalla que muestra el error. Estas son las pantallas de que captura:

* MSG-10
* Bienvenida CICS
* Vacío
* WBGB_1 (entrada)
* WBGB_2 (error)
* Empty_1
* MSG-10_1

Aunque muchas pantallas a obtención los nombres únicos, algunas pantallas son la misma pantalla, por ejemplo, "MSG-10" y "Empty". Para una pantalla repetida, use solo una instancia de esa pantalla en el plan. Estos son ejemplos que muestran el aspecto que podría tener un plan independiente, plan Connect, Disconnect plan y un plan de combinación:

* Plan independiente

  ![Plan de exploración independiente](./media/connectors-create-api-3270/standalone-plan.png)

* Conectar un plan

  ![Conectar un plan](./media/connectors-create-api-3270/connect-plan.png)

* Desconecte el plan

  ![Desconecte el plan](./media/connectors-create-api-3270/disconnect-plan.png)

* Plan combinado

  ![Plan combinado](./media/connectors-create-api-3270/combined-plan.png)

#### <a name="example-identify-repeated-screens"></a>Ejemplo: Identificar las pantallas repetidas

Para que el conector navegar y diferenciar las pantallas, normalmente encontrar texto únicos en una pantalla que puede usar como identificador en las pantallas capturadas. Para pantallas repetidas, puede que necesite más métodos de identificación. El plan de ejemplo tiene una bifurcación que aporte pantallas con un aspecto similares. Una pantalla devuelve un saldo de cuenta, mientras que la otra pantalla devuelve un mensaje de error.

La herramienta de diseño le permite agregar atributos de reconocimiento, por ejemplo, un título de pantalla denominada "Obtener el saldo de cuenta", mediante el reconocimiento de la pantalla editor. En el caso de las pantallas similares, necesita otros atributos. En tiempo de ejecución, el conector utiliza estos atributos para determinar la rama y la bifurcación.

* En la rama que devuelve una entrada válida, que es la pantalla con el saldo de cuenta, puede agregar un campo que tiene una condición de "no" vacía".

* En la rama que devuelva un error, puede agregar un campo que tiene una condición "vacía".

<a name="define-method"></a>

## <a name="define-methods"></a>Definir métodos

En este modo, se define un método que está asociado con el plan de navegación. Para cada parámetro de método, especifique el tipo de datos, como cadena, entero, fecha u hora y así sucesivamente. Cuando haya terminado, puede probar el método en el host en vivo y confirmar que el método funciona según lo previsto. A continuación, generar el archivo de metadatos o el archivo de Host Integration Designer XML (HIDX), que ahora tiene las definiciones de método que se usará para crear y ejecutar una acción para el conector IBM 3270.

1. En la barra de herramientas de la herramienta de diseño 3270, elija **métodos** para que pueda especificar el modo de métodos. 

1. En el **navegación** panel, seleccione la pantalla que tiene los campos de entrada que desee.

1. Para agregar el primer parámetro de entrada para el método, siga estos pasos:

   1. En el **capturar** panel, en la pantalla del emulador 3270, elija el campo completo, no solo texto dentro del campo, que desea como la primera entrada.

      > [!TIP]
      > Para mostrar todos los campos y asegúrese de seleccionar el campo completo, en el **vista** menú, seleccione **todos los campos**.

   1. En la barra de herramientas de la herramienta de diseño, elija **campo Input**. 

   Para agregar más parámetros de entrada, repita los pasos anteriores para cada parámetro.

1. Para agregar el primer parámetro de salida para el método, siga estos pasos:

   1. En el **capturar** panel, en la pantalla del emulador 3270, elija el campo completo, no solo texto dentro del campo, que desea como la primera salida.

      > [!TIP]
      > Para mostrar todos los campos y asegúrese de seleccionar el campo completo, en el **vista** menú, seleccione **todos los campos**.

   1. En la barra de herramientas de la herramienta de diseño, elija **campo de salida**.

   Para agregar más parámetros de salida, repita los pasos anteriores para cada parámetro.

1. Después de agregar todos los parámetros del método, defina estas propiedades para cada parámetro:

   | Nombre de propiedad | Valores posibles | 
   |---------------|-----------------|
   | **Tipo de datos** | Byte, DateTime, Decimal, Int, Long, Short, String |
   | **Técnica de campo de relleno** | Los parámetros admiten estos tipos de relleno, rellenar con espacios en blanco si es necesario: <p><p>- **Tipo**: Escriba caracteres secuencialmente en el campo. <p>- **Rellenar**: Reemplace el contenido del campo con caracteres adicionales, rellenar con espacios en blanco si es necesario. <p>- **EraseEofType**: Desactive el campo y, a continuación, escriba caracteres secuencialmente en el campo. |
   | **Cadena de formato** | Algunos tipos de datos de parámetro usan una cadena de formato, que informa el conector 3270 cómo convertir texto de la pantalla en un tipo de datos. NET: <p><p>- **DateTime**: La cadena de formato de fecha y hora sigue el [cadenas con formato de .NET fecha y hora personalizado](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Por ejemplo, la fecha `06/30/2019` usa la cadena de formato `MM/dd/yyyy`. <p>- **Decimal**: Usa la cadena de formato decimal el [cláusula COBOL Picture](https://www.ibm.com/support/knowledgecenter/SS6SG3_5.2.0/com.ibm.cobol52.ent.doc/PGandLR/ref/rlddepic.html). Por ejemplo, el número `100.35` usa la cadena de formato `999V99`. |
   |||

## <a name="save-and-view-metadata"></a>Guardar y ver los metadatos

Después de definir el método, pero antes de probar el método, guarde toda la información que ha definido hasta ahora en un archivo RAP (.rap).
Puede guardar este archivo RAP en cualquier momento durante cualquier modo. La herramienta de diseño también incluye un archivo de la RAP de ejemplo que puede abrir y revisar, diríjase a la carpeta de instalación de la herramienta de diseño en esta ubicación y abrir el archivo "WoodgroveBank.rap":

`..\Program Files\Microsoft Host Integration Server - 3270 Design Tool\SDK\WoodgroveBank.rap`

Sin embargo, si intenta guardar los cambios realizados en el archivo de ejemplo RAP o generar un archivo HIDX del archivo de ejemplo RAP mientras el archivo se encuentra en la carpeta de instalación de la herramienta de diseño, es posible que obtenga un error "acceso denegado". De forma predeterminada, la herramienta de diseño se instala en la carpeta archivos de programa sin permisos elevados. Si se produce un error, intente una de estas soluciones:

* Copie el archivo de ejemplo en una ubicación diferente.
* Ejecute la herramienta de diseño como administrador.
* Asegúrese del propietario de la carpeta del SDK.

## <a name="test-your-method"></a>El método de prueba

1. Para ejecutar el método en el host en vivo, mientras sigue en modo de métodos, presione la tecla F5 o, en la barra de herramientas de la herramienta de diseño, elija **ejecutar**.

   > [!TIP]
   > Puede cambiar los modos en cualquier momento. En el **archivo** menú, seleccione **modo**y, a continuación, seleccione el modo deseado.

1. Escriba el valor de los parámetros y elija **Aceptar**.

1. Para continuar a la siguiente pantalla, elija **siguiente**.

1. Cuando haya terminado, elija **realiza**, que muestra los valores de parámetro de salida.

<a name="add-metadata-integration-account"></a>

## <a name="generate-and-upload-hidx-file"></a>Generar y cargar el archivo HIDX

Cuando esté listo, generar el archivo HIDX, por lo que puede cargar para la cuenta de integración. La herramienta de diseño de 3270 crea el archivo HIDX en una nueva subcarpeta donde guardó el archivo RAP.

1. En la barra de herramientas de la herramienta de diseño 3270, elija **generar código**.

1. Vaya a la carpeta que contiene el archivo RAP y abra la subcarpeta que creó la herramienta después de generar el archivo HIDX. Confirme que la herramienta crea el archivo HIDX.

1. Inicie sesión en el [portal Azure](https://portal.azure.com)y busque la cuenta de integración.

1. Agregar el archivo HIDX como un mapa a su cuenta de integración por [siga estos pasos similares para agregar mapas](../logic-apps/logic-apps-enterprise-integration-liquid-transform.md), pero cuando se selecciona el tipo de mapa, seleccione **HIDX**.

Más adelante en este tema, al agregar una acción de IBM 3270 a la aplicación lógica por primera vez, se le pedirá que cree una conexión entre la aplicación lógica y el servidor host, ya que proporciona información de conexión, como los nombres para el servidor de host y la cuenta de integración . Después de crear la conexión, puede seleccionar el archivo HIDX agregado anteriormente, el método para ejecutar y los parámetros para usar.

Cuando termine de todos estos pasos, puede usar la acción que se crea en la aplicación lógica para conectarse a la del gran sistema IBM, pantallas de unidad para la aplicación, escribir datos, devolver resultados y así sucesivamente. También puede seguir agregando otras acciones a la aplicación lógica para la integración con otras aplicaciones, servicios y sistemas.

<a name="run-action"></a>

## <a name="run-ibm-3270-actions"></a>Ejecutar acciones de IBM 3270

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. En el último paso en la que desea agregar una acción, elija **nuevo paso**y seleccione **agregar una acción**. 

1. En el cuadro de búsqueda, elija **Enterprise**. En el cuadro de búsqueda, escriba "3270" como filtro. En la lista de acciones, seleccione esta acción: **Se ejecuta un programa del sistema central a través de una conexión de TN3270**

   ![Seleccione la acción de 3270](./media/connectors-create-api-3270/select-3270-action.png)

   Para agregar una acción entre un paso y otro, mueva el puntero sobre la flecha entre ellos. 
   Elija el signo más (**+**) que aparece y seleccione **Agregar una acción**.

1. Si aún no existe ninguna conexión, proporcione la información necesaria para la conexión y elija **crear**.

   | Propiedad | Obligatorio | Value | DESCRIPCIÓN |
   |----------|----------|-------|-------------|
   | **Nombre de la conexión** | Sí | <*connection-name*> | El nombre de la conexión |
   | **Id. de cuenta de integración** | Sí | <*integration-account-name*> | Nombre de la cuenta de integración |
   | **Dirección URL de SAS de cuenta de integración** | Sí | <*integration-account-SAS-URL*> | URL de firma de acceso compartido (SAS) de su cuenta de integración, que puede generar a partir de la configuración de la cuenta de integración en Azure portal. <p>1. En la integración de su cuenta de menú, en **configuración**, seleccione **dirección URL de devolución de llamada**. <br>2. En el panel derecho, copie el **dirección URL de devolución de llamada genera** valor. |
   | **Servidor** | Sí | <*TN3270-server-name*> | El nombre del servidor para el servicio de TN3270 |
   | **Puerto** | Sin  | <*TN3270-server-port*> | El puerto utilizado por el servidor TN3270. Si se deja en blanco, el conector utiliza `23` como el valor predeterminado. |
   | **Tipo de dispositivo** | Sin  | <*IBM-terminal-model*> | El nombre del modelo o un número para el terminal de IBM para emular. Si se deja en blanco, el conector utiliza los valores predeterminados. |
   | **Página de códigos** | Sin  | <*code-page-number*> | El número de página de códigos para el host. Si se deja en blanco, el conector utiliza `37` como el valor predeterminado. |
   | **Nombre de unidad lógica** | Sin  | <*logical-unit-name*> | El nombre de la unidad lógica específica para solicitar desde el host |
   | **¿Quiere habilitar SSL?** | Sin  | Activar o desactivar | Activar o desactivar el cifrado SSL. |
   | **¿Validar el certificado ssl del host?** | Sin  | Activar o desactivar | Activar o desactivar la validación para el certificado del servidor. |
   ||||

   Por ejemplo: 

   ![Propiedades de la conexión](./media/connectors-create-api-3270/connection-properties.png)

1. Proporcione la información necesaria para la acción:

   | Propiedad | Obligatorio | Value | DESCRIPCIÓN |
   |----------|----------|-------|-------------|
   | **Nombre de Hidx** | Sí | <*HIDX-file-name*> | Seleccione el archivo HIDX 3270 que desea usar. |
   | **Nombre del método** | Sí | <*nombre del método*> | Seleccione el método en el archivo HIDX que desea usar. Después de seleccionar un método, el **Agregar nuevo parámetro** aparece la lista para que pueda seleccionar parámetros que se usarán con ese método. |
   ||||

   Por ejemplo: 

   **Seleccione el archivo HIDX**

   ![Seleccione archivo HIDX](./media/connectors-create-api-3270/select-hidx-file.png)

   **Seleccione el método**

   ![Seleccionar método](./media/connectors-create-api-3270/select-method.png)

   **Seleccione los parámetros**

   ![Seleccionar parámetros](./media/connectors-create-api-3270/add-parameters.png)

1. Cuando haya terminado, guarde y ejecute la aplicación lógica.

   Después de la lógica de aplicación termine de ejecutarse, los pasos de aparezca de la ejecución. 
   Pasos correcta muestran marcas de verificación, mientras incorrectos pasos muestran la letra "X".

1. Para revisar las entradas y salidas de cada paso, expanda ese paso.

1. Para revisar los resultados, elija **mostrar salidas sin procesar**.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos sobre el límite, acciones y desencadenadores, que se describen mediante OpenAPI del conector (anteriormente Swagger) descripción, revise el [página de referencia del conector](/connectors/si3270/).

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)

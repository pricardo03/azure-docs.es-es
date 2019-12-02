---
title: Funcionamiento y configuración de Azure Dev Spaces
services: azure-dev-spaces
ms.date: 03/04/2019
ms.topic: conceptual
description: En este artículo se describen los procesos que posibilitan Azure Dev Spaces y cómo se configuran en el archivo de configuración azds.yaml.
keywords: azds.yaml, Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: 9efae0e9d6bc53e08dce604fa79aa29e158ecabd
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280145"
---
# <a name="how-azure-dev-spaces-works-and-is-configured"></a>Funcionamiento y configuración de Azure Dev Spaces

El desarrollo de una aplicación de Kubernetes puede ser complicado. Para ello, necesita archivos de configuración de Docker y Kubernetes. Además, debe averiguar cómo probar la aplicación localmente e interactuar con otros servicios dependientes. Es posible que también deba controlar el desarrollo y las pruebas en varios servicios a la vez y con un equipo de desarrolladores.

Azure Dev Spaces le ayuda a desarrollar, implementar y depurar aplicaciones de Kubernetes directamente en Azure Kubernetes Service (AKS). Además, permite que un equipo comparta un espacio de desarrollo. El uso compartido de un espacio de desarrollo en un equipo permite que los miembros del equipo realicen el desarrollo de forma aislada sin tener que replicar ni simular dependencias u otras aplicaciones en el clúster.

Azure Dev Spaces crea y usa un archivo de configuración para implementar, ejecutar y depurar las aplicaciones de Kubernetes en AKS. Este archivo de configuración reside con el código de la aplicación y se puede agregar al sistema de control de versiones.

En este artículo se describen los procesos que posibilitan Azure Dev Spaces y cómo se configuran en el archivo de configuración de Azure Dev Spaces. Para preparar Azure Dev Spaces rápidamente y verlo en la práctica, complete uno de estos inicios rápidos:

* [Java con la CLI y Visual Studio Code](quickstart-java.md)
* [.NET Core con la CLI y Visual Studio Code](quickstart-netcore.md)
* [.NET Core con Visual Studio](quickstart-netcore-visualstudio.md)
* [Node.js con la CLI y Visual Studio Code](quickstart-nodejs.md)

## <a name="how-azure-dev-spaces-works"></a>Funcionamiento de Azure Dev Spaces

Azure Dev Spaces tiene dos componentes diferenciados con los que se interactúa: el controlador y las herramientas de cliente.

![Componentes de Azure Dev Spaces](media/how-dev-spaces-works/components.svg)

El controlador realiza las acciones siguientes:

* Administra la creación y la selección de espacios de desarrollo.
* Instala el gráfico de Helm de la aplicación y crea objetos de Kubernetes.
* Compila la imagen de contenedor de la aplicación.
* Implementa la aplicación en AKS.
* Lleva a cabo compilaciones incrementales y reinicios cuando el código fuente cambia.
* Administra los registros y los seguimientos HTTP.
* Reenvía stdout y stderr a las herramientas de cliente.
* Permite que los miembros del equipo creen espacios de desarrollo secundarios derivados de un espacio de desarrollo primario.
* Configura el enrutamiento para las aplicaciones dentro de un espacio, así como entre espacios primarios y secundarios.

El controlador, que reside fuera de AKS, impulsa el comportamiento y la comunicación entre las herramientas de cliente y el clúster de AKS. El controlador se habilita mediante la CLI de Azure cuando se prepara el clúster para usar Azure Dev Spaces. Una vez habilitado, se puede interactuar con él mediante las herramientas de cliente.

Las herramientas de cliente permiten al usuario:
* Generar un archivo Dockerfile, un gráfico de Helm y un archivo de configuración de Azure Dev Spaces para la aplicación.
* Crear espacios de desarrollo primarios y secundarios.
* Indicar al controlador que compile e inicie la aplicación.

Mientras se ejecuta la aplicación, las herramientas de cliente también hacen lo siguiente:
* Recibir y mostrar stdout y stderr desde la aplicación que se ejecuta en AKS.
* Usar el [reenvío de puerto](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) para permitir el acceso web a la aplicación mediante http:\//localhost.
* Asociar un depurador a la aplicación que se ejecuta en AKS.
* Sincronizar el código fuente con el espacio de desarrollo cuando se detecta un cambio para las compilaciones incrementales, lo que permite una iteración rápida.

Puede usar las herramientas de cliente desde la línea de comandos como parte del comando `azds`. También puede usar las herramientas de cliente con:

* Visual Studio Code con la [extensión Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds).
* Visual Studio con [Visual Studio Tools para Kubernetes](https://aka.ms/get-vsk8stools).

Este es el flujo básico para configurar y usar Azure Dev Spaces:
1. Prepare el clúster de AKS para Azure Dev Spaces.
1. Prepare el código para que se ejecute en Azure Dev Spaces.
1. Ejecute el código en un espacio de desarrollo.
1. Depure el código en un espacio de desarrollo.
1. Comparta un espacio de desarrollo.

En las secciones siguientes veremos con más detalle cómo funciona Azure Dev Spaces.

## <a name="prepare-your-aks-cluster"></a>Preparación del clúster de AKS

Para preparar el clúster de AKS, debe hacer lo siguiente:
* Compruebe que el clúster de AKS está en una región [compatible con Azure Dev Spaces][supported-regions].
* Compruebe que está ejecutando Kubernetes 1.10.3 o una versión posterior.
* Habilite Azure Dev Spaces en el clúster con `az aks use-dev-spaces`.

Para obtener más información sobre cómo crear y configurar un clúster de AKS para Azure Dev Spaces, consulte una de estas guías de introducción:
* [Introducción a Azure Dev Spaces con Java](get-started-java.md)
* [Introducción a Azure Dev Spaces con .NET Core y Visual Studio](get-started-netcore-visualstudio.md)
* [Introducción a Azure Dev Spaces con .NET Core](get-started-netcore.md)
* [Introducción a Azure Dev Spaces con Node.js](get-started-nodejs.md)

Cuando se habilita Azure Dev Spaces en el clúster de AKS, instala el controlador para el clúster. El controlador es un recurso de Azure independiente situado fuera del clúster que hace lo siguiente a los recursos del clúster:

* Crea o designa un espacio de nombres de Kubernetes que se usará como un espacio de desarrollo.
* Quita cualquier espacio de nombres de Kubernetes que se denomine *azds*, si existe, y crea uno nuevo.
* Implementa una configuración de webhook de Kubernetes.
* Implementa un servidor de admisión de webhook.
    

Además, usa la misma entidad de servicio que emplea el clúster de AKS para realizar llamadas de servicio a otros componentes de Azure Dev Spaces.

![Preparación del clúster para Azure Dev Spaces](media/how-dev-spaces-works/prepare-cluster.svg)

Para poder usar Azure Dev Spaces, debe haber al menos un espacio de desarrollo. Azure Dev Spaces usa los espacios de nombres de Kubernetes dentro del clúster de AKS para los espacios de desarrollo. Cuando instale un controlador, se le pedirá que cree un espacio de nombres de Kubernetes o que elija uno existente para usarlo como primer espacio de desarrollo. Cuando un espacio de nombres se designa como espacio de desarrollo, el controlador agrega la etiqueta *azds.io/space=true* a ese espacio de nombres para identificarlo como espacio de desarrollo. El espacio de desarrollo inicial que cree o designe se seleccionará de forma predeterminada una vez que haya preparado el clúster. Cuando se seleccione un espacio, Azure Dev Spaces lo usará para crear cargas de trabajo.

De forma predeterminada, el controlador crea un espacio de desarrollo denominado *default* mediante la actualización del espacio de nombres *default* existente de Kubernetes. Puede usar las herramientas de cliente para crear espacios de desarrollo y quitar otros existentes. Debido a una limitación en Kubernetes, el espacio de desarrollo *default* no se puede quitar. El controlador también quita los espacios de nombres existentes de Kubernetes que se denominen *azds* para evitar conflictos con el comando `azds` que usan las herramientas de cliente.

El servidor de admisión de webhook de Kubernetes se usa para insertar pods con tres contenedores durante la implementación para la instrumentación: devspaces-proxy, devspaces-proxy-init y devspaces-build. **Estos tres contenedores se ejecutan con acceso a la raíz en el clúster de AKS.** Además, usan la misma entidad de servicio que emplea el clúster de AKS para realizar llamadas de servicio a otros componentes de Azure Dev Spaces.

![Servidor de admisión de webhook de Kubernetes de Azure Dev Spaces](media/how-dev-spaces-works/kubernetes-webhook-admission-server.svg)

El contenedor devspaces-proxy es un contenedor sidecar que controla todo el tráfico TCP dentro y fuera del contenedor de la aplicación y que ayuda a realizar el enrutamiento. El contenedor devspaces-proxy vuelve a enrutar los mensajes HTTP si se usan determinados espacios. Por ejemplo, puede ayudar a enrutar los mensajes HTTP entre aplicaciones en espacios primarios y secundarios. Todo el tráfico que no sea HTTP pasa a través de devspaces-proxy sin modificar. El contenedor devspaces-proxy también registra todos los mensajes HTTP entrantes y salientes y los envía a las herramientas de cliente como seguimientos. El desarrollador puede visualizar estos seguimientos para inspeccionar el comportamiento de la aplicación.

El contenedor devspaces-proxy-init es un [contenedor init](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) que agrega reglas de enrutamiento adicionales en función de la jerarquía de espacio al contenedor de la aplicación. Para agregar reglas de enrutamiento, actualiza el archivo */etc/resolv.conf* del contenedor de la aplicación y la configuración de iptables antes de comenzar. Las actualizaciones de */etc/resolv.conf* permiten la resolución DNS de los servicios en espacios primarios. Las actualizaciones de la configuración de iptables garantizan que todo el tráfico TCP dentro y fuera del contenedor de la aplicación se enrute a través de devspaces-proxy. Todas las actualizaciones de devspaces-proxy-init se producen de forma complementaria a las reglas que agrega Kubernetes.

El contenedor devspaces-build es un contenedor init que tiene el código fuente del proyecto y el socket de Docker montado. El código fuente del proyecto y el acceso a Docker permiten que el pod compile directamente el contenedor de la aplicación.

> [!NOTE]
> Azure Dev Spaces usa el mismo nodo para compilar el contenedor de la aplicación y ejecutarlo. Como resultado, Azure Dev Spaces no necesita un registro de contenedor externo para compilar y ejecutar la aplicación.

El servidor de admisión de webhook de Kubernetes escucha los nuevos pods que se creen en el clúster de AKS. Si un pod se implementa en un espacio de nombres con la etiqueta *azds.io/space=true*, inserta en dicho pod los contenedores adicionales. El contenedor devspaces-build solo se inserta si el contenedor de la aplicación se ejecuta con las herramientas de cliente.

Una vez que haya preparado el clúster de AKS, puede usar las herramientas de cliente para preparar y ejecutar el código en el espacio de desarrollo.

## <a name="prepare-your-code"></a>Preparación del código

Para ejecutar la aplicación en un espacio de desarrollo, debe estar en un contenedor. Además, es necesario que defina cómo se implementará en Kubernetes. Para incluir la aplicación en un contenedor, necesita un archivo Dockerfile. Para definir cómo se implementará la aplicación en Kubernetes, es necesario un [gráfico de Helm](https://docs.helm.sh/). Con el fin de ayudar a crear el archivo Dockerfile y el gráfico de Helm para la aplicación, las herramientas de cliente proporcionan el comando `prep`:

```cmd
azds prep --public
```

El comando `prep` consultará los archivos del proyecto e intentará crear el archivo Dockerfile y el gráfico de Helm para ejecutar la aplicación en Kubernetes. Actualmente, el comando `prep` generará un archivo Dockerfile y un gráfico de Helm con los lenguajes siguientes:

* Java
* Node.js
* .NET Core

Es *necesario* ejecutar el comando `prep` desde un directorio que contenga código fuente. Al ejecutar el comando `prep` desde el directorio correcto, las herramientas de cliente pueden identificar el lenguaje y crear un archivo Dockerfile adecuado para incluir la aplicación en un contenedor. También puede ejecutar el comando `prep` desde un directorio que contenga un archivo *pom.xml* para proyectos de Java.

Si ejecuta el comando `prep` desde un directorio que no contenga código fuente, las herramientas de cliente no generarán un archivo Dockerfile. Además, mostrarán el siguiente mensaje de error: *Dockerfile could not be generated due to unsupported language* (No se pudo generar un archivo Dockerfile debido a un lenguaje no admitido). Este error también se produce si las herramientas de cliente no reconocen el tipo de proyecto.

Al ejecutar el comando `prep`, tendrá la opción de especificar la marca `--public`. Esta marca le indica al controlador que cree un punto de conexión accesible desde Internet para este servicio. Si no especifica esta marca, el servicio solo será accesible desde dentro del clúster o mediante el túnel de localhost que crean las herramientas de cliente. Para habilitar o deshabilitar este comportamiento después de ejecutar el comando `prep`, actualice el gráfico de Helm generado.

El comando `prep` no reemplazará ningún archivo Dockerfile o gráfico de Helm que tenga en el proyecto. Si un archivo Dockerfile o gráfico de Helm existente usa la misma convención de nomenclatura que los archivos generados por el comando `prep`, el comando `prep` omitirá la generación de esos archivos. En caso contrario, el comando `prep` generará su propio archivo Dockerfile o gráfico de Helm junto con los archivos existentes.

El comando `prep` también generará un archivo `azds.yaml` en la raíz del proyecto. Azure Dev Spaces usa este archivo para compilar, instalar, configurar y ejecutar la aplicación. Este archivo de configuración muestra la ubicación del archivo Dockerfile y del gráfico de Helm y, además, proporciona una configuración adicional encima de estos artefactos.

A continuación se muestra un archivo azds.yaml de ejemplo creado mediante la [aplicación de ejemplo de .NET Core](https://github.com/Azure/dev-spaces/tree/master/samples/dotnetcore/getting-started/webfrontend):

```yaml
kind: helm-release
apiVersion: 1.1
build:
  context: .
  dockerfile: Dockerfile
install:
  chart: charts/webfrontend
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
  set:
    replicaCount: 1
    image:
      repository: webfrontend
      tag: $(tag)
      pullPolicy: Never
    ingress:
      annotations:
        kubernetes.io/ingress.class: traefik-azds
      hosts:
        # This expands to [space.s.][rootSpace.]webfrontend.<random suffix>.<region>.azds.io
        # Customize the public URL by changing the 'webfrontend' text between the $(rootSpacePrefix) and $(hostSuffix) tokens
        # For more information see https://aka.ms/devspaces/routing
        - $(spacePrefix)$(rootSpacePrefix)webfrontend$(hostSuffix)
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
      useGitIgnore: true
      args:
        BUILD_CONFIGURATION: ${BUILD_CONFIGURATION:-Debug}
    container:
      sync:
      - "**/Pages/**"
      - "**/Views/**"
      - "**/wwwroot/**"
      - "!**/*.{sln,csproj}"
      command: [dotnet, run, --no-restore, --no-build, --no-launch-profile, -c, "${BUILD_CONFIGURATION:-Debug}"]
      iterate:
        processesToKill: [dotnet, vsdbg]
        buildCommands:
        - [dotnet, build, --no-restore, -c, "${BUILD_CONFIGURATION:-Debug}"]
```

El archivo `azds.yaml` generado por el comando `prep` debería funcionar bien en un escenario de desarrollo simple de un solo proyecto. Si la complejidad del proyecto ha aumentado, es posible que tenga que actualizar este archivo después de ejecutar el comando `prep`. Por ejemplo, el proyecto podría requerir algunos ajustes en el proceso de compilación o de inicio, según las necesidades de desarrollo o depuración. También podría tener varias aplicaciones en el proyecto que requieren varios procesos de compilación o un contenido de una compilación diferente.

## <a name="run-your-code"></a>Ejecución del código

Para ejecutar el código en un espacio de desarrollo, emita el comando `up` en el mismo directorio que el archivo `azds.yaml`:

```cmd
azds up
```

El comando `up` carga los archivos de origen de la aplicación y otros artefactos necesarios para compilar y ejecutar el proyecto en el espacio de desarrollo. Desde aquí, el controlador en el espacio de desarrollo hace lo siguiente:

1. Crea los objetos de Kubernetes para implementar la aplicación.
1. Compila el contenedor para la aplicación.
1. Implementa la aplicación en el espacio de desarrollo.
1. Crea un nombre DNS accesible públicamente para el punto de conexión de la aplicación, si se ha configurado.
1. Usa el *reenvío de puerto* para proporcionar acceso al punto de conexión de la aplicación mediante http://localhost.
1. Reenvía stdout y stderr a las herramientas de cliente.


### <a name="starting-a-service"></a>Inicio de un servicio

Cuando se inicia un servicio en un espacio de desarrollo, las herramientas de cliente y el controlador trabajan conjuntamente para sincronizar los archivos de origen, crear el contenedor y los objetos de Kubernetes y ejecutar la aplicación.

En un nivel más pormenorizado, esto es lo que sucede al ejecutar `azds up`:

1. Los archivos se sincronizan desde el equipo del usuario en un almacenamiento de archivos de Azure exclusivo del clúster de AKS del usuario. Se carga el código fuente, el gráfico de Helm y los archivos de configuración. En la sección siguiente encontrará más detalles sobre el proceso de sincronización.
1. El controlador crea una solicitud para iniciar una nueva sesión. Esta solicitud contiene varias propiedades, incluido un identificador único, el nombre del espacio, la ruta de acceso al código fuente y una marca de depuración.
1. El controlador reemplaza el marcador de posición *$(tag)* en el gráfico de Helm por el identificador de sesión único e instala el gráfico de Helm para el servicio. Si agrega una referencia al identificador de sesión único en el gráfico de Helm, el contenedor implementado en el clúster de AKS para esta sesión específica podrá asociarse a la solicitud de sesión y a la información relacionada.
1. Durante la instalación del gráfico de Helm, el servidor de admisión de webhook de Kubernetes agrega contenedores adicionales al pod de la aplicación para la instrumentación y el acceso al código fuente del proyecto. Los contenedores devspaces-proxy y devspaces-proxy-init se agregan para proporcionar seguimiento de HTTP y enrutamiento de espacio. El contenedor devspaces-build se agrega para proporcionar al pod acceso a la instancia de Docker y al código fuente del proyecto para crear el contenedor de la aplicación.
1. Cuando se inicia el pod de la aplicación, se usan los contenedores devspaces-build y devspaces-proxy-init para crear el contenedor de la aplicación. Después, se inician el contenedor de la aplicación y los contenedores devspaces-proxy.
1. Una vez que se ha iniciado el contenedor de la aplicación, la funcionalidad del lado cliente usa la funcionalidad de *reenvío de puerto* de Kubernetes para proporcionar acceso HTTP a la aplicación a través de http://localhost. El reenvío de puerto conecta el equipo de desarrollo con el servicio en el espacio de desarrollo.
1. Cuando se hayan iniciado todos los contenedores del pod, el servicio se ejecutará. En este momento, la funcionalidad del lado cliente empieza a transmitir en secuencias el seguimiento HTTP, stdout y stderr. Esta información se muestra al desarrollador a través de la funcionalidad del lado cliente.

### <a name="updating-a-running-service"></a>Actualización de un servicio en ejecución

Mientras se ejecuta un servicio, Azure Dev Spaces puede actualizarlo si se modifica alguno de los archivos de origen del proyecto. Además, Dev Spaces lleva a cabo la actualización del servicio de manera diferente en función del tipo de archivo que se haya modificado. Dev Spaces puede actualizar un servicio en ejecución de tres maneras:

* Mediante la actualización de un archivo directamente
* Mediante la recompilación y el reinicio del proceso de la aplicación dentro del contenedor de la aplicación en ejecución
* Mediante la recompilación y la reimplementación del contenedor de la aplicación

![Sincronización de archivos de Azure Dev Spaces](media/how-dev-spaces-works/file-sync.svg)

Algunos archivos del proyecto que son recursos estáticos (como los archivos html, css y cshtml) se pueden actualizar directamente en el contenedor de la aplicación sin necesidad de reiniciar nada. Si se modifica un recurso estático, el nuevo archivo se sincroniza con el espacio de desarrollo y, después, lo usa el contenedor en ejecución.

Para aplicar cambios en archivos como los de código fuente o los de configuración de la aplicación, reinicie el proceso de la aplicación dentro del contenedor en ejecución. Una vez que estos archivos se hayan sincronizado, el proceso de la aplicación se reiniciará en el contenedor en ejecución mediante el proceso *devhostagent*. Al crear inicialmente el contenedor de la aplicación, el controlador reemplaza el comando de inicio de la aplicación por un proceso diferente denominado *devhostagent*. Después, el proceso real de la aplicación se ejecuta como un proceso secundario bajo *devhostagent* y su salida se canaliza mediante la salida de *devhostagent*. El proceso *devhostagent* también forma parte de Dev Spaces y puede ejecutar comandos en el contenedor en ejecución en nombre de Dev Spaces. Al realizar un reinicio, *devhostagent*:

* Detiene el proceso actual o los procesos asociados a la aplicación.
* Recompila la aplicación.
* Reinicia los procesos asociados a la aplicación.

La manera en que *devhostagent* ejecuta los pasos anteriores se establece en el archivo de configuración `azds.yaml`. Esta configuración se detalla en una sección posterior.

Las actualizaciones de archivos del proyecto como los archivos Dockerfile, los archivos csproj o cualquier parte del gráfico de Helm requieren que se recompile y se reimplemente el contenedor de la aplicación. Cuando uno de estos archivos se sincroniza con el espacio de desarrollo, el controlador ejecuta el comando [helm upgrade](https://helm.sh/docs/intro/using_helm/#helm-upgrade-and-helm-rollback-upgrading-a-release-and-recovering-on-failure) y el contenedor de la aplicación se recompila y reimplementa.

### <a name="file-synchronization"></a>Sincronización de archivos

La primera vez que se inicia una aplicación en un espacio de desarrollo, se cargan todos los archivos de código fuente de la aplicación. Durante la ejecución de la aplicación y en los reinicios posteriores, solo se cargan los archivos que se han modificado. Para coordinar este proceso se usan dos archivos: un archivo de cliente y un archivo de controlador.

El archivo de cliente se almacena en un directorio temporal y se le asigna un nombre en función de un hash del directorio del proyecto que se ejecuta en Dev Spaces. Por ejemplo, en Windows tendría un archivo como *Users\USERNAME\AppData\Local\Temp\1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef.synclog* para el proyecto. En Linux, el archivo de cliente se almacena en el directorio */tmp*. Para encontrar el directorio en macOS, ejecute el comando `echo $TMPDIR`.

Este archivo está en formato JSON y contiene lo siguiente:

* Una entrada para cada archivo de proyecto que se sincroniza con el espacio de desarrollo.
* Un identificador de sincronización.
* La marca de tiempo de la última operación de sincronización.

Cada entrada de archivo de proyecto contiene una ruta de acceso al archivo y su marca de tiempo.

El archivo de controlador se almacena en el clúster de AKS. Contiene el identificador de sincronización y la marca de tiempo de la última sincronización.

Se produce una sincronización cuando las marcas de tiempo de sincronización no coinciden entre los archivos de cliente y de controlador. Durante una sincronización, las herramientas de cliente iteran las entradas de archivos en el archivo de cliente. Si la marca de tiempo del archivo es posterior a la marca de tiempo de sincronización, el archivo se sincroniza con el espacio de desarrollo. Una vez completada la sincronización, las marcas de tiempo de sincronización se actualizan en los archivos de cliente y de controlador.

Todos los archivos del proyecto se sincronizan si el archivo de cliente no está presente. Este comportamiento le permite forzar una sincronización completa si elimina el archivo de cliente.

### <a name="how-routing-works"></a>Cómo funciona el enrutamiento

Los espacios de desarrollo se basan en AKS y usan los mismos [conceptos de redes](../aks/concepts-network.md). Azure Dev Spaces también tiene un servicio *ingressmanager* centralizado e implementa su propio controlador de entrada para el clúster de AKS. El servicio *ingressmanager* supervisa los clústeres de AKS con espacios de desarrollo y aumenta el controlador de entrada de Azure Dev Spaces en el clúster con objetos de entrada para el enrutamiento a los pods de la aplicación. El contenedor devspaces-proxy de cada pod agrega un encabezado HTTP `azds-route-as` para el tráfico HTTP a un espacio de desarrollo en función de la dirección URL. Por ejemplo, una solicitud a la dirección URL *http://azureuser.s.default.serviceA.fedcba09...azds.io* obtendría un encabezado HTTP con `azds-route-as: azureuser`. El contenedor devspaces-proxy no agregará un encabezado `azds-route-as` si ya hay alguno.

Cuando se realiza una solicitud HTTP a un servicio desde fuera del clúster, la solicitud se pasa al controlador de entrada. El controlador de entrada enruta la solicitud directamente al pod adecuado según sus objetos de entrada y sus reglas. El contenedor devspaces-proxy del pod recibe la solicitud, agrega el encabezado `azds-route-as` en función de la dirección URL y, después, enruta la solicitud al contenedor de la aplicación.

Cuando se realiza una solicitud HTTP a un servicio desde otro servicio del clúster, la solicitud pasa primero por el contenedor devspaces-proxy del servicio que realiza la llamada. El contenedor devspaces-proxy examina la solicitud HTTP y comprueba el encabezado `azds-route-as`. En función del encabezado, el contenedor devspaces-proxy buscará la dirección IP del servicio asociado con el valor del encabezado. Si se encuentra una dirección IP, el contenedor devspaces-proxy vuelve a enrutar la solicitud a esa dirección IP. Si no se encuentra ninguna dirección IP, el contenedor devspaces-proxy enruta la solicitud al contenedor primario de la aplicación.

Por ejemplo, los servicios *serviceA* y *serviceB* de la aplicación se implementan en un espacio de desarrollo primario denominado *default*. *serviceA* se basa en *serviceB* y realiza llamadas HTTP a este servicio. Azure User crea un espacio de desarrollo secundario basado en el espacio *default* denominado *azureuser*. Azure User también implementa su propia versión de *serviceA* en su espacio secundario. Cuando se realiza una solicitud a *http://azureuser.s.default.serviceA.fedcba09...azds.io* :

![Enrutamiento de Azure Dev Spaces](media/how-dev-spaces-works/routing.svg)

1. El controlador de entrada busca la dirección IP para el pod asociado a la dirección URL, que es *serviceA.azureuser*.
1. El controlador de entrada encuentra la dirección IP para el pod en el espacio de desarrollo de Azure User y enruta la solicitud al pod *serviceA.azureuser*.
1. El contenedor devspaces-proxy del pod *serviceA.azureuser* recibe la solicitud y agrega `azds-route-as: azureuser` como encabezado HTTP.
1. El contenedor devspaces-proxy del pod *serviceA.azureuser* enruta la solicitud al contenedor de la aplicación *serviceA* del pod *serviceA.azureuser*.
1. La aplicación *serviceA* del pod *serviceA.azureuser* realiza una llamada a *serviceB*. La aplicación *serviceA* también contiene código para conservar el encabezado `azds-route-as` existente, que en este caso es `azds-route-as: azureuser`.
1. El contenedor devspaces-proxy del pod *serviceA.azureuser* recibe la solicitud y busca la dirección IP de *serviceB* según el valor del encabezado `azds-route-as`.
1. El contenedor devspaces-proxy del pod *serviceA.azureuser* no encuentra una dirección IP para *serviceB.azureuser*.
1. El contenedor devspaces-proxy del pod *serviceA.azureuser* busca la dirección IP de *serviceB* en el espacio primario, que es *serviceB.default*.
1. El contenedor devspaces-proxy del pod *serviceA.azureuser* encuentra la dirección IP de *serviceB.default* y enruta la solicitud al pod *serviceB.default*.
1. El contenedor devspaces-proxy del pod *serviceB.default* recibe la solicitud y la enruta al contenedor de la aplicación *serviceB* del pod *serviceB.default*.
1. La aplicación *serviceB* del pod *serviceB.default* devuelve una respuesta al pod *serviceA.azureuser*.
1. El contenedor devspaces-proxy del pod *serviceA.azureuser* recibe la respuesta y la enruta al contenedor de la aplicación *serviceA* del pod *serviceA.azureuser*.
1. La aplicación *serviceA* recibe la respuesta y, después, devuelve su propia respuesta.
1. El contenedor devspaces-proxy del pod *serviceA.azureuser* recibe la respuesta del contenedor de la aplicación *serviceA* y la enruta al autor de la llamada original fuera del clúster.

El restante tráfico TCP que no es HTTP pasa a través del controlador de entrada y los contenedores devspaces-proxy sin modificar.

### <a name="how-running-your-code-is-configured"></a>Cómo se configura la ejecución del código

Azure Dev Spaces usa el archivo `azds.yaml` para instalar y configurar el servicio. El controlador usa la propiedad `install` en el archivo `azds.yaml` para instalar el gráfico de Helm y crear los objetos de Kubernetes:

```yaml
...
install:
  chart: charts/webfrontend
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
  set:
    replicaCount: 1
    image:
      repository: webfrontend
      tag: $(tag)
      pullPolicy: Never
    ingress:
      annotations:
        kubernetes.io/ingress.class: traefik-azds
      hosts:
      # This expands to [space.s.][rootSpace.]webfrontend.<random suffix>.<region>.azds.io
      # Customize the public URL by changing the 'webfrontend' text between the $(rootSpacePrefix) and $(hostSuffix) tokens
      # For more information see https://aka.ms/devspaces/routing
      - $(spacePrefix)$(rootSpacePrefix)webfrontend$(hostSuffix)
...
```

De forma predeterminada, el comando `prep` genera el gráfico de Helm. También establece la propiedad *install.chart* en el directorio del gráfico de Helm. Si quiere usar un gráfico de Helm en una ubicación diferente, puede actualizar esta propiedad para usar esa ubicación.

Al instalar los gráficos de Helm, Azure Dev Spaces proporciona una forma de reemplazar los valores del gráfico. Los valores predeterminados del gráfico de Helm se encuentran en `charts/APP_NAME/values.yaml`.

Con la propiedad *install.values*, puede enumerar uno o varios archivos que definen los valores que quiere que se reemplacen en el gráfico de Helm. Por ejemplo, si quiere un nombre de host o una configuración de base de datos en concreto al ejecutar la aplicación en un espacio de desarrollo, puede usar esta funcionalidad de reemplazo. También puede agregar el símbolo *?* al final de cualquiera de los nombres de archivo para establecerlo como opcional.

La propiedad *install.set* le permite configurar uno o varios valores que quiera reemplazar en el gráfico de Helm. Los valores configurados en *install.set* reemplazarán los valores configurados en los archivos que se enumeran en *install.values*. Las propiedades de *install.set* dependen de los valores del gráfico de Helm y pueden variar según el gráfico generado.

En el ejemplo anterior, la propiedad *install.set.replicaCount* indica al controlador cuántas instancias de la aplicación deben ejecutarse en el espacio de desarrollo. Según el escenario, puede aumentar este valor, pero esto tendrá unas consecuencias a la hora de asociar un depurador al pod de la aplicación. Para obtener más información, vea el [artículo sobre la solución de problemas](troubleshooting.md).

En el gráfico de Helm generado, la imagen de contenedor está establecida en *{{ .Values.image.repository }}:{{ .Values.image.tag }}* . El archivo `azds.yaml` define la propiedad *install.set.image.tag* como *$(tag)* de forma predeterminada, que se usa como valor para *{{ .Values.image.tag }}* . Al establecer la propiedad *install.set.image.tag* de este modo, permite que la imagen de contenedor de la aplicación se etiquete de manera diferente cuando se ejecuta Azure Dev Spaces. En este caso, la imagen está etiquetada como *\<value from image.repository>:$(tag)* . Debe usar la variable *$(tag)* como valor de *install.set.image.tag* para que Dev Spaces reconozca y localice el contenedor en el clúster de AKS.

En el ejemplo anterior, `azds.yaml` define *install.set.ingress.hosts*. La propiedad *install.set.ingress.hosts* define un formato de nombre de host para los puntos de conexión públicos. Esta propiedad también usa *$(spacePrefix)* , *$(rootSpacePrefix)* y *$(hostSuffix)* , que son valores proporcionados por el controlador. 

*$(spacePrefix)* es el nombre del espacio de desarrollo secundario, que adopta la forma *SPACENAME.s*. *$(rootSpacePrefix)* es el nombre del espacio primario. Por ejemplo, si *azureuser* es un espacio secundario de *default*, el valor de *$(rootSpacePrefix)* es *default* y el valor de *$(spacePrefix)* es *azureuser.s*. Si el espacio no es de tipo secundario, *$(spacePrefix)* está en blanco. Por ejemplo, si el espacio *default* no tiene ningún espacio primario, el valor de *$(rootSpacePrefix)* es *default* y el valor de *$(spacePrefix)* está en blanco. *$(hostSuffix)* es un sufijo DNS que apunta al controlador de entrada de Azure Dev Spaces que se ejecuta en el clúster de AKS. Este sufijo DNS corresponde a una entrada DNS comodín, por ejemplo, *\*.RANDOM_VALUE.eus.azds.IO*, que se creó al agregar el controlador de Azure Dev Spaces al clúster de AKS.

En el archivo `azds.yaml` anterior, también podría actualizar *install.set.ingress.hosts* para cambiar el nombre de host de la aplicación. Por ejemplo, si quiere simplificar el nombre de host de la aplicación de *$(spacePrefix)$(rootSpacePrefix)webfrontend$(hostSuffix)* a *$(spacePrefix)$(rootSpacePrefix)web$(hostSuffix)* .

Para crear el contenedor para la aplicación, el controlador usa las siguientes secciones del archivo de configuración `azds.yaml`:

```yaml
build:
  context: .
  dockerfile: Dockerfile
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
      useGitIgnore: true
      args:
        BUILD_CONFIGURATION: ${BUILD_CONFIGURATION:-Debug}
...
```

El controlador usa un archivo Dockerfile para compilar y ejecutar la aplicación.

La propiedad *build.context* indica el directorio donde se encuentran los archivos Dockerfile. La propiedad *build.dockerfile* define el nombre del archivo Dockerfile para compilar la versión de producción de la aplicación. La propiedad *configurations.develop.build.dockerfile* configura el nombre del archivo Dockerfile para la versión de desarrollo de la aplicación.

El hecho de tener diferentes archivos Dockerfile para el desarrollo y la producción permite habilitar ciertos elementos durante el desarrollo y deshabilitarlos para las implementaciones de producción. Por ejemplo, puede habilitar la depuración o el registro más detallado durante el desarrollo y deshabilitarlo en un entorno de producción. También puede actualizar estas propiedades si los archivos Dockerfile tienen nombres diferentes o se encuentran en una ubicación diferente.

Para ayudarle a iterar rápidamente durante el desarrollo, Azure Dev Spaces sincronizará los cambios del proyecto local y actualizará de forma incremental la aplicación. Para configurar la sincronización y la actualización, se usa la siguiente sección del archivo de configuración `azds.yaml`:

```yaml
...
configurations:
  develop:
    ...
    container:
      sync:
      - "**/Pages/**"
      - "**/Views/**"
      - "**/wwwroot/**"
      - "!**/*.{sln,csproj}"
      command: [dotnet, run, --no-restore, --no-build, --no-launch-profile, -c, "${BUILD_CONFIGURATION:-Debug}"]
      iterate:
        processesToKill: [dotnet, vsdbg]
        buildCommands:
        - [dotnet, build, --no-restore, -c, "${BUILD_CONFIGURATION:-Debug}"]
...
```

Los archivos y los directorios que sincronizarán los cambios se indican en la propiedad *configurations.develop.container.sync*. Estos directorios se sincronizan inicialmente cuando se ejecuta el comando `up` y, además, cuando se detectan cambios. Si le interesa sincronizar otros directorios con su espacio de desarrollo, puede cambiar esta propiedad.

La propiedad *configurations.develop.container.iterate.buildCommands* especifica cómo se compila la aplicación en un escenario de desarrollo. La propiedad *configurations.develop.container.command* proporciona el comando para ejecutar la aplicación en un escenario de desarrollo. Tal vez le interese actualizar alguna de estas propiedades si quiere usar durante el desarrollo marcas o parámetros adicionales de compilación o tiempo de ejecución.

La propiedad *configurations.develop.container.iterate.processesToKill* enumera los procesos que se van a terminar para detener la aplicación. Es posible que quiera actualizar esta propiedad si le interesa cambiar el comportamiento de reinicio de la aplicación durante el desarrollo. Por ejemplo, si ha actualizado las propiedades *configurations.develop.container.iterate.buildCommands* o *configurations.develop.container.command* para cambiar cómo se compila o se inicia la aplicación, es posible que necesite cambiar qué procesos se detienen.

Cuando prepare el código con el comando `azds prep`, tendrá la opción de agregar la marca `--public`. Al agregar la marca `--public`, se crea una dirección URL accesible públicamente para la aplicación. Si omite esta marca, la aplicación solo será accesible dentro del clúster o mediante el túnel de localhost. Después de ejecutar el comando `azds prep`, puede cambiar esta opción si modifica la propiedad *ingress.enabled* en `charts/APPNAME/values.yaml`:

```yaml
ingress:
  enabled: true
```

## <a name="debug-your-code"></a>Depuración del código

En el caso de las aplicaciones de Java, .NET y Node.js, puede usar Visual Studio Code o Visual Studio para depurar la aplicación que se ejecuta directamente en el espacio de desarrollo. Visual Studio Code y Visual Studio proporcionan herramientas para conectarse al espacio de desarrollo, iniciar la aplicación y asociar un depurador. Después de ejecutar `azds prep`, puede abrir el proyecto en Visual Studio Code o Visual Studio. Visual Studio Code o Visual Studio generarán sus propios archivos de configuración para la conexión, que es independiente de la ejecución de `azds prep`. Desde Visual Studio Code o Visual Studio, puede establecer puntos de interrupción e iniciar la aplicación en el espacio de desarrollo.

![Depuración del código](media/get-started-node/debug-configuration-nodejs2.png)

Al iniciar la aplicación con Visual Studio Code o Visual Studio para la depuración, estos se encargan de iniciarla y conectarla al espacio de desarrollo de la misma manera que la ejecución de `azds up`. Las herramientas de cliente de Visual Studio Code y Visual Studio también proporcionan un parámetro adicional con información específica para la depuración. El parámetro contiene el nombre de la imagen del depurador, la ubicación del depurador en la imagen y la ubicación de destino en el contenedor de la aplicación para montar la carpeta del depurador.

Las herramientas de cliente determinan automáticamente la imagen del depurador mediante un método similar al que se usa durante la generación del archivo Dockerfile y el gráfico de Helm cuando se ejecuta `azds prep`. Una vez que se ha montado el depurador en la imagen de la aplicación, se ejecuta con `azds exec`.

## <a name="sharing-a-dev-space"></a>Uso compartido de un espacio de desarrollo

Al trabajar con un equipo, puede [compartir un espacio de desarrollo con el equipo al completo](how-to/share-dev-spaces.md) y crear espacios de desarrollo derivados. Cualquier persona con acceso de colaborador al grupo de recursos del espacio de desarrollo puede usar un espacio de desarrollo.

También puede crear un espacio de desarrollo que se derive de otro espacio de desarrollo. Al crear un espacio de desarrollo derivado, se agrega la etiqueta *azds.io/parent-space=PARENT-SPACE-NAME* al espacio de nombres del espacio de desarrollo derivado. Además, todas las aplicaciones del espacio de desarrollo primario se comparten con el espacio de desarrollo derivado. Si implementa una versión actualizada de una aplicación en el espacio de desarrollo derivado, solo existirá en este espacio y el espacio de desarrollo primario no se verá afectado. Puede tener un máximo de tres niveles de espacios de desarrollo derivados o espacios *primarios principales*.

El espacio de desarrollo derivado también enrutará las solicitudes de forma inteligente entre sus propias aplicaciones y las aplicaciones compartidas desde su elemento primario. Para llevar a cabo el enrutamiento, se intenta enrutar la solicitud a una aplicación del espacio de desarrollo derivado y se recurre a la aplicación compartida del espacio de desarrollo primario. El enrutamiento recurrirá a la aplicación compartida del espacio primario principal si la aplicación no está en el espacio primario.

Por ejemplo:
* El espacio de desarrollo *default* tiene las aplicaciones *serviceA* y *serviceB*.
* El espacio de desarrollo *azureuser* se deriva de *default*.
* Se implementa una versión actualizada de *serviceA* en *azureuser*.

Al usar *azureuser*, todas las solicitudes a *serviceA* se enrutarán a la versión actualizada en *azureuser*. Una solicitud a *serviceB* primero intentará enrutarse a la versión de *azureuser* de *serviceB*. Dado que no existe, se enrutará a la versión *default* de *serviceB*. Si se quita la versión de *azureuser* de *serviceA*, todas las solicitudes a *serviceA* recurrirán a usar la versión *default* de *serviceA*.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con Azure Dev Spaces, vea las siguientes guías de inicio rápido:

* [Java con la CLI y Visual Studio Code](quickstart-java.md)
* [.NET Core con la CLI y Visual Studio Code](quickstart-netcore.md)
* [.NET Core con Visual Studio](quickstart-netcore-visualstudio.md)
* [Node.js con la CLI y Visual Studio Code](quickstart-nodejs.md)

Para empezar a trabajar con el desarrollo en equipo, consulte los siguientes artículos de procedimientos:

* [Desarrollo en equipo: Java con la CLI y Visual Studio Code](team-development-java.md)
* [Desarrollo en equipo: .NET Core con la CLI y Visual Studio Code](team-development-netcore.md)
* [Desarrollo en equipo: .NET Core con Visual Studio](team-development-netcore-visualstudio.md)
* [Desarrollo en equipo: Node.js con la CLI y Visual Studio Code](team-development-nodejs.md)



[supported-regions]: about.md#supported-regions-and-configurations

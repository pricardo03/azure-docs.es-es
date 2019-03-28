---
title: archivo de inclusión
description: archivo de inclusión
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: DrEsteban
ms.author: stevenry
ms.date: 12/17/2018
ms.topic: include
manager: yuvalm
ms.openlocfilehash: 40c1be20df845b975c023616e38cbb932c985735
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439541"
---
# <a name="team-development-with-azure-dev-spaces"></a>Desarrollo en equipo con Azure Dev Spaces

En este tutorial, aprenderá cómo un equipo de desarrolladores puede colaborar de forma simultánea en el mismo clúster de Kubernetes gracias a Dev Spaces.

## <a name="learn-about-team-development"></a>Aprenda sobre el desarrollo en equipo.
Hasta ahora, ha ejecutado el código de la aplicación como si fuera el único desarrollador que trabaja en la aplicación. En esta sección, aprenderá cómo Azure Dev Spaces simplifica el desarrollo en equipo:
* Habilite un equipo de desarrolladores para trabajar en el mismo entorno, trabajando en un espacio de desarrollo compartido o en espacios de desarrollo distintos, según sea necesario.
* Permitirá que cada desarrollador efectúe una iteración en su código de forma aislada y sin peligro de interrumpir a otros desarrolladores.
* Probará el código de un extremo a otro, antes de confirmarlo, sin tener que crear bocetos o dependencias simuladas.

### <a name="challenges-with-developing-microservices"></a>Dificultades con el desarrollo de microservicios
Por el momento, la aplicación de ejemplo no es muy compleja. Pero en el trabajo de desarrollo del mundo real, las dificultades pronto surgen a medida que se agregan más servicios y crece el equipo de desarrollo. Puede ser poco realista ejecutarlo todo localmente para el desarrollo.

* La máquina de desarrollo puede no tener recursos suficientes para ejecutar a la vez cada uno de los servicios que necesite.
* Algunos servicios pueden tener que estar accesibles públicamente. Por ejemplo, un servicio puede necesitar tener un punto de conexión que responda a un webhook.
* Si desea ejecutar un subconjunto de servicios, tiene que conocer la jerarquía de dependencias completa entre todos los servicios. Determinar esto puede ser difícil, especialmente a medida que el número de servicios vaya aumentando.
* Algunos desarrolladores lo que hacen es simular o realizar bocetos de muchas de sus dependencias de servicios. Este enfoque puede ayudar, pero administrar dichas simulaciones pronto puede afectar al costo del desarrollo. Además, puede ocurrir que el entorno de desarrollo acabe no pareciéndose en nada al de producción, lo que facilita que aparezcan errores imperceptibles.
* De todo ello se desprende que es difícil realizar cualquier prueba de integración. Las pruebas de integración solo pueden suceder de forma realista tras una confirmación, lo que significa que verá problemas más adelante en el ciclo de desarrollo.

    ![](../articles/dev-spaces/media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Trabajo en un espacio de desarrollo compartido
Con Azure Dev Spaces se puede configurar un espacio de desarrollo *compartido* en Azure. Cada desarrollador puede centrarse en su parte de la aplicación y desarrollar de forma iterativa *código de confirmación previa* en un espacio de desarrollo que ya contiene los restantes servicios y recursos en la nube de los que dependen sus escenarios. Las dependencias siempre están actualizadas y los desarrolladores trabajan de una manera que refleja el entorno de producción.

### <a name="work-in-your-own-space"></a>Trabajar en su propio espacio de trabajo
A medida que desarrolla código para su servicio, y antes de estar preparado para insertarlo en el repositorio, el código estará normalmente en buenas condiciones. Aún estará dándole forma, probándolo y experimentando con soluciones de manera iterativa. Azure Dev Spaces proporciona el concepto de un **espacio**, que le permite trabajar de forma aislada sin miedo a que le interrumpan los miembros de su equipo.

## <a name="use-dev-spaces-for-team-development"></a>Uso de Dev Spaces para el desarrollo en equipo
Vamos a explicar estas ideas con un ejemplo concreto utilizando nuestra aplicación de ejemplo *webfrontend* -> *mywebapi*. Imaginemos un escenario donde un desarrollador, Scott, tiene que realizar un cambio en el servicio *mywebapi*, y *solo* en ese servicio. *Webfrontend* no tiene que cambiar como parte de la actualización de Scott.

_Sin_ usar Dev Spaces, Scott tendría varias maneras de desarrollar y probar su actualización, pero ninguna de ellas es ideal:
* Ejecutar TODOS los componentes localmente. Esto requiere una máquina de desarrollo más potente con Docker instalado y, potencialmente, MiniKube.
* Ejecutar TODOS los componentes en un espacio de nombres aislado en el clúster de Kubernetes. Puesto que *webfrontend* no va a cambiar, esto es un desperdicio de los recursos de clúster.
* Ejecutar SOLO *mywebapi* y realizar llamadas REST manuales para probar. Este método no prueba el flujo completo.
* Agregar código centrado en el desarrollo a *webfrontend* que permita al desarrollador enviar solicitudes a una instancia diferente de *mywebapi*. Esto complica el servicio *webfrontend*.

### <a name="set-up-your-baseline"></a>Establecimiento de la base de referencia
En primer lugar, tenemos que implementar una base de referencia de nuestros servicios. Esta implementación representará el "último punto correcto conocido" para poder comparar fácilmente el comportamiento del código local frente a la versión insertada en el repositorio. A continuación, crearemos un espacio secundario asentado en esta base de referencia, para que podamos probar los cambios de *mywebapi* dentro del contexto de la aplicación más grande.

1. Clone [la aplicación de ejemplo de Dev Spaces](https://github.com/Azure/dev-spaces): `git clone https://github.com/Azure/dev-spaces && cd dev-spaces`
1. Extraiga del repositorio la rama remota *azds_updates*: `git checkout -b azds_updates origin/azds_updates`
1. Seleccione el espacio _dev_: `azds space select --name dev`. Cuando se le pida que seleccione un espacio de desarrollo principal, seleccione _\<ninguno\>_.
1. Vaya hasta el directorio _mywebapi_ y ejecute: `azds up -d`
1. Vaya al directorio _webfrontend_ y ejecute: `azds up -d`
1. Ejecute `azds list-uris` para ver el punto de conexión público para _webfrontend_

> [!TIP]
> Los pasos anteriores configuran manualmente una base de referencia, pero se recomienda a los equipos el uso de CI/CD para mantener al día automáticamente la base de referencia con el código confirmado.
>
> Visite nuestra [guía para configurar CI/CD con Azure DevOps](../articles/dev-spaces/how-to/setup-cicd.md) con el fin de crear un flujo de trabajo similar al del siguiente diagrama.
>
> ![Diagrama de CI/CD de ejemplo](../articles/dev-spaces/media/common/ci-cd-complex.png)

En este momento la base de referencia debe estar ejecutándose. Ejecute el comando`azds list-up --all` y verá una salida similar a la siguiente:

```
$ azds list-up --all

Name                          DevSpace  Type     Updated  Status
----------------------------  --------  -------  -------  -------
mywebapi                      dev       Service  3m ago   Running
mywebapi-56c8f45d9-zs4mw      dev       Pod      3m ago   Running
webfrontend                   dev       Service  1m ago   Running
webfrontend-6b6ddbb98f-fgvnc  dev       Pod      1m ago   Running
```

La columna DevSpace muestra que ambos servicios se están ejecutando en un espacio denominado _dev_. Cualquiera que abra la dirección URL pública y vaya a la aplicación web, invocará la ruta de acceso al código insertado en el repositorio y que se ejecuta mediante ambos servicios. Ahora suponga que quiere seguir desarrollando _mywebapi_. ¿Cómo puede realizar cambios de código y probarlos y sin interrumpir a otros desarrolladores que usan el entorno de desarrollo? Para ello, configurará su propio espacio.

### <a name="create-a-dev-space"></a>Creación de un espacio de desarrollo
Para ejecutar su propia versión de _mywebapi_ en un espacio que no sea _dev_, puede crear su propio espacio mediante el comando siguiente:

```cmd
azds space select --name scott
```

Cuando se le solicite, seleccione _dev_ como el **espacio de desarrollo principal**. Esto significa que nuestro nuevo espacio _dev/scott_, derivará del espacio _dev_. En breve veremos cómo nos ayudará esto con las pruebas.

Manteniendo nuestro caso hipotético de introducción, hemos usado el nombre _scott_ en el nuevo espacio para que los compañeros puedan identificar quién está trabajando en él. Pero le puede dar cualquier nombre que desee, con flexibilidad sobre lo que significa, como _sprint4_ o _demo_. En cualquier caso, _dev_ actúa como la base de referencia para todos los programadores que trabajan en una parte de esta aplicación:

![](../articles/dev-spaces/media/common/ci-cd-space-setup.png)

Ejecute el comando `azds space list` para ver una lista de todos los espacios del entorno de desarrollo. La columna _Seleccionado_ indica el espacio de que ha seleccionado actualmente (true/false). En su caso, el espacio denominado _dev/scott_ se seleccionó automáticamente al crearse. Puede seleccionar otro espacio en cualquier momento con el comando `azds space select`.

Vamos a verlo en acción.
---
title: IG-03 Configuración_NUGET
description: Instructivo gestión de la configuración de NuGets
published: true
date: 2023-04-04T21:38:50.360Z
tags: nuget, gestion de la configuracion, instructivos, ig-03, dependencias, librerias, paquetes
editor: markdown
dateCreated: 2023-04-04T19:44:54.567Z
---

# Descripción General

## 1. Objetivos

Este instructivo tiene como propósito establecer la metodología para gestionar la configuración de los paquetes que son identificados como dependencias en un sistema de información .NET mediante la tecnología NUGET.

## 2. ALCANCE

Este instructivo es aplicable a las dependencias generadas para los proyectos de desarrollo .NET. Las actividades de gestión de configuración de las dependencias comprenden:

- Identificar las dependencias.
- Generar paquetes para las dependencias.
- Versionar los paquetes para las dependencias.
- Controlar el cambio de los paquetes.
- Almacenar los paquetes en el repositorio.
- Servir los paquetes en los IDE de desarrollo (Visual Studio).

## 3.	POLITICAS

Todos los productos de trabajo deben ser puestos bajo control del Sistema de Gestión de Configuración, cumpliendo con sus lineamientos con el fin de velar por su integridad.

## 4.	CONDICIONES GENERALES

- Se considera objeto de configuración para desarrollos WEB *(códigos fuentes de las páginas web, librerías javas, archivos .jar, librerías dll, librerías C#.)*{.caption} 
- Los desarrollos en ambiente web .NET se trabajan por proyectos los cuales son realizados en la herramienta VISUAL STUDIO.
- La gestión de configuración para proyectos WEB se realiza a través de SVN Subversión.
- Todo cambio que afecte o no la funcionalidad de un programa que se encuentre en producción debe generar una nueva versión. Cada vez que se genere un nuevo objeto de los sistemas, se debe definir su nombre según lo indicado en los instructivos ID-05 *`Construcción Software`*.
- Los backups de los objetos de configuración se realizan siguiendo el instructivo IA-04 `*Realización Backups`*.

## 5.	DOCUMENTOS REFERENCIADOS  

- [Instructivo ID-05 *Construcción Software*](/Analisis_Diseño/Instructivos/ID-05-Construcción_de_software.md)
- [Instructivo ID-09 *Construcción Software WEB*](/Analisis_Diseño/Instructivos/ID-09-Construcción_Software_WEB.md)
- [Instructivo IA-04 *Realización Backups*](/)
- [Guía GA-03 *Definiciones SGC*](/)
- [FD-14 *Arquitectura e Interfaces_GENNET*](/)
{.links-list}

# 6.	GESTIÓN DE PAQUETES NUGET

## 6.1.	Servidor de paquetes NUGET

Para la correcta gestión de configuración de los paquetes NUGET de ACTSIS, es necesario contar con un _servidor_ de paquetes NUGET como gestor de dependencias. El servidor es encargado de almacenar, controlar la publicación, listar y servir a los IDE como Visual Studio los paquetes NUGET. El servidor, el IDE y la configuración del paquete, intrínsecamente controlan la correcta instalación, actualización y eliminación de los paquetes con sus respectivas dependencias y versiones en cada proyecto .NET.

La creación y configuración del servidor así como la configuración en el IDE, se encuentra documentada en el repositorio SVN https://svn.actsis.com/svn/NUGET/NugetServer

## 6.2.	Identificar dependencias.

Las librerías, proyectos o contenidos desarrollados por ACTSIS o por un tercero, que son usados en otros proyectos .NET de ACTSIS, son identificados como dependencias.

Las dependencias que son desarrolladas por terceros, deben ser revisadas y aprobadas para uso en los proyectos .NET por parte del proyecto GENERAL. Estas librerías se gestionan por medio de un único paquete NUGET, teniendo en cuenta el ámbito de la librería.

> **3rosCore:** Corresponde al paquete NUGET que agrupa todas las librerías usadas a nivel de Core
{.is-info}

> **3rosWeb:** Corresponde al paquete NUGET que agrupa todas las librerías usadas a nivel de proyecto ASP .NET Web Forms.
{.is-info}

Los proyectos desarrollados por ACTSIS que son identificados como dependencias, son los siguientes:

## tipos {.tabset}

### Core <i class="mdi mdi-cpu-64-bit"></i>

Todos los proyectos identificados como **Core** de un sistema, teniendo en cuenta la definición Core de un proyecto .NET del documento **FD-14** Arquitectura e Interfaces_General .NET.

### GENNET <i class="mdi mdi-alpha-g-box"></i>
El proyecto **GENNET**, es identificado como dependencia base para cualquier proyecto ASP.NET Web Forms de ACTSIS, según el documento **FD-14** Arquitectura e Interfaces_General .NET.

### Otros <i class="mdi mdi-set-none"></i>
  
mdi mdi-set-none

# 6.3. Tipos de paquetes NUGET.

Teniendo en cuenta la identificación de las dependencias que son controladas con paquetes NUGET, se definen 2 tipos de paquetes NUGET para su generación.

## tipos_NuGets {.tabset}

### Paquetes NUGET de librería dll

Corresponden a los paquetes que gestionan las dependencias de proyectos .Net que solo generan librerías dll, el destino de estas librerías en los proyectos que la usan, es la carpeta **Bin**. Teniendo en cuenta que el paquete **3rosCore** entrega todas las librerías dll de terceros que son requeridas a nivel de Core, este es tipificado como paquete de librerías dll.

### Paquetes NUGET de contenido

Corresponden a los paquetes que gestionan cualquier tipo de archivo para ser ubicado dentro de la estructura del proyecto .NET. Estos paquetes pueden entregar código no compilado que debe ser compilado por el proyecto que lo usa. Para este caso se encuentra el paquete NUGET del proyecto GENNET, el cual entrega todo el contenido del proyecto ASP .NET Web Forms correspondiente a las pantallas, controles de usuario, scripts y otras clases C# que debe compilar el proyecto que lo usa. Teniendo en cuenta que el paquete 3rosWeb entrega todos los archivos, contenidos, scripts, dlls, xml y cualquier otro tipo de archivo que sea requerido a nivel de proyecto ASP .NET Web Forms desarrollado por un tercero, es tipificados como proyecto de contenido.

## 6.4.	Generar paquetes NUGET.

Para la generación del paquete NUGET se debe tener en cuenta las siguientes instrucciones.

### 6.4.1.	Identificador del paquete NUGET.

Cada paquete debe tener un identificador único en el lugar que se encuentre ubicado, el identificador no distingue entre mayúsculas y minúsculas. Los identificadores no pueden contener espacios ni caracteres no válidos para una dirección URL, por lo anterior, el identificador debe representar el proyecto del cual es generado cumpliendo los lineamientos anteriores.

Para el caso de los proyectos CORE, los cuales normalmente son identificados con la nomenclatura CORE_NOMBRE, el paquete NUGET seria identificado de igual manera pero sin el guion de piso.

Ejemplo:

> CoreSan, CoreSac, CoreGen, GENNET. 

### 6.4.2.	Versión del paquete NUGET.

Cada paquete debe tener una versión específica, que corresponde a un número en el formato **principal.secundaria.revisión[-sufijo]**, donde cada parte corresponde a lo siguiente:

- **Principal:** Versión aprobada del sistema, teniendo en cuenta el documento IG-01 Administración de la Configuración. 
- **Secundaria:** Versión liberada en un ROADMAP teniendo en cuenta el documento IG-01 Administración de la Configuración.
- **Revisión:** Corresponde al número de revisión en el repositorio de versiones Subversión del proyecto que generó el cambio.
- **-Sufijo:** Un guion seguido de una cadena para indicar que el paquete es una versión preliminar, las versiones preliminares se entienden como todas aquellas que no corresponden a un TAG de repositorio. En ACTSIS se usa entonces la cadena -rc (release candidate), para denotar una versión de paquete preliminar  generada de la rama tronco, por esto se dice que es preliminar. Si el paquete corresponde a un TAG del repositorio del proyecto, es considerado una versión estable y no tiene sufijo.

Ejemplos:

> **Coregen.7.01.222-rc**
Indica la versión aprobada y liberada del sistema 7.01, la revisión 222 indica que en el repositorio la revisión 222 es la copia exacta que genera el paquete y finalmente se indica con el sufijo –rc que el paquete de esta revisión corresponde a una revisión del tronco del proyecto.
{.is-info}


> **CoreGen.7.01.222**
Indica la versión aprobada y liberada del sistema 7.01, la revisión 222 indica que en el repositorio la revisión 222 es la copia exacta que genera el paquete y finalmente como no hay sufijo, se indica que la revisión corresponde a un TAG liberado en Subversión.
{.is-info}

### 6.4.3.	Archivos del paquete NUGET.

Teniendo en cuenta los tipos de paquetes a generar, se debe contar con los correspondientes archivos que el paquete NUGET entregará, de la siguiente manera:

- **Paquetes NUGET de librería dll:** Previamente a la creación del paquete se debe generar archivo dll del proyecto que es identificado como dependencia desde la revisión o TAG especifico a entregar, la generación del archivo dll se debe generar compilando el proyecto en modo release.

- **Paquetes NUGET de contenido:** Posterior a la compilación del proyecto identificado como dependencia y en la revisión o TAG específico, se debe tener en cuenta los archivos y/o carpetas que deben ser entregadas por el paquete. Para modificar la salida o archivos a entregar en el paquete en la generación automatica, la configuración de compilación del proyecto puede especificarse para una configuración personalizada que debe nombrarse Nuget y heredando todas las propiedades de la configuración Debug.

Ejemplo GENNET:

#### Configuración Debug

```csproj
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|AnyCPU' ">
	<DebugSymbols>true</DebugSymbols>
	<DebugType>full</DebugType>
	<Optimize>false</Optimize>
	<OutputPath>bin\</OutputPath>
	<DefineConstants>DEBUG;TRACE</DefineConstants>
	<ErrorReport>prompt</ErrorReport>
	<WarningLevel>4</WarningLevel>
	<FilesToIncludeForPublish>OnlyFilesToRunTheApp</FilesToIncludeForPublish>
	<DocumentationFile>bin\presentacion.ACTSIS.XML</DocumentationFile>
	<CodeAnalysisRuleSet>presentacion.ACTSIS.ruleset</CodeAnalysisRuleSet>
	<PublishDatabases>false</PublishDatabases>
</PropertyGroup>
```

#### Configuración Nuget personalizada

```csproj
<PropertyGroup Condition="'$(Configuration)|$(Platform)' == 'Nuget|AnyCPU'">
	<DebugSymbols>true</DebugSymbols>
	<OutputPath>bin\</OutputPath>
	<DefineConstants>DEBUG;TRACE</DefineConstants>
	<DocumentationFile>
	</DocumentationFile>
	<DebugType>full</DebugType>
	<PlatformTarget>AnyCPU</PlatformTarget>
	<ErrorReport>prompt</ErrorReport>
	<CodeAnalysisRuleSet>presentacion.ACTSIS.ruleset</CodeAnalysisRuleSet>
	<PublishDatabases>false</PublishDatabases>
</PropertyGroup>
```

Condición en el contenido especifico web.config para la configuración Nuget, indicando que el archivo solo será incluido si se compila con una configuración diferente a Nuget, de esta manera al generar el paquete de manera automatica, el archivo será excluido en el paquete.

```
<Content Include="Web.config" Condition=" '$(Configuration)' != 'Nuget' ">
	<SubType>Designer</SubType>
</Content>
```

**NUGET de terceros:** Solo se debe tener en cuenta las dlls entregadas por el NUGET de tercero que fue identificado y aprobado para uso de ACTSIS.

### 6.4.4.	Creación del paquete NUGET.

Una vez se cuente con los requisitos de los pasos anteriores, se debe generar el paquete NUGET.

Para esta generación tener en cuenta las recomendaciones para usar una u otra forma de acuerdo a lo siguiente:

- **Manualmente:** La generación manual se sugiere solo cuando no sea posible generar el paquete por medio de un script automatizado o con la herramienta NUGET Package Explorer, este tipo de generación debe solicitarse al rol asignado para administrar los paquetes NUGET y cuente con los permisos requeridos para la publicación.

- **NUGET Package Explorer:** La generación con esta herramienta se sugiere solo cuando no sea posible generar el paquete por medio de un script automatizado, para la generación del paquete se debe tomar de referencia un paquete ya construido y la documentación del repositorio https://svn.actsis.com/svn/NUGET/Templates. Este tipo de generación debe solicitarse al rol asignado para administrar los paquetes NUGET y cuente con los permisos requeridos para la publicación.

- **Script automatizado:** En las tareas de integración continua, los proyectos que son identificados como dependencias de otros, tienen una sección final como tarea y es la generación y publicación del paquete NUGET, la cual está documentada en el archivo de automatización y en el repositorio https://svn.actsis.com/svn/NUGET/Templates. Al ser automática como parte del proceso de integración, no se requiere de personal para la creación del paquete, si la creación automática falla, la creación del paquete se realiza de alguna de las dos formas anteriores.

## 6.5.	Almacenar/Listar paquetes NUGET.

Posterior a la creación de un paquete NUGET, se debe publicar el paquete en el servidor de paquetes NUGET, de esta manera queda disponible para su uso. La publicación del paquete en el servidor se puede realizar de tres maneras diferentes de igual manera que la generación de paquetes:

- **Manualmente:** La publicación manual de paquetes NUGET debe solicitarse al rol asignado para administrar los paquetes NUGET y cuente con los permisos requeridos para la publicación. Para esto se requiere tener en el equipo desde donde se realizará la publicación, el entorno de comandos NUGET.exe disponible como descarga en la URL https://www.NUGET.org/downloads, en este momento se encuentra disponible la versión 4.7.1. El comando para realizar la publicación del paquete es la siguiente:

```bash
NUGET.exe push -Source [URL servidor] -NonInteractive -ApiKey [*****] [Paquete].nupkg
```

Teniendo en cuenta que para ejecutar el comando indicado debe estar ubicado en el mismo nivel de carpeta donde se encuentra ubicado el paquete.

- **[URL servidor]:** Corresponde a la URL del servidor de paquetes NUGET.
- **[*****]:** Corresponde a la clave de seguridad para publicar paquetes en el servidor de paquetes NUGET.
- **[Paquete]:** Corresponde al nombre del paquete generado que se va a publicar.

	-	**NUGET Package Explorer:** La publicación del paquete debe solicitarse al rol asignado para administrar los paquetes NUGET y cuente con los permisos requeridos para la publicación. Esta herramienta no requiere el entorno de comando NUGET.exe. La publicación se realiza con el paquete abierto en la herramienta de la siguiente manera:
  
	1.   Ingresar al menú File y hacer clic en la opción Publish, para ingresar al cuadro de dialogo correspondiente.

	![nugetpackageexplroer_1.png](/recursos/imagenes/nugetpackageexplroer_1.png)
  
  En el cuadro de dialogo Publish Package, ingresar en el campo Publish Url, la URL del servidor de paquetes NUGET y en el cuadro de dialogo Publish key or PAT, la clave de seguridad para publicar paquetes en el servidor. Ninguna de las opciones inferiores requiere ser marcadas. Finalmente, haga clic en el botón Publish, para publicar el paquete.
  
  ![nugetpackageexplroer_2.png](/recursos/imagenes/nugetpackageexplroer_2.png)
  
  Posibles errrores en la publicación del paquete:

- Clave errada: la clave no coincide, se debe confirmar la clave con recursos tecnicos.
- Paquete con tamaño mayor al definido como tamaño maximo en el servidor: Se debe configurar en el servidor de paquetes NUGET para aceptar paquetes de mayor tamaño.
- Versión del paquete ya existe: Se debe revisar el paquete creado, ya que el servidor detecta que ya existe un paquete identico con la misma versión.

- **Script automatizado:** En las tareas de integración continua, los proyectos que son identificados como dependencias de otros, tienen una sección final como tarea y es la generación y publicación del paquete NUGET, la cual está documentada en el archivo de automatización y en el repositorio https://svn.actsis.com/svn/NUGET/Templates.

### 6.6.	Instalar paquetes NUGET.

La tarea operativa de instalar un paquete NUGET está sujeta a que previamente se encuentre identificado en el proyecto correspondiente el uso de la dependencia. Por defecto, los proyectos CORE de un sistema específico tienen dependencia de la librería CoreGen, y estas a su vez de la librería 3rosCore, los proyectos web y los servicios web que requieren el uso de la capa CORE de un sistema, tienen dependencia de la librería CORE del sistema.

Ejemplo:

![configuracion_nugets_ejemplo.png](/recursos/imagenes/configuracion_nugets_ejemplo.png)

Para la instalación de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones, en este mismo repositorio, se encuentran las instrucciones operativas para la instalación de un paquete nuget.

## 6.7.	Actualizar paquetes NUGET.

a tarea operativa de actualizar los paquetes NUGET de GENERAL existentes en un proyecto, está sujeta a que previamente se encuentre identificado en el análisis del requerimiento en desarrollo, la necesidad de actualizar el paquete o exista un requerimiento específico para la actualización.

Para los proyectos WEB .NET, la actualización de los paquetes NUGET se presenta en los siguientes escenarios:

1.	En los proyectos como portales o servicios web, los cuales se encuentran desactualizados respecto al CORE del propio sistema, para estos casos la actualización del CORE debe realizarse por medio de un requerimiento con el alcance respectivo o durante la actualización del mismo servicio si requiere actualizar las clases relacionadas al servicio en el CORE.

2.	Previo a la integración en el tronco de los desarrollos de los proyectos web que tengan una dependencia también en desarrollo como un CORE, primero se debe integrar el proyecto CORE, para generar por medio de las tareas de integración continua el paquete NUGET respectivo, de esta manera antes de integrar el proyecto web se actualiza el NUGET correspondiente con el generado en la integración del CORE.

3.	Previo a la generación de un TAG del proyecto, ya que previamente se debe crear el TAG correspondiente al proyecto del CORE, de esta manera, el integrador obtiene el paquete del CORE correspondiente al TAG y lo actualiza en la rama tronco del proyecto web o de otro tipo, para generar el TAG de este mismo proyecto.

Para la actualización de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones, en este mismo repositorio, se encuentran las instrucciones operativas para la actualización de un paquete nuget.

## 6.8.	Restaurar paquetes NUGET.

La tarea de restauración de paquetes NUGET se requerida cuando se descarga un proyecto .NET de su correspondiente repositorio y se requiere compilar o publicar el proyecto, la restauración descarga las dependencias requeridas, ya que el proyecto no es versionado en el repositorio con las dependencias.

Para la restauración de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones/OrigenPaquetes, en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones/Restaurar se encuentran las instrucciones operativas para la restauración de paquete nuget.

## 6.9.	Desinstalar paquetes NUGET.

La tarea operativa de desinstalar un paquete NUGET existente en un proyecto, está sujeta a que previamente se encuentre identificado en el análisis del requerimiento en desarrollo, la necesidad de eliminar el paquete teniendo en cuenta que no se requiere el uso de las librerías que sirve el paquete.

Para la desinstalación de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones, en este mismo repositorio, se encuentran las instrucciones operativas para la desinstalación de un paquete nuget.

## 6.10.	Eliminar paquetes NUGET.

Como parte del mantenimiento del servidor de paquetes nuget, se definen las siguientes condiciones para la eliminación de paquetes nuget del servidor:

- Solo se permite eliminar los paquetes nuget correspondientes a versiones preliminares, es decir, aquellas que tienen un sufijo que indican esta condición, por ejemplo el sufijo –rc, las cuales corresponden a una revisión de una rama en el repositorio diferente a la rama TAG.
- Los paquetes nuget que corresponden a versiones estables, es decir, que no tienen un sufijo, no deben ser eliminadas.
- Para la eliminación de los paquetes de versiones preliminares se define que serán eliminados los paquetes correspondientes a versiones inferiores a la penúltima versión estable del paquete, o teniendo en cuenta como referencia el repositorio de cada proyecto, se deben eliminar los paquetes correspondientes a revisiones inferiores al penúltimo TAG generado y que correspondan a una rama diferente a TAG.

Ejemplo:

![eliminar_paquetes_nuget.png](/recursos/imagenes/eliminar_paquetes_nuget.png)


![logo-banner-principal.png](/recursos/imagenes/logos/logo-banner-principal.png)
{.align-abstopright}
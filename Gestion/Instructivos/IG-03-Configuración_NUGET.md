---
title: IG-03 Configuración NUGET
description: Instructivo gestión de la configuración de NuGets
published: true
date: 2023-04-24T19:42:10.791Z
tags: nuget, gestion de la configuracion, instructivos, ig-03, dependencias, librerias, paquetes
editor: markdown
dateCreated: 2023-04-04T19:44:54.567Z
---

# Descripción General

## 1. OBJETIVOS

Este instructivo tiene como propósito establecer la metodología para gestionar la configuración de los paquetes que son identificados como dependencias en un sistema de información .NET mediante la tecnología NUGET y DotNet.

## 2. ALCANCE

Este instructivo es aplicable a las dependencias generadas para los proyectos de desarrollo .NET. Las actividades de gestión de configuración de las dependencias comprenden:

- Identificar las dependencias.
- Generar paquetes para las dependencias.
- Versionar los paquetes para las dependencias.
- Controlar el cambio de los paquetes.
- Almacenar los paquetes en el repositorio.
- Servir los paquetes en los IDE de desarrollo (Visual Studio).
- Uso de buenas prácticas con metadatos en `.csproj`/`.nuspec`.

## 3.	POLITICAS

Todos los productos de trabajo deben ser puestos bajo control del Sistema de Gestión de Configuración, cumpliendo con sus lineamientos con el fin de velar por su integridad.

## 4.	CONDICIONES GENERALES

- Se considera objeto de configuración para desarrollos WEB *(códigos fuentes de las páginas web, librerías javas, archivos .jar, librerías dll, librerías C#.)*{.caption} 
- Los desarrollos en ambiente web .NET se trabajan por proyectos los cuales son realizados en la herramienta VISUAL STUDIO, instale la 2017 Community Edition gratuitamente desde [visualstudio.com](https://visualstudio.microsoft.com/es/). 
- La gestión de configuración para proyectos WEB se realiza a través de SVN Subversión.
- Todo cambio que afecte o no la funcionalidad de un programa que se encuentre en producción debe generar una nueva versión. Cada vez que se genere un nuevo objeto de los sistemas, se debe definir su nombre según lo indicado en los instructivos ID-05 *`Construcción Software`*.
- Los backups de los objetos de configuración se realizan siguiendo el instructivo IA-04 `*Realización Backups`*.
- CLI de NuGet. Descargue la versión más reciente de nuget.exe desde nuget.org/downloads y guárdela en la ubicación que prefiera (se descarga directamente el .exe). Después, agregue esa ubicación a la variable de entorno PATH si aún no está.

## 5.	DOCUMENTOS REFERENCIADOS  

- [Instructivo ID-05 *Construcción Software*](/Analisis_Diseño/Instructivos/ID-05-Construcción_de_software.md)
- [Instructivo ID-09 *Construcción Software WEB*](/Analisis_Diseño/Instructivos/ID-09-Construcción_Software_WEB.md)
- [Instructivo IA-04 *Realización Backups*](/)
- [Guía GA-03 *Definiciones SGC*](/)
- [FD-14 *Arquitectura e Interfaces_GENNET*](/)
- [ft-03_entrevista_prueba_técnica_prueba_psicotécnica.doc](/recursos/ft-03_entrevista_prueba_técnica_prueba_psicotécnica.doc)
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
  
Cualquier otro proyecto, modulo o parte de un proyecto, que defina el documento de arquitectura de un sistema, para ser compartido entre otros proyectos.

## 6.3. Tipos de paquetes NUGET.

Teniendo en cuenta la identificación de las dependencias que son controladas con paquetes NUGET, se definen 2 tipos de paquetes NUGET para su generación.

## tipos_NuGets {.tabset}

### Paquetes NUGET de librería dll

Corresponden a los paquetes que gestionan las dependencias de proyectos .Net que solo generan librerías dll, el destino de estas librerías en los proyectos que la usan, es la carpeta **Bin**. Teniendo en cuenta que el paquete **3rosCore** entrega todas las librerías dll de terceros que son requeridas a nivel de Core, este es tipificado como paquete de librerías dll.

### Paquetes NUGET de contenido

Corresponden a los paquetes que gestionan cualquier tipo de archivo para ser ubicado dentro de la estructura del proyecto .NET. Estos paquetes pueden entregar código no compilado que debe ser compilado por el proyecto que lo usa. Para este caso se encuentra el paquete NUGET del proyecto GENNET, el cual entrega todo el contenido del proyecto ASP .NET Web Forms correspondiente a las pantallas, controles de usuario, scripts y otras clases C# que debe compilar el proyecto que lo usa. Teniendo en cuenta que el paquete 3rosWeb entrega todos los archivos, contenidos, scripts, dlls, xml y cualquier otro tipo de archivo que sea requerido a nivel de proyecto ASP .NET Web Forms desarrollado por un tercero, es tipificados como proyecto de contenido.

## 6.4.	Generar paquetes NUGET.

Para la generación del paquete NUGET se debe tener en cuenta las siguientes instrucciones.

### 6.4.1. Convenciones de nombrado

Cada paquete debe tener un identificador único en el lugar que se encuentre ubicado, el identificador no distingue entre mayúsculas y minúsculas. Los identificadores no pueden contener espacios ni caracteres no válidos para una dirección URL, por lo anterior, el identificador debe representar el proyecto del cual es generado cumpliendo los lineamientos anteriores.

Para el caso de los proyectos CORE, los cuales normalmente son identificados con la nomenclatura CORE_NOMBRE, el paquete NUGET seria identificado de igual manera pero sin el guion de piso.

#### Estilos de nombrado

Dentro del ámbito de la programación, se emplean diversos estilos para sustituir los `espacios ( )` en la definición de elementos, incluyendo nombres de variables, funciones, clases, y en la codificación de URLs, entre otros ejemplos, en el caso de la creación de paquetes NuGets en ACTSIS se utiliza los siguientes:

#### convenciones_nombrado {.tabset}

##### PascalCase

También llamado `StudlyCase`. Es muy similar al `camelCase`, solo que en este caso **la primera letra va en mayúscula**. El nombre proviene del lenguaje de programación. Ejemplos:

Ejemplos:

> **EstoEstaBien**
{.is-success}

> estoYaNo
{.is-danger}

> esto_menos
{.is-danger}

##### FlatCase o dot notation

En este estilo, se reemplazan los *espacios* `por puntos (.)`, todo el texto se escribe en minúsculas y las palabras se separan por medio de un punto. Cabe destacar que flat case es conocido por tener como excepción que las iniciales de cada palabra se escriben en mayúscula.

Ejemplos:

> **Actsis.Esta.Bien**
{.is-success}

> actsis.Log
{.is-danger}

> esto.menos
{.is-danger}

#### Prefijos

Para mantener el identificador único y evitar problemas de compatibilidad al restaurar o instalar paquetes NuGets, se sugiere agregar `Actsis` como prefijo.

#### Ejemplos de uso
> CoreSan, CoreSac, CoreGen, Actsis.Echarts.Webforms

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

## 6.5.	Configuración de metadatos

Este punto se centrará en procedimientos recomendados especificos del paquete, como metadados y el empaquetado, para así crear bibliotecas de alta calidad.

### 6.5.1. Creación del archivo .nuspec

La creación de un manifiesto completo normalmente comienza con un archivo `.nuspec` básico generado a travez de un comando del CLI nuget.exe.

```cli
nuget spec
```

> Los archivos `.nuspec` generados contienen marcadores de posición que se deben modificar antes de crear el paquete con el comando `nuget pack`. Si `.nuspec` contiene marcadores de posición, se produce un error en el comando.
{.is-info}

El `.nuspec` generado será muy básico, ya que carece de metadatos como lenguaje, icon, License, readme, etc. Usar la siguiente plantilla para configurar todos los metadatos recomendados.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package >
	<metadata>
    <!-- Nombre o el identificador del paquete. -->
		<id>Actsis.ECharts.Webforms</id>
    
    <!-- Versión del paquete NuGet. -->
		<version>8.0.$rev$</version>
    
    <!-- Título descriptivo del paquete que se puede usar en algunas pantallas de interfaz de usuario.  -->
		<title>Librería de controles ECharts</title>
    
    <!-- Lista separada por comas de los autores del paquete, que a menudo usan el "nombre descriptivo" de la persona o de la organización. -->
		<authors>GENERAL</authors>
    
    <!-- Lista separada por comas de los creadores de paquetes que usan nombres de perfil. -->
		<owners>$owner$</owners>
    
    <!-- Descripción breve del paquete para su visualización en la interfaz de usuario. -->
		<description>Controles personalizados para gráficas Apache ECharts en Webforms.</description>
    
    <!-- Descripción de los cambios efectuados en esta versión del paquete. -->
		<releaseNotes>$changes$</releaseNotes>
    
    <!-- Valor booleano que especifica si el cliente debe pedir al consumidor que acepte la licencia del paquete antes de instalarlo. -->
		<requireLicenseAcceptance>false</requireLicenseAcceptance>
    
    <!-- Metadatos del repositorio, que constan de cuatro atributos opcionales: type, url y branchcommit. -->
		<repository type="svn" url="$repositoryUrl$" branch="$repositoryBranch$" commit="$repositoryCommit$"/>
    
    <!-- Una dirección URL de la página principal del paquete. -->
		<projectUrl>$projectUrl$</projectUrl>
    
    <!-- Lista de etiquetas y palabras clave, delimitadas por espacios, que describen el paquete y ayudan a detectar los paquetes a través de búsquedas y filtrados. -->
		<tags>General ACTSIS</tags>
    
    <!-- Información de copyright del paquete. -->
		<copyright>Copyright 2023</copyright>
    
    <!-- Es una ruta de acceso a un archivo de imagen dentro del paquete. -->
		<icon>icon.png</icon>
    
    <!-- Es una ruta de acceso a un archivo del readme dentro del paquete. -->
		<readme>README.md</readme>
    
    <!-- Identificador de configuración regional del paquete. -->
		<language>es-CO</language>
    
    <!-- Es una ruta de acceso a un archivo del licencia dentro del paquete. -->
		<license type="file">LICENSE</license>
	</metadata>
	<files>
    <!-- text -->
		<file src="icon.png" target="" />
		<file src="README.md" target="" />
		<file src="LICE*" target="" />
	</files>
</package>

```

### files {.tabset}

#### icon <i class="mdi mdi-package-variant"></i>

*Compatible con **NuGet 5.3.0** y versiones posteriores*

Es una ruta de acceso a un archivo de imagen dentro del paquete, que a menudo se muestra en interfaces de usuario como Nuget Gallery como el icono del paquete. El tamaño del archivo de imagen está limitado a 1 MB. Los formatos de archivo admitidos incluyen **JPEG** y **PNG**. Se recomienda una resolución de imagen de **128 x 128**.

Por ejemplo, agregaría lo siguiente a nuspec al crear un paquete mediante nuget.exe:

```xml
<package>
  <metadata>
    ...
    <icon>images\icon.png</icon>
    ...
  </metadata>
  <files>
    ...
    <file src="..\icon.png" target="images\" />
    ...
  </files>
</package>
```

#### README <i class="mdi mdi-read"></i>

*Compatible con la versión **preliminar 2 y posteriores de NuGet 5.10.0***

Al empaquetar un archivo Léame, debe usar el `readme` elemento para especificar la ruta de acceso del paquete, en relación con la raíz del paquete. Además de esto, debe asegurarse de que el archivo se incluye en el paquete. Los formatos de archivo admitidos incluyen solo Markdown (.md).

Por ejemplo, agregaría lo siguiente a nuspec al crear un paquete mediante nuget.exe:

```xml
<package>
  <metadata>
    ...
    <icon>images\icon.png</icon>
    ...
  </metadata>
  <files>
    ...
    <file src="..\icon.png" target="images\" />
    ...
  </files>
</package>
```

#### LICENSE <i class="mdi mdi-license"></i>

*Compatible con **NuGet 4.9.0 y versiones posteriores***

Expresión de licencia SPDX o ruta de acceso a un archivo de licencia dentro del paquete, que a menudo se muestra en interfaces de usuario como Nuget Gallery. Si va a conceder licencias al paquete bajo una licencia común, como MIT o BSD-2-Clause, use el [identificador de licencia SPDX](https://spdx.org/licenses/) asociado. 
Por ejemplo:

`<license type="expression">MIT</license>`

Si usa una licencia personalizada que no es compatible con expresiones de licencia, puede empaquetar un `.txt` archivo o `.md` con el texto de la licencia. Por ejemplo:

```xml
<package>
  <metadata>
    ...
    <license type="file">LICENSE.txt</license>
    ...
  </metadata>
  <files>
    ...
    <file src="licenses\LICENSE.txt" target="" />
    ...
  </files>
</package>
```

en caso de no usar una extensión solo debe cambiar el file por el siguiente ejemplo:

`<file src="LICE*" target="" />`

### Estructura de carpeta

Para usar correctamente la plantilla de los metadatos, ubicar los archivos icon, readme y license en la misma carpeta del proyecto.

```diagram
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSIxNzhweCIgaGVpZ2h0PSI0MDFweCIgdmlld0JveD0iLTAuNSAtMC41IDE3OCA0MDEiIGNvbnRlbnQ9IiZsdDtteGZpbGUgaG9zdD0mcXVvdDtlbWJlZC5kaWFncmFtcy5uZXQmcXVvdDsgbW9kaWZpZWQ9JnF1b3Q7MjAyMy0wNC0xMlQxODowODozMS45MzJaJnF1b3Q7IGFnZW50PSZxdW90O01vemlsbGEvNS4wIChXaW5kb3dzIE5UIDEwLjA7IFdpbjY0OyB4NjQ7IHJ2OjEwOS4wKSBHZWNrby8yMDEwMDEwMSBGaXJlZm94LzExMS4wJnF1b3Q7IGV0YWc9JnF1b3Q7ZEhMRzBUbmpsRVhRR3VKajl1ZjMmcXVvdDsgdmVyc2lvbj0mcXVvdDsyMS4xLjcmcXVvdDsgdHlwZT0mcXVvdDtlbWJlZCZxdW90OyZndDsmbHQ7ZGlhZ3JhbSBpZD0mcXVvdDtkYlZFdnQ5dWFJQlR2ZzlLZEhWbiZxdW90OyBuYW1lPSZxdW90O1DDoWdpbmEtMSZxdW90OyZndDs3VmpmYjVzd0VQNXJlSTBBRTVJOHRnbkxKcTNWdER6c21lRUxlRE1ZR2VkSDk5ZlBGRHZnNHJXVjVrWktHK1VoNXZQNXVQdnVQZ3ZiUTh2eXVPWnBYZHd4RE5RTGZYejAwTW9Md3lCZStQS3ZSUjQ2WkQ2UE9pRG5CQ3VqSHRpUVA2QkF0UzdmRVF5TllTZ1lvNExVSnBpeHFvSk1HRmpLT1R1WVpsdEd6YmZXYVE0allKT2xkSXorSUZnVUtvdHcxdU9mZ2VTRmZyUE11SnNwVTIyc01tbUtGTFBEQUVLSmg1YWNNZEdOeXVNU2FFdWU1cVZiOStrZnM2ZkFPRlRpTlF0UXQyQ2YwcDNLVGNVbEhuU3luTzBxREsyOTc2SGJRMEVFYk9vMGEyY1BzcndTSzBSSjVWTWdoMXRDNlpKUnhoL1hJb2phbjhUSGNhbFE5OEFGSEFlUWluTU5yQVRCSDZTSm5wMnJZRlhUQkxxTERuMEo0ZzRwQnVSSHZqSkxWZEh6aytlZUZ6bFExTmhwbW81b1dyR3NjVXBWRXJVL04xU0Z5SCtScWtEYm1HVDlQMWV6bDFzS0tuelR5bEErVmF3Q2t4ZVR4RVp3OXZ1a3N2QTVmZ0FicWgyek04aCtha2xlWXh4b0tzamUxTHFORWZXR2I0eklTUG8rWFpqa24zWXQ3YUpoTzU2QldqWFU1aE5IVDZzNGNpUlNub01ZT1hvczBDbnRWOVZzYnFsWlRDVXZ0NWpzNVRCdmgrdmtYcVBTMzJEaVluUndLdkU1ZExDNDZzQWtQMTVNWnJFaktWaDh1Vk9EMXRuemNxZzUreVUvTGlaWjA0NHVYQmtvT0tNeWdzREM3MGVXQmdyZFNjUG15NkUwd212cFRMcm5Ea3RuOGVXd2RPTnZmYjJGVmJ1bWh1eHlOcXZaT1RlcjZOcnhCdnRSNUs3amJiNGNkdno0MkVZeVZrM3FLbmZhNno4RGpMZDljUWN6Z1Q5REMzZ2JGVVRvbkNxSXJ5b3cySi82N2xSZzgrVlFCZU1EK2Zma1puV1hURXI4SG1Sd3pydU53SFpRL3RBeWNIaW9zL2x5S0lQeGVmenJsMlZ5djBuZWdRaW1iM2l4SVIvNysraU8rUDVXSHlWL0FRPT0mbHQ7L2RpYWdyYW0mZ3Q7Jmx0Oy9teGZpbGUmZ3Q7Ij48ZGVmcy8+PGc+PHJlY3QgeD0iMCIgeT0iMCIgd2lkdGg9IjYiIGhlaWdodD0iNDAwIiBmaWxsPSIjZTRlNGU0IiBzdHJva2U9InJnYigwLCAwLCAwKSIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjQ3IiB5PSIwIiB3aWR0aD0iMTMwIiBoZWlnaHQ9IjQwIiBmaWxsPSIjZTRlNGU0IiBzdHJva2U9InJnYigwLCAwLCAwKSIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMjhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAyMHB4OyBtYXJnaW4tbGVmdDogNDhweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7IiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiByZ2IoMCwgMCwgMCk7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPkRvY3M8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMTEyIiB5PSIyNCIgZmlsbD0icmdiKDAsIDAsIDApIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPkRvY3M8L3RleHQ+PC9zd2l0Y2g+PC9nPjxwYXRoIGQ9Ik0gNyAyMCBMIDQ3IDIwIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cmVjdCB4PSI0NyIgeT0iNjAiIHdpZHRoPSIxMzAiIGhlaWdodD0iNDAiIGZpbGw9IiNlNGU0ZTQiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDEyOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDgwcHg7IG1hcmdpbi1sZWZ0OiA0OHB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiIGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6IHJnYigwLCAwLCAwKTsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigwLCAwLCAwKTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+PGRpdj5HRU48L2Rpdj48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMTEyIiB5PSI4NCIgZmlsbD0icmdiKDAsIDAsIDApIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPkdFTjwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA3IDc5Ljc2IEwgNDcgNzkuNzYiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxyZWN0IHg9IjQ3IiB5PSIxMjAiIHdpZHRoPSIxMzAiIGhlaWdodD0iNDAiIGZpbGw9IiNlNGU0ZTQiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDEyOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDE0MHB4OyBtYXJnaW4tbGVmdDogNDhweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7IiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiByZ2IoMCwgMCwgMCk7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPjxkaXY+cHJvamVjdC5jc3Byb2o8L2Rpdj48L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMTEyIiB5PSIxNDQiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5wcm9qZWN0LmNzcHJvajwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA3IDEzOS43NiBMIDQ3IDEzOS43NiIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA3IDE5OS43NiBMIDQ3IDE5OS43NiIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHJlY3QgeD0iNDciIHk9IjE4MCIgd2lkdGg9IjEzMCIgaGVpZ2h0PSI0MCIgZmlsbD0iI2U0ZTRlNCIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTI4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMjAwcHg7IG1hcmdpbi1sZWZ0OiA0OHB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiIGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6IHJnYigwLCAwLCAwKTsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigwLCAwLCAwKTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+cHJvamVjdC5udXNwZWM8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMTEyIiB5PSIyMDQiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5wcm9qZWN0Lm51c3BlYzwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA3IDI1OS43NiBMIDQ3IDI1OS43NiIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHJlY3QgeD0iNDciIHk9IjI0MCIgd2lkdGg9IjEzMCIgaGVpZ2h0PSI0MCIgZmlsbD0iI2IxZGRmMCIgc3Ryb2tlPSIjMTA3MzllIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDEyOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDI2MHB4OyBtYXJnaW4tbGVmdDogNDhweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7IiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiByZ2IoMCwgMCwgMCk7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPmljb24ucG5nPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjExMiIgeT0iMjY0IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+aWNvbi5wbmc8L3RleHQ+PC9zd2l0Y2g+PC9nPjxwYXRoIGQ9Ik0gNyAzMTkuNzYgTCA0NyAzMTkuNzYiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxyZWN0IHg9IjQ3IiB5PSIzMDAiIHdpZHRoPSIxMzAiIGhlaWdodD0iNDAiIGZpbGw9IiNiMWRkZjAiIHN0cm9rZT0iIzEwNzM5ZSIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMjhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAzMjBweDsgbWFyZ2luLWxlZnQ6IDQ4cHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyIgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDAsIDAsIDApOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5SRUFETUUubWQ8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMTEyIiB5PSIzMjQiIGZpbGw9InJnYigwLCAwLCAwKSIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5SRUFETUUubWQ8L3RleHQ+PC9zd2l0Y2g+PC9nPjxwYXRoIGQ9Ik0gNyAzNzkuNzYgTCA0NyAzNzkuNzYiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxyZWN0IHg9IjQ3IiB5PSIzNjAiIHdpZHRoPSIxMzAiIGhlaWdodD0iNDAiIGZpbGw9IiNiMWRkZjAiIHN0cm9rZT0iIzEwNzM5ZSIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMjhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAzODBweDsgbWFyZ2luLWxlZnQ6IDQ4cHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyIgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDAsIDAsIDApOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5MSUNFTlNFPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjExMiIgeT0iMzg0IiBmaWxsPSJyZ2IoMCwgMCwgMCkiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+TElDRU5TRTwvdGV4dD48L3N3aXRjaD48L2c+PC9nPjxzd2l0Y2g+PGcgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5Ii8+PGEgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMCwtNSkiIHhsaW5rOmhyZWY9Imh0dHBzOi8vd3d3LmRpYWdyYW1zLm5ldC9kb2MvZmFxL3N2Zy1leHBvcnQtdGV4dC1wcm9ibGVtcyIgdGFyZ2V0PSJfYmxhbmsiPjx0ZXh0IHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iMTBweCIgeD0iNTAlIiB5PSIxMDAlIj5UZXh0IGlzIG5vdCBTVkcgLSBjYW5ub3QgZGlzcGxheTwvdGV4dD48L2E+PC9zd2l0Y2g+PC9zdmc+
```



### Descargar plantilla `.nuspec`

- [Descargar plantilla `.nuspec` *nombreproyecto.nuspec*](/recursos/gestion/formatos/nombreproyecto.nuspec)
{.links-list}

### 6.5.2. Creación de metadata usando DotNet

Otra forma de empaquetar los proyectos es usando el CLI Dotnet, esta herramienta es la mas reciente y más utilizada en las ultimas versiones de proyectos .NET, no es necesario el uso de los archivos `.nuspec` ya que toda la configuración puede ir en el proyecto `.csproj`, a diferencia del .nuspec, algunos nombres de los metadatos cambian ligeramente, por lo que no se puede usar la misma configuración.

Ejemplo de los metadatos `.csproj`

```xml
  <PropertyGroup>
    <GeneratePackageOnBuild>False</GeneratePackageOnBuild>
    <id>Actsis.ECharts.Standard</id>
    <VersionSuffix Condition="'$(VersionSuffix)' != ''">$(VersionSuffix)</VersionSuffix>
    <VersionSuffix Condition="'$(VersionSuffix)' == ''"></VersionSuffix>
    <Version Condition="'$(RevNumber)' != ''">8.0.$(RevNumber)-$(VersionSuffix)</Version>
    <Version Condition="'$(RevNumber)' == ''">8.0.0-$(VersionSuffix)</Version>
    <Version Condition="'$(VersionSuffix)' == ''">8.0.$(RevNumber)</Version>
    <Version Condition="'$(RevNumber)' == ''">8.0.0</Version>

    <Authors Condition="'$(AuthorsName)' != ''">$(AuthorsName)</Authors>
    <Authors Condition="'$(AuthorsName)' == ''">GENERAL</Authors>

    <Company Condition="'$(CompanyName)' != ''">$(CompanyName)</Company>
    <Company Condition="'$(CompanyName)' == ''">ACTSIS Ltda</Company>

    <Product Condition="'$(ProductName)' != ''">$(ProductName)</Product>
    <Product Condition="'$(ProductName)' == ''">Actsis.ECharts.Standard</Product>

    <Title Condition="'$(ProjectTitle)' != ''">$(ProjectTitle)</Title>
    <Title Condition="'$(ProjectTitle)' == ''">Actsis.ECharts.Standard</Title>

    <PackageOwnership Condition="'$(Ownership)' != ''">$(Ownership)</PackageOwnership>
    <PackageOwnership Condition="'$(Ownership)' == ''">ACTSIS</PackageOwnership>

    <PackageProjectUrl Condition="'$(ProjectUrl)' != ''">$(ProjectUrl)</PackageProjectUrl>
    <PackageReleaseNotes Condition="'$(ReleaseNotesContent)' != ''">$(ReleaseNotesContent)</PackageReleaseNotes>
    <RepositoryUrl Condition="'$(RepositoryUrl)' != ''">$(RepositoryUrl)</RepositoryUrl>
    <RepositoryType>svn</RepositoryType>
    <RepositoryBranch Condition="'$(RepositoryBranch)' != ''">$(RepositoryBranch)</RepositoryBranch>
    <RepositoryCommit Condition="'$(RepositoryCommit)' != ''">$(RepositoryCommit)</RepositoryCommit>

    <PackageTags>General ACTSIS</PackageTags>
    <PackageRequireLicenseAcceptance>false</PackageRequireLicenseAcceptance>
    <PackageLicenseFile>LICENSE</PackageLicenseFile>
    <Description>Librería .Net Standar para manejo de graficas Apache ECharts.</Description>
    <Copyright>Copyright 2023</Copyright>
    <PackageIcon>icon.png</PackageIcon>
    <PackageReadmeFile>README.md</PackageReadmeFile>
    <PublishRepositoryUrl>true</PublishRepositoryUrl>
  </PropertyGroup>

  <ItemGroup>
    <None Include="README.md" Pack="true" PackagePath="\" />
    <None Include="icon.png" Pack="true" PackagePath="\" />
    <None Include="LICENSE" Pack="true" Visible="false" PackagePath="" />
  </ItemGroup>
```

### files {.tabset}

#### PackageIcon <i class="mdi mdi-package-variant"></i>

Al empaquetar un archivo de imagen de icono, use PackageIcon la propiedad para especificar la ruta de acceso del archivo de icono, en relación con la raíz del paquete. Además, asegúrese de que el archivo está incluido en el paquete. El tamaño del archivo de imagen está limitado a **1 MB**. Los formatos de archivo admitidos incluyen **JPEG** y **PNG**. Se recomienda una resolución de imagen de **128 x 128**.

Por ejemplo, agregaría lo siguiente a nuspec al crear un paquete mediante nuget.exe:

```xml
<PropertyGroup>
    ...
    <PackageIcon>icon.png</PackageIcon>
    ...
</PropertyGroup>

<ItemGroup>
    ...
    <None Include="images\icon.png" Pack="true" PackagePath="\"/>
    ...
</ItemGroup>
```

#### PackageReadmeFile <i class="mdi mdi-read"></i>

Al empaquetar un archivo Léame, debe usar la propiedad para especificar la `PackageReadmeFile` ruta de acceso del paquete, en relación con la raíz del paquete. Además de esto, debe asegurarse de que el archivo está incluido en el paquete. Los formatos de archivo admitidos incluyen solo Markdown (.md).

Por ejemplo:

```xml
<PropertyGroup>
    ...
    <PackageReadmeFile>readme.md</PackageReadmeFile>
    ...
</PropertyGroup>

<ItemGroup>
    ...
    <None Include="docs\readme.md" Pack="true" PackagePath="\"/>
    ...
</ItemGroup>
```

#### PackageLicenseFile <i class="mdi mdi-license"></i>

Al usar una expresión de licencia, use la PackageLicenseExpression propiedad . Para obtener un ejemplo, consulte [Ejemplo de expresión de licencia](https://github.com/NuGet/Samples/tree/main/PackageLicenseExpressionExample).

```xml
<PropertyGroup>
    <PackageLicenseExpression>MIT</PackageLicenseExpression>
</PropertyGroup>
```

En algunos escenarios, como al empaquetar un archivo de licencia, es posible que desee incluir un archivo sin una extensión. Por motivos históricos, NuGet&MSBuild trate las rutas de acceso sin una extensión como directorios.

```xml
<PropertyGroup>
	<TargetFrameworks>netstandard2.0</TargetFrameworks>
  <PackageLicenseFile>LICENSE</PackageLicenseFile>
</PropertyGroup>

<ItemGroup>
	|<None Include="LICENSE" Pack="true" PackagePath=""/>
</ItemGroup>
```

### 6.5.3. Tokens de reemplazo

Como se habrá dado cuenta, en los archivos de configuración de metadatos, muchos datos están en variables, al crear un paquete, el nuget pack comando reemplaza los tokens delimitados por $-delimitados en el `.nuspec` nodo del `<metadata>` archivo por valores que proceden de un archivo de proyecto o del pack modificador del -properties comando.

En la línea de comandos, especifique valores de token con `dotnet pack --configuration Release /p:Ownership="ACTSIS"` o `nuget pack -Properties Configuration=Release;owners="ACTSIS Ltda";`.

Por ejemplo, puede usar un token como `$owners$` y `$desc$` en .nuspec o .csproj y proporcionar los valores en el momento del empaquetado del siguiente modo:

### tokens {.tabset}

#### .nuspec

```ps
nuget pack MyProject.csproj -Properties 
		Configuration=Release;owners=janedoe,harikm,kimo,xiaop;desc="Awesome app logger utility"
```

#### .csproj

```ps
dotnet pack MyProject.csproj --configuration Release
    /p:Ownership=janedoe,harikm,kimo,xiaop /p:Description="Awesome app logger utility"
```

### 6.5.4. Uso de XML Data Transform (XDT) para configurar automáticamente web.config durante la instalación del paquete Nuget

Las transformaciones de archivos de configuración permiten modificar archivos ya existentes en un proyecto de destino, como el web.config y app.config. Por ejemplo, supongamos que el paquete necesita agregar un elemento a la sección de módulos dentro del archivo de configuración. Esta transformación se logra mediante la inclusión de archivos especiales dentro del paquete que describen las secciones que deben agregarse a los archivos de configuración. Además, cuando se desinstala el paquete, estos cambios se reversen automáticamente, lo que convierte a la transformación en un proceso bidireccional.

Por ejemplo, quiere modificar el web config para agregar el siguiente un atributo en el web.config al instalar el paquete Nuget.
`<add tagPrefix="eCharts" namespace="Actsis.ECharts.Webforms.Charts"  assembly="Actsis.ECharts.Webforms" />`

1. Se debe crear el XDT con la transformación `Actsis.ECharts.Webforms.install.xdt`.
```xml
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.web>
    <pages>
      <controls>        
        <add tagPrefix="eCharts" namespace="Actsis.ECharts.Webforms.Charts" assembly="Actsis.ECharts.Webforms" xdt:Transform="Insert" />
      </controls>
    </pages>
  </system.web>
</configuration>
```

2. Agregar el `Actsis.ECharts.Webforms.install.xdt` al proyecto `.csproj`.
```xml
<ItemGroup>
	<None Include="Actsis.ECharts.Webforms.install.xdt" />
	<Content Include="Actsis.ECharts.Webforms.install.xdt" />
</ItemGroup>
```

Entonces cuando instale el paquete, al compilar se realizará la transformación `Actsis.ECharts.Webforms.install.xdt`.

## 6.6.	Almacenar/Listar paquetes NUGET.

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

## 6.7.	Instalar paquetes NUGET.

La tarea operativa de instalar un paquete NUGET está sujeta a que previamente se encuentre identificado en el proyecto correspondiente el uso de la dependencia. Por defecto, los proyectos CORE de un sistema específico tienen dependencia de la librería CoreGen, y estas a su vez de la librería 3rosCore, los proyectos web y los servicios web que requieren el uso de la capa CORE de un sistema, tienen dependencia de la librería CORE del sistema.

Ejemplo:

```diagram
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSI2MTFweCIgaGVpZ2h0PSIyMjFweCIgdmlld0JveD0iLTAuNSAtMC41IDYxMSAyMjEiIGNvbnRlbnQ9IiZsdDtteGZpbGUgaG9zdD0mcXVvdDtlbWJlZC5kaWFncmFtcy5uZXQmcXVvdDsgbW9kaWZpZWQ9JnF1b3Q7MjAyMy0wNC0wNVQxNjoxMjo1NC4zOTBaJnF1b3Q7IGFnZW50PSZxdW90O01vemlsbGEvNS4wIChXaW5kb3dzIE5UIDEwLjA7IFdpbjY0OyB4NjQ7IHJ2OjEwOS4wKSBHZWNrby8yMDEwMDEwMSBGaXJlZm94LzExMS4wJnF1b3Q7IGV0YWc9JnF1b3Q7cmVCOEl1RnVHUXpvYUJ1OUN3OE0mcXVvdDsgdmVyc2lvbj0mcXVvdDsyMS4xLjQmcXVvdDsgdHlwZT0mcXVvdDtlbWJlZCZxdW90OyZndDsmbHQ7ZGlhZ3JhbSBpZD0mcXVvdDtXVlNJU0dUQ1hzUlIxOWsyRUlPVCZxdW90OyBuYW1lPSZxdW90O1DDoWdpbmEtMSZxdW90OyZndDs3VmhOYzVzd0VQMDF2bllBR1d3ZkUwTGNRNXRENlV6U293b2JVQ3V6akJBMjdxK3ZBR0VnU2pMdWwra3c4Y1hzMDY1QTcrMkh6WUw0dTJvcmFKNSt4Qmo0d3JIaWFrRnVGbzVqVzY2bHZtcmsyQ0t1dDJ5QlJMQllPL1ZBeUg1QUY2blJrc1ZRakJ3bElwY3NINE1SWmhsRWNvUlJJZkF3ZG50RVByNXJUaE13Z0RDaTNFVHZXU3pURmwwN3F4NS9EeXhKdXp2YjNxWmQyZEhPV1ora1NHbU1od0ZFZ2dYeEJhSnNyM2FWRDd3bXIrT2xqYnQ5WWZYMFlBSXllVTZBNXIyUXgrNXNFS3VqYWhPRlRESEJqUEtnUjY4Rmxsa005UWFXc25xZkQ0aTVBbTBGZmdNcGoxbzNXa3BVVUNwM1hLOUN4ZVRENFBwTHZkVTdWMXMzbGQ2NU1ZNmRrVWx4ZkJnYWc2amE3TU1hcTRzekNkRWNGVmlLU0ovWjBUbEVSUUxhaTdSUXpjWWdUSk80QmR5QnVvdHlFTUNwWlB0eFlsQ2RYOG5KcjVkQVhXZ1ZubGRFUDh1ZThsSnZTZ1FXUGdvd3BPcUZxSms4cEV4Q21OUG1VQWRWZUdQU0h4bm5QbklVVFN5eHYxSWIxSUd1Q3lud093eFdMTXNMcm03ckNNemtBSDlzUHE5UnVnY2hvWHFWcm02VjZQelhEZUJVRDRlK25Pd09Td2VsNUZsL3puRFhSZWFiOUZFcDlxZThPS3NDaUZrQjdsUVZZTnR6MStkM0pmRW1rOFNadXlSL3BXUldVK2xEaktGUkQ0d3RaRE9hR2M3NnljendMamd6WElOZ1JlNG55RlZXUTNFZnpvam01WEpNczcyKzVHZ21iMzNtYVoveHpENnpucXJQZUdZWkJIZDN3ZWNaNS85RjI0dzkrLzlqdjU3L3EvOG8vMjMzVFo4ejlObE1wYy9xMmQ5QklZMW0zS0RJOG9JTmFtMFFIRjc1OHhvQW5qUGhBTmlZL0lMWXM0aGhvV0RGOVl5Si9wZUpyTXorcFdxek5uZzFUWUtmJmx0Oy9kaWFncmFtJmd0OyZsdDsvbXhmaWxlJmd0OyI+PGRlZnMvPjxnPjxwYXRoIGQ9Ik0gMTIwIDcwIEwgMTQwIDcwIEwgMTQwIDExMCBMIDE0My42MyAxMTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDE0OC44OCAxMTAgTCAxNDEuODggMTEzLjUgTCAxNDMuNjMgMTEwIEwgMTQxLjg4IDEwNi41IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjAiIHk9IjQwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiByeD0iOSIgcnk9IjkiIGZpbGw9IiMxYmExZTIiIHN0cm9rZT0iIzAwNmVhZiIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiA3MHB4OyBtYXJnaW4tbGVmdDogMXB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiIGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNmZmZmZmY7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMjU1LCAyNTUsIDI1NSk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPjNyb3NDb3JlPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjYwIiB5PSI3NCIgZmlsbD0iI2ZmZmZmZiIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj4zcm9zQ29yZTwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSAyNzAgMTEwIFEgMjkwIDExMCAyOTAgNzAgUSAyOTAgMzAgMzAzLjYzIDMwIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSAzMDguODggMzAgTCAzMDEuODggMzMuNSBMIDMwMy42MyAzMCBMIDMwMS44OCAyNi41IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxwYXRoIGQ9Ik0gMjcwIDExMCBMIDMwMy42MyAxMTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDMwOC44OCAxMTAgTCAzMDEuODggMTEzLjUgTCAzMDMuNjMgMTEwIEwgMzAxLjg4IDEwNi41IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxwYXRoIGQ9Ik0gMjcwIDExMCBRIDI5MCAxMTAgMjkwIDE1MCBRIDI5MCAxOTAgMzAzLjYzIDE5MCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gMzA4Ljg4IDE5MCBMIDMwMS44OCAxOTMuNSBMIDMwMy42MyAxOTAgTCAzMDEuODggMTg2LjUgWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iMTUwIiB5PSI4MCIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgcng9IjkiIHJ5PSI5IiBmaWxsPSIjMWJhMWUyIiBzdHJva2U9IiMwMDZlYWYiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTEwcHg7IG1hcmdpbi1sZWZ0OiAxNTFweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7IiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiAjZmZmZmZmOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDI1NSwgMjU1LCAyNTUpOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5Db3JlR2VuPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjIxMCIgeT0iMTE0IiBmaWxsPSIjZmZmZmZmIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPkNvcmVHZW48L3RleHQ+PC9zd2l0Y2g+PC9nPjxyZWN0IHg9IjMxMCIgeT0iMCIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgcng9IjkiIHJ5PSI5IiBmaWxsPSIjMWJhMWUyIiBzdHJva2U9IiMwMDZlYWYiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMzBweDsgbWFyZ2luLWxlZnQ6IDMxMXB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiIGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNmZmZmZmY7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMjU1LCAyNTUsIDI1NSk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPkdlblJlcG9ydGVzV1M8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMzcwIiB5PSIzNCIgZmlsbD0iI2ZmZmZmZiIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj5HZW5SZXBvcnRlc1dTPC90ZXh0Pjwvc3dpdGNoPjwvZz48cGF0aCBkPSJNIDQzMCAxMTAgUSA0MzAgMTEwIDQ4My42MyAxMTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDQ4OC44OCAxMTAgTCA0ODEuODggMTEzLjUgTCA0ODMuNjMgMTEwIEwgNDgxLjg4IDEwNi41IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjMxMCIgeT0iODAiIHdpZHRoPSIxMjAiIGhlaWdodD0iNjAiIHJ4PSI5IiByeT0iOSIgZmlsbD0iIzFiYTFlMiIgc3Ryb2tlPSIjMDA2ZWFmIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDExOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDExMHB4OyBtYXJnaW4tbGVmdDogMzExcHg7Ij48ZGl2IHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyIgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogI2ZmZmZmZjsgIj48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigyNTUsIDI1NSwgMjU1KTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+R0VOTkVUPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjM3MCIgeT0iMTE0IiBmaWxsPSIjZmZmZmZmIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPkdFTk5FVDwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA0MzAgMTkwIFEgNDYwIDE5MCA0NjAgMTUwIFEgNDYwIDExMCA0ODMuNjMgMTEwIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA0ODguODggMTEwIEwgNDgxLjg4IDExMy41IEwgNDgzLjYzIDExMCBMIDQ4MS44OCAxMDYuNSBaIiBmaWxsPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48cGF0aCBkPSJNIDQzMCAxOTAgUSA0MzAgMTkwIDQ4My42MyAxOTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDQ4OC44OCAxOTAgTCA0ODEuODggMTkzLjUgTCA0ODMuNjMgMTkwIEwgNDgxLjg4IDE4Ni41IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjMxMCIgeT0iMTYwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjYwIiByeD0iOSIgcnk9IjkiIGZpbGw9IiMxYmExZTIiIHN0cm9rZT0iIzAwNmVhZiIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3Qgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyIgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAxMThweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxOTBweDsgbWFyZ2luLWxlZnQ6IDMxMXB4OyI+PGRpdiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiIGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNmZmZmZmY7ICI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMjU1LCAyNTUsIDI1NSk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPkNvcmVTYWM8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iMzcwIiB5PSIxOTQiIGZpbGw9IiNmZmZmZmYiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+Q29yZVNhYzwvdGV4dD48L3N3aXRjaD48L2c+PHJlY3QgeD0iNDkwIiB5PSI4MCIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgcng9IjkiIHJ5PSI5IiBmaWxsPSIjMWJhMWUyIiBzdHJva2U9IiMwMDZlYWYiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTEwcHg7IG1hcmdpbi1sZWZ0OiA0OTFweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7IiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiAjZmZmZmZmOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDI1NSwgMjU1LCAyNTUpOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5TQUNORVQ8L2Rpdj48L2Rpdj48L2Rpdj48L2ZvcmVpZ25PYmplY3Q+PHRleHQgeD0iNTUwIiB5PSIxMTQiIGZpbGw9IiNmZmZmZmYiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+U0FDTkVUPC90ZXh0Pjwvc3dpdGNoPjwvZz48cmVjdCB4PSI0OTAiIHk9IjE2MCIgd2lkdGg9IjEyMCIgaGVpZ2h0PSI2MCIgcng9IjkiIHJ5PSI5IiBmaWxsPSIjMWJhMWUyIiBzdHJva2U9IiMwMDZlYWYiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiIHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMTE4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTkwcHg7IG1hcmdpbi1sZWZ0OiA0OTFweDsiPjxkaXYgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7IiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiAjZmZmZmZmOyAiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDI1NSwgMjU1LCAyNTUpOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij5TZXJ2aWNpb3MgU0FDPC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjU1MCIgeT0iMTk0IiBmaWxsPSIjZmZmZmZmIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPlNlcnZpY2lvcyBTQUM8L3RleHQ+PC9zd2l0Y2g+PC9nPjwvZz48c3dpdGNoPjxnIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIvPjxhIHRyYW5zZm9ybT0idHJhbnNsYXRlKDAsLTUpIiB4bGluazpocmVmPSJodHRwczovL3d3dy5kaWFncmFtcy5uZXQvZG9jL2ZhcS9zdmctZXhwb3J0LXRleHQtcHJvYmxlbXMiIHRhcmdldD0iX2JsYW5rIj48dGV4dCB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjEwcHgiIHg9IjUwJSIgeT0iMTAwJSI+VGV4dCBpcyBub3QgU1ZHIC0gY2Fubm90IGRpc3BsYXk8L3RleHQ+PC9hPjwvc3dpdGNoPjwvc3ZnPg==
```



Para la instalación de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones, en este mismo repositorio, se encuentran las instrucciones operativas para la instalación de un paquete nuget.

## 6.8.	Actualizar paquetes NUGET.

a tarea operativa de actualizar los paquetes NUGET de GENERAL existentes en un proyecto, está sujeta a que previamente se encuentre identificado en el análisis del requerimiento en desarrollo, la necesidad de actualizar el paquete o exista un requerimiento específico para la actualización.

Para los proyectos WEB .NET, la actualización de los paquetes NUGET se presenta en los siguientes escenarios:

1.	En los proyectos como portales o servicios web, los cuales se encuentran desactualizados respecto al CORE del propio sistema, para estos casos la actualización del CORE debe realizarse por medio de un requerimiento con el alcance respectivo o durante la actualización del mismo servicio si requiere actualizar las clases relacionadas al servicio en el CORE.

2.	Previo a la integración en el tronco de los desarrollos de los proyectos web que tengan una dependencia también en desarrollo como un CORE, primero se debe integrar el proyecto CORE, para generar por medio de las tareas de integración continua el paquete NUGET respectivo, de esta manera antes de integrar el proyecto web se actualiza el NUGET correspondiente con el generado en la integración del CORE.

3.	Previo a la generación de un TAG del proyecto, ya que previamente se debe crear el TAG correspondiente al proyecto del CORE, de esta manera, el integrador obtiene el paquete del CORE correspondiente al TAG y lo actualiza en la rama tronco del proyecto web o de otro tipo, para generar el TAG de este mismo proyecto.

Para la actualización de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones, en este mismo repositorio, se encuentran las instrucciones operativas para la actualización de un paquete nuget.

## 6.9.	Restaurar paquetes NUGET.

La tarea de restauración de paquetes NUGET se requerida cuando se descarga un proyecto .NET de su correspondiente repositorio y se requiere compilar o publicar el proyecto, la restauración descarga las dependencias requeridas, ya que el proyecto no es versionado en el repositorio con las dependencias.

Para la restauración de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones/OrigenPaquetes, en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones/Restaurar se encuentran las instrucciones operativas para la restauración de paquete nuget.

## 6.10.	Desinstalar paquetes NUGET.

La tarea operativa de desinstalar un paquete NUGET existente en un proyecto, está sujeta a que previamente se encuentre identificado en el análisis del requerimiento en desarrollo, la necesidad de eliminar el paquete teniendo en cuenta que no se requiere el uso de las librerías que sirve el paquete.

Para la desinstalación de un paquete NUGET en un proyecto .NET, se requiere configurar en el IDE el origen de fuentes de los paquetes según las instrucciones indicadas en el repositorio https://svn.actsis.com/svn/NUGET/Instrucciones, en este mismo repositorio, se encuentran las instrucciones operativas para la desinstalación de un paquete nuget.

## 6.11.	Eliminar paquetes NUGET.

Como parte del mantenimiento del servidor de paquetes nuget, se definen las siguientes condiciones para la eliminación de paquetes nuget del servidor:

- Solo se permite eliminar los paquetes nuget correspondientes a versiones preliminares, es decir, aquellas que tienen un sufijo que indican esta condición, por ejemplo el sufijo –rc, las cuales corresponden a una revisión de una rama en el repositorio diferente a la rama TAG.
- Los paquetes nuget que corresponden a versiones estables, es decir, que no tienen un sufijo, no deben ser eliminadas.
- Para la eliminación de los paquetes de versiones preliminares se define que serán eliminados los paquetes correspondientes a versiones inferiores a la penúltima versión estable del paquete, o teniendo en cuenta como referencia el repositorio de cada proyecto, se deben eliminar los paquetes correspondientes a revisiones inferiores al penúltimo TAG generado y que correspondan a una rama diferente a TAG.

Ejemplo:

![eliminar_paquetes_nuget.png](/recursos/imagenes/eliminar_paquetes_nuget.png)

![nuget](https://upload.wikimedia.org/wikipedia/commons/2/25/NuGet_project_logo.svg){.align-abstopright}
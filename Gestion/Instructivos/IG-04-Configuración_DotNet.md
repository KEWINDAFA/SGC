---
title: IG-04 Configuración DotNet
description: Instructivo de configuración DotNet en General
published: true
date: 2023-04-05T16:05:54.297Z
tags: nuget, dotnet
editor: markdown
dateCreated: 2023-04-04T22:18:22.565Z
---

# Descripción General

## 1. OBJETIVOS

Este instructivo tiene como propósito establecer la metodología para gestionar la configuración de los paquetes NuGets, la compilación, restauración y publicación de aplicaciones .Net Core.

## 2. ALCANCE

Este instructivo es aplicable a las dependencias generadas para los proyectos de desarrollo .NET Core. Las actividades de gestión de configuración de las dependencias comprenden:

1. Restaurar paquetes
	-	en construcción
2. Compilar proyectos
	-	en construcción
3. 
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
- [Instructivo IG-03 *Configuración_NUGET*](/Gestion/Instructivos/IG-03-Configuración_NUGET)
{.links-list}

# 6.	GESTIÓN DE PAQUETES NUGET

## 6.1.	Servidor de paquetes NUGET

![DotNet](https://upload.wikimedia.org/wikipedia/commons/thumb/7/7d/Microsoft_.NET_logo.svg/1200px-Microsoft_.NET_logo.svg.png){.align-abstopright}
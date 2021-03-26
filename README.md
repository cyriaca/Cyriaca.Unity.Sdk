# Cyriaca.Unity.Sdk
 
 [![NuGet](https://img.shields.io/nuget/v/Cyriaca.Unity.Sdk.svg)](https://www.nuget.org/packages/Cyriaca.Unity.Sdk/)

This project provides a layer on top of the locally installed `Microsoft.NET.Sdk`
that provides additional features for development of libraries that depend on / target Unity projects.

To use this SDK, replace the `Sdk` attribute (or nested element) of your root `Project` element with `Cyriaca.Unity.Sdk/1.0.0`.

e.g. `<Project Sdk="Cyriaca.Unity.Sdk/1.0.0">`

# Properties

## UnityDirectory

Base editor directory (`Hub/Editor` folder containing versioned Unity editors).

## UnitySubDirectory

Sub-folder for assemblies under Unity editor directory (e.g. `Editor\Data` on Windows).

## Unity

Unity editor version (e.g. 2020.2.1f1).

User-defined, required by some ItemGroup proxies.

## UnityProject

Path to Unity project.

User-defined, required by some ItemGroup proxies.

## PackageDependencyOutputDirectory

Optional target directory to copy PackageReference assets (assemblies, resources) to.

# ItemGroups

## UnityReference

Shortcut ItemGroup for Unity editor assemblies.

Requires `Unity` property.

`Path_to_Editor\<Location>\<Name>.dll`

`Location` metadata defaults to `Managed`.

## UnityDirectReference

ItemGroup for Unity editor assemblies.

`Path_to_Editor\<Name>`

Requires `Unity` property.

## UnityProjectReference

Shortcut ItemGroup for project-level assemblies.

`Path_to_Project\<Location>\<Name>.dll`

Requires `UnityProject` property and `Location` metadata.

## UnityDirectProjectReference

ItemGroup for project-level assemblies.

`Path_to_Project\<Name>`

Requires `UnityProject` property.

## UnityPackageCacheReference

Shortcut ItemGroup for PackageCache assemblies.

`Path_to_Project\Library\PackageCache\<Location>\<Name>.dll`

Requires `UnityProject` property and `Location` metadata.

## UnityDirectPackageCacheReference

ItemGroup for PackageCache assemblies.

`Path_to_Project\Library\PackageCache\<Name>`

Requires `UnityProject` property.

## UnityScriptAssemblyReference

Shortcut ItemGroup for ScriptAssembly assemblies.

`Path_to_Project\Library\ScriptAssemblies\<Location>\<Name>.dll`

Requires `UnityProject` property and `Location` metadata.

## UnityDirectScriptAssemblyReference

ItemGroup for ScriptAssembly assemblies.

`Path_to_Project\Library\ScriptAssemblies\<Name>`

Requires `UnityProject` property.

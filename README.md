# Cyriaca.Unity.Sdk
 
 [![NuGet](https://img.shields.io/nuget/v/Cyriaca.Unity.Sdk.svg)](https://www.nuget.org/packages/Cyriaca.Unity.Sdk/)

This MSBuild SDK provides a layer on top of the locally installed `Microsoft.NET.Sdk` (the default .NET MSBuild SDK
used by .NET Core / .NET 5 etc.) that provides additional functions for development of libraries for Unity projects.
It mainly provides shorthand ItemGroups for less verbose references to Unity assemblies, both Editor-provided and
project-specific, although some like ScriptAssemblies will need the project to be opened / imported by Unity before
the assemblies are available on disk.

This SDK also allows for easy consumption / updating of NuGet assets directly within a project
([details below](#nuget-outputs)) and support for writing and using analyzers, or, more importantly,
source generators ([details below](#source-generators)), without breaking out of `Assets/` if desired.

This does not supplant asmdef files. The source code would still be consumed by Unity unless placed
in a Unity-ignored directory (in which case, yes: besides specific reflection / code generation e.g. for ECS not
working, you could output your own assemblies along with any NuGet package assets you need, however, asmdefs
are what you need if you just want to split up your code).

To use this SDK, replace the `Sdk` attribute (or nested element) of your root `Project` element with `Cyriaca.Unity.Sdk/1.0.0`,
e.g. `<Project Sdk="Cyriaca.Unity.Sdk/1.0.0">`.

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

# Additional Information

## NuGet Outputs

When the `PackageDependencyOutputDirectory` property is set, a project will output the artifacts for the
NuGet packages it references to the specified directory. This may be useful in writing code entirely
inside a generic .NET ecosystem while explicitly producing assemblies/etc from the packages that Unity projects
are informed of as well. It can also be a fairly neat way of managing NuGet dependencies in general, using a
single csproj to track the packages, provided that you run `dotnet restore` whenever you change the dependencies
and manually delete any files left over from unused packages.

## Source Generators

Using the `EmitCompilerGeneratedFiles` and `CompilerGeneratedFilesOutputPath` properties from the Roslyn
source generator API, it is possible to add generated source code from a Roslyn source generator to your
project so Unity sees it and treats it the same as any other source code in the Unity project.
The files might need to be removed from the MSBuild project the generator acts on if the code is being
written back into that same project, e.g. with `<Compile Remove="YourGeneratedFolder\**"/>`

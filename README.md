# BuildingCrew

A scaffolding module to replace (or, more likely, supplement) [Plaster](https://github.com/PowerShellOrg/Plaster) - incorporating much of Plaster's UI code, but defining manifests using a DSL and allowing for the use of arbitrary templating engines.

## Overview

The premise behind module is that the user supplies the the materials and the blueprint (along with an optional cut sheet with the details) to the `BuildingCrew`, who then builds the structure. If a cut sheet is not provided, the `BuildingCrew` will ask the user to fill in the details.

### Comparison to `Plaster`

As stated previously, the user interface borrows heavily from the `Plaster` project, as do the file creation routines. However, there are a few key differences:

- Rather than relying on XML manifests, `BuildingCrew` uses a DSL (Based closely on `Plaster`'s XML schema for familiarity).
- In addition to the `text`, `choice`, and `multichoice` prompt types, `BuildingCrew` adds a `textarray` type that continues to prompt until no more input is received.
  - Additionally, `subprompt`s can prompt based on information received from previous answers
- The default templating engine is [Scriban](https://github.com/scriban/scriban), but others can be used if desired.
  - Any custom template engine control commands will be stored in the [blueprint.ps1](#the-blueprint) file.

## Installation

Install from the PowerShell Gallery

> Note: This module is not yet released to the Gallery.

```powershell
Install-Module BuildingCrew
```

## Examples

### The Materials

Materials are the source files and/or folders which will be copied to the destination location. The files can be of any type. However, templates must be in a format that is appropriate for the templating engine. They can be located anywhere on disk, bu they must all reside in the same folder structure.

### The Blueprint

The blueprint is a special material. There can be only one and it must reside in the root of the materials folder hierarchy and must be named `Blueprint.ps1`.

A blueprint file uses a DSL to define the metadata, parameters, contents, and template engine controls the `BuildingCrew` will leverage to build the file structure.

### The Cut Sheet

A JSON-formatted string representing the variables in the blueprint and the responses provided by the user.

### The Templates

A single JSON-formatted string representing the cut sheet for the run is passed to the template engine controlling function. The string can be parsed for input to the templating engine.

See the [Scriban](https://github.com/scriban/scriban) project for examples of templating with Scriban.

### Running the Module

To kick off the scaffolding process, run:

```powershell
Start-BuildingCrew -Materials "path\to\materials\directory\" -DestinationDirectory "path\to\destination\"
```

or

```powershell
Start-BuildingCrew -Materials "path\to\materials\directory\" -DestinationDirectory "path\to\destination\" -CutSheet "path\to\cutsheet.json"
```

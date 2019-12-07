# Why Developers Should Love PowerShell

## Why you should care about it

* It is powerful. One line commands for complex tasks.
   * .NET ecosystem is fully integrated with it
   * Invoke CMDLET for SQL
   * It's fast (allows you to be faster)
* It's everywhere
   * It's in IDEs and PS Core is also available on Mac and Linux
* It's familiar
   * Most cmds from regular command prompt are available
   * Objects from C# are available

## Concepts

* PowerShell Console
* Commandlets + Parameters
* Aliases
* Variables
* Pipelines

## PowerShell Console

### Commandlet function syntax

`Verb`-`Command` `-parameters`

#### Verbs

`Get-Verb`

> Add, Clear, Close, Copy, Enter, Exit, Find, Format...



### Command types

* Alias
   * Command shortcuts to functions or commandlets (`gci` for `Get-ChildItem`)
* Function
   * Written in pure PowerShell syntax
* Commandlet
   * A C#/.NET program written with the PowerShell SDK

## Commandlets and Parameters

#### Parameters

Auto complete helpers

## Variables

#### Defined with `$` then a name

`$string = "a string"`

Type is inferred, no need to set. Types can be changed:

`$string = 1`

`$string.GetType()` >> `Int32`

`Get-Variable` >> Shows all available variables in use

## Working with objects

### Can work with XML, JSON, etc.

``` powershell
[xml]$cdLib = Get-Content .\someFile.xml
$cdLib.SOMEPROPERTY.NESTEDPROPERTY[0];

$cdLib.SOMEPROPERTY.NESTEDPROPERTY | Select-Object -ExpandProperty NestedNestedProperty | Measure-Object -AllStats

# Pipes the XML object to a select statement, which selects the nested year object, which then pipes the years to the measure-object commandlet which does some maths
```

## Pipelines

Pipelines act as a series of commands which take the previous commands output as input.

## Scripting

### IDEs
* VS Code
* ISE (Integrated Shell Environment, gross, not supported in core/cross-platform environments)

### Script Files and Param()
``` powershell
param($param1, $param2)
if ($param1 -eq "foo") {
    "Hey, you must be a developer!"
}

Write-Host "$param1 $param2" -ForegroundColor Magenta

# ./demo.ps1 -param1 foo -param2 bar
# >> Hey, you must be a developer!
# >> foo bar

# ./demo.ps1
# >> 

param([Parameter(Mandatory=$true)]$param1, [Parameter(Mandatory=$true)]$param2)
if ($param1 -eq "foo") {
    "Hey, you must be a developer!"
}

Write-Host "$param1 $param2" -ForegroundColor Magenta
# ./demo.ps1
# >> ERROR
```

### Functions

Declare functions in script files

``` powershell
function Get-RandomNumberCustom {
    $rand = Get-Random -Minimum 1 - Maximum 10
    return $rand
}

Set-Alias -Name "grnc" -Value Get-RandomNumberCustom

# Get-RandomNumberCustom
# grnc

# With params:
# Can also use default values (= 1)
function Get-RandomNumberCustom([int]$min = 1, [int]$max = 10) {
    $rand = Get-Random -Minimum $min -Maximum $max
    return $rand
}

Set-Alias -Name "grnc" -Value Get-RandomNumberCustom
# grnc -min 1 -max 10
```

#### If you have mandatory set on a property or argument, it will override any defaults and give error.

In order to have the function available in the command line you need to use `. .\demo.ps1` to source it.


### Operators

`Look up documentation for them, there's a lot`

### Error Handling

``` powershell
function Do-Something {
    throw 
}

function Handle-Something {
    try {
        Do-Something
    }
    catch {
        Write-Host "Exception!!!"
    }
}
```

## Important Commands and Tips

* `Get-Help` >> Help for everything
* Tab to autocomplete all the things
* Up/Down arrows
* ctrl+r == reverse lookup previously run commands
* PowerShell profile
   * A profile ps1 file that you can define functions, commandlets, etc
   * `$PROFILE`
   * Profiles will be in different locations depending on which terminal you're using (terminal vs powershell core vs VS Code terminal)
* Web requests
``` powershell
$headers = {
    "Content-Type": "application/json"
}

$baseUrl = "https://regres.in"

$result = Invoke-WebRequest -Method Get -Uri "$baseUrl/api/users/page=2" -Headers $headers

$result

$result.GetType().FullName
$result.Content | ConvertFrom-Json | Select-Object -ExpandProperty data 

# Invoke-RestMethod is much like Invoke-WebRequest, but assumes and translates response data to JSON for you
$result = Invoke-RestMethod -Method Get -Uri ... 
$result.GetType().FullName
$result.data[0]
```

## Gotchas

1. Working with strings
``` powershell
$s = 'string'
$s = "s"
"HELLO, $s" # good
'HELLO, $s' # doesn't do interpolation

$h = @{"name", "k"}
"HELLO, $h.name" # doesn't work
"HELLO, $(h.name)" # works
$declarejsonobjectasstring = @"
{
    "prop": "value"
}"@
```
1. Is it an array?



# UNIT TESTING AVAILABLE IN POWERSHELL >> `Pester`













# Idea

Git wrapper which clones a directory and then registers the path so that you can easily CD to a project rather than navigating file structure
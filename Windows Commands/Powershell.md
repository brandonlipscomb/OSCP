# PowerShell Overview

## 1. PowerShell Basics
PowerShell is both a scripting language and a shell environment designed for Windows, built on the .NET framework. This foundation allows it to directly utilize .NET functions, providing powerful capabilities for system administration and automation.

## 2. Cmdlets and .NET Integration
The primary commands in PowerShell, called **cmdlets** (pronounced "command-lets"), are constructed using the .NET framework. This deep integration means that PowerShell can execute .NET functions directly, making it more versatile than many traditional shell environments.

## 3. Object-Oriented Output
Unlike other scripting languages where command outputs are often plain text, PowerShell outputs are **objects**. This object-oriented approach allows users to perform operations on the output directly, such as filtering, modifying, or passing data between cmdlets seamlessly. The ability to work with objects rather than just text makes PowerShell particularly powerful for complex tasks.

## 4. Cmdlet Syntax
PowerShell cmdlets follow a standard naming convention of **Verb-Noun** (e.g., `Get-Command`), which makes them easy to understand and use. The **verb** indicates the action (like Get, Start, Stop), and the **noun** describes the object being acted upon.

## 5. Common Verbs
- **Get**: Retrieve data (e.g., `Get-Process` retrieves a list of processes).
- **Start**: Initiate a process or action (e.g., `Start-Service` starts a service).
- **Stop**: Halt a process or service (e.g., `Stop-Process` stops a process).
- **Read**: Read data from a source (e.g., `Read-Host` reads input from the user).
- **Write**: Output data to a destination (e.g., `Write-Output` writes to the console).
- **New**: Create new objects or resources (e.g., `New-Item` creates a new file or directory).
- **Out**: Direct output to a specific destination (e.g., `Out-File` sends output to a file).

## 6. Approved Verbs List
PowerShell maintains a list of approved verbs to ensure consistency and clarity in cmdlet naming. Using these approved verbs helps maintain readability and predictability in scripts.

This structure and approach make PowerShell a powerful tool for automation, system management, and scripting in a Windows environment, leveraging the full capabilities of the .NET framework.

# Basic Commands
## Get-Help
Get-Help displays information about a cmdlet. To get help with a particular command, run the following:
```powershell
Get-Help <COMMAND-NAME>
```
You can also understand how exactly to use the command by passing in the -examples:
```powershell
Get-Help <COMMAND-NAME> -Examples
```
## Get-Command
Get-Command gets all the cmdlets installed on the current Computer. The great thing about this cmdlet is that it allows for pattern matching like the following:
### 1. Get all the cmdlets for a certain verb
```powershell
Get-Command <Verb>-*
```
### 2. Get all the cmdlets for a certain noun
```powershell
Get-Command *-<Noun>
```


<!---
<LICENSE id="CC BY-SA 4.0">
    
    Image-Render Automation Functions module documentation
    Copyright 2022 Robert Bosch GmbH and its subsidiaries
    
    This work is licensed under the 
    
        Creative Commons Attribution-ShareAlike 4.0 International License.
    
    To view a copy of this license, visit 
        http://creativecommons.org/licenses/by-sa/4.0/ 
    or send a letter to 
        Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.
    
</LICENSE>
--->
# Workspace Basics

A *workspace* is a folder that contains all the information and data needed for your processing. Catharsys provides a number of *actions* that are configured by *configuration projects* in your workspace. Each configuration project contains at least a *launch* configuration file, that specifies the actions that can be executed, like rendering, image post-processing, etc.

## Folder Structure

A workspace folder must have a specific structure, to be regarded as a workspace by the Catharsys tools:

> [workspace folder]
> - `package.json`
> - `config`
>     - [config project 1]
>         - `launch`[`.json`, `.json5`, `.ison`]
>     - [config project 2]
>         - `launch`[`.json`, `.json5`, `.ison`]
>     - [...]

For example, a workspace with the name `myws` and configuration projects `trial-01` and `trial-02` has the minimal structure:

> `myws`
> - `package.json`
> - `config`
>     - `trial-01`
>         - `launch.json5`
>     - `trial-02`
>         - `launch.json5`

## The `package` file

The `package` file defines some fundamental properties for the whole workspace. Here is an example,

:::{code-block} json
{
    "sDTI": "/package/catharsys/workspace:3.0",
    "sName": "Simple",
    "sVersion": "1.0.1",
    "sCatharsysVersion": "3.0"
}
:::

The elements have the following meaning:

`sDTI`
: The type identifier of the `package` file data structure

`sName`
: The name of the workspace

`sVersion`
: The version of the workspace

`sCatharsysVersion`
: The Catharsys version this workspace has been developed for

## Configuration Conventions

Currently, all configurations in Catharsys are given in JSON files. Each JSON file basically defines a data structure. To enable the configuration processing to check whether a given JSON file is of the correct data type, the *Data Type Information* string is included for all relevant dictionaries (`{}`). This string is referenced by the key `sDTI`. The lower case `s` stands for `string` and hints to the data type of the element. 

The *DTI* string consists of a hierarchical type name and a two element version, for example `/package/catharsys/workspace:3.0`. All actions, Blender modifiers and generators are referenced by such a DTI string. The version numbers have the following meaning:
- A configuration with version `X.Y` is backward compatible to all configuration with version `X.Z`, where `Z < Y`.
- Configurations with different major versions are in general incompatible.

For example, a configuration version `1.1` may introduce additional parameters, but all parameters of version `1.0` are still valid and have the same meaning. Therefore, a function that is written to process version `1.1` can also process version `1.0` files, but not vice versa. 

All pre-defined Catharsys configuration elements follow the convention that their name starts with a lower case letter identifying the element data type:

| First letter | Data Type  | Example       |
|:------------:| ---------- | ------------- |
|      `i`     | integer    | `iFrameStart` |
|      `f`     | float      | `fSpeed`      |
|      `s`     | string     | `sName`       |
|      `b`     | bool       | `bDoProcess`  |
|      `x`     | [any type] | `xValue`      |

You do not need to follow this convention when defining your own variables, but it can be helpful for others reading your configuration to immediately understand what data type to expect.


## Initializing Workspace

To initialize your workspace for use with VS-Code, run the following command inside the workspace folder and make sure you are in the *Anaconda environment you want to use for the workspace*.

:::{admonition} Shell
`cathy code init`
:::

This creates a workspace file, which you should use when working with VS-Code. The workspace file defines a shell that automatically loads the correct Anaconda environment.

To initialize a workspace configuration project for Blender rendering use the command,

:::{admonition} Shell
`cathy blender init -c [configuration project name] --addons`
::: 

In the above example, the configuration project name could be `trial-01` or `trial-02`. If you run the command without the `--addons` flag, it will check whether the Catharsys modules are installed in Blender and install them when needed. This can be useful, when you updated to a newer Catharsys version. But this only needs to be done once, independent of the configuration. To initialize all configurations at once, use the command,

:::{admonition} Shell
`cathy blender init --all --addons`
:::

## What's next

Now have a look at the most basic configuration: {doc}`just render <level-1>`.

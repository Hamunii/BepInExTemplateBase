# BepInEx Template for _GameName_

- [BepInEx Template for _GameName_](#bepinex-template-for-gamename)
  - [Installing](#installing)
    - [From NuGet (Recommended)](#from-nuget-recommended)
    - [Manually](#manually)
  - [Creating a Project](#creating-a-project)
    - [Project Structure](#project-structure)
    - [Setting Up The Config File](#setting-up-the-config-file)
    - [Thunderstore Packaging](#thunderstore-packaging)
    - [GitHub Actions Publishing](#github-actions-publishing)

Template creator: After forking, replace the following, e.g.:

- `_GameName_` → `Lethal Company` (name from Steam directory)
- `_GameNameNoSpaces_` → `LethalCompany` (name with spaces removed)
- `_GameNameShortNoSpacesLowercase_` → `lc` (used for template `shortName`)
- `_TemplateAuthorNoSpaces_` → `LethalCompanyModding` (GitHub repo & NuGet package prefix)
- `_ThunderstoreGameIdentifier_` → `lethal-company` (see: <https://thunderstore.io/api/experimental/community/>)

You can then remove this section from the README.

## Installing

.NET templates must be installed before they can be used. This means that when you install the template, it doesn't create a new project for you, but now you have the ability to do that.

> [!NOTE]  
> You must use .NET SDK 10 or newer to use this template. You can check your .NET SDK version by running the following in a terminal: `dotnet --version`. To download .NET SDK, see: <https://dotnet.microsoft.com/en-us/download>

### From NuGet (Recommended)

Run the following command:

```bash
dotnet new install _TemplateAuthorNoSpaces_.BepInExTemplate
```

> [!TIP]  
> You can run `dotnet new update` to update all your dotnet templates. You should do this get the latest versions of everything with the latest fixes and improvements!

### Manually

If you're contributing to the template or prefer a manual installation:

1. Clone or download this repository
2. Open a terminal at the root of the repository
3. Run:

```bash
dotnet new install .
```

To update:

```bash
dotnet new install . --force
```

To uninstall:

```bash
dotnet new uninstall .
```

Once installed, the template will be available as `_GameName_ BepInEx Plugin` with an alias `_GameNameShortNoSpacesLowercase_mod`.

## Creating a Project

Open a terminal in your _GameName_ modding directory, and run:

> [!NOTE]  
> You should [set up a Thunderstore team first](<https://thunderstore.io/settings/teams/create/>) so you can use its name in the optional `--ts-team` argument so the template can give you a mostly correctly configured packaging setup.

```sh
dotnet new _GameNameShortNoSpacesLowercase_mod --output ModName --guid com.github.YourAccount.ModName --ts-team YourThunderstoreTeam
```

> [!TIP]  
> If you are developing a public API, add the `--library` option for included NuGet metadata!
>
> You can also use `--no-tutorial` to get rid of tutorial comments in the template. Note that this doesn't get rid of _all_ comments.
>
> You can run `dotnet new _GameNameShortNoSpacesLowercase_mod --help` to see all available options.

This will create a new directory with the mod name which contains the project.

You now have a (mostly) working setup. See [Setting Up The Config File](#setting-up-the-config-file) and [Thunderstore Packaging](#thunderstore-packaging) for more.

### Project Structure

This example demonstrates what files should appear and where:

```sh
~/Workspace/_GameNameNoSpaces_$ dotnet new _GameNameShortNoSpacesLowercase_mod --output MyCoolMod --guid com.github._TemplateAuthorNoSpaces_.MyCoolMod --ts-team _TemplateAuthorNoSpaces_
The template "_GameNameNoSpaces_ BepInEx Plugin" was created successfully.

~/Workspace/_GameNameNoSpaces_$ cd MyCoolMod/
~/Workspace/_GameNameNoSpaces_/MyCoolMod$ tree
.
├── CHANGELOG.md
├── Config.Build.user.props.template
├── Directory.Build.props
├── Directory.Build.targets
├── icon.png
├── LICENSE
├── MyCoolMod.sln
├── README.md
└── src
    └── MyCoolMod
        ├── MyCoolMod.csproj
        ├── Plugin.cs
        └── thunderstore.toml

3 directories, 12 files
```

The C# source files for your mod are located in `./src/<project-name>/`. All files above that are more generic project configuration files.

The `Directory.Build.*` files contain shared configuration for all projects in subdirectories. The project is configured so that it's easy to add new projects into your project solution. Even if you don't need that, it's good to follow this standard project structure.

### Setting Up The Config File

At the root of your new project you should see `Config.Build.user.props.template` this is a special file that is the template for the project's user-specific config. Make a copy of this file and rename it `Config.Build.user.props` without the template part.

This file will copy your assembly files to a plugins directory and it can be used to configure your paths to the game files and BepInEx plugins directory if the defaults don't work for you.

### Thunderstore Packaging

This template comes with Thunderstore packaging built-in, using [TCLI](<https://github.com/thunderstore-io/thunderstore-cli>). You should configure the `src/<project-name>/thunderstore.toml` file for your mod, such as setting the description for your mod.

You can build Thunderstore packages by running:

```sh
dotnet build -c Release -v d
```

> [!NOTE]  
> You can learn about different build options with `dotnet build --help`.  
> `-c` is short for `--configuration` and `-v d` is `--verbosity detailed`.

The built package will be found at `artifacts/thunderstore/`.

You can also directly publish to Thunderstore by including `-property:PublishTS=true` in the command.

> [!NOTE]
> For publishing to Thunderstore, you need a Thunderstore API token. The publishing to Thunderstore option is intended to be used via automated GitHub actions workflows, so you don't need to worry about it.

### GitHub Actions Publishing

Coming soon.

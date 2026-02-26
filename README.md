## mdcdi1315's Mods Base Library Template

This is a template repository that creates a new mod based on my Base Mods Library mod.

Depending on the versions and modloaders available, this repository will reflect those changes.

> [!WARNING]
You should select the correct branch when creating your repository. Each branch represents a supported Minecraft version.

Delete the contents of this file and replace them with your own description once done.

### Mod creation instructions:

#### `gradle.properties`

Most of the mod configuration, for example, it's name and the Minecraft versions that the mod will support must be defined in the `gradle.properties` file.

The file describes what each property is.

#### The `platform_agnostic` project

This project contains all the Minecraft only code of your project, as well as my bare library abstractions.

You might still need to interact with the underlying mod loader, depending on what you want to do each time.

#### Working with the library

From now on the library is distributed through GitHub Packages.

As such you need a [GitHub account](https://www.github.com) and a [classic API token](https://www.docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) with the `read:packages` access.

Then, the template automatically injects for you the BML library into the project.

See the `github.username` and `github.token` properties for more information.

#### Using the library's Language Providers

The BML library does also provide mod language providers to 
avoid the need of declaring your mod to the BML library
with the mod-loader entry point and then be initialized.
This is done for NeoForge as follows:

1. Open the `META-INF/neoforge.mods.toml` file.

2. Modify the `modLoader` field value to be `bml_java_fml` instead.

3. Modify the `loaderVersion` field value to be `[1.0.0,)` instead.

4. To the `[[mods]]` declaration, add a field `server_mod_instance_class_name`:
~~~TOML
server_mod_instance_class_name="example.mod.ServerModInstance"
~~~
This declares that you declare a server-side mod and it's mod instance is declared through the name specified in `server_mod_instance_class_name`.

Optional: If you have a client-side mod part as well, you can declare it as described in Step 4 but using as a property name the `client_mod_instance_class_name` instead.

> [!NOTE] 
Fabric does not need a language provider since it explicitly supports class names in the entry point declaration.
See the provided `fabric.mod.json` file under the `fabric` project, go the `entrypoints` section and add the class names in 
the `mdcdi1315_basemodslib_server` array field for server-side mods and in the `mdcdi1315_basemodslib_client` for client-side mods. 

> [!NOTE] 
A similar process for Forge is done. 
You just modify instead the `modLoader` field value to be `bml_java_fml` and you add the mod instance class names properties as described above.
Do not modify the `loaderVersion` field as Forge requires it to have the Forge-Specific version each time.

> [!NOTE] 
This is **RECOMMENDED** for all new modders that are now diving into Minecraft modding, 
as it efficiently manages the mod instance creation and takes away the effort of declaring the mod to the loader. 

#### Working with Mixin

Typically, you will work with Sponge's Mixin project for modifying the Minecraft code itself.

Mixin works with a file called `mixins.json` that contains the modifier classes to be used by your mod.

Multiple such files may exist in the root path of a mod package.

In each project, you need to declare a file named as `MOD_ID.mixins.PROJECT_NAME.json`.
Where `MOD_ID` your mod's name, and `PROJECT_NAME` the name of the project as it is declared on the folders.

> [!CAUTION]
The `platform_agnostic` project must contain only one Mixins file definition, and that must be named as `MOD_ID.mixins.json`.

It has the following file structure.

> [!CAUTION]
Make sure to remove the comments when you finalize the files as otherwise it will cause a runtime error.

> [!NOTE] 
Even in the case that you do not declare any Mixin classes, you still need to provide the files as empty because all the mod loaders expect from you to have declared a Mixin class. 
However, specifying config files with no any Mixin classes is perfectly valid.

~~~JSON
{
	"required": true,
	"minVersion": "0.8",
	"package": "", // This must contain the Mixin package name of your mod. Example: com.github.your_name.mod_id.mixin
	"refmap": "${mod_id}.refmap.json", // Leave this as is.
	"compatibilityLevel": "JAVA_17", 
	"client": [
            // Declare class names here that represent mixins running on the Minecraft client only.
            // IMPORTANT: Only the class name is needed: You have qualified the full path to the class by using the 'package' property above.
	],
	"mixins": [
            // Declare class names here that represent mixins running on both Minecraft environments (client, server).
            // IMPORTANT: Only the class name is needed: You have qualified the full path to the class by using the 'package' property above.
	],
	"injectors": {
		"defaultRequire": 1
	}
}
~~~

#### What's next? 

The real fun starts now! You now need to implement your mod's classes.

You should start first from the mod loaders documentation. 
The `gradle.properties` file contains links for them.

Credits:

This template repository is heavily based on jaredlll08's [Multi Loader Template repository](https://github.com/jaredlll08/MultiLoader-Template).

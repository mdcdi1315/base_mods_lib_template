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

You do still need to interact with the underlying mod loader, depending on what you want to do each time.

#### Working with the library and external 'Developer Packages'

The Base Mods Library is distributed through a Developer Package.

You need to set up your project in order to finally include the library in your development environment.

-> Downloading the Developer Package:

Head over to the [Base Mods Library Repository Releases](https://github.com/mdcdi1315/mdcdi1315_base_mods_lib/releases) tab.

From there, select the latest stable version.

Down in the 'Assets' spoiler, expand it and download the 'bml-&lt;VERSION&gt;.zip' file.

Where &lt;VERSION&gt; the version number of the release.

Now, open the file with an archiver and extract the `mdcdi1315_base_mods_lib_dev_package.zip` file for the Minecraft distribution you are currently using.

Place the expanded file on the `deps` directory. 

> [!CAUTION]
After doing that, you should also verify that the `mdcdi1315_base_mods_lib_version` property is set to the version number mentioned in the release file. If is not, the build system will fail to apply the developer package.

Now, run Gradle with the task named `expand_dev_archives`. This will expand the developer package and set up the environment appropriately.

That was it!

If you want in the future to update the library due to a new feature that you want to make use of, replace with the newer developer package and run the `update_dep_archives` task instead.

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

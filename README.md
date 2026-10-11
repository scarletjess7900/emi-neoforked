# EMI
EMI is a featureful and accessible item and recipe viewer for Minecraft.

> **Unofficial build for Minecraft 26.3 (NeoForge).** This tree is not the official EMI.
> It is the `26.3` branch of [link-fgfgui/emi](https://github.com/link-fgfgui/emi) at commit `2fb0d300`
> (an unofficial port to Minecraft 26.3, itself a fork of [emilyploszaj/emi](https://github.com/emilyploszaj/emi)),
> with these changes:
> - Built against NeoForge 26.3.0.69-beta: `NeoForgeRenderTypes` is no longer an enum there, and the port's own
>   build crashes on it. This build was run on NeoForge 26.3.0.69 and 26.3.0.70.
> - `EmiStackConvertible`: the two `@Environment(EnvType.CLIENT)` annotations are removed. On NeoForge they are
>   built as `@OnlyIn`, which NeoForge 26.x no longer acts on and answers with a warning screen at every start.
> - `.github/workflows/build.yml`: the two steps that published to the port author's CurseForge project are removed.
> - Versions of the NeoForge build are `<EMI version>-<Minecraft version>-<version of this fork>`, e.g.
>   `1.1.24-26.3-beta-0.0.1`; set in `gradle.properties` (`mod_version`, `minecraft_version`, `fork_version`).
>
> MIT licensed like the original, see `LICENSE`. Please do not report problems of this build to the original author.
>
> Building: JDK 25, `./gradlew :neoforge:build`; the mod is `neoforge/build/libs/emi-<version>.jar`.
> The Fabric part of the tree is as in the port and was not built or tried here.

![EMI Interface](https://user-images.githubusercontent.com/14813658/224562247-1588064e-39ef-475a-9108-d7a357af6939.png)

![Recipe Tree](https://user-images.githubusercontent.com/14813658/224562258-1a5ee67a-fd7f-489f-9eed-ae67c184ddac.png)

## Developers
To add EMI to your project as a dependency you need to add the following to your `build.gradle`:
```gradle
repositories {
	maven {
		name = "TerraformersMC"
		url = "https://maven.terraformersmc.com/"
	}
}
```

How EMI gets added to your dependencies varies based on modloader and setup.
The Gradle property `emi_version` should be something like `1.0.0+1.19.4` with EMI's version and Minecraft's version.
Here are common dependency setups for different loaders and build systems.

```gradle
dependencies {
	// Fabric
	modCompileOnly "dev.emi:emi-fabric:${emi_version}:api"
	modLocalRuntime "dev.emi:emi-fabric:${emi_version}"

	// Forge (see below block as well if you use Forge Gradle)
	compileOnly fg.deobf("dev.emi:emi-forge:${emi_version}:api")
	runtimeOnly fg.deobf("dev.emi:emi-forge:${emi_version}") 

	// NeoForge
	compileOnly "dev.emi:emi-neoforge:${emi_version}:api"
	runtimeOnly "dev.emi:emi-neoforge:${emi_version}" 

	// Architectury
	modCompileOnly "dev.emi:emi-xplat-intermediary:${emi_version}:api"

	// MultiLoader Template/VanillaGradle
	compileOnly "dev.emi:emi-xplat-mojmap:${emi_version}:api"
}
```

For Forge Gradle users, you will need to enable Mixin refmaps in your client sourceset. This can be done by adding 2 lines inside of your client runs, to look like below.

```gradle
runs {
	client {
		// Add these two lines
		property 'mixin.env.remapRefMap', 'true'
		property 'mixin.env.refMapRemappingFile', "${projectDir}/build/createSrgToMcp/output.srg"

		// The rest of the code that was already here
		// ...
	}
}
```

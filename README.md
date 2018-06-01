# UnityAndroidManifestCallback
This piece of code is mostly targeted towards developers of plugins for Unity Android. If you utilize this way of updating the manifest instead of providing yours in `Assets/Plugins/Android`, you can minimize potential conflicts with other plugins or even apply attributes which would otherwise be strictly set by Unity.

## Android Manifest in Unity
The way how Unity creates the Android Manifest for the application is described in details here: https://docs.unity3d.com/Manual/android-manifest.html.
Adding something to the manifest is quite straightforward; the easiest way is to write a plugin (e.g., AAR) with the necessary bits in the manifest, and rely on the manifest merger to do the rest of the job for you.

However, there are cases when you want to change something in the manifest, and it may not be as easy. The manifest merger gives top priority to the main manifest, which is normally provided by Unity.
In this case, you can provide your own main manifest at `Assets/Plugins/Android/AndroidManifest.xml`. It completely overrides the default main manifest provided by Unity.

Unfortunately, this can still be not enough:
1.	If the user adds multiple plugins which want to override the main manifest at `Assets/Plugins/Android/AndroidManifest.xml`, conflicts will arise which require manual resolution.
2.	Unity additionally processes the manifest after merging, and you can't really affect it. Attributes like `launchMode`, `screenOrientation`, `configChanges` and others are set during this step. Most of them are crucial for Unity to function correctly, but there may be good reasons to override some. The only known way to deal with it is to export the project and build it externally (for example, in Android Studio).

## The correct approach
The `IPostGenerateGradleAndroidProject` interface introduced in Unity 2018.1 can help mitigate these two issues (and even more). It has a callback which is being called after the Gradle project has been created by Unity, but before it's built. In this timeframe some final touches can be applied to the manifest or, for example, `build.gradle` file.
The script provided in this repo, for example, updates the LAUNCH activity and the theme in the manifest. Feel free to modify it to your taste.

## System Requirements
-	Unity 2018.1 or later
-	Gradle build system (not Internal)

## Usage
1.	Copy the `Assets/Editor/ModifyUnityAndroidAppManifestSample.cs` script to your project, retain the path
2.	Update the script to modify the manifest as required. **NOTE: THE SCRIPT IN THIS REPO IS JUST A SAMPLE AND NEEDS YOUR MODIFICATIONS**
3.	Enjoy

## Useful Links
-	https://docs.unity3d.com/Manual/android-manifest.html
-	https://developer.android.com/studio/build/manifest-merge

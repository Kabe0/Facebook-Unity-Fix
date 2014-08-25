Facebook-Unity-Fix - Reverted to version 5.1
==================

##Update##
For people experiencing problems with facebook CanvasFacebook.dll. It appears that version 5.2.1 they broke the canvas feature. I have reverted the sdk while keeping the changes to the keyhash file.

##Description##
The Facebook SDK makes the assumtion that the keystore file you are going to use is the .android/debug.keystore file when in reality unity uses the keystore the user selects in the *Publish Settings* under android. I have gone through and edited two of the SDK Files in order to provide easy functionality to allow users to quickly get the actual keystore value that facebook uses when published to an Android phone. Now you get a real message that asks you to setup your keystore file in the android publish menu under your build settings, and I have created a refresh button that allows you to check your keystore value in the Facebook Settings GUI.

This is a copy of the Facebook Unity SDK 5.1 which you can get the original source here.
https://developers.facebook.com/docs/unity/downloads/?campaign_id=282184128580929&placement=SDK_list

Adding the fix so that facebook GUI displays the key that it actually uses on the Android device instead of the default .android\debug.keystore file. Changes are as follows...

##Instructions##
To use either zip, clone, or grab the FacebookSDK5.2.1-Fix.unitypackage file and extract into the assets folder in your unity project.

Once it's setup, go into your publish settings for android, and create a keystore file with a debug or release key.
*Make sure you define the keystore password and alias passwords otherwise the "Debug Android Key Hash" will be incorrect.*
Now go into the Facebook Settings (located in the top menu) and press the refresh button in the inspector. You should see the correct key appear in the "Debug Android Key Hash".

##Code Modifications From base Facebook SDK 5.1##

####Changes for FacebookAndroidUtils.cs####
- Removed static string debugKeyHash (not always debug key)
- Created static string playerSettingKeyHash
- Updated GetKeyHash method to support a fourth parameter that being the key alias password as it may not always be the same as the actual password for the file.
- Updated call in DebugKeyHash that called GetKeyHash to now use PlayerSettings.Android.keyaliasName, PlayerSettings.Android.keyaliasPass, and PlayerSettings.Android.keystorePass.
- Removed method DebugKeyStorePath as its now useless as it was never being used in the first place.
- Updated HasAndroidKeystoreFile() method to check PlayerSettings.Android.keystoreName instead of DebugKeyStorePath.
- Added method refresh() to allow the user to check the key again without having to restart or delete the reference file.

####Changes for FacebookSettingsEditor.cs####
- Updated class AndroidUtilsGUI() to now display the message "Your keystore file is missing! You can create one by going into Unity's Build Settings->Android->Publish Settings." whenever the keystore file is missing.
- Added a refresh button that calls the FacebookAndroidUtils refresh method.

Hope these changes help anyone that's using the Facebook SDK for Unity3d!


NOTE: This project is not owned by me in any way. I have just put up a small fix to allow developers easy access to the real keystore value.

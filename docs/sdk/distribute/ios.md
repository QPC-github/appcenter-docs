---
title: App Center Distribute for iOS
description: Using in-app updates in App Center Distribute
keywords: sdk, distribute
author: lucen-ms
ms.author: lucen
ms.date: 09/13/2021
ms.topic: article
ms.assetid: f91fcd0b-d5e6-4c74-89a8-f71c2ee57556
ms.tgt_pltfrm: ios
dev_langs:
 - swift
 - objc
---

# App Center Distribute – iOS In-app updates
> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [Unity](unity.md)
> * [MAUI/Xamarin](xamarin.md)

App Center Distribute will let your users install a new version of the app when you distribute it via App Center. With a new version of the app available, the SDK will present an update dialog to the users to either download or postpone the new version. Once they choose to update, the SDK will start to update your application.

> [!NOTE]
> There are a few things to consider when using in-app updates:
> 
> 1. If you've released your app in the App Store, in-app updates will be disabled.
> 2. If you're running automated UI tests, enabled in-app updates will block your automated UI tests as they'll try to authenticate against the App Center backend. We recommend to not enable App Center Distribute for your UI test target.

> [!NOTE]
> In the `4.0.0` version of App Center breaking changes were introduced. Follow the [Migrate to App Center SDK 4.0.0 and higher](../getting-started/migration/apple-sdk-update.md) section to migrate App Center from previous versions.

> [!IMPORTANT]
> App Center SDK doesn't support multiple window apps that were introduced in iOS 13.

## Add in-app updates to your app

Follow the [Get started](~/sdk/getting-started/ios.md) section if you haven't configured the SDK in your application.

### 1. Add the App Center Distribute module

The App Center SDK is designed with a modular approach – you only need to integrate the modules of the services that you're interested in.

#### Integration via Cocoapods

If you're integrating App Center into your app via Cocoapods, add the following dependency to your podfile and run `pod install`.

```ruby
pod 'AppCenter/Distribute'
```

#### Integration via Carthage

1. Add the following dependency to your `Cartfile` to include App Center Distribute.

    ```ruby
    # Use the following line to get the latest version of App Center
    github "microsoft/appcenter-sdk-apple"
    ```

    ```ruby
    # Use the following line to get the specific version of App Center
    github "microsoft/appcenter-sdk-apple" ~> X.X.X
    ```

1. Run `carthage update`.
1. Open your application target's **General** settings tab. Drag and drop the **AppCenterDistribute.framework** file from the **Carthage/Build/iOS** folder to the **Linked Frameworks and Libraries** section in XCode.
1. Drag and drop **AppCenterDistributeResources.bundle** from **AppCenterDistribute.framework** into XCode's Project Navigator.
1. A dialog will appear, make sure your app target is checked. Then click **Finish**.

#### Integration via Swift Package Manager

1. From the Xcode menu click **File > Swift Packages > Add Package Dependency**.
1. In the dialog that appears, enter the repository URL: `https://github.com/microsoft/appcenter-sdk-apple.git`.
1. In **Version**, select **Up to Next Major** and take the default option.
1. Choose the **AppCenterDistribute** in the **Package Product** column.

#### Integration by copying the binaries into your project

[!INCLUDE [ios manual integration](../distribute/includes/ios-manual-integration.md)]

### 2. Start App Center Distribute

App Center only uses the specific modules you invoke in your application. You must explicitly call each of them when starting the SDK.

#### 2.1 Add the import for App Center Distribute

Open the project's **AppDelegate.m** file in Objective-C or **AppDelegate.swift** file in Swift and add the following import statements:

```objc
@import AppCenter;
@import AppCenterDistribute;
```
```swift
import AppCenter
import AppCenterDistribute
```

#### 2.2 Add the `start:withServices:` method

Add `Distribute` to your `start:withServices:` method to start App Center Distribute service.

Insert the following line to start the SDK in the project's **AppDelegate.m** class for Objective-C or **AppDelegate.swift** class for Swift in the `didFinishLaunchingWithOptions` method.

```objc
[MSACAppCenter start:@"{Your App Secret}" withServices:@[[MSACDistribute class]]];
```
```swift
AppCenter.start(withAppSecret: "{Your App Secret}", services: [Distribute.self])
```

Make sure you've replaced `{Your App Secret}` in the code sample above with your App Secret. Also check out the [Get started](~/sdk/getting-started/ios.md) section if you haven't configured the SDK in your application.

#### 2.3 Modify the project's **Info.plist**

1. In the project's **Info.plist** file, add a new key for `URL types` by clicking the '+' button next to "Information Property List" at the top. If Xcode displays your **Info.plist** as source code, refer to the tip below.
2. Change the key type to Array.
3. Add a new entry to the array (`Item 0`) and change the type to Dictionary.
4. Under `Item 0`, add a `URL Schemes` key and change the type to Array.
5. Under your `URL Schemes` key, add a new entry (`Item 0`).
6. Under `URL Schemes` > `Item 0`, change the value to `appcenter-{APP_SECRET}` and replace `{APP_SECRET}` with the App Secret of your app.

> [!TIP]
> If you want to verify that you modified the Info.plist correctly, open it as source code. It should contain the following entry with your App Secret instead of `{APP_SECRET}`:
> ```xml
> <key>CFBundleURLTypes</key>
> <array>
> 	<dict>
> 		<key>CFBundleURLSchemes</key>
> 		<array>
> 			<string>appcenter-{APP_SECRET}</string>
> 		</array>
> 	</dict>
> </array>
> ```

## Use private distribution group

By default, Distribute uses a public distribution group. If you want to use a private distribution group, you'll need to explicitly set it via `updateTrack` property.

```objc
MSACDistribute.updateTrack = MSACUpdateTrackPrivate;
```
```swift
Distribute.updateTrack = .private
```

> [!NOTE]
> The default value is `UpdateTrack.public`. This property can only be updated before the `AppCenter.start` method call. Changes to the update track aren't persisted when the application process restarts, thus if the property isn't always updated before the `AppCenter.start` call, it will be public, by default.

After this call, a browser window will open up to authenticate the user. All the subsequent update checks will get the latest release on the private track.

If a user is on the **private track**, it means that after the successful authentication, they'll get the latest release from any private distribution groups they're a member of.
If a user is on the **public track**, it means that they'll get the latest release from any public distribution group.

## Disable Automatic Check for Update

By default, the SDK automatically checks for new releases:
 * When the application starts.
 * When the application goes into background then in foreground again.
 * When enabling the Distribute module if previously disabled.

If you want to check for new releases manually, you can disable automatic check for update.
To do this, call the following method before the SDK start:

```objc
[MSACDistribute disableAutomaticCheckForUpdate];
```
```swift
Distribute.disableAutomaticCheckForUpdate()
```

> [!NOTE]
> This method must be called before the `AppCenter.start` method call.

Then you can use the `checkForUpdate` API which is described in the following section.

## Manually Check for Update

```objc
[MSACDistribute checkForUpdate];
```
```swift
Distribute.checkForUpdate()
```

This sends a request to App Center and display an update dialog in case there's a new release available.

> [!NOTE]
> A manual check for update call works even when automatic updates are enabled. A manual check for update is ignored if another check is already being done. The manual check for update won't be processed if the user has postponed updates (unless the latest version is a mandatory update).

## Customize or localize the in-app update dialog

### 1. Customize or localize text

You can easily provide your own resource strings if you want to localize the text displayed in the update dialog. Look at [this strings file](https://github.com/Microsoft/AppCenter-SDK-Apple/blob/master/AppCenterDistribute/AppCenterDistribute/Resources/en.lproj/AppCenterDistribute.strings). Use the same string name/key and specify the localized value to be reflected in the dialog in your own app strings files.

### 2. Customize the update dialog

You can customize the default update dialog's appearance by implementing the `DistributeDelegate` protocol. You need to register the delegate before starting the SDK as shown in the following example:

```objc
[MSACDistribute setDelegate:self];
```
```swift
Distribute.delegate = self;
```

Here is an example of the delegate implementation that replaces the SDK dialog with a custom one:

```objc
- (BOOL)distribute:(MSACDistribute *)distribute releaseAvailableWithDetails:(MSACReleaseDetails *)details {

  // Your code to present your UI to the user, e.g. an UIAlertController.
  UIAlertController *alertController = [UIAlertController
      alertControllerWithTitle:@"Update available."
                       message:@"Do you want to update?"
                preferredStyle:UIAlertControllerStyleAlert];

  [alertController
      addAction:[UIAlertAction actionWithTitle:@"Update"
                                         style:UIAlertActionStyleCancel
                                       handler:^(UIAlertAction *action) {
                                         [MSACDistribute notifyUpdateAction:MSACUpdateActionUpdate];
                                       }]];

  [alertController
      addAction:[UIAlertAction actionWithTitle:@"Postpone"
                                         style:UIAlertActionStyleDefault
                                       handler:^(UIAlertAction *action) {
                                         [MSACDistribute notifyUpdateAction:MSACUpdateActionPostpone];
                                       }]];

  // Show the alert controller.
  [self.window.rootViewController presentViewController:alertController animated:YES completion:nil];
  return YES;
}
```
```swift
func distribute(_ distribute: Distribute, releaseAvailableWith details: ReleaseDetails) -> Bool {

  // Your code to present your UI to the user, e.g. an UIAlertController.
  let alertController = UIAlertController(title: "Update available.",
                                        message: "Do you want to update?",
                                 preferredStyle:.alert)

  alertController.addAction(UIAlertAction(title: "Update", style: .cancel) {_ in
    Distribute.notify(.update)
  })

  alertController.addAction(UIAlertAction(title: "Postpone", style: .default) {_ in
    Distribute.notify(.postpone)
  })

  // Show the alert controller.
  self.window?.rootViewController?.present(alertController, animated: true)
  return true;
}
```

In case you return `YES`/`true` in the above method, your app should obtain user's choice and message the SDK with the result using the following API.

```objc
// Depending on the user's choice, call notifyUpdateAction: with the right value.
[MSACDistribute notifyUpdateAction:MSACUpdateActionUpdate];
[MSACDistribute notifyUpdateAction:MSACUpdateActionPostpone];
```
```swift
// Depending on the user's choice, call notify() with the right value.
Distribute.notify(.update);
Distribute.notify(.postpone);
```

If you don't call the above method, the `releaseAvailableWithDetails:`-method will repeat whenever your app is entering to the foreground.

### 3. Execute code if no updates are found

In cases when the SDK checks for updates and doesn't find any updates available newer than the one currently used, a `distributeNoReleaseAvailable:` from `MSACDistributeDelegate` delegate callback is invoked. This allows you to execute custom code in such scenarios.

Here are examples which show how to display alert UI when no updates are found:

```objc
- (void)distributeNoReleaseAvailable:(MSACDistribute *)distribute {
  UIAlertController *alert = [UIAlertController alertControllerWithTitle:nil
                                                                 message:NSLocalizedString(@"No updates available", nil)
                                                          preferredStyle:UIAlertControllerStyleAlert];
  [alert addAction:[UIAlertAction actionWithTitle:NSLocalizedString(@"OK", nil) style:UIAlertActionStyleDefault handler:nil]];
  [self.window.rootViewController presentViewController:alert animated:YES completion:nil];
}
```

```swift
  func distributeNoReleaseAvailable(_ distribute: Distribute) {
    let alert = UIAlertController(title: nil, message: "No updates available", preferredStyle: .alert)
    alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
    self.window?.rootViewController?.present(alert, animated: true)
  }
```

## Enable or disable App Center Distribute at runtime

You can enable and disable App Center Distribute at runtime. If you disable it, the SDK won't provide any in-app update functionality but you can still use Distribute service in App Center portal.

```objc
[MSACDistribute setEnabled:NO];
```
```swift
Distribute.enabled = false
```

To enable App Center Distribute again, use the same API but pass `YES`/`true` as a parameter.

```objc
[MSACDistribute setEnabled:YES];
```
```swift
Distribute.enabled = true
```

The state is persisted in the device's storage across application launches.

> [!NOTE]
> This method must only be used after `Distribute` has been started.

## Check if App Center Distribute is enabled

You can also check if App Center Distribute is enabled or not:

```objc
BOOL enabled = [MSACDistribute isEnabled];
```
```swift
var enabled = Distribute.enabled
```

> [!NOTE]
> This method must only be used after `Distribute` has been started, it will always return `false` before start.

## Don't initialize App Center Distribute during development

If in private mode, App Center Distribute will open up its UI/browser at application start. While this is an expected behavior for your end users, it could be disruptive for you during the development stage of your application. We don't recommend initializing `Distribute` for your `DEBUG` configuration.

 ```objc
 #if DEBUG
     [MSACAppCenter start:@"{Your App Secret}" withServices:@[[MSACAnalytics class], [MSACCrashes class]]];
 #else
     [MSACAppCenter start:@"{Your App Secret}" withServices:@[[MSACAnalytics class], [MSACCrashes class], [MSACDistribute class]]];
 #endif
 ```
 ```swift
 #if DEBUG
     AppCenter.start(withAppSecret: "{Your App Secret}", services: [Analytics.self, Crashes.self])
 #else
     AppCenter.start(withAppSecret: "{Your App Secret}", services: [Analytics.self, Crashes.self, Distribute.self])
 #endif
 ```

## Perform clean up right before the application closes for update

Implement the `DistributeDelegate` protocol and register the delegate as shown in the following example:

```objc
[MSACDistribute setDelegate:self];
```
```swift
Distribute.delegate = self;
```

The `distributeWillExitApp:` delegate method will be called right before the app gets terminated for the update installation:

```objc
- (void)distributeWillExitApp:(MSACDistribute *)distribute {
  // Perform the required clean up here.
}
```
```swift
func distributeWillExitApp(_ distribute: Distribute) {
  // Perform the required clean up here.
}
```

## How do in-app updates work?

> [!NOTE]
> For in-app updates to work, an app build should be downloaded from the link. It won't work if installed from an IDE or manually.

The in-app updates feature works as follows:

1. This feature will ONLY work with builds that are distributed using **App Center Distribute** service. It won't work when the debugger is attached or if the iOS Guided Access feature is turned on..
2. Once you integrate the SDK, build a release version of your app and upload it to App Center, users in that distribution group will be notified for the new release via an email.
3. When each user opens the link in their email, the application will be installed on their device. It's important that they use the email link to install the app - App Center Distribute doesn't support in-app-updates for apps that have been installed from other sources (e.g. downloading the app from an email attachment). When an application is downloaded from the link, the SDK saves important information from cookies to check for updates later, otherwise the SDK doesn’t have that key information.
4. If the application sets the track to private, a browser will open to authenticate the user and enable in-app updates. The browser won't open again as long as the authentication information remains valid even when switching back to the public track and back to private again later. If the browser authentication is successful, the user is redirected back to the application automatically. If the track is public (which is the default), the next step happens directly.

   * On iOS 9 and 10, an instance of `SFSafariViewController` will open within the app to authenticate the user. It will close itself automatically after the authentication succeeded.
   * On iOS 11, the user experience is similar to iOS 9 and 10 but iOS 11 will ask the user for their permission to access login information. This is a system level dialog and it can't be customized. If the user cancels the dialog, they can continue to use the version they're testing, but they won't get in-app-updates. They'll be asked to access login information again when they launch the app the next time.

5. A new release of the app shows the in-app update dialog asking users to update your application if it's:

   * a higher value of `CFBundleShortVersionString` or
   * an equal value of `CFBundleShortVersionString` but a higher value of `CFBundleVersion`.
   * the versions are the same but the build unique identifier is different.

> [!TIP]
> If you upload the same ipa a second time, the dialog will **NOT** appear as the binaries are identical. If you upload a **new** build with the same version properties, it will show the update dialog. The reason for this is that it's a **different** binary.

## How do I test in-app updates?

You need to upload release builds (that use the Distribute module of the App Center SDK) to the App Center Portal to test in-app updates, increasing version numbers every time.

1. Create your app in the App Center Portal if you haven't already.
1. Create a new distribution group and name it so you can recognize it's meant for testing the in-app update feature.
1. Add yourself (or all people who you want to include on your test of the in-app update feature). Use a new or throw-away email address for this, that wasn't used for that app on App Center. This ensures that your experience is close to the experience of your real testers.
1. Create a new build of your app that includes **App Center Distribute** and contains the setup logic as described below. If the group is private, don't forget to set the private in-app update track before start using the [updateTrack property](#use-private-distribution-group).
1. Click on the **Distribute new release** button in the portal and upload your build of the app.
1. Once the upload has finished, click **Next** and select the **Distribution group** that you created as the **Destination** of that app distribution.
1. Review the Distribution and distribute the build to your in-app testing group.
1. People in that group will receive an invite to be testers of the app. Once they accept the invite, they can download the app from the App Center Portal from their mobile device. Once they have in-app updates installed, you're ready to test in-app updates.
1. Bump the version name (`CFBundleShortVersionString`) of your app.
1. Build the release version of your app and upload a new build of your app like you did in the previous step and distribute this to the **Distribution Group** you created earlier. Members of the Distribution Group will be prompted for a new version the next time the app starts.

> [!TIP]
> Have a look at the information on how to [utilize App Center Distribute](~/distribution/index.md) for more detailed information about **Distribution Groups** etc.
> While it's possible to use App Center Distribute to distribute a new version of your app without adding any code, adding App Center Distribute to your app's code will result in a more seamless experience for your testers and users as they get the in-app update experience.

## Disable forwarding of the application delegate's methods calls to App Center services

The App Center SDK uses swizzling to improve its integration by forwarding itself some of the application delegate's methods calls. Method swizzling is a way to change the implementation of methods at runtime. If for any reason you don't want to use swizzling (e.g. because of a specific policy) then you can disable this forwarding for all App Center services by following the steps below:

1. Open the project's **Info.plist** file.
2. Add `AppCenterAppDelegateForwarderEnabled` key and set the value to `0`. This disables application delegate forwarding for all App Center services.
3. Add the `openURL` callback in the project's `AppDelegate` file.

```objc
- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation {

  // Pass the url to MSACDistribute.
  return [MSACDistribute openURL:url];
}
```
```swift
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {

  // Pass the URL to App Center Distribute.
  return Distribute.open(url)
}
```

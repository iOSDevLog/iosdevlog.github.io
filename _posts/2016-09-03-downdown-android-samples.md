---
layout: post
title: "下载 Android Samples 示例"
description: ""
category: 
tags: [Android]
---


Android Studio 里面有一个 "Import an Android code sample"，因为各种原因无法下载。

![1](/assets/images/Android/Samples/1.png)

初始界面

![2](/assets/images/Android/Samples/2.png)

点击 Import an Android code sample 后

![3](/assets/images/Android/Samples/3.png)

导入失败

不过我们可以从 *github* 上面下载这些示例

网址: <https://github.com/googlesamples>

这里有 *google* 的示例代码，可以发现以 "android-" 开头的就是 Android Samples。还有以 "androidtv-" 开头的。

通过查看 *github* 文档

网址: <https://api.github.com/>

找到

{% highlight javascript %}
"organization_url": "https://api.github.com/orgs/{org}",
{% endhighlight %}

把 {org} 替换成 *googlesamples*

<https://api.github.com/orgs/googlesamples/repos>

得到了 json 数据

找到 *clone* 地址

{% highlight javascript %}
"clone_url": "https://github.com/googlesamples/android-BasicManagedProfile.git",
{% endhighlight %}

> 通过 *?page={number}*，等到分页数据，例如 <https://api.github.com/orgs/googlesamples/repos?page=2>

> 可以通过 JSONExport <https://github.com/Ahmed-Ali/JSONExport> 导出到 Android/iOS 中 Gson/Realm/Java/Objective-C/Swift/CoreData/SwiftyJSON 等格式的文件。

写脚本或者程序可以把所有 "android-" 示例程序下载下来。

最后生成如下 downloadAndroidSamples.sh，可以用 bash 完成下载。

我已经把下载完成的文件上传到百度网盘，

百度网盘： <https://pan.baidu.com/s/1bpDSHBL>

{% highlight bash %}
#!/usr/bin/env bash

git clone https://github.com/googlesamples/android-BasicManagedProfile.git
git clone https://github.com/googlesamples/android-Camera2Basic.git
git clone https://github.com/googlesamples/android-Camera2Video.git
git clone https://github.com/googlesamples/android-ElevationBasic.git
git clone https://github.com/googlesamples/android-ElevationDrag.git
git clone https://github.com/googlesamples/android-ClippingBasic.git
git clone https://github.com/googlesamples/android-JobScheduler.git
git clone https://github.com/googlesamples/android-NavigationDrawer.git
git clone https://github.com/googlesamples/android-play-publisher-api.git
git clone https://github.com/googlesamples/android-fit.git
git clone https://github.com/googlesamples/android-FloatingActionButtonBasic.git
git clone https://github.com/googlesamples/android-PdfRendererBasic.git
git clone https://github.com/googlesamples/android-RevealEffectBasic.git
git clone https://github.com/googlesamples/android-ActionBarCompat-Basic.git
git clone https://github.com/googlesamples/android-ActionBarCompat-ListPopupMenu.git
git clone https://github.com/googlesamples/android-ActionBarCompat-ShareActionProvider.git
git clone https://github.com/googlesamples/android-ActionBarCompat-Styled.git
git clone https://github.com/googlesamples/android-ActivityInstrumentation.git
git clone https://github.com/googlesamples/android-AdvancedImmersiveMode.git
git clone https://github.com/googlesamples/android-AppRestrictions.git
git clone https://github.com/googlesamples/android-BasicAccessibility.git
git clone https://github.com/googlesamples/android-BasicAndroidKeyStore.git
git clone https://github.com/googlesamples/android-BasicContactables.git
git clone https://github.com/googlesamples/android-BasicGestureDetect.git
git clone https://github.com/googlesamples/android-BasicImmersiveMode.git
git clone https://github.com/googlesamples/android-BasicMediaDecoder.git
git clone https://github.com/googlesamples/android-BasicMediaRouter.git
git clone https://github.com/googlesamples/android-BasicMultitouch.git
git clone https://github.com/googlesamples/android-BasicNetworking.git
git clone https://github.com/googlesamples/android-BasicNotifications.git
git clone https://github.com/googlesamples/android-BasicRenderScript.git
git clone https://github.com/googlesamples/android-BasicSyncAdapter.git
git clone https://github.com/googlesamples/android-BasicTransition.git
git clone https://github.com/googlesamples/android-BatchStepSensor.git
git clone https://github.com/googlesamples/android-BluetoothLeGatt.git
git clone https://github.com/googlesamples/android-BorderlessButtons.git
git clone https://github.com/googlesamples/android-CardEmulation.git
git clone https://github.com/googlesamples/android-CardReader.git
git clone https://github.com/googlesamples/android-CustomChoiceList.git
git clone https://github.com/googlesamples/android-CustomNotifications.git
git clone https://github.com/googlesamples/android-CustomTransition.git
git clone https://github.com/googlesamples/android-DisplayingBitmaps.git
git clone https://github.com/googlesamples/android-DoneBar.git
git clone https://github.com/googlesamples/android-HorizontalPaging.git
git clone https://github.com/googlesamples/android-ImmersiveMode.git
git clone https://github.com/googlesamples/android-MediaRecorder.git
git clone https://github.com/googlesamples/android-MediaRouter.git
git clone https://github.com/googlesamples/android-NetworkConnect.git
git clone https://github.com/googlesamples/android-RenderScriptIntrinsic.git
git clone https://github.com/googlesamples/android-RepeatingAlarm.git
git clone https://github.com/googlesamples/android-SlidingTabsBasic.git
git clone https://github.com/googlesamples/android-SlidingTabsColors.git
git clone https://github.com/googlesamples/android-StorageClient.git
git clone https://github.com/googlesamples/android-StorageProvider.git
git clone https://github.com/googlesamples/android-SwipeRefreshLayoutBasic.git
git clone https://github.com/googlesamples/android-SwipeRefreshListFragment.git
git clone https://github.com/googlesamples/android-SwipeRefreshMultipleViews.git
git clone https://github.com/googlesamples/android-TextLinkify.git
git clone https://github.com/googlesamples/android-TextSwitcher.git
git clone https://github.com/googlesamples/android-google-accounts.git
git clone https://github.com/googlesamples/android-BluetoothChat.git
git clone https://github.com/googlesamples/android-MediaEffects.git
git clone https://github.com/googlesamples/android-SynchronizedNotifications.git
git clone https://github.com/googlesamples/android-CardView.git
git clone https://github.com/googlesamples/android-DrawableTinting.git
git clone https://github.com/googlesamples/android-LNotifications.git
git clone https://github.com/googlesamples/android-RecyclerView.git
git clone https://github.com/googlesamples/android-testing.git
git clone https://github.com/googlesamples/android-play-location.git
git clone https://github.com/googlesamples/android-ActivitySceneTransitionBasic.git
git clone https://github.com/googlesamples/android-AppRestrictionEnforcer.git
git clone https://github.com/googlesamples/android-AppRestrictionSchema.git
git clone https://github.com/googlesamples/android-DocumentCentricApps.git
git clone https://github.com/googlesamples/android-DocumentCentricRelinquishIdentity.git
git clone https://github.com/googlesamples/android-HdrViewfinder.git
git clone https://github.com/googlesamples/android-Interpolator.git
git clone https://github.com/googlesamples/android-AgendaData.git
git clone https://github.com/googlesamples/android-DataLayer.git
git clone https://github.com/googlesamples/android-DelayedConfirmation.git
git clone https://github.com/googlesamples/android-ElizaChat.git
git clone https://github.com/googlesamples/android-FindMyPhone.git
git clone https://github.com/googlesamples/android-Flashlight.git
git clone https://github.com/googlesamples/android-Geofencing.git
git clone https://github.com/googlesamples/android-GridViewPager.git
git clone https://github.com/googlesamples/android-JumpingJack.git
git clone https://github.com/googlesamples/android-Quiz.git
git clone https://github.com/googlesamples/android-RecipeAssistant.git
git clone https://github.com/googlesamples/android-SkeletonWearableApp.git
git clone https://github.com/googlesamples/android-SpeedTracker.git
git clone https://github.com/googlesamples/android-Timer.git
git clone https://github.com/googlesamples/android-WatchViewStub.git
git clone https://github.com/googlesamples/android-MessagingService.git
git clone https://github.com/googlesamples/android-MediaBrowserService.git
git clone https://github.com/googlesamples/android-Notifications.git
git clone https://github.com/googlesamples/android-WatchFace.git
git clone https://github.com/googlesamples/android-AppUsageStatistics.git
git clone https://github.com/googlesamples/android-DirectorySelection.git
git clone https://github.com/googlesamples/android-PermissionRequest.git
git clone https://github.com/googlesamples/android-ScreenCapture.git
git clone https://github.com/googlesamples/android-play-all.git
git clone https://github.com/googlesamples/android-wear-xamarin-watchface.git
git clone https://github.com/googlesamples/android-testing-templates.git
git clone https://github.com/googlesamples/android-performance.git
git clone https://github.com/googlesamples/android-nearby.git
git clone https://github.com/googlesamples/android-codelab-watchface.git
git clone https://github.com/googlesamples/android-UniversalMusicPlayer.git
git clone https://github.com/googlesamples/android-ads.git
git clone https://github.com/googlesamples/android-play-places.git
git clone https://github.com/googlesamples/android-nearby-cpp.git
git clone https://github.com/googlesamples/android-BeamLargeFiles.git
git clone https://github.com/googlesamples/android-DeviceOwner.git
git clone https://github.com/googlesamples/android-NfcProvisioning.git
git clone https://github.com/googlesamples/android-XYZTouristAttractions.git
git clone https://github.com/googlesamples/android-play-games-in-motion.git
git clone https://github.com/googlesamples/android-topeka.git
git clone https://github.com/googlesamples/android-ndk.git
git clone https://github.com/googlesamples/android-AlwaysOn.git
git clone https://github.com/googlesamples/android-BluetoothAdvertisements.git
git clone https://github.com/googlesamples/android-testdpc.git
git clone https://github.com/googlesamples/android-credentials.git
git clone https://github.com/googlesamples/android-FingerprintDialog.git
git clone https://github.com/googlesamples/android-ConfirmCredential.git
git clone https://github.com/googlesamples/android-RuntimePermissions.git
git clone https://github.com/googlesamples/android-RuntimePermissionsBasic.git
git clone https://github.com/googlesamples/android-AutoBackupForApps.git
git clone https://github.com/googlesamples/android-ActiveNotifications.git
git clone https://github.com/googlesamples/android-Camera2Raw.git
git clone https://github.com/googlesamples/android-fit-xamarin.git
git clone https://github.com/googlesamples/android-vision.git
git clone https://github.com/googlesamples/android-custom-lint-rules.git
git clone https://github.com/googlesamples/android-play-billing.git
git clone https://github.com/googlesamples/android-DirectShare.git
git clone https://github.com/googlesamples/android-MidiScope.git
git clone https://github.com/googlesamples/android-MidiSynth.git
git clone https://github.com/googlesamples/android-audio-high-performance.git
git clone https://github.com/googlesamples/android-AsymmetricFingerprintDialog.git
git clone https://github.com/googlesamples/android-WearCompanionLibrary.git
git clone https://github.com/googlesamples/android-WclDemoSample.git
git clone https://github.com/googlesamples/android-RuntimePermissionsWear.git
git clone https://github.com/googlesamples/android-WearSpeakerSample.git
git clone https://github.com/googlesamples/android-WeaveLedToggler.git
git clone https://github.com/googlesamples/android-gcmnetworkmanager.git
git clone https://github.com/googlesamples/android-architecture.git
git clone https://github.com/googlesamples/android-vulkan-tutorials.git
git clone https://github.com/googlesamples/android-MultiWindowPlayground.git
git clone https://github.com/googlesamples/android-DirectBoot.git
git clone https://github.com/googlesamples/android-ScopedDirectoryAccess.git
git clone https://github.com/googlesamples/android-OurStreets.git
git clone https://github.com/googlesamples/android-WearDrawers.git
git clone https://github.com/googlesamples/android-unsplash.git
git clone https://github.com/googlesamples/android-play-awareness.git
git clone https://github.com/googlesamples/android-AccelerometerPlay.git
{% endhighlight %}



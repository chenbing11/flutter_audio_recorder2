# flutter_audio_recorder2
<p align="left">
  <a href="https://pub.dartlang.org/packages/flutter_audio_recorder2">
    <img alt="pub version" src="https://img.shields.io/pub/v/flutter_audio_recorder2.svg?style=flat-square">
  </a>
</p>

English

Flutter Audio Record 2 Plugin that supports `Record` `Pause` `Resume` `Stop` and provide access to audio level metering properties `average power` `peak power`
#### Works for both `Android` and `iOS`

<img src="https://user-images.githubusercontent.com/10917606/64927086-b2bcda00-d838-11e9-9ab8-bad78a95f02c.gif" width="30%" height="30%" />

## Installation
add `flutter_audio_recorder2` to your `pubspec.yaml`

## iOS Permission
1. Add usage description to Info.plist
```
<key>NSMicrophoneUsageDescription</key>
<string>Can We Use Your Microphone Please</string>
```
2. Then use `hasPermission` api to ask user for permission when needed

## Android Permission
1. Add `uses-permission` to `./android/app/src/main/AndroidManifest.xml` in xml root level like below
```
    ...
    </application>
    <uses-permission android:name="android.permission.RECORD_AUDIO"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    ...
</manifest>
```
2. Then use `hasPermission` api to ask user for permission when needed

## Configuration
#### iOS Deployment Target is 8.0 above
#### Android
- AndroidX: use latest version (`0.5.x`)
- Legacy Android: use old version (`0.4.9`)

## Usage
Recommended API Usage: `hasPermission` => `init` > `start` -> (`pause` <-> `resume`) * n -> `stop`, call `init` again before `start` another recording

#### Always check permission first(it will request permission if permission has not been set to true/false yet, otherwise it will return the result of recording permission)
```
bool hasPermission = await FlutterAudioRecorder2.hasPermissions;
```

#### `Initialize` (run this before `start`, so we could check if file with given name already exists)
```
var recorder = FlutterAudioRecorder2("file_path.mp4"); // .wav .aac .m4a
await recorder.initialized;
```

or

```
var recorder = FlutterAudioRecorder2("file_path", audioFormat: AudioFormat.AAC); // or AudioFormat.WAV
await recorder.initialized;
```

##### Sample Rate
```
var recorder = FlutterAudioRecorder2("file_path", audioFormat: AudioFormat.AAC, sampleRate: 22000); // sampleRate is 16000 by default
await recorder.initialized;
```

##### Audio Extension and Format Mapping
| Audio Format  | Audio Extension List |
| ------------- | ------------- |
| AAC  | .m4a .aac .mp4  |
| WAV  | .wav  |

#### Start recording
```
await recorder.start();
var recording = await recorder.current(channel: 0);
```

#### Get recording details
```
var current = await recording.current(channel: 0);
// print(current.status);
```
You could use a timer to access details every 50ms(simply cancel the timer when recording is done)
```
new Timer.periodic(tick, (Timer t) async {
        var current = await recording.current(channel: 0);
        // print(current.status);
        setState(() {
        });
      });
```

##### Recording
| Name  | Description |
| ------------- | ------------- |
| path  | String  |
| extension  | String  |
| duration  | Duration  |
| audioFormat  | AudioFormat  |
| metering  | AudioMetering  |
| status  | RecordingStatus  |

##### Recording.metering
| Name  | Description |
| ------------- | ------------- |
| peakPower  | double  |
| averagePower  | double  |
| isMeteringEnabled  | bool  |

##### Recording.status
`Unset`,`Initialized`,`Recording`,`Paused`,`Stopped`


#### Pause
```
await recorder.pause();
```

#### Resume
```
await recorder.resume();
```

#### Stop (after `stop`, run `init` again to create another recording)
```
var result = await recorder.stop();
File file = widget.localFileSystem.file(result.path);
```

## Example
Please check example app using Xcode.


## Getting Started

This project is a starting point for a Flutter
[plug-in package](https://flutter.dev/developing-packages/),
a specialized package that includes platform-specific implementation code for
Android and/or iOS.

For help getting started with Flutter, view our
[online documentation](https://flutter.dev/docs), which offers tutorials,
samples, guidance on mobile development, and a full API reference.


## 解决引入 flutter_audio_recorder报错
 And because if flutter_audio_recorder any and process >=3.0.12 then intl >=0.14.0 <0.17.0 (1), if flutter_audio_recorder any and path_provider_linux <0.2.0-nullsafety then intl >=0.14.0 <0.17.0.

    And because path_provider >=1.6.10 <2.0.0-nullsafety depends on path_provider_linux ^0.0.1, if flutter_audio_recorder any and path_provider >=1.6.10 <2.0.0-nullsafety then intl >=0.14.0 <0.17.0.

    And because h3yunapp depends on flutter_localizations any from sdk which depends on intl 0.17.0, flutter_audio_recorder is incompatible with path_provider >=1.6.10 <2.0.0-nullsafety
    
flutter_audio_recorder: ^0.5.5 改成 flutter_audio_recorder2: ^0.0.2

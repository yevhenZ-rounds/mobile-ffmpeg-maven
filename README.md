# mobile-ffmpeg-maven

Maven repository with [MobileFFmpeg](https://github.com/tanersener/mobile-ffmpeg) 4.4 builds for Android.
The original `com.arthenica:mobile-ffmpeg-*` artifacts are no longer resolvable (JCenter shutdown), so they are published here.

- Rebuilt with Android NDK 28
- `minSdk` 24+ (the original non-LTS packages)
- **Native libraries for 64-bit ABIs (`arm64-v8a`, `x86_64`) are aligned to 16KB pages**, as required for Android 15+ / Google Play
- ABIs: `armeabi-v7a`, `arm64-v8a`, `x86`, `x86_64`

## Source

These artifacts were built from the source code in
[zixieTools/mobile-ffmpeg-android-16kb](https://github.com/zixieTools/mobile-ffmpeg-android-16kb),
a fork of [tanersener/mobile-ffmpeg](https://github.com/tanersener/mobile-ffmpeg) (v4.4) adapted to Android NDK 28 and 16KB page alignment.

## Usage

```gradle
// settings.gradle (or the root build.gradle)
repositories {
    maven {
        url "https://raw.githubusercontent.com/yevhenZ-rounds/mobile-ffmpeg-maven/main"
        content { includeGroup "com.rounds.android" }
    }
}

// app/build.gradle
dependencies {
    implementation 'com.rounds.android:mobile-ffmpeg-min-16kb:4.4'
}
```

The Java API is unchanged (`com.arthenica.mobileffmpeg.*`).

## Artifacts

`groupId`: `com.rounds.android`, `version`: `4.4`

| artifactId | External libraries | License |
|---|---|---|
| `mobile-ffmpeg-min-16kb` | none | LGPL v3 |
| `mobile-ffmpeg-min-gpl-16kb` | vid.stab, x264, x265, xvidcore | **GPL v3** |
| `mobile-ffmpeg-https-16kb` | gmp, gnutls | LGPL v3 |
| `mobile-ffmpeg-https-gpl-16kb` | gmp, gnutls, vid.stab, x264, x265, xvidcore | **GPL v3** |
| `mobile-ffmpeg-audio-16kb` | lame, libilbc, libvorbis, opencore-amr, opus, shine, soxr, speex, twolame, vo-amrwbenc, wavpack | LGPL v3 |
| `mobile-ffmpeg-video-16kb` | fontconfig, freetype, fribidi, kvazaar, libaom, libass, libiconv, libtheora, libvpx, libwebp, snappy | LGPL v3 |
| `mobile-ffmpeg-full-16kb` | all of the above except the GPL libraries | LGPL v3 |
| `mobile-ffmpeg-full-gpl-16kb` | everything | **GPL v3** |

All packages include the `zlib` and `MediaCodec` Android system libraries.

Using a `-gpl` artifact makes your application subject to the GPL.

## Rules for maintainers

- Published versions are immutable. Gradle caches them; never change the files of an existing version.
  Publish fixes as a new version (`4.4.1`, ...).
- Commit only finished, tested builds. Every version adds its AARs (about 445 MB) to the repository history.

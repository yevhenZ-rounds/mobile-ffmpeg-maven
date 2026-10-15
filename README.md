# mobile-ffmpeg-maven

[MobileFFmpeg](https://github.com/tanersener/mobile-ffmpeg) 4.4 builds for Android, published for use as Gradle dependencies.
The original `com.arthenica:mobile-ffmpeg-*` artifacts are no longer resolvable (JCenter shutdown), so they are published here.

- Rebuilt with Android NDK 28
- `minSdk` 24+ (the original non-LTS packages)
- **Native libraries for 64-bit ABIs (`arm64-v8a`, `x86_64`) are aligned to 16KB pages**, as required for Android 15+ / Google Play
- ABIs: `armeabi-v7a`, `arm64-v8a`, `x86`, `x86_64`

## Source

These artifacts were built from the source code in
[zixieTools/mobile-ffmpeg-android-16kb](https://github.com/zixieTools/mobile-ffmpeg-android-16kb),
a fork of [tanersener/mobile-ffmpeg](https://github.com/tanersener/mobile-ffmpeg) (v4.4) adapted to Android NDK 28 and 16KB page alignment.

## Usage (JitPack)

```gradle
// settings.gradle
dependencyResolutionManagement {
    repositories {
        mavenCentral()
        maven { url 'https://jitpack.io' }
    }
}

// app/build.gradle
dependencies {
    implementation 'com.github.yevhenZ-rounds.mobile-ffmpeg-maven:mobile-ffmpeg-min-16kb:4.4'
}
```

Replace `mobile-ffmpeg-min-16kb` with the artifact you need (see below). The Java API is unchanged (`com.arthenica.mobileffmpeg.*`).

### Without JitPack

The same files are available as a plain Maven repository from GitHub, under a different groupId (`com.rounds.android`):

```gradle
repositories {
    maven {
        url "https://raw.githubusercontent.com/yevhenZ-rounds/mobile-ffmpeg-maven/main"
        content { includeGroup "com.rounds.android" }
    }
}
dependencies {
    implementation 'com.rounds.android:mobile-ffmpeg-min-16kb:4.4'
}
```

## Artifacts

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

- Published versions are immutable. Gradle and JitPack cache them; never change the files of an existing version or move its tag.
  Publish fixes as a new version (`4.4.1`, ...).
- To add a version: put the AARs, POMs and checksums under `com/rounds/android/<artifactId>/<version>/`, commit, then push a git tag named like the version.
  `jitpack.yml` installs every AAR found in that tree and JitPack builds on the first request for the tag.
- JitPack caches a failed build of a tag. Check a change first with a branch build
  (`...:mobile-ffmpeg-min-16kb:main-SNAPSHOT`) before tagging.
- JitPack expects the coordinates of the committed `.pom` files; keep them consistent with `jitpack.yml`.
- Commit only finished, tested builds. Every version adds its AARs (about 445 MB) to the repository history.

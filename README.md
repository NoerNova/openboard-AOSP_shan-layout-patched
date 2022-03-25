# Patched openboard

---

This project is a patched for [openboard](https://github.com/openboard-team/openboard) to support Shan language layout.

##### Challenges:

1. The base project [AOSP Keyboard](https://android.googlesource.com/platform/packages/inputmethods/LatinIME/) LatinIME not support for Shan's language yet.
2. Keyboard texts table build tools from [tools/make-keyboard-text - platform/packages/inputmethods/LatinIME - Git at Google](https://android.googlesource.com/platform/packages/inputmethods/LatinIME/+/refs/heads/master/tools/make-keyboard-text/) using ```java.util.Locale``` btw, openjdk11 - openjdk18 [Java 11 Supported Locales](https://www.oracle.com/java/technologies/javase/jdk11-suported-locales.html) do not support shn (Shan's language code) yet.

so if add values_shn to tools/make-keyboard-text/src/main/resources will failed with ```Unknown locale format: shn```

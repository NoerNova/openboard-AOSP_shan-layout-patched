# Patched openboard - AOSP

---

This project is a patched for [openboard](https://github.com/openboard-team/openboard) to support Shan language layout.

#### Challenges:

1. The base project [AOSP Keyboard](https://android.googlesource.com/platform/packages/inputmethods/LatinIME/) LatinIME not support for Shan's language yet.
2. Keyboard texts table build tools original from [tools/make-keyboard-text - platform/packages/inputmethods/LatinIME - Git at Google](https://android.googlesource.com/platform/packages/inputmethods/LatinIME/+/refs/heads/master/tools/make-keyboard-text/) currently limit **MAX_LENGTH_OF_LANGUAGE = 2** which not support for three-letter codes language yet.

so if add values_shn to tools/make-keyboard-text/src/main/resources will failed with ```Unknown locale format: shn```

### Compiling and installing

#### Method 1. from https://github.com/openboard-team/openboard
1. Clone the repo

   ```sh
   git clone https://github.com/openboard-team/openboard
   ```

2. copy/add values_shn/donottranslate-more-keys.xml to tools/make-keyboard-text/resources/

3. Edit tools/make-keyboard-text/src/main/java/com/android/inputmethod/keyboard/tools/LocaleUtils.java (***Workaround solution for three-letter codes, for now***)

```java
	... 
}

   private static final int MIN_LENGTH_OF_LANGUAGE = 2;
   private static final int MAX_LENGTH_OF_LANGUAGE = 3; // change this from 2 to 3
   private static final int LENGTH_OF_SCRIPT = 4;
   private static final int MIN_LENGTH_OF_REGION = 2;
   private static final int MAX_LENGTH_OF_REGION = 2;
   private static final int LENGTH_OF_AREA_CODE = 3;
   
   private static boolean isValidLanguage(final String text) {
   
  ...
    
```

4. Generate the new version of [KeyboardTextsTable.java](https://github.com/openboard-team/openboard/blob/master/app/src/main/java/org/dslul/openboard/inputmethod/keyboard/internal/KeyboardTextsTable.java):

   ```sh
   ./gradlew tools:make-keyboard-text:makeText
   ```

5. Edit app/src/main/java/org/dslul/openboard/inputmethod/latin/utils/ScriptUtils.java (Workaround solution for now)

```java
...
	public class ScriptUtils {
	   ...
	    public static final int SCRIPT_MALAYALAM = 12;
	    public static final int SCRIPT_MYANMAR = 13;
	    public static final int SCRIPT_SHAN = 14; // add this as new language script
	    public static final int SCRIPT_SINHALA = 15;
	    public static final int SCRIPT_TAMIL = 16;
	    public static final int SCRIPT_TELUGU = 17;
	    public static final int SCRIPT_THAI = 18;
	   ...
	   
	   static {
	     ...
	   	mLanguageCodeToScriptCode.put("ml", SCRIPT_MALAYALAM);
	      	mLanguageCodeToScriptCode.put("my", SCRIPT_MYANMAR);
	      	mLanguageCodeToScriptCode.put("sh", SCRIPT_SHAN); // add this script
	      	mLanguageCodeToScriptCode.put("si", SCRIPT_SINHALA);
	      	mLanguageCodeToScriptCode.put("ta", SCRIPT_TAMIL);
	      	mLanguageCodeToScriptCode.put("te", SCRIPT_TELUGU);
	      	mLanguageCodeToScriptCode.put("th", SCRIPT_THAI);
	     ...
	    }
	     ...

	     public static boolean isLetterPartOfScript(final int codePoint, final int scriptId) {
		    ...
			case SCRIPT_MYANMAR:
			case SCRIPT_SHAN: // add this to SCRIPT_MYANMAR case cause Shan's unicode is in Myanmar's unicode block
			    // Myanmar has three unicode blocks :
			    // Myanmar U+1000..U+109F
			    // Myanmar extended-A U+AA60..U+AA7F
			    // Myanmar extended-B U+A9E0..U+A9FF
			    return (codePoint >= 0x1000 && codePoint <= 0x109F
			           || codePoint >= 0xAA60 && codePoint <= 0xAA7F
				   || codePoint >= 0xA9E0 && codePoint <= 0xA9FF);
			...
		    ...
```

6. copy/add app/src/main/res/values-shn/*

7. copy/add app/src/main/res/xml/*_shan*.xml

8. Compile the project.

   ```sh
   sh ./gradlew assembleDebug
   ```

9. Connect your phone and install the debug APK

   ```sh
   adb install ./app/build/outputs/apk/debug/app-debug.apk
   ```

   


#### Method 2. from https://github.com/NoerNova/openboard
1. Clone patched repo

   ```sh
   git clone https://github.com/NoerNova/openboard
   ```

2. Compile the project.

   ```sh
   sh ./gradlew assembleDebug
   ```

3. Connect your phone and install the debug APK

   ```sh
   adb install ./app/build/outputs/apk/debug/app-debug.apk
   ```

   

### Credits

- [openboard-team](https://github.com/openboard-team)
- [AOSP Keyboard](https://android.googlesource.com/platform/packages/inputmethods/LatinIME/)

### LICENSE
GPL-3.0

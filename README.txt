CBOOT - Boot Animation Manager
==============================
Backup, restore, convert and apply custom Android boot animations
(bootanimation.zip) on rooted devices.

Package:  com.jozo.cboot
Made by tt:@josipzzz | GitHub: jopatopa  (https://github.com/jopatopa)


REQUIREMENTS
------------
- Android 8.0 (API 26) or newer
- ROOT (Magisk, KernelSU or APatch)
- A Magisk/KernelSU/APatch module manager is strongly recommended: on
  modern phones the system partition is read-only (EROFS / dynamic
  partitions), so CBoot installs animations as a systemless module there.


HOW TO USE
----------
1. Open CBoot and grant root when asked. If root is missing you get a
   dialog with Retry and Exit.

2. BACKUPS tab
   - The Device compatibility card shows your screen size (the best size
     for an animation), Android version, system partition type, the active
     boot animation and whether CBoot can apply animations on this device.
   - Tap "Backup current boot animation" to save the active animation.
   - Each backup shows a thumbnail, date/time, resolution and size.
     Restore re-applies it; the bin icon deletes it.
   - The first time you apply anything, CBoot automatically saves your
     original (stock) animation first.

3. CREATE tab
   - Select an image, GIF or video.
   - Clips longer than 10 seconds open the trimmer automatically. Drag the
     two handles to choose start and end (max 10 s). "Trim" reopens it.
   - Choose frame rate (15/24/30/60), resolution (Auto = your screen,
     1080p, 720p), loop (infinite, once or a custom count) and whether to
     fill the screen (crop) or keep black bars.
   - "Convert & preview" builds the frames and zip, then plays a live
     preview. Changing any option clears the preview.
   - "Apply to system" validates the file, shows the Safe Mode check, then
     installs it and offers to reboot. Changes show only after a reboot.

4. SETTINGS tab
   - Design: Apple iOS Liquid Glass, Google Material 3 (You; dynamic
     colors on Android 12+), Xiaomi HyperOS / MIUI, Classic / Retro.
   - Appearance: Light, Dark or System default.
   - Language: English, Hrvatski, Deutsch, Espanol, Francais.
   - Safe mode verification: warns before applying unverified animations.
   - Emergency rescue: export a flashable Magisk module (.zip) or a shell
     script (.sh). Both remove CBoot's module and restore your original
     boot animation.
   - Everything is saved automatically.


HOW APPLYING WORKS
------------------
- Module manager found: a module "cboot_bootanim" is created in
  /data/adb/modules. Nothing on /system is touched. To undo it, remove the
  module in Magisk/KernelSU.
- No module manager: the target partition is remounted read-write, the
  file is copied with chmod 644 and verified. This fails on read-only
  (EROFS) systems.
- If a bootanimation-dark.zip sits next to the stock file, it is replaced
  too, so dark mode does not show the old animation.
- Boot animations must be stored uncompressed. CBoot builds zips that way
  and its Safe Mode check rejects compressed ones.


IF A BOOT ANIMATION CAUSES PROBLEMS
-----------------------------------
- Magisk/KernelSU: boot to safe mode / disable modules, or delete
  /data/adb/modules/cboot_bootanim from a custom recovery.
- Or flash the rescue module exported from Settings, or run the rescue
  script:   su -c "sh /sdcard/CBoot/cboot_rescue.sh"
  (CBoot copies the stock animation to /sdcard/CBoot when you export the
  script; keep that folder and the script together.)
- Export the rescue files BEFORE applying something new.


NOTES AND LIMITS
----------------
- Samsung (One UI) uses its own boot format, so a custom bootanimation.zip
  may be ignored there.
- Video frames are decoded with Android's own decoder (no FFmpeg), which
  is slow for long clips at 60 FPS. 10 s at 30 FPS is a good size.
- Large animations (tens of MB) can slow boot; Safe Mode warns above 60 MB.
- No storage permission is needed: files are picked and exported with the
  system file picker and root handles protected paths.


BUILDING FROM SOURCE
--------------------
1. Open the CBoot folder in Android Studio (JDK 17, AGP 8.5.2).
2. Let Gradle sync, then run the "app" configuration or build an APK.
3. If the Gradle wrapper jar is missing, run "gradle wrapper" once or let
   Android Studio create it.

WARNING
-------
Changing boot files can cause slow boots or bootloops. Use at your own
risk and keep a rescue module ready.

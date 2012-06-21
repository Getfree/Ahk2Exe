Scripted replacement for Ahk2Exe
================================

Installation
------------

Just copy everything to your AutoHotkey\Compiler folder (make sure to backup the existing Ahk2Exe.exe!)
The source code to the compiler can be found at https://github.com/fincs/Ahk2Exe.

TODO
----

Handle FileInstall on same-line If* commands.


Custom additions by Getfree
--------------------------
The functionality of the FileInstall command has been extended in two way:

1. You can customize how resource files are added to the final exe by specifying its resource-type/resource-name/resource-language in the second parameter of FileInstall.

2. If the file to be added is a AHK script, then it is pre-processed first so that the full working script is added to the compiled .exe (just as the main script is).

In order to trigger this enhanced behavior you have to prepend an asterisk to the second parameter (which is an invalid character for a file name, so there is no risk of breaking old code)


Example usage:

FileInstall script.ahk,*      ; add a script pre-processed automatically (with all its #includes expanded)
FileInstall another.ahk,.     ; add "another.ahk" as FileInstall normaly does (no pre-processing)
FileInstall dialogBox_eng.mht, *html/dialog.mht/0x409    ; add an mht file in english
FileInstall dialogBox_esp.mht, *html/dialog.mht/0x340A   ; add the spanish version of the same file
FileInstall logo.ico, *14/1         ; add an icon in position 1 so it's showed as the program icon
FileInstall suspend1.ico, *14/206   ; replace the suspend tray icon
FileInstall pause1.ico, *14/207     ; replace the pause tray icon


The syntax for the second parameters is:

*[ resource-type [ / [ resource-id ] [ / resource-lang ] ] ]

- resource-type and resource-id can be a string or a number (Dec or Hex), resource-lang only a number
- For standard resource types, like RCData, Icons, Icon groups, etc... the number Id has to be used
- Expresions between [] can be ommited
- If something is ommited, the defaults are:
        - resource-type = 10 (RCData)
        - resource-id = the file name (FileInstall's first parameter)
        - resource-lang = 0x409 (english)
- If there is no leading asterisk, the parameters is interpreted in its usual way
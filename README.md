# SpellChecker Plugin

## 1. Introduction
The SpellChecker Plugin is a spellchecker plugin for the Qt Creator IDE. 
This plugin spell checks Comments and String Literals in source files for spelling mistakes and suggest the correct spelling for misspelled words, if possible. 

Currently the plugin only checks C++ files and uses the Hunspell Spell Checker to check words for spelling mistakes.
The plugin provides an options page in Qt Creator that can be used to configure the parsers as well as the spell checkers available.

To download a pre-built version of the plugin, refer to section 2. 

To build the plugin self, see section 6.

The motivation for this plugin is that my spelling is terrible and I was looking for a plugin that could spell check my Doxygen comments. I feel that good comments are essential to any piece of software. I could not find one suitable to my needs so I decided to challenge myself to see if I can implement one myself. In one of my courses on varsity I investigated the Qt Creator source code and was amazed with the whole plugin system, thus it also inspired me to try and
contribute to such a project. 

I did look a lot at the code in the TODO plugin as a basis for my own implementation. I do not think that I have violated any licenses doing so since I have never just copied code from the plugin. If there are any places that can be problematic regarding any licenses, please contact me so that I can resolve the issues. I want to contribute to the Open Source Community, but it is not my intention to step on any toes in the process. 

## 2. Pre-Build Plugins
I do create pre-build releases for each version of the plugin that I tag. The latest release can be obtained from the "Releases" page. Read the README.txt file associated with the release for information on how to install the plugin into the relevant Release version of Qt Creator.

Although I try to create binaries for the latest version of QtCreator, there is a bit of a delay from when a new version of QtCreator is released and a new version of the plugin is released.

## 3. Using The Plugin
After opening Qt Creator and the plugin loaded successfully the following steps must be performed before starting to use the SpellChecker plugin:
1. Make sure the plugin is enabled
   - Go to "*Help*" -> "*About Plugins...*" and make sure the SpellChecker plugin under Utilities is enabled. 
1. Set the Spell Checker
   - Go to "*Tools*" -> "*Options...*" 
   - In the Options page, go to the "*Spell Checker*" options page.
   - On the "*SpellChecker*" tab, select the required Spell Checker in the dropdown box. 
      Currently only the Hunspell Spell Checker will be available, but perhaps in future more might be added. 
   - Set the "*Dictionary*" and "*User Dictionary*" paths for the spell checker to use. <br>
      - English Dictionaries can be downloaded from: http://cgit.freedesktop.org/libreoffice/dictionaries/tree/en
      - Both the *.dic and *.aff files for the selected dictionary must be downloaded to the same folder.
      - **NB**: Make sure to use the `"plain"` link on the above page when downloading the dictionary files and not the file names directly.  
      - The *User Dictionary* is a custom file used to store words added to the dictionary of the spell checker. If such a file does not exist, the plugin will create the file for this purpose. The plugin will attempt to create this file in the User Resource path. On Windows this is in `%APPDATA%\QtProject\` and on Linux this is in `~/.config/QtProject/`. 
   - After setting the Spell Checker, restart QtCreator.
1. Set the Parser Options
   - For the different available parsers there will be tabs with the settings for the parser. 
      Currently there is only one parser available, a C++ Parser. Change its settings according to what is required. For more information
      regarding this parser, refer to section 5. 
1. Get commenting
    - While typing comments or opening a C++ text editor, the plugin will parse the comments and check words. If a spelling mistake is made the spell checker will add it to the output pane at the bottom of the page. There a user can perform the following actions on a misspelled word:
      * **Give Suggestions**: The spell checker will suggest alternative spellings for the misspelled word. The user can then replace the misspelled word with a suggested word. 
      * **Ignore Word**: The word will be ignored for the current Qt Creator instance. Closing and opening Qt Creator will once again flag the word as a spelling mistake
      * **Add Word**: The word will be added to the user dictionary and will be not again be marked as a spelling mistake until the word is manually removed from the user dictionary. 
      * **Feeling Lucky**: The word will be replaced with the first suggestion for the word. This option will only be available if there are at least one suggestion for the word.
   - Right clicking on a misspelled word will also allow the user to perform the above actions using the popup menu.
   - Under "*Tools*" -> "*Spell Checker*" the above actions can also be performed.

## 4. Useful Widgets
The following useful widgets are added to the QtCreator user interface to allows the user to interact with the plugin:
- **Output Pane** at the bottom of the IDE that shows the number of mistakes in the current editor, the misspelled words as well as suggestions for the words. From the pane there are controls to handle the mistakes.
- **Navigation Widget** that can be added that shows all documents that have mistakes along with the number of mistakes on that page. Note that this widget will only show parsed files based on the "Only check current editor" setting of the plugin.
- **Give Suggestions Widget** will give the user the option to replace all occurrences of a mistake in the current file with the specified word. 

## 5. Settings
The following settings are available to the plugin
### 5.1. Parse current file vs current project
On the "SpellChecker" tab of the Spell Checker Options page is a setting "Only check current editor". If this setting is set the plugin will only parse the current open editor, reparsing it when changes are made. The results of parsed files will be remembered and still get listed in the Navigation Widget when a new file is opened. 

When this setting is not set the plugin will parse all files in the project when a new project is switched to. For large projects this might take a bit of time to parse all files in the project. This has been successfully tested with the QtCreator sources. 
### 5.2. Projects to ignore
A list of projects that will not be checked for spelling mistakes if opened, even of the setting is enabled to scan complete projetcs. 
### 5.3. C++ Document Parser
The C++ parser can be configured to parse only Comments, only String Literals or both. 

The parser also has settings that affects how the following types of words will be handled:
- Email Addresses
- Qt Keywords
- Words in CAPS
- Words containing numb3ers
- Words\_with\_underscores
- CamelCase words
- Words that appear in source
- Words.with.dots
- Website Addresses

Apart from these settings, the plugin also attempts to remove Doxygen Tags in Doxygen comments, in an effort to reduce the number of false positives. 

## 6. Building The Plugin
As is, the plugin requires the Qt Creator source files as well as the Hunspell spell checker library. The next section describes how to get and compile Hunspell.

It is assumed that the Qt Creator source files are already downloaded to the system. <br>
For now all steps are done on Windows using MinGW, but as time and testing continues the steps and tests will include other compilers and operating systems. 
For the initial version Qt 5.4.0 was used along with QtCreator 3.3.

### 6.1. Setting up Hunspell 1.3.2
[Hunspell](http://hunspell.sourceforge.net/) is an Open Source spell checker used in many Open Source applications. 

#### 6.1.1. MinGW
For MinGW the hunspell-mingw repository on github was used to build hunspell. This was the easiest solution to get the lib ready for MinGW. 
1. Get the sources
   - Download the sources using the zip archive or git from https://github.com/zdenop/hunspell-mingw
1. Build the sources using the steps given on the above page
   - In a command window, navigate to the folder where the sources are located and run qmake and then run mingw32-make

#### 6.1.2. MSVC
1. Get the sources
   - Download the sources for 1.3.2 from http://sourceforge.net/projects/hunspell/files/Hunspell/1.3.2/
1. Build the sources
   - Open the hunspell.sln file in the folder src\win_api using Visual Studio
   - Build the debug\_dll and release\_dll of the libhunspell project

#### 6.1.3. GCC (Linux)
On Linux, make sure that libhunspell-1.3.* and libhunspell-dev is installed. This should be all that is needed regarding Hunspell. 

### 6.2. Building The Plugin
The following steps can be used to build the plugin for a local build of Qt Creator. If the idea is to use the plugin with one of the release versions of Qt Creator, make sure that the compiler and Qt Creator sources match exactly with the setup for the release version of Qt Creator. If this is not possible, the plugin will not be usable with a Qt Creator release version. 
1. Get the Sources
   - Download the sources from the github project page
1. Set up the paths to the required locations
   - Rename the spellchecker_local_paths.pri.example to spellchecker_local_paths.pri
   - Update the renamed file and make sure that all of the variables point to the correct locations. Read the comments in the example for more information
1. Build the plugin
   - Open the pro file of the plugin in Qt Creator
   - Build the plugin. If Hunspell was build correctly and the local paths was set up correctly, the plugin should build correctly.
1. Run Qt Creator
   - Make sure that the correct Hunspell DLL can be obtained by either adding the path to it to the system path or by copying the correct dll to the Qt Creator folder.
   - Run Qt Creator. If everything was done correctly, Qt Creator should open and load the plugin without any problems.
1. Verify the plugin has loaded
   - In the running Qt Creator, go to "*Help*" -> "*About Plugins...*". Under Utilities "*SpellChecker*" should be listed and enabled (enable it if it was not enabled).

## TODO
The following list is a list with a hint into priority of some outstanding tasks I want to do. 
- [x] Get correct Qt Versions to make deployment versions and upload somewhere (Under Releases). 
- [x] Get all spelling mistakes for a active project. The idea was to 1st finish this before releasing, this has changed to start to track the code and get it into a repository. 
- [x] Underline words that are spelling mistakes (red squiggly lines, did attempt once, but did not work).
- [ ] Parse and ignore website URLs correctly. (Some work done on this but needs more testing/tweaks)
- [x] Test in other OS's (Linux, etc.)
  - [x] Make releases for other OS's.
- [x] Spell check string literals
- [x] Update to Hunspell 1.3.3


## About Pillar and Microdown

Pillar was the name of both an editorial compilation chain (think of as text as input produces html, LaTeX as output) and a textual format. 
Nowadays Pillar is just the name of the compilation chain and Microdown is the format of the input texts.




### Actions 

Pillar supports multiple actions. 
Here is a list 

- `archetype` will install a given template in the current repository
- `build` will build the output in the format specified (either html or pdf)
- `referenceCheck` will produce a report on the anchors and references - missing, duplicate, unreferenced anchors are reported.
- `codeCheck` will produce a report to mention the code that are out of sync. See below for a better explanation.



#### Pillar help
We are about to revisit the command line of Pillar but here is the current situation. 

```
pillar --help
Pillar is a markup syntax and associated tools to write and generate documents lie books in PDF, websites in html, or slides

Usage: pillar [--help] [--version]

Options:
    --help      Prints this documentation
    --version   Prints the version of your Pharo VM

Commands:
    archetype   Create a pillar project in the current directory, which contains a basic template and skeleton pillar files following the archetype convention
    build       Export format not found in pillar.config. Please edit it and add a valid export format at "defaultExport" label
    codeCheck   Check code in all the codeblocks in given file or book, then generate a report
    generateTests
                Generate tests from examples in the codeblocks of Pillar file(s)
    serve       Serve of the current project
    updateBuild
                Update build/archetypes using folder in parent directory
    updateTemplate
                Update a given template
    referenceCheck
                Check the duplicated or unknown references as well as missing input files
    convertAlternateSlide
                Create a microdown slide file in the current directory, which contains the conversion in microdown format of the argument.
    convertBook
                Create a microdown book in the current directory, which contains the conversion in microdown format of the argument.
    convertChapter
                Create a microdown book chapter in the current directory, which contains the conversion in microdown format of the argument.
    convertSlide
                Create a microdown slide file in the current directory, which contains the conversion in microdown format of the argument.
```


## About Sync

When you write technical documentation you often need to refer to the implementation of a given class or method. 
Usually this is easy, you copy and paste the code of the method.

Now the problem occurs when 





## About reference checker

```
pharopillarImage.image clap pillar referenceCheck index.md
```

```
File index.md has reference problems.

## Duplicated Anchors:

	Anchor sec:references is duplicated in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part0-Preamble/1-ObjectStructure/objectStructure.md
	Anchor fig:classes is undefined in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part0-Preamble/1-ObjectStructure/objectStructure.md
	Anchor fig:runtime is undefined in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part0-Preamble/0-RuntimeSystemOverview/runtime.md
	Anchor Ephemerons is undefined in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part3-MemoryManagement/GarbageCollector/ephemerons.md
	Anchor objectHeader is undefined in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part3-MemoryManagement/GarbageCollector/ephemerons.md
	Anchor specialObjectArray is undefined in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part3-MemoryManagement/GarbageCollector/ephemerons.md


## Undefined Anchors:

	Anchor sec:references is duplicated in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part0-Preamble/1-ObjectStructure/objectStructure.md
	Anchor fig:classes is undefined in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part0-Preamble/1-ObjectStructure/objectStructure.md
	Anchor fig:runtime is undefined in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part0-Preamble/0-RuntimeSystemOverview/runtime.md
	Anchor Ephemerons is undefined in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part3-MemoryManagement/GarbageCollector/ephemerons.md
	Anchor objectHeader is undefined in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part3-MemoryManagement/GarbageCollector/ephemerons.md
	Anchor specialObjectArray is undefined in file: /Users/ducasse/Workspace/FirstCircle/MyBooks/Bk-Writing/PharoBooks2/Booklet-PharoVirtualMachine/Part3-MemoryManagement/GarbageCollector/ephemerons.md
```


## About Pillar and Microdown

Pillar was the name of both an editorial compilation chain (think of as text as input produces html, LaTeX as output) and a textual format. 
Nowadays Pillar is just the name of the compilation chain and Microdown is the format of the input texts.

Pillar is an architecture to produce documents. Depending on the kind of documents produces it will schedule different tasks. 

In the past Pillar defined visitors on Pillar ASTs to perform associated treatments (generate HTML, LaTeX). Nowadays such behavior got migrated to Microdown itself. 


### Pillar actions 

Pillar supports multiple actions. 
Here is a list 

- `archetype` will install a given template in the current repository
- `build` will build the output in the format specified (either html or pdf)
- `referenceCheck` will produce a report on the anchors and references - missing, duplicate, unreferenced anchors are reported.
- `codeCheck` will produce a report to mention the code that are out of sync. See below for a better explanation.
- `convert*` convert book, chapter, slides from Pillar to Microdown. 

We are about to revisit all the command line implementation and exposition to the user. 

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
    check   Check code in all the codeblocks in given file or book, then generate a report
    serve       Serve of the current project
    updateBuild
                Update build/archetypes using folder in parent directory
    updateTemplate
                Update a given template
```


## About tests

Microdown document can also include the possibility to test whether a given expression returns a correct value. 
For example the following example shows that the writer can ensure that the execution of `3+ 4` is really given 7.
Here the example is trivial but it shows that we can make sure that examples in books are up to date.


```
	```example=true
	3 + 4 
	>>> 7
	```
```

The following one expresses that the code snippet should raise an error.

```
	```example=true&expectedFailure=true
	1 / 0 
	>>> 12		
	```
```


```
pharo pillarImage.image clap pillar codeCheck index.md
```

The class `MicBookTesterVisitorTest` defines several tests covering this feature.


## About reference checker

The reference checker reports the following problems:
- unreferenced anchor
- missing anchors
- duplicated anchors

It starts from a document and analyses only the documents that can be reached from this first document.
It means that you can have files with draft contents without getting impacting by the reference checker. 

```
pharo pillarImage.image clap pillar check index.md
```

Note that the shell script will change in the future but its essence will stay the same. 

Here is a possible output. 

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

The subclasses of class `MicFileTest` define several tests covering such behavior. 



## About Sync

When you write a technical documentation you often need to refer to the implementation of a given class or method. 
Usually this is easy, you copy and paste the code of the method.

Now the problem occurs when the code of the library changes. In this case you get an obsolete documentation and it is difficult to know what changed. Note that using the life feature that automatically displays a method or class body does not solve the problem. Because there may be other texts referring to the code snippets that get inconsistent. 

Microdown offers sync features

```
    ```sync=true&origin=MicMethodBodySyncTest>>#simpleCode'.
	methodDef := 
'simpleCode
	"This is not the definition of simpleCode"
	
	^ 100 slowFactorial + 100'
     ```
```

The class `MicMethodBodySyncTest` provides tests for such functionality.



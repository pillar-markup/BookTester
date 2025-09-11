Microdown is a markup language. It integrates with the pillar compilation chain.

It is the basis for
- Slides 
- Books in PDF
- Books on the web
- Documentation 
- Web page generation

We will learn about the Visitor, document model, optionally parsing.
We want to improve the testing of the books and the support for the book writers.

Microdown is a markup language compatible with a subset of markdown. 
It is used by the Pharo community to produce slides, booklets, and documentation. 
Microdown heavily uses Visitors. 
The repository is at 

```
https://github.com/pillar-markup/microdown
```

### Loading

Pay attention to follow the loading instructions to **develop** Microdown as defined on the readme on the project. You should execute them else you may have problems since a different version of Microdown 
preloaded in Pharo may conflict with the version you will load. 


## Getting started

### Model
Have a look at the document model
- What is the root of a document?
- What is the class of the object that represents code block?
- What is the class that represents italic font?


### From text to objects

- Read the tests to understand how to get an object representing the text?
- Turn the following text into a document object

We will reuse the following text in other exercises. So we store it into a variable. 
```
miniDoc := '
# Microdown 

Microdown is a super cool markdown description language.
It supports basic markdown but also extensions that are important to write books.


# Architecture

Microdown proposes visitors to 

- Export in LaTeX, HTML
- Dump the document tree as objects
- Checkers

## Visitors

Visitors just visit the structure tree and perform the corresponding actions. 

## A builder

A builder proposes an API to produce Microdown instructions without manipulating text directly. 
It decouples the structure from the actual textual representation. It allows developers to script documentation. 
'
```

- Check that the root of the document is effectively an instance of the class you identified earlier. 
- Navigate some children of the root. 


### Check the Visitors	

Check the subclass of the MicrodownVisitor

### Builder Programmatically writing a textual document

While visitors often go from objects to textual representations, we often need to generate textual 
representations of the manipulated document objects.

We could also simply manipulate strings and concatenate them by hand. This, however, would expose
our program to any simple language evolution. Identify the solution proposed by Microdown. 



## Topic: Support for hiding corrections

Imagine the file Chapter1/File1.md 

```
		```exercice
               Foo>> bar
					some code
        ```
```

We want that the document is transformed into 

```
## Chapt
@Chap1

		```exercice
               Foo>> bar
			… Your code …
              ```
```

and that another file `Chapter1/File1Solution.md` is generated with the 	

```
  	 	Here is the solution of the code *@Code1@* in Chapter *@Chap1@*
  		```
        Foo>> bar
			some code
        ```

```

## Topic: Better Life and Sync functionality

Microdown got support the possibility to generate in the output the definition of classes or methods without copy pasting it. 

The following definition 

```
	```life=true|OrderedCollection >> #add:
	```
```
will include the definition of the method `OrderedCollection>>#add:` that is actually used to produce the book or document. 

- We should verify that this is still the case.
- Check the strategy architecture subclasses of `MicBookTesterStrategy`
- We should reintroduce the life feature
- Read the tests defined in MicMethodBodySyncTest


Microdown supports the possibility to verify whether a given piece of code (method or class definition) 
is in sync with the actual version of the code. 



## Topic: Book web publication

We would like to have different css style for books. We need a simpler way to do book publication on the web
for example http://books.pharo.org/booklet-AMiniSchemeInPharo/html/book.html is generated from 
https://github.com/SquareBracketAssociates/Booklet-AMiniSchemeInPharo




## Topic: Book Sanitizer

The idea is to propose rules of language style to be followed. For example, making sure that there is no
“allows to do” but “allows one to do” in the text. 

When writing documents, we often have some writing guidelines. 

#### Guideline sample
Here are some guidelines about writing style and spelling

- Write "backend", not "back-end" or "back end".
- Write "subpresenter", not "sub-presenter".
- Methods without comment have an empty line after the method selector.
- Methods do not have a period on the last line.
- Write "Pharo image", not "Pharo Image.
- Do not use protocol references because they are not useful and may change.
- Write "Section 6.1", not "section 6.1".
- Write "Figure 6.2", not "figure 6.2".
- Caption should start with an uppercase and terminate by a period.
- Titles (section, chapter, ...) should just have the first letter capitalized.
```
### The great book.
```
- Figure caption should end with a period and be capitalized.
```
![The great figure.]()
```
- Class names should be surrounded by `
- Only use tab for code indentation
- For paragraphs terminate the label with a period
```
#### Cap. 
```

#### Job

- The author should declare a set of rules that should apply to the book
- In a first pass he can select a list of rule to be applied.
- We should identify a couple of rules:
	- Headers of section should not be capitalized
 	- Headers of section should be capitalized but not stop words
  	- The authors should be able to specify conversion list
  	- subpresenters should be used instead of sub-presenter sub presenters
  	- bytecode instead of byte code 	 
- The book sanitizer can report problems of the document tree to reflect them. 
- A subsequent version could change the files to reflect such change so that the user can save them.

A sanitizer should be configurable to take into account book guidelines.






## Topic:  Table of contents

We would like to have a table of content builder. Given a book, the Toc builder will generate a microdown document tree containing references to the corresponding book entities (chapter, section).

To achieve this exercise we propose several solutions. 

#### Generating a plain text TOC

Define a simple Visitor that will generate a text. For example

```
vis := SimpleTOCGenerator new.
vis visit: (Microdown parse: miniDoc)
vis contents
>

'
Microdown
Architecture
  Visitors
  A builder
'

#### Controlling the level

Now we can also want to only show sections whose nested in higher than a certain level.

```
vis := SimpleTOCGenerator new.
vis showOnlyAbove: 1.  
vis visit: (Microdown parse: miniDoc)
vis contents
>

'
Microdown
Architecture
'

#### Showing numbers

Now we may want to get the TOC numbered

```
vis := SimpleTOCGenerator new.
vis showOnlyAbove: 1.  
vis numbered.
vis visit: (Microdown parse: miniDoc)
vis contents
>

'
1 Microdown
2 Architecture
'

#### Producing Microdown

Now we would like to be able to produce Microdown text that represents the TOC.
This solution should use the textual builder.

```
vis := SimpleTOCGenerator new.
vis showOnlyAbove: 1.  
vis visit: (Microdown parse: miniDoc)
vis contents
>

'
# Microdown
# Architecture
'

## Topic: Book management enhancements

There is a first version of a book checker. It checks unreferenced figure, doubly defined anchors. The goal of the project is to verify that the checks are correcly
performed

- Automatic Numbering. By default the mapping between the number of ## in microdown and the latex level (section or subsection) is hardcoded and we would like to let the user expresses it needs. Here we should have a look at Pillar and its configuration to pass the configuration specification to the printer. 

### Link checker

In Microdown, the writer can refer to figures and anchors as well as to web site as shown here after.

```
## A Section 
@anchor1

![A caption](figures/fig.png label=figanchor)

[ Pharo web site ](https://pharo.org)
https://pharo.org

In Section *@anchor1@* we can find Fig. *@figanchor@*.
```

We would like to have a checker that reports to the users the set of references (defined using the `*@xxx@*` instruction  that are not found. 







## Automatic Numbering 

When writing a book users do not want to be forced to write in the exact same level of nesting than the template interpretation. For example, in this book # is to represent a LaTeX part, and ## for a chapter. However, it would possible to simply change the numbering so that a writer can write # for title and to use meta data at the level of the file to control this. 

```
{  "nesting":0 }
```

could mean that # is for a chapter title, ## a section, ### a paragraph

Conversely 

```
{  "nesting":1 }
```
could mean that ## is for a chapter title, ### a section,  


## Topic: Rendered math expression downloader

For web or in image rendering,  a math expression is rendered asking a web service to produce the LaTeX.  We would like to be able to cache such rendered png. The idea is that the math expressions should get a unique id and based on this idea the rendered png should be stored and used in place of the service request.

To render math in non latex mode, the microdown renderer in Pharo performs a request to an online service. A cool feature is to change the logic to do change the logic so that 


- After each request to the server, save the gif or png representing the render math expression with a unique name in the ressources folder of the current file directory. A possible name is to take two letters of path elements plus a counter. So the 3 expressions in foo/bar/ will be named foba3.png

- [optional] Change the math code to reflect that the corresponding rendered expression is available 

``` 
$$
    \frac{1}{2}
$$

```

into 

``` 
$$ % renderedAs=foba3.png
    \frac{1}{2}
$$

```

- Modify the system so that the request is only performed if there is no ressources for the given expression. When the ressources is found, it should use the corresponding rendered result graphic object.

### Better contents encoding

Note that computing the name based on the order of appareance in the text is not really robust to change. If the user inserts a new math expression this will invalidate the pre existing renderer. Propose a better system based on the expression contents for example the hash of a zip. 






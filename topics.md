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
for example http://books.pharo.org/booklet-AMiniSchemeInPharo/html/book.html is generated from https://github.com/SquareBracketAssociates/Booklet-
AMiniSchemeInPharo


## Topic: Book management enhancements

There is a first version of a book checker. It checks unreferenced figure, doubly defined anchors. The goal of the project is to verify that the checks are correcly
performed

- Automatic Numbering. By default the mapping between the number of ## in microdown and the latex level (section or subsection) is hardcoded and we would like to let the user expresses it needs. Here we should have a look at Pillar and its configuration to pass the configuration specification to the printer. 

## Topic: Book Sanitizer

The idea is to propose rules of language style to be followed. For example, making sure that there is no
“allows to do” but “allows one to do” in the text. 

## Topic: Rendered math downloader

For web or in image rendering,  a math expression is rendered asking a web service to produce the LaTeX.  We would like to be able to cache such rendered png. The idea is that the math expressions should get a unique id and based on this idea the rendered png should be stored and used in place of the service request.

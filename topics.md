Microdown is a markup language. It integrates with the pillar compilation chain.

It is the basis for
- Slides 
- Books in PDF
- Books on the web
- Documentation 
- Web page generation

We will learn about the Visitor, document model, optionally parsing.
We want to improve the testing of the books and the support for the book writers.

## Repository
http://github.com/pillar-markup/microdown is the general repository of Microdown.
We have some bug to fix and extensions.

## Possible ideas for improvement

The following list presents some ideas 

### Some little appetizers

The chapter 17 on Microdown in the book https://github.com/SquareBracketAssociates/Booklet-AdvancedMicroProjects describes different possible projects



### Support for hiding corrections


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

### Better Life and Sync functionality

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



### Book web publication

We would like to have different css style for books. We need a simpler way to do book publication on the web
for example http://books.pharo.org/booklet-AMiniSchemeInPharo/html/book.html is generated from https://github.com/SquareBracketAssociates/Booklet-
AMiniSchemeInPharo


### Book management enhancements

There is a first version of a book checker. It checks unreferenced figure, doubly defined anchors. The goal of the project is to verify that the checks are correcly
performed

- Automatic Numbering. By default the mapping between the number of ## in microdown and the latex level (section or subsection) is hardcoded and we would like to let the user expresses it needs. Here we should have a look at Pillar and its configuration to pass the configuration specification to the printer. 

### Book Sanitizer

The idea is to propose rules of language style to be followed. For example, making sure that there is no
“allows to do” but “allows one to do” in the text. 

### Rendered math downloader

For web or in image rendering,  a math expression is rendered asking a web service to produce the LaTeX.  We would like to be able to cache such rendered png. The idea is that the math expressions should get a unique id and based on this idea the rendered png should be stored and used in place of the service request.

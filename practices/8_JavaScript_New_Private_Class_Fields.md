# JavaScript’s New Private Class Fields, and How to Use Them

ES6 introduced classes to JavaScript, but they’re too simplistic for complex
applications. Class fields (also referred to as class properties) aim to deliver
simpler constructors with private and static members. The proposal is
currently at TC39 stage 3: candidate and could appear in ES2019 (ES10).

A quick recap of ES6 classes is useful before we examine class fields in more
detail.

## ES6 Class Basics

JavaScript’s prototypal inheritance model can appear confusing to developers
with an understanding of the classical inheritance used in languages such as
C++, C#, Java and PHP. JavaScript classes are primarily syntactical sugar, but
they offer more familiar object-oriented programming concepts.

A class is a template which defines how objects of that type behave. The
following Animal class defines generic animals (classes are normally denoted
with an initial capital to distinguish them from objects and other types):

pagina 88

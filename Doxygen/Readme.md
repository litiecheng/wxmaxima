About the doxygen documentation                         {#mainpage}
===============================

This is the documentation for the C++ code of wxMaxima. If you are looking
on information how to use the program instead you might want to consult
the online manual instead that is shipped with the program and
(after compilation of the program) can be found at
[info/wxmaxima.html](../../info/wxmaxima.html) in the directory the
contents of the tarball or the git repo the code is in.

How to get documentation for the code?
--------------------------------------

 - Install [Doxygen](http://www.stack.nl/~dimitri/doxygen/),
 - Optionally install [GraphViz](http://www.graphviz.org/)
   (it generates call graphs, inheritance graphs and caller graphs
   so it really makes the documentation more usable)
 - make sure these two utilities are in the search path of your
   system
   
 - cd into the [Doxygen](../) directory of the [wxMaxima sources](../../) and type:

        make

If your system is set up for compiling code and the project has
already been compiled on the current system (see the file
[INSTALL](../../INSTALL) for details) this should generate a file
named index.html that will redirect to the documentation.

Where to start reading?
-----------------------

This naturally depends on what you want to archieve.
 - The worksheet is mostly handled by the class MathCtrl
 - All the cells that are displayed in the worksheet and all the sub-
   cells that represent individual elements inside one of these cells
   are child objects of MathCell. This object also contains most of
   the logic that allows to connect the individual cells to a list
   as well as the pointers MathCell::m_previous and MathCell::m_next.
 - The undo buffer and the conversion of cells into characters for
   drag-and-drop or saving files are handles by the class EditorCell
 - The menus are generated by wxMaximaFrame and most of the menu actions
   are handled by the class wxMaxima.
One of the most important concepts that are important to know is that
everything that is displayed in 

Things you have to know
-----------------------
 - C++ provides the magic that allows to attach objects of all imaginable
   cell types to a list of the type MathCell *
 - MathCell provides a second list (MathCell::m_nextToDraw and
   MathCell::m_previousToDraw that represents the order the objects
   are displayed in: Some things like Fractions and text contained
   between parenthesis might be layouted differently when they contain
   a line break.
 - There are several ways child objects from MathCell can form trees:
   - Some objects like sqrt() or a pair of parenthesis might contain
     a list.
   - Super- and subscripts are lists.
   - GroupCells allows to move its childs to a separate list that isn't
     displayed (which enables folding of chapters).
 - Methods that operate on lists typically start processing the list from
   the element they were called from. This means that they only will work
   as expected (in other words: process the complete list) only if they
   are called for the first element of the list.
   wxMaxima currently makes sure that this requirement is met by treating
   the first element of a list as the list itself.
   Searching the first element of a given list would be simple, though:

        MathCell *ListStart=this;
        while(ListStart->m_previous) ListStart = ListStart->m_previous;

Naming rules
------------

Keeping the code more or less homogenous increases the readability. In
order to archieve that wxMaxima uses a few naming rules:
 - The names of member variables are prefixed with "m_" for "member".
 - The names of member functions (aka methods) are written in CamelCase.
 - The names of enums do not really matter as they are rarely used and if
   they are they are used in context where it is opvious that they name an
   enum type so there aren't any rules for the names of enums right now.
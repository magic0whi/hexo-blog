---
title: vim-usage
lang: zh-cn
category: computer
date: 2020-09-05 13:39:28
tags:
---

<!-- more -->

u to undo
U to restore whole line(this action is not undo, and is undoable)
Ctrl-R redo

:sp[lit]  horizontally split current screen into two windows (with same filebuffer)
:vs[plit]  vertically split current screen into two windows (with same filebuffer)
CTRL-W + <h, j, k, l>  jump to another window.

d$ delete to end
x delete one char
dw delete one word(and ' ')
de delete one word(without ' ')
d3w delete three words
dd delete one line
d2d delete two line

o  open a line BELOW the cursor and start Insert mode.
O   open a line ABOVE the cursor.
a  insert text AFTER the cursor.
A  insert text after the end of the line.
y  yanks (copies) text
p  puts (pastes) it(copied or deleted text).
R  Replace mode until  <ESC>  is pressed.

rx  to replace one char with "x"
ce  change until the end of a word
c$  change until the end of line

ctrl+g to show your location and the file status
G  to move to the bottom of the file
501G  to move to the 501 lines of the file
gg  to move to the start of the file

v  (visual) select text, then you can type  d  to delete text or save to file with  :w FILENAME

% press "%" on a bracket can fast move the curser to the matching bracket
NOTE: This is very useful in debugging a program with unmatched parentheses!

/ typing / followed by a phrase searches FORWARD for the phrase.
? Typing  ?  followed by a phrase searches BACKWARD for the phrase.on
n to find the next occurrence in the same direction
N to search in the opposite direction
CTRL-O takes you back to older positions, CTRL-I to newer positions.
NOTE: When the search reaches the end of the file it will continue at the
      start, unless the 'wrapscan' option has been reset.

Typing ":set xxx" sets the option "xxx".  Some options are:
        'ic' 'ignorecase'       ignore upper/lower case when searching
        'is' 'incsearch'        show partial matches for a search phrase
        'hls' 'hlsearch'        highlight all matching phrases
Prepend "no" to switch an option off:   :set noic

:help  or press <F1>  to open a help window.
:help cmd  to find help on  cmd 

:e ~/.vimrc Create a vimrc startup script to keep your preferred settings.
Now read the example "vimrc" file contents: 
        :r $VIMRUNTIME/vimrc_example.vim
When typing a  :  command, press CTRL-D to see possible completions.
     Press <TAB> to use one completion.

command:
:s/old/new  to substitute 'new' for 'old'
:s/old/new/g  to substitute 'new' for 'old' all occurrence of "thee" in the line
:#,#s/old/new/g    where #,# are the line numbers of the range of lines where the substitution is to be done.
:%s/old/new/g      to change every occurrence in the whole file.
:%s/old/new/gc     to find every occurrence in the whole file, with a prompt whether to substitute or not.

:!xxxxx  executes an external command (such as :!ls)
:w FILENAME  writes the current Vim file to disk with name FILENAME
:r FILENAME  retrieves disk file FILENAME and puts it below the cursor position
:r !xxx  read the output of the command (such as :r !ls)

Explain:
d is delete operator

motion:
w - to the start of the next word
e - to the end of the current word
$ - to the end of the line


   ** Typing a number before a motion repeats it that many times. **

  1. Move the cursor to the start of the line below marked --->.

  2. Type  2w  to move the cursor two words forward.

  3. Type  3e  to move the cursor to the end of the third word forward.

  4. Type  0  (zero) to move to the start of the line\

dd Due to the frequency of whole line deletion, the designers of Vi decided
  it would be easier to simply type two d's to delete a line.
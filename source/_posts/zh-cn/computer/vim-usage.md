---
title: vim-usage
lang: zh-cn
category: computer
date: 2020-09-05 13:39:28
tags:
toc: true
---

Learn Vim - Shart Smart Way

Here comes a cheatsheet

<!-- more -->

`<s-a>` means `Shift+A`
`<c-a>` means `Ctrl+A`

TODO: works to be merged
`<c-o,i>`, Jump old / newer position.
`<c-^>` Go to previously edited buffer.
`:tag`    Jump to tag definition

## Ch 02 - Buffers, Windows, and Tabs

### Buffers

| `:bnext` / `:blast` / `:bfirst` / `:blast` | Traverse buffers                 |
|--------------------------------------|----------------------------------|
| `:qall` / `:wqall`                     | Close/Save close                |
| `:buffers` / `:ls` / `:files`            | List buffers                     |
| `:bdelete`                           | Remove current buffer                    |
| `:ball` / `:vertical ball`                              | display all buffers horizontally / vertically |

### Windows: A window is how you are viewing a buffer through

| `:split [filename]` / `:vsplit [filename]` | Split window                          |
|------------------------------------------|---------------------------------------|
| `<c-w> h`,`j`,`k`,`k`                      | Navigate                              |
| `<c-w> v`,`s`                          | Opens a new vertical/horizontal split |
| `<c-w> c`                                  | Closes a window                       |
| `<c-w> o`                                  | Closes all other windows              |

### Tabs: A tab is a collection of windows

| `:tabnew [filename]` | Open a new tab     |
|----------------------|--------------------|
| `:tabclose`          | Close current tab  |
| `:tabnext` / `:tabprev`           | Go to next / previous tab     |
| `:tablast` / `:tabfirst`          | Go to last / first tab     |

## Ch03. Searching Files

### Searching in Files With Grep

| `:find` / `:edit` | `:find` finds file in path, `:edit` doesn't. Using `:set path+=<your-path-here>` to set. |
|---|---|
| 1. `:vim /pattern/ file` <br> 2. `:grep -R "pattern"  /path/to/search/` | Vim's search uses Quickfix Window. |
| `:copen` / `:cclose` | Open/Close the quickfix window |
| `:cnext` / `:cprev` | Go to the next / previous error |
| `:colder` / `:cnewer` | Go to the older / newer error list |
| `:cfirst` / `:clast` | Go to the first / last item in error list |

### Browsing Files With Netrw

| `:Explore`  | Starts netrw on current file                             |
|-------------|----------------------------------------------------------|
| `:Sexplore` | No kidding. Starts netrw on split top half of the screen |
| `:Vexplore` | Starts netrw on split left half of the screen            |
| `%`         | Create a new file                                        |
| `d`         | Create a new directory                                   |
| `R`         | Rename a file or directory                               |
| `D`         | Delete a file or directory                               |

### Fzf

Fzf syntax:
* `^` prefix exact match. e.g. `^welcome`.
* `$` suffix exact match. e.g. `friends$`.
* `'` exact match. e.g. `'welcome my friends`.
* `|` "or" match. e.g. `friends | foes`.
* `!` inverse match. e.g. `welcome !friends`

| `:Files` or `<c-f>`      | Find files                                      |
|---------------------|-------------------------------------------------|
| `:Buffers` or `<\-b>`    | Find files in buffers                           |
| `:Rg` or `<\-f>`         | Find in files context                           |
| `:BLines` or `<\-/>`     | Find lines in current buffer                    |
| `:Marks` or `<\-'>`      | Find marks                                      |
| `:Commits` or `<\-g>`    | Find git commits                                |
| `:Helptags` or `<\-H>`   | Find help tags                                  |
| `:History` or `<\-h> h`  | Find in opened files' history (list `:oldfiles`) |
| `:History:` or `<\-h> :` | Find in command history                         |
| `:History/` or `<\-h> /` | Find in search history                          |

#### Search and Replace in Multiple Files

First method
```vim
:grep "pizza"
:cfdo %s/pizza/donut/g | update
```

Second method
```vim
:%bd | e#    "Close all buffer except current file.
:Files       "Select all files you want
:bufdo %s/pizza/donut/g | update
```

## Ch04. Vim Grammar

### Grammar Rule

There is only one grammar rule in Vim language: `Verb (Operator) + Noun (Motion)`

| `y`   | Yank text (copy)                                     |
|-------|------------------------------------------------------|
| `d`   | Delete text and save to register                     |
| `c`   | Delete text, save to register, and start insert mode |
| `yy`  | yank entire line                                     |
| `dd`  | delete entire line                                   |
| `cc`  | change entire line                                   |
| `y$`  | yank to the end of the line                          |
| `dw`  | delete to the beginning of the next word             |
| `c}`  | change to the end of paragraph                       |
| `y2h` | yank two characters to the left                      |
| `d2w` | delete the next two words                            |
| `c2j` | change the next two lines                            |

### More Nouns (Text Objects)

Two types of text objects:
* `i + object`    Inner text object, select without the white space or the surrounding objects
* `a + object`    Outer text object, select including the white space or the surrounding objects

e.g. `gUiw` to uppercase current word.

Common text objects:
| `w`                                            | A word                            |
|------------------------------------------------|-----------------------------------|
| `p`                                            | A paragraph                       |
| `s`                                            | A sentence                        |
| `t`                                            | XML tags                          |
| `(` / `{` / `[` / `<` or `>` / `]` / `}` / `)` | A pair of `( )` / `{ }` / `[ ]` / `< >` |
| `"` / `'` / `` ` ``                            | A pair of `" "` / `' '` / `` ` ` ``   |
| `%` |   Navigate to another match, usually works for `()`, `[]`, `{}` |

## Ch05. Moving in a File

### Character Navigation

A word is a sequence of characters containing *only* `a-zA-Z0-9_`. A WORD is a sequence of all characters except white space (a white space means either space, tab, and EOL).

| `gj` / `gk` | Down/Up in a soft-wrapped line                         |
|-----------|--------------------------------------------------------|
| `w` / `W`   | Move forward to the beginning of the next word/WORD    |
| `e` / `E`   | Move forward one word to the end of the next word/WORD |
| `b` / `B`   | Move backward to beginning of the previous word/WORD   |
| `ge` / `gE` | Move backward to end of the previous word/WORD         |

### Current Line Navigation

| `0`       | Go to the first char                                      |
|-----------|-----------------------------------------------------------|
| `^`       | Go to the first nonblank char                             |
| `$`       | Go to the last char                                       |
| `g_`      | Go to the last non-blank char                             |
| `n\|`     | Go the column n                                           |
| `f` / `F` | Search forward / backward for a char                        |
| `t` / `T` | Search forward / backward for a char, stopping before match |
| `;`       | Repeat the last char search (same direction)              |
| `,`       | Repeat the last char search (opposite direction)          |

### Sentence and Paragraph Navigation

A sentence ends with either `. ! ?` followed by an EOL, a space, or a tab.
A paragraph begins after each empty line and also at each set of a paragraph macro specified by the pairs of characters in paragraphs option.

| `(` / `)`   | Jump to the previous / next sentence  |
|-------------|---------------------------------------|
| `{` / `}`   | Jump to the previous / next paragraph |
| `[[` / `]]` | Jump to the previous / next section   |

### Line Number Navigation

| `gg`    | Go to the first line      |
|---------|---------------------------|
| `G`     | Go to the last line       |
| `nG`    | Go to line n              |
| `n%`    | Go to n% in file          |
| `<c-g>` | See total lines in a file |

### Window Navigation

| `H`  | Go to top of screen     |
|------|-------------------------|
| `M`  | Go to medium screen     |
| `L`  | Go to bottom of screen  |
| `nH` | Go `n` line from top    |
| `nL` | Go `n` line from bottom |

### Scrolling

| `<c-e,d,f>` | Scroll down a line / half screen / whole screen       |
|-------------|-------------------------------------------------------|
| `<c-y,u,b>` | Scroll up a line / half screen / whole screen         |
| `zt`        | Bring the current line near the top of your screen    |
| `zz`        | Bring the current line to the middle of your screen   |
| `zb`        | Bring the current line near the bottom of your screen |

### Search Navigation

| `/` / `?`           | Search forward / backward for a match                               |
|---------------------|---------------------------------------------------------------------|
| `n` / `N`           | Repeat last search in same / opposite direction of previous search  |
| `:noh` `<esc><esc>` | Turn off match highlights                                           |
| `*`                 | Search for whole word under cursor forward, same as type `/\<one\>` |
| `#`                 | Search for whole word under cursor backward                         |
| `g*`                | Search for word under cursor forward                                |
| `g#`                | Search for word under cursor backward                               |

### Marking Position

There is a difference between marking with lowercase letters (a-z) and uppercase letters (A-Z). Lowercase alphabets are local marks and uppercase alphabets are global marks (sometimes known as file marks).

| `ma`     | Mark position with mark `a`        |
|----------|------------------------------------|
| `` `a``  | Jump to exact position of mark `a` |
| `'a`     | Jump to line of mark `a`           |
| `:marks` | View all marks                     |

### Jump

Any motion that moves farther than a word and current line navigation is probably a jump.

| `''`      | Jump back to the last line in current buffer before jump     |
|-----------|--------------------------------------------------------------|
| ` `` `    | Jump back to the last position in current buffer before jump |
| `` `[``   | Jump to beginning of previously changed / yanked text        |
| `` `]``   | Jump to the ending of previously changed / yanked text       |
| `` `<``   | Jump to the beginning of last visual selection               |
| `` `>``   | Jump to the ending of last visual selection                  |
| `` `0``   | Jump back to the last edited file when exiting vim           |
| `:jumps`  | See jump list                                                |
| `<c-o,i>` | Move up / down the jump list                                 |
| `m'5j`    | Add current location to jump list, follows a move            |

## Ch06. Insert Mode

### Ways to Go to Insert Mode

| `i` / `I` / `gI`  | Insert text before the cursor / first non-blank character of the line / start of line |
|-------------------|---------------------------------------------------------------------------------------|
| `a` / `A`         | Append text after the cursor / end of line                                            |
| `o` / `O`         | Starts a new line below / above the cursor and insert text                            |
| `s` / `S` or `cc` | Delete the character / line under the cursor and insert text                          |
| `gi`              | Insert text in same position where the last insert mode was stopped                   |

### Different Ways to Exit Insert Mode

There are a few different ways to return to the normal mode while in the insert mode:
| `<esc>` | Exit insert mode and go to normal mode |
|---------|----------------------------------------|
| `<c-[>` | Exit insert mode and go to normal mode |
| `<c-c>` | Exit and do not check for abbreviation |

### Repeating Insert Mode

`10i`: Vim will repeat the text 10 times.

### Deleting Chunks in Insert Mode

| `c-h` | Delete one char    |
|-------|--------------------|
| `c-w` | Delete one word    |
| `c-u` | Delete entire line |

### Insert From Register

`c-r <register symbol>`

### Scrolling Inside Insert Mode

`<c-x> <c-y>`: Scroll up
`<c-x> <c-e>`: Scroll down

### Autocompletion

`<c-x> <c-l,n,i,f>`: Insert a whole line / a text from current file / a text from included files / a file name
`c-n` / `c-p`: Find the next / previous word match, or navigate up / down the pop-up completion menu

### Executing a Normal Mode Command

| `<c-o> zz`                         | Center window                                       |
|------------------------------------|-----------------------------------------------------|
| `<c-o> H`,`M`,`L`                  | Jump to top / middle / bottom of the window         |
| `<c-o> 'a`                         | Jump to mark a                                      |
| `<c-o> 100ihello`                  | Insert "hello" 100 times                            |
| `<c-o> !! curl https://google.com` | Run `curl`                                          |
| `<c-o> !! pwd`                     | Run `pwd`                                           |
| `<c-o> dtz`                        | Delete from current location till the letter `z`    |
| `<c-o> d`                          | Delete from current location to the end of the line |

## Ch07. the Dot Command

### What Is a Change?

Any time you update (add, modify, or delete) the content of the current buffer, you are making a change.
The exceptions are updates done by command-line commands (the commands starting with `:`) do not count as a change.

Every action from the moment you press the insert command operator until you exit the insert command is considered as a change.

### Multi-line Repeat

Let's look at another example:
```plaintext
zlet zzone = "1";
zlet zztwo = "2";
zlet zzthree = "3";
let four = "4";
```

Let's remove all the z's. Starting from the first character on the first line, visually select only the first z from the first three lines with blockwise visual mode (`Ctrl-Vjj`). If you're not familiar with blockwise visual mode, I will cover them in a later chapter. Once you have the three z's visually selected, delete them with the delete operator (`d`). Then move to the next word (`w`) to the next z. Repeat the change two more times (`..`). The full command is `Ctrl-vjjdw..`.

### Including a Motion in a Change

Replace all "let" with "const" in the following expressions:
```plaintext
let one = "1";
let two = "2";
let three = "3";
```
There is a faster way to accomplish this. After you searched `/let`, run `cgnconst<Esc>` then `. .`.

`gn` is a motion that searches forward for the last search pattern (in this case, `/let`) and automatically does a visual highlight.

## Ch08. Registers

The Ten Register Types
1. The unnamed register (`""`).
2. The numbered registers (`"0-9`).
3. The small delete register (`"-`).
4. The named registers (`"a-z`).
5. The read-only registers (`":`, `".`,and `"%`).
6. The alternate file register (`"#`).
7. The expression register (`"=`).
8. The selection registers (`"*` and `"+`).
9. The black hole register (`"_`).
10. The last search pattern register (`"/`).

### Register Operators

To use registers, you need to first store them with operators. Here are some operators that store values to registers:
```plaintext
y    Yank (copy)
c    Delete text and start insert mode
d    Delete text
p    Paste the text after the cursor
P    Paste the text before the cursor
10"ap  Paste text in register a ten times
```

### Calling Registers From Insert Mode

The syntax to call register a from insert mode is:
```plaintext
Ctrl-R a
```

### The Unnamed Register

It stores the last text you yanked, changed, or deleted.

By default, `p` (or `P`) is connected to the unnamed register.

### The Numbered Registers

#### The Yanked Register

If you yank an entire line of text (`yy`), Vim actually saves that text in two registers:

1. The unnamed register (`p`).
2. The yanked register (`"0p`).

Any other operations (like delete) will not be stored in register 0.

#### The Non-zero Numbered Registers

When you change or delete a text that is at least **one line long**, that text will be stored in the numbered registers 1-9 sorted by the most recent.

As a side note, these numbered registers are automatically incremented when using the dot command.

### The Small Delete Register

Changes or deletions less than one line are not stored in the numbered registers 0-9, but in the small delete register (`"-`).

### The Named Register

To yank a word into register a, you can do it with `"ayiw`.
- `"a` tells Vim that the next action (delete / change / yank) will be stored in register a.
- `yiw` yanks the word.

Sometimes you may want to add to your existing named register. In this case, you can append your text instead of starting all over. To do that, you can use the uppercase version of that register. For example, suppose you have the word "Hello " already stored in register a. If you want to add "world" into register a, you can find the text "world" and yank it using A register (`"Ayiw`).

### The Read-only Registers

```plaintext
.    Stores the last inserted text
:    Stores the last executed command-line
%    Stores the name of current file
```

### The Alternate File Register

In Vim, `#` usually represents the alternate file. An alternative file is the last file you opened. To insert the name of the alternate file, you can use `"#p`.

### The Expression Register

```plaintext
"=1+1<Enter>p
Ctrl-R =1+1    Evaluate mathematical expression from insert mode
```

You can also get the values from any register via the expression register when appended with `@`. If you wish to get the text from register a:
```plaintext
"=@a<Enter>p
```

Similarly, to get values from register a while in insert mode:
```plaintext
Ctrl-r =@a
```

### The Selection Registers

Vim has two selection registers: `quotestar` (`"*`) and `quoteplus` (`"+`). You can use them to access copied text from external programs.

If you yank a word from Vim with `"+yiw` or `"*yiw`, you can paste that text in the external program with `Ctrl-V` (or `Cmd-V`). Note that this only works if your Vim program comes with the `+clipboard` option (to check it, run `:version`).

### The Black Hole Register

You can use the black hole register (`"_`). To delete a line and not have Vim store the deleted line into any register, use `"_dd`.

### The Last Search Pattern Register

To paste your last search (`/` or `?`), you can use the last search pattern register (`"/`). To paste the last search term, use `"/p`.

### Viewing the Registers

To view all your registers, use the `:register` command. To view only registers "a, "1, and "-, use `:register a 1 -`.

There is a plugin called [vim-peekaboo](https://github.com/junegunn/vim-peekaboo) that lets you to peek into the contents of the registers when you hit `"` or `@` in normal mode and `Ctrl-R` in insert mode.

### Clearing a Register

- `qaq`
- `:call setreg('a', 'hello world')`
- `:let @a = ''`

### Putting the Content of a Register

```plaintext
:10put a    Paste text from register a to below line 10.
:g/end/put _    Insert blank lines below all lines contain the text "end", by using black hole register
```

## Ch09. Macros

### Basic Macros

Suppose you have this text and you want to uppercase everything on each line:
```plaintext
hello
vim
macros
are
awesome
```

With your cursor at the start of the line "hello", run:
```plaintext
qa0gU$jq
```

The breakdown:
- `qa` starts recording a macro in the a register.
- `0` goes to beginning of the line.
- `gU$` uppercases the text from your current location to the end of the line.
- `j` goes down one line.
- `q` stops recording.

To replay it, run `@a`. Just like many other Vim commands, you can pass a count argument to macros. For example, running `3@a` executes the macro three times.

### Command Line Macro

```plaintext
:2,3 normal @a    execute macro between lines 2 and 3, ':normal' allows the user to execute any normal mode command passed as argument.
```

### Executing a Macro Across Multiple Files

Suppose you have multiple `.txt` files, each contains some texts. Your task is to uppercase the first word only on lines containing the word "donut". Assume you have `0W~j` in register a (the same macro as before). How can you quickly accomplish this?

First file:
```plaintext
## savory.txt
a. cheddar jalapeno donut
b. mac n cheese donut
c. fried dumpling
```

Second file:
```plaintext
## sweet.txt
a. chocolate donut
b. chocolate pancake
c. powdered sugar donut
```

Third file:
```plaintext
## plain.txt
a. wheat bread
b. plain donut
```

Here is how you can do it:
- `:args *.txt` to find all `.txt` files in your current directory.
- `:argdo g/donut/normal @a` executes the global command `g/donut/normal @a` on each file inside `:args`.
- `:argdo update` executes `update` command to save each file inside `:args` when the buffer has been modified.

### Recursive Macro

You can recursively execute a macro by calling the same macro register while recording that macro. Suppose you have this list again and you need to toggle the case of the first word:
```plaintext
a. chocolate donut
b. mochi donut
c. powdered sugar donut
d. plain donut
```

This time, let's do it recursively. Run:
```plaintext
qaqqa0W~j@aq
```

Here is the breakdown of the steps:
- `qaq` records an empty macro a. It is necessary to start with an empty register because when you recursively call the macro, it will run whatever is in that register.
- `qa` starts recording on register a.
- `0` goes to the first character in the current line.
- `W` goes to the next WORD.
- `~` toggles the case of the character under the cursor.
- `j` goes down one line.
- `@a` executes macro a.
- `q` stops recording.

Now you can just run `@a` and watch Vim execute the macro recursively.

How did the macro know when to stop? When the macro was on the last line, it triedto run `j`, since there was no more line to go to, it stopped the macro execution.

### Appending a Macro

Record a macro in register a: `qa0W~q` (this sequence toggles the case of the next WORD in a line). If you want to append a new sequence to also add a dot at the end of the line, run:
```plaintext
qAA.<Esc>q
```

The breakdown:
- `qA` starts recording the macro in register A.
- `A.<Esc>` inserts at the end of the line (here `A` is the insert mode command, not to be confused with the macro A) a dot, then exits insert mode.
- `q` stops recording macro.

Now when you execute `@a`, it not only toggles the case of the next WORD, it also adds a dot at the end of the line.

### Amending a Macro

Suppose that between uppercasing the first word and adding a period at the end of the line, you need to add the word "deep fried" right before the word "donut" *(because the only thing better than regular donuts are deep fried donuts)*.

```plaintext
a. chocolate donut
b. mochi donut
c. powdered sugar donut
d. plain donut
```

First, let's call the existing macro with `:put a`:
```plaintext
0W~A.^[
```
You can't literally type `<Esc>`. You will have to write the internal code representation for the `<Esc>` key. While in insert mode, you press `Ctrl-V` followed by `<Esc>`. Vim will print `^[`. `Ctrl-V` is an insert mode operator to insert the next non-digit character *literally*. Your macro code should look like this now:
```plaintext
0W~$bideep fried ^[A.^[
```
At the start of the line, run `"ay$` to store the yanked text in register a.

### Macro Redundancy

You can easily duplicate macros from one register to another. For example, to duplicate a macro in register a to register z, you can do `:let @z = @a`. `@a` represents the content of register a.

### Series vs Parallel Macro

```plaintext
import { FUNC1 } from "library1";
import { FUNC2 } from "library2";
import { FUNC3 } from "library3";
import foo from "bar";
import { FUNC4 } from "library4";
import { FUNC5 } from "library5";
```

If you want to record a macro to lowercase all the uppercased "FUNC", this macro should work:
```platintext
qa0f{gui{jq
```

Running `99@a`, only executes the macro three times. It does not execute the macro on last two lines because the execution fails to run `f{` on the "foo" line. This is expected when running the macro in series. You can always go to the next line where "FUNC4" is and replay that macro again. But what if you want to get everything done in one go?

Run the macro in parallel.

Recall from earlier section that macros can be executed using the  command line command `:normal` (ex: `:3,5 normal @a` executes macro a on lines 3-5). If you run `:1,$ normal @a`, you will see that the macro is being executed on all lines except the "foo" line. It works!

## Ch10. Undo

### Breaking the Blocks

`Ctrl-G u` creates undo break in insert mode.

It is also useful to create an undo breakpoint when deleting chunks in insert mode with `Ctrl-W` (delete the word before the cursor) and `Ctrl-U` (delete all text before the cursor). A friend suggested to use the following maps:
```plaintext
inoremap <c-u> <c-g>u<c-u>
inoremap <c-w> <c-g>u<c-w>
```

With these, you can easily recover the deleted texts.

### Undo Tree

In Vim, every time you press `u` and then make a different change, Vim stores the previous state's text by creating an "undo branch". In this example, after you typed "two", then pressed `u`, then typed "three", you created an leaf branch that stores the state containing the text "two". At that moment, the undo tree contained at least two leaf nodes: the main node containing the text "three" (most recent) and the undo branch node containing the text "two". If you had done another undo and typed the text "four", you would have at three nodes: a main node containing the text "four" and two nodes containing the texts "three" and "two".

To traverse each undo tree nodes, you can use `g+` to go to a newer state and `g-` to go to an older state. The difference between `u`, `Ctrl-R`, `g+`, and `g-` is that both `u` and `Ctrl-R` traverse only the *main* nodes in undo tree while `g+` and `g-` traverse *all* nodes in the undo tree.

[vim-mundo](https://github.com/simnalamburt/vim-mundo) plugin is very useful to help visualize Vim's undo tree.

### Persistent Undo

```plaintext
:wundo file.undo  save undo file
:rundo file.undo  load undo file
```

If you want to have an automatic undo persistence, one way to do it is by adding these in vimrc:
```plaintext
set undodir=~/.vim/undo_dir
set undofile
```
make sure that you create the actual `undo_dir` directory inside `~/.vim` directory.

### Time Travel

You can use `:undolist` to see when the last change was made.

```plaintext
:earlier 10s    Go to the state 10 seconds before
:earlier 10m    Go to the state 10 minutes before
:earlier 10h    Go to the state 10 hours before
:earlier 10d    Go to the state 10 days before
:later 10s    go to the state 10 seconds later
:later 10m    go to the state 10 minutes later
:later 10h    go to the state 10 hours later
:later 10d    go to the state 10 days later
:later 10     go to the newer state 10 times
:later 10f    go to the state 10 saves later
```

## Ch11. Visual Mode

### The Three Types of Visual Modes

```
v         Character-wise visual mode
V         Line-wise visual mode
Ctrl-V    Block-wise visual mode
gv        Go to the previous visual mode.
```

### Visual Mode Navigation

With `o` or `O` in visual mode, the cursor jumps from the beginning to the end of the highlighted block, allowing you to expand the highlight area.

### Visual Mode Grammar

There is no noun in visual mode, select the text block first, then the command follows.

### Adding Text on Multiple Lines

```plaintext
const one = "one"
const two = "two"
const three = "three"
```

With your cursor on the first line:
- Run block-wise visual mode and go down two lines (`Ctrl-V jj`).
- Highlight to the end of the line (`$`).
- Append (`A`) then type ";".
- Exit visual mode (`<Esc>`).

Alternatively, you can also use the `:normal` command to add text on multiple lines:
- Highlight all 3 lines (`vjj`).
- Type `:normal! A;`.

### Incrementing Numbers

Vim has `Ctrl-X` and `Ctrl-A` commands to decrement and increment numbers. When used with visual mode, you can increment numbers across multiple lines.

```plaintext
<div id="app-1"></div>
<div id="app-1"></div>
<div id="app-1"></div>
<div id="app-1"></div>
<div id="app-1"></div>
```

- Move your cursor to the "1" on the second line.
- Start block-wise visual mode and go down 3 lines (`Ctrl-V 3j`). This highlights the remaining  "1"s. Now all "1" should be highlighted (except the first line).
- Run `g Ctrl-A`.

```plaintext
set nrformats+=alpha
```
By adding `alpha`, an alphabetical character is now considered as a number. If you have the following HTML elements:

```plaintext
<div id="app-a"></div>
<div id="app-a"></div>
<div id="app-a"></div>
<div id="app-a"></div>
<div id="app-a"></div>
```

## Ch12. Search and Substitute

### Smart Case Sensitivity

Vim has a `smartcase` option to search for case insensitive string if the search pattern *contains at least one uppercase character*.

Inside your vimrc, add:
```plaintext
set ignorecase smartcase
```

You can use `\C` pattern anywhere in your search term to tell Vim that the subsequent search term will be case sensitive. If you do `/\Chello`, it will strictly match "hello", not "HELLO" or "Hello".

### Repeating Search

You can repeat the previous search with `//`.

You can quickly traverse the search history by first pressing `/`, then press `up`/`down` arrow keys (or `Ctrl-N`/`Ctrl-P`)

### Repeating the Last Substitution

You can repeat the last substitute command with either the normal command `&` or by running `:s`. If you have just run `:s/good/awesome/`, running either `&` or `:s` will repeat it.

If `/good` was done recently and you leave the first substitute pattern argument blank, like in `:s//awesome/`, it works the same as running `:s/good/awesome/`.

### Substitution Range

Here are some range variations you can pass:

- `:,3s/let/const/` Substitute from current line to line 3.
- `:1,s/let/const/` Substitute from line 1 to current line.
- `:3s/let/const/` -it does substitution on line 3 only.
- In Vim, `%` usually means the entire file. If you run `:%s/let/const/`, it will do substitution on all lines.

In addition to numbers, you can also use these symbols as range:
- `.` means the current line. A range of `.,3` means between the current line and line 3.
- `$` means the last line in the file. `3,$` range means between line 3 and the last line.
- `+n` means n lines after the current line. You can use it with `.` or without. `3,+1` or `3,.+1` means between line 3 and the line after the current line.

### Substitution Flags

```plaintext
&    Reuse the flags from the previous substitute command.
g    Replace all matches in the line.
c    Ask for substitution confirmation.
e    Prevent error message from displaying when substitution fails.
i    Perform case insensitive substitution.
I    Perform case sensitive substitution.
```

By the way, the repeat-substitution commands (`&` and `:s`) do not retain the flags. To quickly repeat the last substitute command with all the flags, run `:&&`.

### Changing the Delimiter

When it is hard to tell which forward slashes (`/`) are part of the substitution pattern and which ones are the delimiters. You can change the delimiter with any single-byte characters (except for alphabets, numbers, or `"`, `|`, and `\`). Let's replace them with `+`:
```plaintext
:s+https:\/\/mysite.com\/a\/b\/c\/d\/e+hello+
```

## Ch13. the Global Command

### Global Command Overview

The global command has the following syntax:
```plaintext
:g/pattern/command
```
The global command executes `command` against each line that matches the `pattern`.

If you have the following expressions:
```plaintext
const one = 1;
console.log("one: ", one);

const two = 2;
console.log("two: ", two);

const three = 3;
console.log("three: ", three);
```

To remove all lines containing "console", you can run:
```plaintext
:g/console/d
```

### Inverse Match

To run the global command on non-matching lines, you can run:
```plaintext
:g!/pattern/command
```
or
```plaintext
:v/pattern/command
```

### Reversing the Entire Buffer

To reverse the entire file, run:
```plaintext
:g/^/m 0
```

### Aggregating All Todos

To copy all TODOs to the end of the file for easier introspection, run:
```
:g/TODO/t $
```
`:t` (copy) method copys all matches to an address.

### Black Hole Delete

```
:g/console/d_
```

### Reduce Multiple Empty Lines to One Empty Line

You can also run the global command with the following form: `:g/pattern1/,/pattern2/command`. With this, Vim will apply the `command` within `pattern1` and `pattern2`.
```plaintext
:g/^$/,/./-1j
```

With that in mind, let's break down the command:
- `/^$/` represents an empty line (a line with zero character).
- `/./` represents a non-empty line (a line with at least one character). The `-1` means the line above that.
- `command` is `j`, the join command (`:j`). In this context, this global command joins all the given lines.

### Advanced Sort

If you have the following expressions:
```
const arrayB = [
  "i",
  "g",
  "h",
  "b",
  "f",
  "d",
  "e",
  "c",
  "a",
]

const arrayA = [
  "h",
  "b",
  "f",
  "d",
  "e",
  "a",
  "c",
]
```

If you need to sort the elements inside the arrays, but not the arrays themselves, you can run this:
```plaintext
:g/\[/+1,/\]/-1sort
```

## Ch14. External Commands

### Reading the STDOUT of a Command Into Vim

```plaintext
:r !cmd     Read the STDOUT of an external command into the current buffer
:r file1.txt
```

### Writing the Buffer Content Into an External Command

```plaintext
:w !cmd    Pass the text in the current buffer as the STDIN for an external command
```

### Filtering Texts

If you give `!` a range, it can be used to filter texts. Suppose you have the following texts:
```plaintext
hello vim
hello vim
```

Let's uppercase the current line using the `tr` (translate) command. Run:
```plaintext
:.!tr '[:lower:]' '[:upper:]'
```

### Normal Mode Command

To uppercase the current line and the line below, you can run:
```
!jtr '[a-z]' '[A-Z]'
```

## Ch15. Command-line Mode

### Command-line Mode Shortcuts

```plaintext
Ctrl-B    Go to the start of the line
Ctrl-E    Go to the end of the line
Ctrl-H    Delete one character
Ctrl-W    Delete one word
Ctrl-U    Delete the entire line
Ctrl-F    Edit the command like a normal textfile, same with q:
Ctrl-R Ctrl-W  Get the word under the cursor
Ctrl-R Ctrl-A  Get the WORD under cursor
Ctrl-R Ctrl-L  Get the line line under cursor
Ctrl-R Ctrl-F  Get the filename under the cursor
:his :    View command history
:his /    View search history, same with :his ?
q/        Open search history window and edit, same with q?
```

## Ch16. Tags

To jump to a definition, you can use `Ctrl-]` in the normal mode.
However, you need to generate the tag file first.

Here I recommand universal ctags, install it by `pacman -S ctags`
let's generate a basic tag file. Run:
```console
$ ctags -R .
```

### The Tag File

```plaintext
:set tags?
```

You don't have to store your tag file inside your project, you can keep them separate.

To add a new tag file location, use the following:
```plaintext
set tags+=path/to/my/tags/file
```

### Generating Tags for a Large Project

```console
$ ctags -R --exclude=.git --exclude=vendor --exclude=node_modules --exclude=db --exclude=log .
```

```plaintext
:tselect pancake    selective tag jumps, type numerical key to jump.
:tjump donut         selective tag jumps, prompt only when > 2 options.
g Ctrl-]            normal mode key for tjump, much useful.
Ctrl-X Ctrl-]       auto completion with tags.
:tags               a list of the tags you have jumped to. Called tag stack.
:pop                to "pop" the tag stack.
```

### Using Plugins to generate Tags on save

I use [vim-gutentags](https://github.com/ludovicchabant/vim-gutentags).

## Ch17. Fold

```plaintext
##Manual Fold
zfj    'zf' is the fold operator, whereas 'j' is the motion, it can be either a text object. You can fold from visual mode.
zo    open a foleded text.
zc    close he fold.
:,+1fold    fold in command-line mode
zR    open all folds.
zM    close all folds.
za    toggle a fold.

:set foldmethod?    to see which folding method you are currently using. There are six different fold methods: manual, indent, expression, syntax, diff, marker. manual is the default.

##Indent Fold
:set foldmethod=indent
:set shiftwidth=1    set the how much spaces the Vim consider as an indent fold.

##Syntax Fold
:set foldmethod=syntax    fold is determined by syntax language highlighting. Using a language syntax plugin like vim-polyglot, this will work right out of box.

##Diff Fold
$ vimdiff file1.txt file2.txt
$ vim -d file1.txt file2.txt    Same as vimdiff

##Marker Fold
:set foldmethod=marker    Vim looks for special markers defined by 'foldmarker' option.
:set foldmarker=coffee1,coffee2

##Persisting Fold
## Goto see Chapter View
```

### Expression Fold

Expression fold allows you to define an expression to match for a fold. if the 'foldexpr' return 0, then the line is not folded.
`:set foldmethod=expr`

Suppose you have a list of breakfast foods and you want to fold all breakfast items starting with "p":
```
donut
pancake
pop-tarts
protein bar
salmon
scrambled eggs
```

Next, change the `foldexpr` to capture the expressions starting with "p":

```
:set foldexpr=getline(v:lnum)[0]==\\"p\\"
```

The expression above looks complicated. Let's break it down:
- `:set foldexpr` sets up the `'foldexpr'` option to accept a custom expression.
- `getline()` is a Vimscript function that returns the content of any given line. If you run `:echo getline(5)`, it will return the content of line 5.
- `v:lnum` is Vim's special variable for the `'foldexpr'` expression. Vim scans each line and at that moment stores each line's number in `v:lnum` variable. On line 5, `v:lnum` has value of 5. On line 10, `v:lnum` has value of 10.
- `[0]` in the context of `getline(v:lnum)[0]` is the first character of each line. When Vim scans a line, `getline(v:lnum)` returns the content of each line. `getline(v:lnum)[0]` returns the first character of each line. On the first line of our list, "donut", `getline(v:lnum)[0]` returns "d". On the second line of our list, "pancake", `getline(v:lnum)[0]` returns "p".
- `==\\"p\\"` is the second half of the equality expression. It checks if the expression you just evaluated is equal to "p". If it is true, it returns 1. If it is false, it returns 0. In Vim, 1 is truthy and 0 is falsy. So on the lines that start with an "p", it returns 1. Recall if a `'foldexpr'` has a value of 1, then it has a fold level of 1.

## Ch18. Git

### Diffing

`vimdiff` or `vim -d`

```
]c    jump to next diff
[c    jump to previous diff
:diffput    put out the text from the current buffer to another buffer.
:diffget    get the text from another buffer to the current buffer.
If you have multiple buffers, use :diffput fileN.txt and :diffget fileN.txt
```

### Vim As a Merge Tool

Change the default merge tool to use `vimdiff`:
```console
git config merge.tool vimdiff
git config merge.conflictstyle diff3
git config mergetool.prompt false
git config mergetool.keepBackup false    # Git create a backup file in case things don't go well, so disable it.
```

When there comes a conflict, run:
```console
git mergetool
```

Vim displays four windows.
- `LOCAL`  is the change in "local", what you are merging into.
- `BASE` is the common ancestor between `LOCAL` and `REMOTE` to compare how they diverge.
- `REMOTE` is what is being merged into.
- The fourth window contains the git's merge conflict texts. You cursor should be on this window. Your can run `:diffget BASE` or `:diffget REMOTE` to get the change from that.

### Git Inside Vim

```
:!git add %         " git add current file
:!git checkout #    " git checkout the other file
:windo !git add %   " add multiple files in different window
```

### Vim-fugitive

The [vim-fugitive](https://github.com/tpope/vim-fugitive) plugin allows you to run the git CLI without leaving the Vim editor. You will find that some commands are better when executed from inside Vim.

```
:Git             "display a git summary window, you can do
  Ctrl-N/Ctrl-P  "to go up or down the file list.
  -              "to stage or unstage the file under the cursor.
  s              "to stage the file under the cursor.
  u              "to unstage the file under the cursor.
  > or <         "to display or hide an inline diff of the file under the cursor.

:Git blame       "display a split blame window, you can do
  q              "to close the the blame window.
  A              "to resize the author column.
  C              "to resize the commit column.
  D              "to resize the data / time column.

:Gdiffsplit      "runs a 'vimdiff' of the current file against the index or work tree. Or run :Gdiffsplit <commit> to specify a commit.

:Gwrite          "Stage the changes of current file. Like 'git add <current=file>'
:Gread           "Restore the file. Like 'git checkout <current-file>'. You can undo this action.
:Gclog           "display the commit history. Like 'git log', but this one using quickfix, so you can use ':cnext' and ':cprevious' to traverse the log information. Furthmore, you can run ':Gclog' arguments just like before, such as ':Gclog --after="January 1" --before="March 14"'
```

## Ch19. Compile

```
:make    "Vim looks for a makefile in the current directory and execute it. You specify a command and other arguments just like running it from terminal. It uses quickfix to store any error.

:set makeprg=g++\ %\ -o\ %<    "to change the ':make' command.
" '\ ' is to escape the space after 'g++'.
" '%<' represents the current file name without an extension (hello.cpp becomes hello)

autocmd BufWritePost *.cpp make    "add in '.vimrc', to auto-compile on save.

:compiler ruby    "vim runs the '$VIMRUNTIME/compiler/ruby.vim' script and changes the 'makeprg' to use the ruby.

"To create a simple Typescript compiler, in your '~/.vim/compiler/typescript.vim':
CompilerSet makeprg=tsc
CompilerSet errorformat=%f:\ %m
" '%f' represents the error file, whereas '%m' represents the error message.
```

### Plugin: Vim-dispatch

[vim-dispatch](https://github.com/tpope/vim-dispatch)

```
###Async Make
:Make    "Vim will run make asynchronously. You can continue doing whatever your were doing.

###Async Dispatch
:Dispatch    "like the ':compiler' and the ':!' command. It can run any external command asynchronously. e.g. ':Dispatch bundle exec rspec %'

###Automating Dispatch
"Vim-dispatch has 'b:dispatch' buffer variable that you can configure to evaluate specific command automatically.
"If you add this in your '.vimrc', each time you enter a file that ends with '_spec.rb', running ':Dispatch' automatically 'executes bundle exec rspec %':
autocmd BufEnter *_spec.rb let b:dispatch = 'bundle exec rspec %'
```

## Ch20. Views, Sessions, and Viminfo

```
##View
"A View is the smallest subset of the three (View, Session, Viminfo). It is a collection of settings for one window.

:set viewoptions?
:set viewoptions+=localoptions    "tell View to remember the 'localoptions'
:mkview 1                          "save the View, Vim let you save 9 numbered Views
:loadview 1                        "load the View
:set viewdir?                     "see where did Vim save View files

###Automating View Creation
To autosave file name with txt extensions, add these in your '.vimrc':
autocmd BufWinLeave *.txt mkview
autocmd BufWinEnter *.txt silent loadview

##Sessions
"If a View saves the settings of a window, a Session saves the information of all windows (including the layout).

:mksession    "by default, this will save a Session file (Session.vim) in the current directory. Otherwise, you can pass an argument like ':mksession ~/some/where/else.vim'
:source Session.vim     "To load a Session.
"You can also load a Session file from the terminal:
$ vim -S Session.vim

:set sessionoptions?
:set sessionoptions-=terminal    "If you don't want to save 'terminal'.
- `blank` stores empty windows
- `buffers` stores buffers
- `folds` stores folds
- `globals` stores global variables (must start with an uppercase letter and contain at least one lowercase letter)
- `options` stores options and mappings
- `resize` stores window lines and columns
- `winpos` stores window position
- `winsize` stores window sizes
- `tabpages` stores tabs
- `unix` stores files in Unix format

##Viminfo
Viminfo stores:
- The command-line history.
- The search string history.
- The input-line history.
- Contents of non-empty registers.
- Marks for several files.
- File marks, pointing to locations in files.
- Last search / substitute pattern (for 'n' and '&').
- The buffer list.
- Global variables.

:wv ~/.viminfo_extra    "although you will use only one Viminfo file, you can create multiple Viminfo files.
:rv ~/.viminfo_extra

"You can also load a Viminfo file from the terminal:
$ vim -i ~/.viminfo_extra
$ vim -i NONE    # To start Vim without Viminfo
To make it permant, you can add this in your vimrc file:
set viminfo="NONE"

:set viminfo?
You will get:

```
!,'100,<50,s10,h
```

This looks cryptic. Let's break it down:
- `!` saves global variables that start with an uppercase letter and don't contain lowercase letters. Recall that `g:` indicates a global variable. For example, if at some point you wrote the assignment `let g:FOO = "foo"`, Viminfo will save the global variable `FOO`. However if you did `let g:Foo = "foo"`, Viminfo will not save this global variable because it contains lowercase letters.
- `'100` represents marks. In this case, Viminfo will save the local marks (a-z) of the last 100 files. Be aware that if you tell Viminfo to save too many files, Vim can start slowing down. 1000 is a good number to have.
- `<50` tells Viminfo how many maximum lines are saved for each register (50 in this case). If I yank 100 lines of text into register a (`"ay99j`) and close Vim, the next time I open Vim and paste from register a (`"ap`), Vim will only paste 50 lines max. If you don't give maximum line number, *all* lines will be saved. If you give it 0, nothing will be saved.
- `s10` sets a size limit (in kb) for a register. In this case, any register greater than 10kb size will be excluded.
- `h` disables highlighting (from `hlsearch`) when Vim starts.
```

## Multiple File Operations

### Different Ways to Execute a Command in Multiple Files

Vim has eight ways to execute commands across multiple files:
- arg list (`argdo`)
- buffer list (`bufdo`)
- window list (`windo`)
- tab list (`tabdo`)
- quickfix list (`cdo`)
- quickfix list filewise (`cfdo`)
- location list (`ldo`)
- location list filewise (`lfdo`)

```
##Argument List
:args file1 file2 file3  "create a list. To view the list of files, run ':args'
:arga file5 file 5       "add 'file4' and 'file5' into your initial files list.

##Location List
"you may only have one quickfix list, whereas you can have as many location list as windows. Vim creates a distinct location list for each window.
:lvim /bagel/ **/*.md    "the location variant for the ':vimgrep' command
:lopen
:lclose
:lfirst
:llast
:lnext
:lprev
:lgrep
:lmake
```

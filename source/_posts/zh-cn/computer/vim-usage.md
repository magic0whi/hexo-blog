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

`<S-a>` means `Shift+A`
`<C-a>` means `Ctrl+A`
`<CR>` means `<Enter>` or `<Return>`
i\_`<C-p>` means `<C-p>` in Insert Mode
v\_b\_`g <C-a>` means `g <C-a>` in Visual Block-Wise Mode
{`a`,`b`,`c`} means either a or b or c
[`a`,`b`,`c`] means either a or b or c or none.

TODO: works to be merged
`<C-`{`o`,`i`}`>`, Jump old / newer position.
`<C-^>` Go to previously edited buffer.
`:tag`    Jump to tag definition
`:t` (copy) method copies all matches to an address.
`:j` join command join the lines
`sort` sort command sort the lines
`<C-`{`t`,`d`}`>` indent current line forward / backward in insert mode.
`>` or `<` indent visual block forward / backward
Spell Check:
{`]`,`[`}`s` Jump to next / prev wrong word
`z`[`u`]{`g`,`w`,`G`,`w`} Add / Undo add good / wrong word under cursor to `spellfile` / or save to current session (Which is temporarily if you don't save session)
`z=` Find suggestion of the word under cursor


## Ch 01. Starting Vim

Open Vim with two horizontal windows: `vim -o2 `[`file1`]` `[`file2`]
Open Vim with two vertical windows: `vim -O2 `[`file1`]` `[`file2`]

## Ch 02 - Buffers, Windows, and Tabs

### Buffers

|                                    |                                               |
| --                                 | --                                            |
| `:b`{`next`,`prev`,`first`,`last`} | Traverse buffers                              |
| `:`[`w`]`qall`                     | Close / Save close                            |
| `:buffers` / `:ls` / `:files`      | List buffers                                  |
| `:bdelete`                         | Remove current buffer                         |
| `:`[`vertical `]`ball`             | display all buffers horizontally / vertically |

### Windows

A window is how you are viewing a buffer through

|                                            |                                                                                              |
| --                                         | --                                                                                           |
| `:`[`v`]`split `[`filename`]               | Split window                                                                                 |
| `<C-w> `{`h,j,k,l`}                        | Navigate                                                                                     |
| `<C-w> `{`v,s`}                            | Opens a new vertical / horizontal / split                                                    |
| `<C-w> `{`c,o`}                            | Closes focused window / Close all other windows                                              |
| `<C-w> `{`r`,`R`,`H`,`J`,`K`,`L`}          | Rotate focused window {down,right} / {up,left} / far left / far bottom / far top / far right |
| `:`[`vertical `]`resize `{`80`,`+5`}       | Set focused window to 80 rows / columns, or increment 5 rows / columns                       |
| `<C-w> `{(`-`,`+`,`_`),(`<`,`>`,`\|`),`=`} | Decrease  1 / Increase 1 / Maximize row or column for horizontal / vertical split. Equal balance for horizontal, vertical split |

### Tabs

A tab is a collection of windows

|                                |                                         |
| --                             | --                                      |
| `:tabnew `[`filename`]         | Open a new tab                          |
| `:tabclose`                    | Close current tab                       |
| `:tab`{`next,prev,last,first`} | Go to next / previous /last / first tab |

## Ch03. Searching Files

### Searching in Files With Grep

|                                                                    |                                                                                          |
| --                                                                 | --                                                                                       |
| `:find` / `:edit`                                                  | `:find` finds file in path, `:edit` doesn't. Using `:set path+=<your-path-here>` to set. |
| 1. `:vim /pattern/ file` <br> 2. `:grep -R "pattern" /search/path` | Vim's search uses Quickfix Window.                                                       |
| `:c`{`open`,`close`,`next`,`prev`,`older`,`newer`,`first`,`last`}  | Open / Close / Navigate the quickfix window                                              |

### Browsing Files With Netrw

|                     |                                                                         |
| --                  | --                                                                      |
| `:`[`S,V`]`Explore` | Starts netrw on current file {with split top / left half of the screen} |
| `%` / `d`           | Create a new file / dir                                                 |
| `R` / `D`           | Rename / Delete a file or directory                                     |

### Fzf

Fzf syntax:
- `^` prefix exact match. e.g. `^welcome`.
- `$` suffix exact match. e.g. `friends$`.
- `'` exact match. e.g. `'welcome my friends`.
- `|` "or" match. e.g. `friends | foes`.
- `!` inverse match. e.g. `welcome !friends`

|                          |                                                  |
| --                       | --                                               |
| `:Files` or `<C-f>`      | Find files                                       |
| `:Buffers` or `<\-b>`    | Find files in buffers                            |
| `:Rg` or `<\-f>`         | Find in files context                            |
| `:BLines` or `<\-/>`     | Find lines in current buffer                     |
| `:Marks` or `<\-'>`      | Find marks                                       |
| `:Commits` or `<\-g>`    | Find git commits                                 |
| `:Helptags` or `<\-H>`   | Find help tags                                   |
| `:History` or `<\-h> h`  | Find in opened files' history (list `:oldfiles`) |
| `:History:` or `<\-h> :` | Find in command history                          |
| `:History/` or `<\-h> /` | Find in search history                           |

#### Search and Replace in Multiple Files

- First method
  ```vim
  :grep "pizza"
  :cfdo %s/pizza/donut/g | update
  ```
- Second method
  ```vim
  :%bd | e#    "Close all buffer except current file.
  :Files       "Select all files you want
  :bufdo %s/pizza/donut/g | update
```

## Ch04. Vim Grammar

### Grammar Rule

There is only one grammar rule in Vim language: `Verb (Operator) + Noun (Motion)`

|       |                                                      |
| --    | --                                                   |
| `y`   | Yank text (copy)                                     |
| `d`   | Delete text and save to register                     |
| `c`   | Delete text, save to register, and start insert mode |
| `~`   | Toggle the case of the character under the cursor    |
| `gU`  | Uppercase text                                       |
| `gu`  | Lowercase text                                       |
| `yy`  | yank entire line                                     |
| `dd`  | delete entire line                                   |
| `cc`  | change entire line                                   |

Some examples:
|        |                                          |
| --     | --                                       |
| `y$`   | Yank to the end of the line              |
| `dw`   | Delete to the beginning of the next word |
| `c}`   | Change to the end of paragraph           |
| `y2h`  | Yank two characters to the left          |
| `d2w`  | Delete the next two words                |
| `c2j`  | Change the next two lines                |
| `gUiw` | Uppercase current word                   |

### More Nouns (Text Objects)

Two types of text objects:
- `i + object`    Inner text object, select without the white space or the surrounding objects
- `a + object`    Outer text object, select including the white space or the surrounding objects

Common text objects:
|                                                |                                                               |
| --                                             | --                                                            |
| `w`                                            | A word                                                        |
| `p`                                            | A paragraph                                                   |
| `s`                                            | A sentence                                                    |
| `t`                                            | XML tags                                                      |
| `(` / `{` / `[` / `<` or `>` / `]` / `}` / `)` | A pair of `( )` / `{ }` / `[ ]` / `< >`                       |
| `"` / `'` / `` ` ``                            | A pair of `" "` / `' '` / `` ` ` ``                           |
| `%`                                            | Navigate to another match, usually works for `()`, `[]`, `{}` |

## Ch05. Moving in a File

### Character Navigation

A word is a sequence of characters containing *only* `a-zA-Z0-9_`. A WORD is a sequence of all characters except white space (a white space means either space, tab, and EOL).

|              |                                                        |
| --           | --                                                     |
| `g`{`j`,`k`} | Down / Up in a soft-wrapped line                       |
| `w` / `W`    | Move forward to the beginning of next word / WORD      |
| `b` / `B`    | Move backward to the beginning of previous word / WORD |
| `e` / `E`    | Move forward to the end of next word / WORD            |
| `g`{`e`,`E`} | Move backward to the end of previous word / WORD       |

### Current Line Navigation

|            |                                                             |
| --         | --                                                          |
| `0` / `^`  | Go to the first char / non-blank char                       |
| `$` / `g_` | Go to the last char non-blsnk char                          |
| `n\|`      | Go to column n                                              |
| `f` / `F`  | Search forward / backward for a char                        |
| `t` / `T`  | Search forward / backward for a char, stopping before match |
| `;` / `,`  | Repeat the last char search on same / opposite direction    |

### Sentence and Paragraph Navigation

A sentence ends with either `. ! ?` followed by an EOL, a space, or a tab.
A paragraph begins after each empty line and also at each set of a paragraph macro specified by the pairs of characters in paragraphs option.

|             |                                       |
| --          | --                                    |
| `(` / `)`   | Jump to the previous / next sentence  |
| `{` / `}`   | Jump to the previous / next paragraph |
| `[[` / `]]` | Jump to the previous / next section   |

### Line Number Navigation

|            |                           |
| --         | --                        |
| `gg` / `G` | Go to first / last line   |
| `nG`       | Go to line n              |
| `n%`       | Go to n% in file          |
| `<C-g>`    | See total lines in a file |

### Window Navigation

|                 |                                       |
| --              | --                                    |
| `H` / `M` / `L` | Go to top / medium / bottom of screen |
| `n`{`H`,`L`}    | Go `n` line from top / bottom         |

### Scrolling

|                       |                                                                |
| --                    | --                                                             |
| `<C-`{`e`,`d`,`f`}`>` | Scroll down a line / half screen / whole screen                |
| `<C-`{`y`,`u`,`b`}`>` | Scroll up a line / half screen / whole screen                  |
| `z`{`t`,`z`,`b`}      | Bring the current line to top / middle / bottom of your screen |

### Search Navigation

|                        |                                                                        |
| --                     | --                                                                     |
| `/` / `?`              | Search forward / backward for a match                                  |
| `n` / `N`              | Repeat last search in same / opposite direction of previous search     |
| `:noh` or `<Esc><Esc>` | Turn off match highlights                                              |
| `*`                    | Search for whole word under cursor forward, same as type `/\<one\>`    |
| `#`                    | Search for whole word under cursor backward                            |
| `g`{`*`,`#`}           | Search for word under cursor forward / backward                        |
| `gn`                   | Searches forward for the last search pattern and do a visual selection |

### Marking Position

There is a difference between marking with lowercase letters (a-z) and uppercase letters (A-Z). Lowercase alphabets are local marks and uppercase alphabets are global marks (a.k.a. file marks).

|                  |                                           |
| --               | --                                        |
| `ma`             | Mark position with mark `a`               |
| {`'`,`` ` ``}`a` | Jump to line / exact position of mark `a` |
| `:marks`         | View all marks                            |

### Jump

Any motion that moves farther than a word and current line navigation is probably a jump.

|                   |                                                                  |
| --                | --                                                               |
| `''` / ` `` `     | Jump back to line / exact position in current buffer before jump |
| `` `[`` / `` `]`` | Jump to beginning / ending of previously changed or yanked text  |
| `` `<`` / `` `>`` | Jump to the beginning / ending of last visual selection          |
| `` `0``           | Jump back to the last edited file when exiting vim               |
| `:jumps`          | See jump list                                                    |
| `<C-`{`o`,`i`}`>` | Move up / down the jump list                                     |
| `m'5j`            | Add current location to jump list, follows a move                |

## Ch06. Insert Mode

|                                     |                                                                                            |
| --                                  | --                                                                                         |
| `i` / `I` / `gI`                    | Insert text before the {cursor, first non-blank character of the line, start of line}      |
| `a` / `A`                           | Append text after the cursor / end of line                                                 |
| `o` / `O`                           | Starts a new line below / above the cursor and insert text                                 |
| `s` / `S` (=`cc`)                   | Delete the character / line under the cursor and insert text                               |
| `gi`                                | Insert text in same position where the last insert mode was stopped                        |
| `<Esc>` or `<C-[>`                  | Exit insert mode
| `<C-c>`                             | Exit insert mode and do not check for abbreviation                                         |
| `<C-`{`h`,`w`,`u`}`>`               | Delete one char / one word / entire line                                                   |
| `<C-r> a`                           | Insert text from register `a`                                                              |
| `<C-x> <C-`{`y`,`e`}`>`             | Scroll up / down                                                                           |
| `<C-x> <C-`{`l`,`n`,`i`,`f`,`]`}`>` | Auto completion line / text from current file / text from included files / filename / tag  |
| `<C-`{`n`,`p`}`>`                   | Find the next / previous word match, or navigate up / down the auto completion pop-up menu |
| `<C-o> <normal cmd>`                | Excute a normal mode command                                                               |
| `<C-o> 100ihello`                   | Insert "hello" 100 times                                                                   |
| `<C-o> !! curl https://google.com`  | Run `curl` and insert stdout                                                               |
| `<C-o> !! pwd`                      | Run `pwd` and insert stdout                                                                |
| `<C-o> dtz`                         | Delete from current location till the letter `z`                                           |
| `<C-o> D`                           | Delete from current location to the end of the line                                        |

### Repeating Insert Mode

`10i`: Vim will repeat the text 10 times.

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
Let's remove all the z's. Input `<c-v>jjdw..`.

### Including a Motion in a Change

Replace all "let" with "const" in the following expressions:
```plaintext
let one = "1";
let two = "2";
let three = "3";
```
To accomplish this. After you searched `/let`, run `cgnconst<esc>..`.

## Ch08. Registers

The Ten Register Types:
1. Unnamed register (`"`).
2. Numbered registers (`0-9`).
3. Small delete register (`-`).
4. Named registers (`a-z`).
5. Read-only registers (`:`, `.`, and `%`).
6. Alternate file register (`#`).
7. Expression register (`=`).
8. Selection registers (`*` and `+`).
9. Black hole register (`_`).
10. Last search pattern register (`/`).

### Register Operators

|           |                                          |
| --        | --                                       |
| `"a`      | Access / Store text into register `a`    |
| `p` / `P` | Paste the text after / before the cursor |
| `10"ap`   | Paste text from register `a` ten times   |
| `"ayiw`   | Yank a word into register `a`            |

### The Unnamed Register

It stores the last text you yanked, changed, or deleted.

By default, `p` / `P` is connected to the unnamed register.

### The Numbered Registers

#### The Yanked Register

If you yank an entire line of text (`yy`), Vim actually saves that text in two registers:
1. The unnamed register (`"`).
2. The yanked / zero register (`0`).

#### The Non-zero Numbered Registers

When you change or delete a text that is at least **one line long**, that text will be stored in the numbered registers 1-9 sorted by the most recent.

As a side note, these numbered registers are automatically incremented when using the dot command `.`.

### The Small Delete Register

Changes or deletions **less than one line** are not stored in the numbered registers 0-9, but in the small delete register (`-`).

### The Named Register

You can append your text to the register. Using the uppercase version of that register. For example, suppose you have the word "Hello " already stored in register `a`. If you want to add "world" into register `a`, you can find the text "world" and yank it using A register (`"Ayiw`).

### The Read-only Registers

- `.` stores the last inserted text
- `:` stores the last executed command-line
- `%` stores the name of current file

### The Alternate File Register

In Vim, `#` usually represents the alternate file. An alternative file is the last file you opened. To insert the name of the alternate file, you can use `"#p`.

### The Expression Register

`"=1+1<Enter>p` / `<C-r> =1+1`: Evaluate mathematical expression "1+1" and paste it / from insert mode
`"=@a<Enter>p` / `<C-r> =@a`: Get values from register `a` and paste it / from insert mode

### The Selection Registers

Vim has two selection registers: star (`*`) and plus (`+`). You can use them to access copied text from external programs.

If you yank a word from Vim with `"+yiw` or `"*yiw`, you can paste that text in the external program with `<c-v>`. Note that this only works if your Vim program comes with the `+clipboard` option (to check it, run `:version`).

### The Black Hole Register

You can use the black hole register (`_`). To delete a line and not have Vim store the deleted line into any register, use `"_dd`.

### The Last Search Pattern Register

`"/p`: Paste your last search (`/` or `?`)

### Viewing the Registers

To view all your registers, use the `:register` command. To view only registers `a`, `1`, and `-`, use `:register a 1 -`.

### Clearing a Register

- `qaq` Using Macro recording
- `:call setreg('a', 'hello world')`
- `:let @a = ''`

### Putting the Content of a Register (Command-line)

`:10put a`: Paste text from register `a` to below line 10.
`:g/end/put _`: Insert blank lines below all lines contain the text "end", by using black hole register

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

`:2,3 normal @a`: execute macro between lines 2 and 3, `:normal` allows the user to execute any normal mode command passed as argument.

### Recursive Macro

Suppose you have this list again and you need to toggle the case of the first word:
```plaintext
a. chocolate donut
b. mochi donut
c. powdered sugar donut
d. plain donut
```

This time, let's do it recursively:
```plaintext
qaqqa0W~j@aq
```

Now you can just run `@a` and watch Vim execute the macro recursively.

How did the macro know when to stop? When the macro was on the last line, it tried to run `j`, since there was no more line to go to, it stopped the macro execution.

### Executing a Macro Across Multiple Files

Suppose you have three txt files. Your task is to uppercase the first word only on lines containing the word "donut". Assume you have `0W~j` in register `a`. How can you quickly accomplish this?

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
1. `:args *.txt` to find all `.txt` files in your current directory and store in list `:args`.
2. `:argdo g/donut/normal @a` executes the global command `g/donut/normal @a` on each file inside `:args`.
3. `:argdo update` executes `update` command to save each file inside`:args` when the buffer has been modified.

### Appending a Macro

Use the uppercase of registers to append a macro.

e.g. `qAiHello<esc>q`

### Amending a Macro

For example:
1. First `:put a` to put macro's contexts in register `a`.
2. Change the macro's context as usual text. Be aware that some operations like `<Esc>`, you can't literally type `<Esc>`. To achieve that, (in insert mode) press {c,i,s}\_`<C-v>` followed by `<Esc>`. Vim will print `^[`, this is the internal code representation for the `<Esc>` key.
3. Yank the text into register `a`, such as `0"ay$`.

### Macro Redundancy

`:let @z = @a`. Duplicate a macro in register `a` to register `z`.

### Series vs Parallel Macro

TL; DR: Using command-line `:normal` and specify a range, e.g. `:1,$ normal @a`.

You want lowercase all the uppercased "FUNC", then you record this macro `qa0f{gui{jq`:
```plaintext
import { FUNC1 } from "library1";
import { FUNC2 } from "library2";
import { FUNC3 } from "library3";
import foo from "bar";
import { FUNC4 } from "library4";
import { FUNC5 } from "library5";
```
Running `99@a`, only executes the macro three times. It does not execute the macro on last two lines because the execution fails to run `f{` on the "foo" line. You can always go to the next line where "FUNC4" is and replay that macro again. But what if you want to get everything done in one go?

Run the macro in parallel.

Recall from earlier section that macros can be executed using the command-line command `:normal`. If you run `:1,$ normal @a`, you will see that the macro is being executed on all lines except the "foo" line. It works!

## Ch10. Undo

### Undo Tree

In Vim, every time you press `u` and then make a different change, Vim stores the previous state's text by creating an "undo branch".
[vim-mundo](https://github.com/simnalamburt/vim-mundo) plugin is very useful to help visualize Vim's undo tree.

|                                |                                                                                                                             |
| --                             | --                                                                                                                          |
| `<C-g> u`                      | creates undo break in insert mode.                                                                                          |
| `g`{`+`,`-`}                   | Both `u` and `<C-r>` traverse only the *main* nodes in undo tree while `g+` and `g-` traverse *all* nodes in the undo tree. |
| `:`{`w`,`r`}`undo file.undo`   | Save / Load undo file                                                                                                       |
| `:undolist`                    | See undo list.                                                                                                              |
| `:earlier 10`[`f`,`s`,`m`,`d`] | Go to the state 10 times / saves / seconds / minutes / hours / days older                                                   |
| `:later 10`                    | Go to the state 10 times newer                                                                                              |

## Ch11. Visual Mode

|                        |                                                        |
| --                     | --                                                     |
| `v` / `V` / `<C-v>`    | Character-wise / Line-wise / Block-wise visual mode    |
| `gv`                   | Go to the previous visual mode                         |
| `<C-`{`x`,`a`}`>`      | Decrement / Increment numbers, alphabetical characters |
| v\_b\_`g <C-`{`x`,`a`} | Decrement / Increment across multiple lines            |

### Visual Mode Navigation

Press `o` or `O` in visual mode, the cursor jumps from the beginning to the end of the highlighted block, allowing you to expand the highlight area.

### Adding Text on Multiple Lines

With your cursor on the first line:
1. Run block-wise visual mode and go down two lines (`<c-v> jj`).
2. Highlight to the end of the line (`$`).
3. Append (`A`) then type text you want.
4. Exit visual mode.

Alternatively, you can also use the `:normal` command to add text on multiple lines:
1. Highlight all 3 lines (`vjj`).
2. Type `:normal! A;`.

## Ch12. Search and Substitute

You can use `\C` pattern anywhere in your search term to tell Vim that the subsequent search term will be case sensitive. If you do `/\Chello`, it will strictly match "hello", not "HELLO" or "Hello".

### Repeating Search

`//` Repeat the previous search
s_`<`{`up`,`down`,`c-`{`n`,`p`}}`>` to traverse search history

### Repeating the Last Substitution

`&` or `:s` or leave the first substitute pattern argument black `:s//awesome/`

By the way, the repeat-substitution commands (`&` and `:s`) do not retain the flags. To quickly repeat the last substitute command with all the flags, run `:&&`.

### Substitution Range

Here are some range variations you can pass:

- `:,3s/let/const/` Substitute from current line to line 3.
- `:1,s/let/const/` Substitute from line 1 to current line.
- `:3s/let/const/`  Substitute on line 3 only.
- `:%s/let/const/`  Sbstitution on all lines.

In addition to numbers, you can also use these symbols as range:
- `.` means the current line. A range of `.,3` means between the current line and line 3.
- `$` means the last line in the file. `3,$` range means between line 3 and the last line.
- `+n` means n lines after the current line. You can use it with `.` or without. `3,+1` or `3,.+1` means between line 3 and the line after the current line.

### Substitution Flags

- `&` Reuse the flags from the previous substitute command.
- `g` Replace all matches in the line.
- `c` Ask for substitution confirmation.
- `e` Prevent error message from displaying when substitution fails.
- `i` / `I` Perform case insensitive / sensitive substitution.

### Changing the Delimiter

 You can change the delimiter with any single-byte characters (except for alphabets, numbers, or `"`, `|`, and `\`) when it is hard to tell which forward slashes (`/`) are part of the substitution pattern and which ones are the delimiters.

## Ch13. the Global Command

### Global Command Overview

|                                           |                                                              |
| --                                        | --                                                           |
| `:g/pattern/cmd`                          | Executes `cmd` against each line that matches the `pattern`. |
| `:`{`g!`,`v`}`/pattern/command`             | Run the global command on non-matching lines                 |
| `:g/console/d`                            | Remove all lines containing "console"                        |
| `:g/^/m 0`                                | Reverse the entire file / buffer                             |
| `:g/TODO/t $`                             | Aggregate all todos to the end of file                       |
| `:g/console/d_`                           | Black hole delete that matches the `pattern`                 |
| `:g/pattern1/,/pattern2/cmd`              | Apply the `cmd` within `pattern1` and `pattern2`             |
| `:g/^$/,/./-1j`                           | Reduce multiple empty lines to one empty line, `/^$/` represents an empty line. `/./` represents a non-empty line (a line with at least one character), `-1` means the line above that |

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

|                                                 |                                                                                                      |
| --                                              | --                                                                                                   |
| `:`{`r`,`w`}` <filename>` / `:`{`r`,`w`}` !cmd` | {Read,Write} a file / the {STDOUT,STDIN} of an external command to {current buffer,external command} |
| `:.!tr '[:lower:]' '[:upper:]'`                 | Uppercase current line                                                                               |
| `!jtr '[a-z]' '[A-Z]'`                          | Uppercase the current line and the line below, the difference is this is a normal mode operator      |

## Ch15. Command-line Mode

|                                     |                                                        |
| --                                  | --                                                     |
| `<C-`{`b`,`e`}`>`                   | Go to the start / end of the line                      |
| `<C-`{`h`,`w`,`u`}`>`               | Delete one character / one word / entire line          |
| `<C-r> <C-`{`w`,`a`,`l`,`f`}`>`     | Get the word / WORD / line / filename under the cursor |
| `:his `{`:`,`/`,`?`}                | View command / search history                          |
| `q`{`:`,`/`,`?`} or  {c,s}\_`<C-f>` | Open editable command / search history                 |

## Ch16. Tags

Here I recommand universal ctags, install it by `pacman -S ctags`
`$ ctags -R .` manual generate tag files under cuurent directory
`$ ctags -R --exclude=.git --exclude=vendor --exclude=node_modules --exclude=db --exclude=log .`

|                                 |                                                        |
| --                              | --                                                     |
| `:set tags?`                    | See tag files path                                     |
| `set tags+=path/to/tags/file`   | Add a new tag files location                           |
| `<C-]>`                         | Jump to **a** definition                               |
| `:tselect <pattern>`            | Selective tag jumps, type numerical key to jump.       |
| `g <C-]>` or `:tjump <pattern>` | Selective tag jumps, prompt only when > 2 options.     |
| `:tags`                         | List of the tags you have jumped to. Called tag stack. |
| `:pop`                          | "pop" the tag stack.                                   |

## Ch17. Fold

There are six different fold methods: `manual`, `indent`, `expression`, `syntax`, `diff`, `marker`. `manual` is the default.
You can persisting fold by save the View. Goto see Chaper Views, Sessions, and Viminfo

|                                                |                                                             |
| --                                             | --                                                          |
| `zf`<`any motion`>                             | Fold operator, follows a motion, or use it in visual mode   |
| `z`{`o`,`c`,`a`}                               | Open / Close / Toggle a foleded text.                       |
| `:,+1fold`                                     | Fold in command-line mode                                   |
| `z`{`R`,`M`}                                   | Open / close all folds.                                     |
| `:set foldmethod?`                             | See which folding method you are currently using.           |
| `:set foldmethod=`{`indent`,`syntax`,`marker`} | Set Indent / Syntax / Marker Fold mode                      |
| `:set shiftwidth=1`                            | Set the how much spaces the Vim consider as an indent fold. |
| `:set foldmarker=coffee1,coffee2`              | Will fold texts between `coffee1` and `coffee2`             |

- Syntax Fold determined by syntax language highlighting. Using syntax plugin like `vim-polyglot` will work out of box
- Maker Fold determined by special markers defined in 'foldmarker' option.

### Diff Fold

`$ vimdiff <file1> <file2>` or `$ vim -d <file1> <file2>`

### Expression Fold

Expression fold allows you to define an expression to match for a fold. if the `foldexpr` return `0`, then the line is not folded.

`:set foldmethod=foldexpr`

Suppose you want to fold all breakfast items starting with "p":
```plaintext
donut
pancake
pop-tarts
protein bar
salmon
scrambled eggs
```

```vim
:set foldexpr=getline(v:lnum)[0]==\\"p\\"
```

Let's break it down:
- `getline()` is a Vimscript function that returns the content of any given line. If you run `:echo getline(5)`, it will return the content of line 5.
- `v:lnum` is Vim's special variable for the `'foldexpr'` expression. Vim scans each line and at that moment stores each line's number in `v:lnum` variable. On line 5, `v:lnum` has value of 5. On line 10, `v:lnum` has value of 10.
- `[0]` in the context of `getline(v:lnum)[0]` is the first character of each line. When Vim scans a line, `getline(v:lnum)` returns the content of each line. `getline(v:lnum)[0]` returns the first character of each line. On the first line of our list, "donut", `getline(v:lnum)[0]` returns "d". On the second line of our list, "pancake", `getline(v:lnum)[0]` returns "p".
- `==\\"p\\"` is the second half of the equality expression. It checks if the expression you just evaluated is equal to "p". If it is true, it returns 1. If it is false, it returns 0. In Vim, 1 is truthy and 0 is falsy. So on the lines that start with an "p", it returns 1. Recall if a `'foldexpr'` has a value of 1, then it has a fold level of 1.

## Ch18. Git

### Diffing

|                                                     |                                                                                          |
| --                                                  | --                                                                                       |
| `vimdiff` or `vim -d`                               |                                                                                          |
| {`]`,`[`}`c`                                        | jump to next / previous diff                                                             |
| `d`{`p`,`o`} or `:diff`{`put`,`get`}` `[`filename`] | {put,obtain(get)} text from {current,another} buffer to {another,current} buffer / file. |

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

```vim
:!git add %         "Git add current file
:!git checkout #    "Git checkout the other file
:windo !git add %   "Add multiple files in different window
```

### Vim-fugitive

[vim-fugitive](https://github.com/tpope/vim-fugitive)

- `:Git` Display a git summary window, here you can do:
  |                |                                                             |
  | --             | --                                                          |
  | `<C-`{`n`,`p`} | Go up or down the file list.                                |
  | `-`            | Stage or unstage the file under the cursor.                 |
  | `s`            | Stage the file under the cursor.                            |
  | `u`            | Unstage the file under the cursor.                          |
  | `>` / `<`      | Display / Hide an inline diff of the file under the cursor. |
- `:Git blame`   Display a split blame window, here you can do:
  |                |                                           |
  | --             | --                                        |
  | `q`            | Close the the blame window.               |
  | `A`/ `C` / `D` | Resize the author / commit / {date,time} column. |
- `:Gdiffsplit`: Runs a 'vimdiff' of the current file against the index or work tree. Or run :Gdiffsplit <commit> to specify a commit.
- `:Gwrite`: Stage the changes of current file. Like `git add <current=file>`
- `:Gread`: Restore the file. Like `git checkout <current-file>`. You can undo this action.
- `:Gclog`:  Display the commit history. Like `git log`, but this one using quickfix, so you can use `:cnext` and `:cprevious` to traverse the log information. Furthmore, you can run `:Gclog` arguments just like before, such as `:Gclog --after="January 1" --before="March 14"`

## Ch19. Compile

`:make` Vim looks for a makefile in the current directory and execute it. It uses quickfix to store any error.
`:set makeprg=g++\ %\ -o\ %<` Change the `:make` command. `\ ` is to escape the space after `g++`. `%<` Represents the current file name without an extension (`hello.cpp` becomes `hello`)

`autocmd BufWritePost *.cpp make` Add in `.vimrc`, to auto-compile on save.

`:compiler ruby` Vim runs the `$VIMRUNTIME/compiler/ruby.vim` script and changes the `makeprg` to use the ruby.

To create a simple Typescript compiler:
```vim ~/.vim/compiler/Typescript.vim
CompilerSet makeprg=tsc
CompilerSet errorformat=%f:\ %m "'%f' represents the error file, whereas '%m' represents the error message.
```

### Plugin: Vim-dispatch

[vim-dispatch](https://github.com/tpope/vim-dispatch)

- Async Make
  `:Make` Vim will run make asynchronously. You can continue doing whatever your were doing.
- Async Dispatch
  `:Dispatch` Like the ':compiler' and the ':!' command. It can run any external command asynchronously. e.g. ':Dispatch bundle exec rspec %'
- Automating Dispatch
  Vim-dispatch has `b:dispatch` buffer variable that you can configure to evaluate specific command automatically.
  add this in your `.vimrc`, each time you enter a file that ends with `_spec.rb`, running `:Dispatch` automatically executes `bundle exec rspec %`:
  ```vim ~/.vimrc
  autocmd BufEnter *_spec.rb let b:dispatch = 'bundle exec rspec %'
  ```

## Ch20. Views, Sessions, and Viminfo

### View

View is the smallest subset of the three (View, Session, Viminfo). It is a collection of settings for one window

|                                  |                                                     |
| --                               | --                                                  |
| `:set viewoptions?`              |                                                     |
| `:set viewoptions+=localoptions` | Remember the `localoptions`                         |
| `:set viewdir?`                  | See where did Vim save View files                   |
| `:`{`mk`,`load`}`view 1`         | Save / Load View, Vim let you save 9 numbered Views |

### Automating View Creation

e.g. To autosave txt files:
```vim ~/.vimrc
autocmd BufWinLeave *.txt mkview
autocmd BufWinEnter *.txt silent loadview
```

### Sessions

View saves the settings of a window, Session saves the information of all windows (including the layout).

|                                                 |                                                        |
| --                                              | --                                                     |
| `:mksession `[`/path/to/mysession.vim`]         | By default Vim save `Session.vim` in current directory |
| `:source Session.vim` or `$ vim -S Session.vim` | Load a Session. You can also load from terminal        |
| `:set sessionoptions?`                          | Show session options                                   |
| `:set sessionoptions-=terminal`                 | If you don't want to save `terminal`                   |

Session Options:
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

### Viminfo

Viminfo stores:
- The command-line history.
- The search string history.
- The input-line history.
- Contents of non-empty registers.
- Marks for several files.
- File marks, pointing to locations in files.
- Last search / substitute pattern (for `n` and `&`).
- The buffer list.
- Global variables.

- `:`{`w`,`r`}`v ~/.viminfo_extra` Save / Viminfo file
- `$ vim -i ~/.viminfo_extra` Load Viminfo file from terminal.
- `$ vim -i NONE` Start Vim without Viminfo. To make it permant, you can add `set viminfo="NONE"` in your vimrc file

`:set viminfo?`
You will get like as:

```plaintext
!,'100,<50,s10,h
```

This looks cryptic. Let's break it down:
- `!` saves global variables that start with an uppercase letter and don't contain lowercase letters.
  Recall that `g:` indicates a global variable. For example, if at some point you wrote the assignment `let g:FOO = "foo"`, Viminfo will save the global variable `FOO`. However if you did `let g:Foo = "foo"`, Viminfo will not save this global variable because it contains lowercase letters.
- `'100` saves the local marks (a-z) of the last 100 files. 1000 is a good number to have.
- `<50` maximize 50 lines saved for each register. If you don't give maximum line number, *all* lines will be saved. If you give it 0, nothing will be saved.
- `s10` size limit (in kb) for each register.
- `h` disables highlighting (from `hlsearch`) when Vim starts.

There are more parameters you can set:
- `:100` stores 100 command-line history.
- `%` saves and restore the buffer list.
- `n~/.vim/.viminfo` sets Viminfo file path

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

### Argument List

`:arg`{`s`,`a`}` file1 file2 file3`  Initialize / Append argument list. Run `:args` standalone to view file list.

### Location List

You may only have one quickfix list, whereas you can have as many location list as windows. Vim creates a distinct location list for each window.
|                                                          |                                                            |
| --                                                       | --                                                         |
| `:l`{`vim`,`grep`}` /bagel/ **/*.md`                     | location list variant for the `:vimgrep` / `:grep` command |
| `:l`{`open`,`close`,`first`,`last`,`next`,`prev`,`make`} | bunch of location list vraiant commands                    |


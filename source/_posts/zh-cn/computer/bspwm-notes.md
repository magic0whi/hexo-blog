---
title: bspwm-notes
toc: true
category: computer
date: 2022-04-20 16:46:23
tags:
---

Cheatsheet for bspwm

<!-- more -->

## Concepts

A desktop is (a pointer to) a tree. There are 1-10 (Roman Numbers) desktops (trees).
Monitor is just your real monitor, they divide the distribution of ten trees. Such as eDP-1 for first five tree (1-5), HDMI-1 for last five trees (5-10).
One leaf node holds one window (node &asymp; window)

### Manual mode

Manual Mode: User preselect the insertion point. If you don't preselect, new node position will be determined by automatic scheme
```plaintext
            a                          a                          a
           / \                        / \                        / \
          1   b         --->         c   b         --->         c   b
          ^  / \                    / \ / \                    / \ / \
            2   3                  4  1 2  3                  d  1 2  3
                                   ^                         / \
                                                            5   4
                                                            ^

+-----------------------+  +-----------------------+  +-----------------------+
|           |           |  |           |           |  |     |     |           |
|           |     2     |  |     4     |     2     |  |  5  |  4  |     2     |
|           |           |  |     ^     |           |  |  ^  |     |           |
|     1     |-----------|  |-----------|-----------|  |-----------|-----------|
|     ^     |           |  |           |           |  |           |           |
|           |     3     |  |     1     |     3     |  |     1     |     3     |
|           |           |  |           |           |  |           |           |
+-----------------------+  +-----------------------+  +-----------------------+

            X                          Y                          Z
```

### Automatic Mode

Automatic Mode: The insertion position of new node determined by automatic scheme (`longest_side`, `alternate`, `spiral`) and initial polarity (`first_child` or `second_child`). `bspc config automatic_scheme {longest_side,alternate,spiral}`

- Longest side scheme: the split direction was chosen based on dimensions of the tiling rectangle (whether height or width is longer) and the initial polarity.
  The following scenario set the initial polarity to `second_child`:
  ```plaintext
              1                          a                          a
              ^                         / \                        / \
                          --->         1   2         --->         1   b
                                           ^                         / \
                                                                    2   3
                                                                        ^

  +-----------------------+  +-----------------------+  +-----------------------+
  |                       |  |           |           |  |           |           |
  |                       |  |           |           |  |           |     2     |
  |                       |  |           |           |  |           |           |
  |           1           |  |     1     |     2     |  |     1     |-----------|
  |           ^           |  |           |     ^     |  |           |           |
  |                       |  |           |           |  |           |     3     |
  |                       |  |           |           |  |           |     ^     |
  +-----------------------+  +-----------------------+  +-----------------------+

              X                          Y                          Z
  ```
- Alternate scheme: If parent is split horizontally, the child will be split vertically and vise versa.
- Spiral scheme: It is best shows as image.
  ```plaintext
              a                          a                          a
             / \                        / \                        / \
            1   b         --->         1   c         --->         1   d
               / \                        / \                        / \
              2   3                      4   b                      5   c
              ^                          ^  / \                     ^  / \
                                            3   2                      b   4
                                                                      / \
                                                                     3   2
  
  +-----------------------+  +-----------------------+  +-----------------------+
  |           |           |  |           |           |  |           |           |
  |           |     2     |  |           |     4     |  |           |     5     |
  |           |     ^     |  |           |     ^     |  |           |     ^     |
  |     1     |-----------|  |     1     |-----------|  |     1     |-----------|
  |           |           |  |           |     |     |  |           |  3  |     |
  |           |     3     |  |           |  3  |  2  |  |           |-----|  4  |
  |           |           |  |           |     |     |  |           |  2  |     |
  +-----------------------+  +-----------------------+  +-----------------------+
  
              X                          Y                          Z
  ```
  New node take the place of insertion point, whole tree rooted as its parent reattached and swapped at first depth. 

## Cheatsheet

|                              |                                                                                   |                                                     |
| --                           | --                                                                                | --                                                  |
| `<D-Return>`             | Open terminal                                                                     |                                                     |
| `<D-Space>`              | Open menu launcher                                                                |                                                     |
| `<D-Esc>`                | sxhkd reload                                                                      |                                                     |
| `<D-M>`-`q`,`r`          | Quit / Restart bspwm                                                              | `bspc `{`quit`,`wm -r`}                             |
| `<D-`[`S`]`>`-`w`          | Close / Kill focused node                                                         | `bspc node -`{`c`,`k`}                              |
| `<D>`-`m`                  | Alter tiled / monocle layout                                                      | `bspc desktop -l next`                              |
| `<D>`-`y`                  | Send the newest marked node to the newest preselected node (only current desktop) |                                                     |
| `<D>`-`g`                  | Swap focused node and the node has biggest window                                 |                                                     |
| `<D>`-`t`,`<S>-t`,`s`,`f` | Set node state to tiled / pseudo tilted / floating / fullscreen                |                                                     |
| `<D-C>`-`m`,`x`,`y`,`z`  | Set node flag to marked / locked / sticky / private                              |                                                     |
| `<D-`[`S`]`>`-`h`,`j`,`k`,`l` | Traverse / Swap node to west / south / north / east                        |                                                     |
| `<D>`-`b`                  | Traverse brother node                                                             |                                                     |
| `<D>`-`p`                  | Select parent node                                                                |                                                     |
| `<D>`-`comma`,`period` | Traverse first / second node while selecting a parent node                        |                                                     |
| `<D-`[`S`]`>`-`c`          | Traverse next / prev node in current desktop                                  |                                                     |
| `<D>`-`{`,`}`          | Traverse prev / next desktop in current monitor                               |                                                     |
| `<D>`-`` ` ``,`<Tab>`  | Traverse last node / desktop                                                      |                                                     |
| `<D>`-`o`,`i`          | Traverse older / newer node in history                                   |                                                     |
| `<D-`[`S`]`>`-`0-9`    | Traverse desktop 0-9 / Send focused node to desktop 0-9                           |                                                     |
| `<D-C>`-`h`,`j`,`k`,`l`,`1-9` | Preselect direction / ratio for new node to insert                         | `bspc node `{`-p `{`west`,`souch`,`north`,`east`},`-o 0.`{`1-9`}} |
| `<D-C-`[`S`]`-Space>`    | Cancel the preselection for focused node / desktop                                |                                                     |
| `<D-M-`[`S`]`>`-`h`,`j`,`k`,`l` | (Resize) Expand / Contract node by moving one of its side outward / inward |                                         |
| `<D-Left`,`Upwn`,`UP`,`Right>` | Move a floating node                                                  |                                                    |

## Node flag features

- Marked (MK) node can be send to preselected node.
- Locked (L) node cannot be deleted.
- Sticky (S) node behaves like a sticky note.
- Private (\$) node cannot derive new node as its brother. 

## See Also

1. [bspwm(1)](https://man.archlinux.org/man/bspwm.1.en)

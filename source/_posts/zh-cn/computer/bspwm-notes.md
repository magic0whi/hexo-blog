---
title: bspwm-notes
toc: true
category: computer
date: 2022-04-20 16:46:23
tags:
---

<!-- more -->

## Concepts

A desktop is (a pointer to) a tree.
One leaf node holds one window (node &asymp; window)

Manual Mode: User preselect the insertion point.
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

Automatic Mode: The insertion position of new node determined by automatic scheme (`longest_side`, `alternate`, `spiral`) and initial polarity (`second_child`)
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
- Spiral scheme:
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
\[TODO\]

## Cheatsheet

|                                |                                                                           | |
| --                             | --                                                                        | -- |
| `<super-Return>`               | Open terminal                                                             | |
| `<super-Space>`                | Open menu launcher                                                        | |
| `<super-Esc>`                  | sxhkd reload                                                              | |
| `<super-M>-{q,r}`              | Quit / Restart bspwm                                                      | `bspc {quit,wm -r}` |
| `<super-{S}>-w`                | Close / Kill focused node                                                 | `bspc node -{c,k}` |
| `<super>-m`                    | Alter tiled / monocle layout                                              | `bspc desktop -l next` |
| `<super>-y`                    | Send the newest marked node to the newest preselected node                ||
| `<super>-g`                    | Swap focused node and the node has biggest window                         ||
| `<super>-{t,<S>-t,s,f}`        | Set focused node state to tiled / pseudo tilted / floating / fullscreen   ||
| `<super-C>-{m,x,y,z}`          | Set focused node flag to marked / locked / sticky / private               ||
| `<super-{S}>-{h,j,k,l}`        | Focus / Swap node to west / south / north / east                          ||
| `<super>-{p,b,comma,period}`   | Focus the parent / brother / first / second node                          ||
| `<super-{S}>-c`                | Focus next / previous node in focused desktop                             ||
| `<super>-{{,}}`                | Focus next / previous desktop in focused monitor                          ||
| ``<super>-{`,<Tab>}``          | Focus last node / desktop                                                 ||
| `<super>-{o,i}`                | Focus older or newer node in focus history                                ||
| `<super-{S}>-{0-9}`            | Focus desktop 0-9 / Send focused node to desktop 0-9                      ||
| `<super-C>-{h,j,k,l,1-9}`      | Preselect direction / ratio for new node to insert                        | `bspc node {-p {west,souch,north,east},-o 0.{1-9}}`|
| `<super-C-{S}-Space>`          | Cancel the preselection for focused node / desktop                        ||
| `<super-M-{S}>-{h,j,k,l}`      | Expand / Contract focused node by moving one of its side outward / inward ||
| `<super-{Left,Upwn,UP,Right}>` | Move a floating node                                                      ||

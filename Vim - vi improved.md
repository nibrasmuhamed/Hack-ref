# Vim

Vim is a highly configurable text editor built to make creating and changing any kind of text very efficient. It is included as "vi" with most UNIX systems and with Apple OS X.

## vim setup
installing vim on debian based distros
```
sudo apt install vim
```

- vi
- vim
- editor

vim has three modes
1. normal
2. insert
3. visual

## vim commands
 to get started
 ```
 vim filename
 ```
 save and quit on normal mode
 ```
 :wq
 ZZ
 ```
 navigation on normal mode
 ```
 h -
 j -
 k -
 l -
 x - delete a character
 S - delete a line
 0 - points to start of line
 $ - points to end of the line
 gg - points to start of file
 G - jumps end of file
 I - insert before the line
 A - insert after the line
 o - open new line below
 O - open new line above
 w - jump by word
 b - jump by word back
 W - jump with white space
 r - replace one character
 R - replace mode - override the curser pointed line
 :linenum - jumps to line
 cw - chage word from curser point
 D - delete rest of line
 cc - change the line
 dd - delete the line
 ciw - change inner word ()
 diw - delete inner word ()
 % - jump to bracket
 
 ```
 undo on normal mode
 ```
 u - undo
 C-r - redo
 ```
 delete one character and replace in normal mode
 ```
 s - delete a character which curser points and changes to insert mode
 ```
 copy and paste
 ```
 yy - copies the line
 p - pastes the line below
 P - paste above
 ```
 
 configurations
 ```
 :set number - set line numbers
 :set nonumber - unset line number
 
 ```
 
 visual mode
 ```
 d - delete
 c - change
 y - copy
 ```
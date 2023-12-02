# Mode

vim has four type of Modes:

- command mode
  - this is default mode. to switch to other mode we should switch to this mode first by pressing (( esc )) key.
- command-line mode
  - to switch to this mode press (( : )) from (( command mode )).
- insert mode
  - to switch to this mode press (( i )) from (( command mode )).
- visual mode
  - to switch to this mode press (( v )) from (( command mode )).

***

# Create new file

- vim
  - it open a new vim editor
- press (( : )) to change to command-line mode.
- write command (( edit sample.txt)) and press enter.
  - this command create a new file named (( sample.txt )) and open it with vim in command mode.
- press (( i )) to change to insert mode.
- press (( Esc )) to go to command mode.
- press (( : )) to go to command-line mode.
- write command (( w )) and press enter to save the changes. after this command you will be back in open file in command mode.
- press (( : )) to go to command-line mode.
- write command (( q )) and press enter to close the file.



***

# Save and Quit

- press (( : )) in command mode to change to command-line mode.
- write command (( wq )) and press enter.



***

# Open a file

- vim f2
  - this command will open file (( f2 )) in command mode.



***

# Close without saving

- press (( : )) to go to command-line mode.
- write command (( q! )) and press enter.



***

# Copy

- in command mode set your cursor at beginning of where you want to copy.
- press (( v )) to switch to visual mode.
- press left or right arrow keys to select the desired text.
- after finishing your selection, press (( y )).
- navigate your cursor where you want to paste the copied text.
- press (( p )) to paste the text.
- press (( : )) to switch to 
- write command (( wq )) to save and exit.

***

# Replace text

to replace text with new one in file we can use command (( [range]s/{pattern}/{string}/[flags] )) in command-line mode, which consists of four sections as follow:

- [range]s
  - this is the range of lines in which you consider the replacement occur. for example 5,10 means from line 5 to 10. use (( % )) to indicate entire lines, (( . )) to point to the current line, and (( $ )) to point to the end line. 
- {pattern}
  - this is the text we want to be replaced. it could be regular expression.
- {string}
  - this is the text to replace.
- {flags}
- this flag is used to change the behavior. (( c )) is confirming before replacing, (( i )) is to ignore case-sensitive replacement, (( g )) for making the change globally ( each matching found in each entire line, otherwise the first occurrence is included ).



## Example1: 

**%s/Hello/Hi/g**

this example replaces the (( Hi )) with (( Hello )) in entire lines of the file globally.



## Example 2:

**2,4s/Hi/Hello and Welcome/gci**

in lines 2 and 3 and 4, the word (( Hi )) with checking case-insensitively will be replaced globally with (( Hello and Welcome )) and in each replacement a confirmation will be asked from you.



***

# Undo, Redo

to undo press (( u )) in command mode. this will undo the last action. you can undo to 2 previous action by pressing (( u )) 2 times, or in command mode press the number of previous action that in our example is 2, and the press (( u )).

to redo press (( ctrl + r )) in command mode. this will redo the last undo in its stack. you can redo to 2 prescinding action by pressing (( ctrl + r )) 2 times, or in command mode press the number of prescinding action that in our example is 2, and the press (( ctrl + r )). 



***

# Paste

to paste a text from clipboard to vim,  switch to insert mode by pressing (( i )), and then press and hold (( shift )) and right click on somewhere in opened file in vim, then select (( paste )) item from context menu. 

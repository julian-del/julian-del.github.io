---
layout: post
title:  "Time-saving Python scripts"
date:   2022-09-11 12:00:00
preview: img/posts/2022-09-10-time-savers/preview.webp
---
TODO
- discuss environment management: venv
- NB also add to new ghub repo

### Virtual environments

### Book-finder script
The shebang tells the terminal that the script should be executed as a python script, and to use the python installed in a particular location. 
```python
#!/usr/bin/env python
# book.py - Launches a bookfinder.com and amazon.com.au search window
```
Next, the script imports the required packages. The `webbrowser` package is a default with Python 3, and allows a web browser to be controlled by a script. Similarly, `sys` comes with Python and is a module for interacting with the command line and the Python interpreter. Finally, `pyperclip` gives Python access to the system clipboard.
```
import webbrowser
import sys
import pyperclip
```
The script requires the user to type the following into the terminal: `book '<author name>' '<book title>'`. It will then open browser tabs for bookfinder.com, amazon.com and bookdepository.com, and search all sites for the given book.

The script starts by retrieving the author and title from the command's arguments on the CLI. `sys.argv` increments from 0 up, where 0 represents the command, 1 represents the first arguement, and so on. An `if` statement is used for cases when no arguments are provided on the command line. See further below for what happens in that case.
```python
if len(sys.argv) > 1:
    # get book author from CLI
    author = sys.argv[1]
    # get book title from CLI
    title = sys.argv[2]
```
Next, a browser tab is opened, and the search strings are added to the site URLs, using the various formattting required by each website:
```python
    webbrowser.open('https://www.bookfinder.com/search/?author=' + author + '&title=' + title + '&lang=en&isbn=&new_used=*&destination=au&currency=AUD&mode=basic&st=sr&ac=qr')
    webbrowser.open('https://www.amazon.com.au/s?k=' + author + '+' + title)
    webbrowser.open('https://www.bookdepository.com/search?searchTerm=' + author + '+' + title)
```
The second way the script may be used is by first copying a book title to the clipboard. Then, when `book` is run from the command line, the script will pull in the title and then open the various websites:
```python
else:
    # get title from clipboard
    title = pyperclip.paste()

    webbrowser.open('https://www.bookfinder.com/search/?author=&title=' + title + '&lang=en&isbn=&new_used=*&destination=au&currency=AUD&mode=basic&st=sr&ac=qr')
    webbrowser.open('https://www.amazon.com.au/s?k=' + title)
    webbrowser.open('https://www.bookdepository.com/search?searchTerm=' + title)

```
### Thesaurus.com time-saver
This script works in the same way as the book script above. In this case, the user inputs `thes '<word>'` into the terminal and a browser opens thesaurus.com and searches for the word:

### Google Maps time-saver
For this script, the user inputs `map '<address>'` and is taken to a Google Maps page for the address.

### Editing the PATH environment variable
- creating `bin` directory for all my own scripts
- updating PATH variable

### Making scripts executable
- making scripts executable with chmod

---
layout: post
title:  "Time-saving Python scripts"
date:   2022-09-11 12:00:00
preview: img/posts/2022-09-10-time-savers/preview.webp
---
*The scripts in this article are also on [my GitHub profile](https://github.com/julian-del/time-savers){:target="_blank"}{:rel="noopener noreferrer"}.*

Python is a flexible and easy-to-learn language, making it perfect for automating frequent tasks. In this post, I will run through how I wrote three simple scripts that make my life easier on a day-to-day basis. The scripts are inspired by Al Sweigart and his book [*Automate the Boring Stuff With Python*](https://automatetheboringstuff.com/){:target="_blank"}{:rel="noopener noreferrer"}.

### Virtual environments
Before discussing the actual Python scripts, it is worth mentioning the utility of doing development work with virtual environments. Virtual environments allow you to isolate a project's required binary (in this case, Python 3) and modules from the rest of the system. This can prevent system bloat; it also ensures projects' conflicting dependencies do not cause issues with each other. 

When developing the scripts in this article, I used `venv`, which comes installed with Python 3, as my environment manager. Below are the basic commands that allow you to use `venv` and recreate a project's environment if necessary:
```bash
#create virtual environment (called venv) inside new project folder called project_name
python3 -m venv project_name/venv

# activate the virtual environment
source project_name/venv/bin/activate 

# self explanatory:
deactivate 

# list all packages and send to txt file
pip freeze > requirements.txt 

# create new venv using a requirements file
pip install -r requirements.txt

# delete venv
rm -rf venv
```

### Book-finder script
The first line of the script, known as the shebang, tells the terminal that the script should be executed as a python script, and to use the python installed in a particular location. It is common to use `/usr/bin/env` because this tells the computer to use whatever is the first installation of the given language that's in the user's `PATH`. In this way, the script is more flexible and can be used by different users without editing the script to specify a new path.
```python
#!/usr/bin/env python3
# book.py - Launches a bookfinder.com and amazon.com.au search window
```
Next, the script imports the required packages. The `webbrowser` package is a default with Python 3, and allows a web browser to be controlled by a script. Similarly, `sys` comes with Python and is a module for interacting with the command line and the Python interpreter. Finally, `pyperclip` gives Python access to the system clipboard.
```python
import webbrowser
import sys
import pyperclip
```
The script requires the user to type the following into the terminal: `book '<author name>' '<book title>'`. It will then open browser tabs for bookfinder.com, amazon.com and bookdepository.com, and search all sites for the given book.

The script starts by retrieving the author and title from the command's arguments on the CLI. `sys.argv` increments from 0 up, where 0 represents the command, 1 represents the first argument, and so on. An `if` statement is used for cases when no arguments are provided on the command line. See further below for what happens in that case.
```python
if len(sys.argv) > 1:
    # get book author from CLI
    author = sys.argv[1]
    # get book title from CLI
    title = sys.argv[2]
```
Next, a browser tab is opened, and the search strings are added to the site URLs, using the various formatting required by each website:
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
### Thesaurus time-saver
This script works in the same way as the book script above. In this case, the user inputs `thes '<word>'` into the terminal and a browser opens thesaurus.com and searches for the word (shebang and library imports are the same as for the first script):
```python
if len(sys.argv) > 1:
	# get word from command line
	word = sys.argv[1]
else:
	# get word from clipboard
	word = pyperclip.paste()

webbrowser.open('https://www.thesaurus.com/browse/' + word)
```
### Google Maps time-saver
For this script, the user inputs `map '<address>'` and is taken to a Google Maps page for the address.
```python
if len(sys.argv) > 1:
	# get address from command line
	address = ' '.join(sys.argv[1:])
else:
	# get address from clipboard
	address = pyperclip.paste()

webbrowser.open('https://www.google.com/maps/search/' + address)
```
### Editing the `PATH` environment variable
After developing the scripts in this article, I copied the executable files to `~/bin`. This is the folder I use to store scripts I have written myself. I then added the necessary packages to my system version of Python 3. This means I can run the scripts from any folder in my terminal. It also means I don't have to specify the particular binary inside a virtual environment, meaning I could send the script to a friend and they would not have to edit it before use.

The `PATH` environment variable tells the system where to look for executables when a terminal command is entered. Because I want to run my scripts out of `~/bin` , I needed to add that location to my `$PATH`. The below command adds a line to my zsh configuration file, amending my existing path by adding on the new folder:
```bash
echo 'export PATH=$HOME/bin:$PATH' >> ~/.zshrc
```
### Making scripts executable
In order to run a script from the command line by entering only the script’s name, the script must have its default user permissions changed, so that all users are allowed to execute the script. This is done with the `chmod` command:
```bash
chmod +x script_name.py
```
### Conclusion
The process of writing these scripts was a lot of fun. I increased my knowledge of virtual environments, web scraping with Python, Unix terminal commands and file permissioning. And I use the scripts every day, so it was time well-spent.
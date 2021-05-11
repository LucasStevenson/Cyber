# To be, or not to be

> Points: 100

> Category: programming

## Description

Suffer the slings and arrows of outrageous figlet.

## Attachments

`nc tirefire.org 11051`

## Solution

```sh
$ nc tirefire.org 11051

This above all: to thine own self be true
(and me too, 250 times in 90 seconds or less)
 _                   _
| |    __ ___      _( )___
| |   / _` \ \ /\ / /// __|
| |__| (_| |\ V  V /  \__ \
|_____\__,_| \_/\_/   |___/


And it must follow: Law's

__      ___ __ ___  _ __   __ _
\ \ /\ / / '__/ _ \| '_ \ / _` |
 \ V  V /| | | (_) | | | | (_| |_
  \_/\_/ |_|  \___/|_| |_|\__, ( )
                          |___/|/

And it must follow: wrong,
          _
__      _| |__   ___
\ \ /\ / / '_ \ / _ \
 \ V  V /| | | | (_) |
  \_/\_/ |_| |_|\___/


And it must follow:
```

As we can see, the challenge here is to read the ascii-art text and input it's normal text 250 times in less than 90 seconds. Obviously this can't be done by hand, so we need to write a script that can quickly automate it for us.

After searching up the name of the challenge (to be or not to be), we can see that _"To be, or not to be" is the opening phrase of a soliloquy given by Prince Hamlet in the so-called "nunnery scene" of William Shakespeare's play Hamlet, Act 3, Scene 1._ The [wikipedia page](https://en.wikipedia.org/wiki/To_be,_or_not_to_be) on it has the entire text. The third line ("_The slings and arrows of outrageous fortune,_") looks like the description of this challenge EXCEPT for the last word. The word "fortune" is replaced by "figlet", which refers to the **pyfiglet** module. I used this [geeksforgeeks article](https://www.geeksforgeeks.org/python-ascii-art-using-pyfiglet-module/) to learn how to use it.

Now the plan is simple:

1. Because we already know where the server is getting the words from (Hamlet, Act 3, Scene 1), we can take each word and run it through pyfiglet.

2. Save the original word and it's ascii-art text form to a dictionary. Key value pair will look like `{ word: ascii-art-word }`

3. Read the ascii-art text the server is giving us, and get the corresponding normal plaintext by checking the dictionary

4. Put the word into the input field, and repeat until we get the flag.

Below is a python script that does the steps described above

```py
import pyfiglet
from pwn import *

def get_key(val):
    return list(table.keys())[list(table.values()).index(val)]

poem = """
To be, or not to be, that is the question:
Whether 'tis nobler in the mind to suffer
The slings and arrows of outrageous fortune,
Or to take Arms against a Sea of troubles,
And by opposing end them: to die, to sleep;
No more; and by a sleep, to say we end
The heart-ache, and the thousand natural shocks
That Flesh is heir to? 'Tis a consummation
Devoutly to be wished. To die, to sleep,
To sleep, perchance to Dream; aye, there's the rub,
For in that sleep of death, what dreams may come,
When we have shuffled off this mortal coil,
Must give us pause. There's the respect
That makes Calamity of so long life:
For who would bear the Whips and Scorns of time,
The Oppressor's wrong, the proud man's Contumely,
The pangs of dispised Love, the Law's delay,
The insolence of Office, and the spurns
That patient merit of th'unworthy takes,
When he himself might his Quietus make
With a bare Bodkin? Who would Fardels bear,
To grunt and sweat under a weary life,
But that the dread of something after death,
The undiscovered country, from whose bourn
No traveller returns, puzzles the will,
And makes us rather bear those ills we have,
Than fly to others that we know not of?
Thus conscience does make cowards of us all,
And thus the native hue of Resolution
Is sicklied o'er, with the pale cast of Thought,
And enterprises of great pitch and moment,
With this regard their Currents turn awry,
And lose the name of Action. Soft you now,
The fair Ophelia? Nymph, in thy Orisons
Be all my sins remember'd.
""".replace("\n", " ").split(" ")

table = {}
table = { i: pyfiglet.figlet_format(i, font = "standard") + "\n" for i in poem if i not in table }

r = remote("tirefire.org", 11051)
r.recvuntil(")\n")
while 1:
    try:
        word = r.recvuntil("\n\n").decode()
        print(word)
        print(get_key(word))
        r.sendline(get_key(word).encode())
        r.recv()
    except Exception as e:
        break
print(r.recvline())
```

### Flag

> ictf{haml3t_or_figlet_th4t_is_th3_question}

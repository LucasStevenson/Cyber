# sources-adventure

> Points: 150

> Category: Web/Crypto

## Description

The ICTF employee login portal has been improperly managed for years now. Can you find the flag?

## Attachments

https://sources-adventure.max49.repl.co/

## Solution

#### Tools used

1. [z3](https://github.com/Z3Prover/z3) to find **p** and **q**
2. [mod inverse calc](https://www.dcode.fr/modular-inverse) to find **d**
3. [mod exponentiation calc](https://www.dcode.fr/modular-exponentiation) to find **m**
4. [JS deobfuscater](http://deobfuscatejavascript.com/)

When we open the website's source code we can see this comment

```html
<--Remember to hide logins.txt-->
```

When we go to https://sources-adventure.max49.repl.co/logins.txt, there's a list of usernames. In the source code, there's a list of corresponding passwords.

```
usernames = {"random123", "abrink", "umask", "ictf{n0t_th3_fl@g}", "adminguy",
"bademployee"}

<--passwords = {"funny321", "ictf{not_the_flag}", "077",
"ruth123", "Pa$$w0rd10", "we_really_need_to_fire_you"}-->
```

Logging in as **adminguy**, we are greeted with quite a few alerts telling us about what's new about the website. After clicking through all the alerts, we see a notification.

```
New message from boss#1526

Note to adminguy#5132: whenever you next log in, make sure to secure companyinfo.txt

If this is not done by the morning, don't bother showing up to work tomorrow
```

The URL redirects to a youtube video named "Jebaited Song", but right before it loads the youtube page, we can see a bunch of text. Naturally, I thought there would be something important in that block of text, but it turned out to just be the repl.it TOS. (I found out by doing `curl -Lv https://sources-adventure.max49.repl.co/companyinfo.txt`. _Ngl I spent an embarrassingly long time going through it to find a possible hint_ 😅)

Going back to the main page, I noticed a **success.js** file which had obfuscated code in it.

```js
eval(
  (function (p, a, c, k, e, d) {
    e = function (c) {
      return c;
    };
    if (!"".replace(/^/, String)) {
      while (c--) {
        d[c] = k[c] || c;
      }
      k = [
        function (e) {
          return d[e];
        },
      ];
      e = function () {
        return "\\w+";
      };
      c = 1;
    }
    while (c--) {
      if (k[c]) {
        p = p.replace(new RegExp("\\b" + e(c) + "\\b", "g"), k[c]);
      }
    }
    return p;
  })(
    '60 11(){9 19="23 26 10 14 10 33 32 3 4 31.34 29";0("28 27 12 2 1!");0("7 24 25 15 22 3 21 20 6\'18 17 16 3!");0("30 1 8 36 49 59 58 57 56 6 35 4 55 54 2 53 8 52 51 50.");9 5=48("37 6 47 2 1 46 45?","44 13 5 43");0("42! 7 41 40 13 5 12 2 39");0("38 4 1!")};11();',
    10,
    61,
    "alert|website|our|in|the|review|you|We|has|var|may|successful_login|to|your|or|new|logged|last|ve|info|since|store|things|The|have|many|flag|back|Welcome|file|Our|javascriptflag|be|not|txt|appreciate|never|Do|Enjoy|developers|send|will|Great|here|Type|good|looks|think|prompt|looked|this|into|put|team|work|hard|hope|we|and|better|function".split(
      "|"
    ),
    0,
    {}
  )
);
```

After deobfuscating it with an [online tool](http://deobfuscatejavascript.com), we get this

```js
function successful_login() {
  var info = "The flag may or may not be in the javascriptflag.txt file";
  alert("Welcome back to our website!");
  alert("We have many new things in store since you've last logged in!");
  alert(
    "Our website has never looked better and we hope you appreciate the hard work our team has put into this."
  );
  var review = prompt(
    "Do you think our website looks good?",
    "Type your review here"
  );
  alert("Great! We will send your review to our developers");
  alert("Enjoy the website!");
}
successful_login();
```

https://sources-adventure.max49.repl.co/javascriptflag.txt brings us to the crypto part of the challenge

```
Congrats for making it this far! Now, you're really close to getting the flag... there's just one more step...

>> p and q are the roots of: x^2 - 1193466094530100901475895368534405836196484x + 241510240104661323964376801789257464125939916664383218559230902825798380145013495683

>> e = 65537

>> c = 43677751328737341941473102215716106126803393903674451929850781476645362591571674826
```

To solve for **p** and **q**, we'll use [z3](https://github.com/Z3Prover/z3)

```py
# Python3
from z3 import *
PplusQ = 1193466094530100901475895368534405836196484
n = 241510240104661323964376801789257464125939916664383218559230902825798380145013495683
p, q = Ints("p q")

solve(
    p>2,
    q>2,
    p*q == n,
    p+q == PplusQ,
)
```

Running the program gives us p and q.

```
[p = 935229856103142174425095485455569202766851,
q = 258236238426958727050799883078836633429633]
```

Now we have enough information to find **d**. Once we find it, we can find **m**.

```
totient(n) = (p-1)(q-1) = 241510240104661323964376801789257464125938723198288688458329426930429845739177299200

d = e^-1 mod totient(n) = 81274827128313860873008077322155017338260502313780872239322146435610880079612361473

m = c^d mod N = 727364659517628673504689921807290703715746367536540467097298748484778365
```

Decode **m** and get the flag

```py
m = hex(727364659517628673504689921807290703715746367536540467097298748484778365)[2:]

print(bytes.fromhex(m).decode())
```

### Flag

> ictf{rsa_wa$_ju$t_unn3c3$$ary}

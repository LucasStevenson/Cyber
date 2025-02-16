# sources-adventure-hardened

> Points: 100

> Category: Web

## Description

The boss isn't happy that you all were able to retrieve his flag last time. Now, with the new features he's implemented, he's sure no one will ever get his flag now.

(NOTE: this challenge does not require the use of enumeration tools or injections)

## Attachments

https://sources-adventure-hardened.max49.repl.co/

## Solution

The first part of this challenge is to login as an employee. The source code shows these comments/messages

```html
<!-- finally figured out how to hide logins.txt boss -->
<!-- the logins are now stored in a much safer place -->
<!-- where are they stored now? -boss -->
<!-- I don't want to reveal the exact location just in case we're being tracked, but the file can't be requested by search engine crawlers anymore, making it SUPER secure ;) -->
<!-- Good work admin! This will be reflected in this month's paycheck -->
<!-- thanks boss -->
```

The middle message says that the file can't be requested by search engine crawlers. This means that it's in `/robots.txt`

We can see a disallowed `/classified_info` route. https://sources-adventure-hardened.max49.repl.co/classified_info shows us the employee login info we need to login

```
Employee Login Information:

"gdrake": "empl0y33_2",
"abrink": "g00d_emp10y33",
"mrardul": "I_Love_Roos",
"hgreen": "ruth123",
"almostadmin": "Pa$$w0rd10",
"bademployee": "getting_fired_tomorrow"
```

Signing in as `almostadmin` (it doesn't matter which one you choose) prompts us with some alerts just like last time. After clicking through them, we see a notification from the boss that tells us we're fired and are going to be replaced with [rooYay2](https://sources-adventure-hardened.max49.repl.co/rooYay2resume).

We can see 2 html comments on rooYay2's resume page

```html
<!--rooYay2 will be hired. Make his employee password his favorite emoji please. It will be easier for him to remember (case sensitive) -->
<!-- also create a custom employee portal for rooYay2 based on his interests please -->
```

We can see that his favorite emoji is `rooNoBooli` by scrolling over his profile picture. We can also see that cookies are his favorite snack. This is a hint for later on.

Now we have enough information to log into rooYay2's account (username=rooYay2, password=rooNoBooli). After logging in, we can see a new notification from the boss welcoming him and telling him about the company policies.

The 4th rule tells him that he's "`responsible for all rules hidden throughout our website, as well as in between the lines ;)`". There's a div tag with id="lines" that has a commented out p tag that just says "`It's time for the sources adventure`". _(that was kinda anticlimatic)_

The previous step referenced cookies, so let's check the website's cookies

> "adminID=320e00e73a7d24a9a44cde87e98d150e10e72b05ccffb74e7332354fdd85c5e5; adminPass=d24b240bef19d324d1ae19691591fbd942b6014ff750e0c501ad6f4d1b127ddd"

These look like SHA256 hashes (their length of 64 is a major hint). Putting them into [crackstation](https://crackstation.net/), we get

| Hash                                                             | Type   | Result            |
| ---------------------------------------------------------------- | ------ | ----------------- |
| 320e00e73a7d24a9a44cde87e98d150e10e72b05ccffb74e7332354fdd85c5e5 | sha256 | panel             |
| d24b240bef19d324d1ae19691591fbd942b6014ff750e0c501ad6f4d1b127ddd | sha256 | "991560128"licypz |

Logging in with those credentials (`username=panel`, `password="991560128"licypz`), we see a notification from the boss. He's thanking the admin for his good work and at the end, he says "`As always, the company salaries are stored at the payroll page. Have a nice day!`"

https://sources-adventure-hardened.max49.repl.co/payroll brings us to a page with a json file that we have to download. In this file, there are parts of the flag that we have to string together. Below is a python script that does just that.

```py
vals = eval(open("payrollinformationictf.json", 'r').read())
arr = []
for i in vals:
    arr.insert(0,i["ID Code"])
print(''.join(arr))
```

### Flag

> ictf{d3v3l0p3r_t00ls_ar3_gr3at!\_6c6f6c}

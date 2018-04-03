# Regular Expressions (RegEx) Tutorial
Notes taken from the 16-part YouTube [Tutorial](https://www.youtube.com/playlist?list=PL4cUxeGkcC9g6m_6Sld9Q4jzqdqHd2HiD) on RegEx by [The Net Ninja](https://www.youtube.com/channel/UCW5YeuERMmlnqo4oq8vwUpg). Please do check out his channel. The content I've watched so far has been really well-explained.
These notes are my interpretation of the video tutorials, and the plan is to flesh out these notes with information and examples from other resources (e.g. MDN, freeCodeCamp, etc.)

## Contents
1. Intro: "what is RegEx?"
2. Simple RegEx patterns
3. Character sets
4. Ranges
5. Repeating characters
6. Metacharacters
7. Special characters
8. Starting & ending patterns
9. Alternate characters
10. Creating a form
11. Making RegEx in JS
12. Telephone RegEx
13. Testing a RegEx pattern
14. Matching a username
15. Email regex pattern
16. "Finishing touches"


## 1. Intro: What Is RegEx?
Regular expressions (RegEx or RegExp for short) are objects that allow us to check a series of characters for matches. One example of when this can be useful is in form input validation: e.g. checking the email address that a user has entered contains an "@" sign and ends with a valid TLD (.com, .org...)
RegEx is something that many developers might not be too comfortable with, and may try to avoid. It's useful, powerful, and we can definitely get to grips with it :).

Further down, we will work on creating a feedback form with input validation. Relevant files will be added to this repo.

### Example
Example of a relatively complex-looking RegEx that these notes will help you to understand:
```javascript
/^([a-z\d\.-]+)@([a-z\d-]+)\.([a-z]{2,8})(\.[a-z]{2,8})?$/i
```
Spoiler: the above could be used to match a valid email address. See my example and notes on it at [this regex101 page](https://regex101.com/r/9FwTS7/3). See if you can figure out what needs to be changed in the RegEx to accept the final email listed (under "LIMITATIONS (false negative)").


## 2. I: RegEx101
TheNetNinja also recommends [regex101.com](https://regex101.com) to practice on. Note to change your "flavour" (left menu) to JavaScript, as the site defaults to python.

### RegEx101 Flags
Also note that to implement RegEx flags (to be discussed later) on this site, you should click the option at the right hand side of the RegEx input field, rather than typing them in manually. 

For now, you should ensure that all RegEx flags are unticked (so untick the default global flag).

## 2. II: Simple RegEx Patterns
Let's create a RegEx that looks for the word "ninja":
```javascript
/ninja/
```
The above regex would match "ninja" on its own, regardless of whether any other characters appear before or after the word. It will only match the first instance of "ninja" that it comes across, too. To see this in action, you can type in your own examples in the "test string" field on RegEx101.

### A couple of flags
#### Global
To match all instances of the word ninja that appear, we need to add the global flag. On RegEx101, you can select this at the right hand side of the RegEx input field. In javascript, it would look like the following: 
```javascript
/ninja/g
```

#### Case-insensitive
The RegEx above will now match every instance of the word "ninja", but it's case sensitive, so would not match "Ninja", "NINJA", "ninJa" etc. To get around this, we could use the "i" flag:
```javascript
/ninja/gi
```

### Other flags, to be covered in more detail in an update
| Flag | Description                     |
|:----:| ------------------------------- |
| m    | Multiline: match the beginning and end of each line, not just the beginning/end of the whole input. |
| u    | Unicode: treat the pattern as a sequence of Unicode code points |
| y    | Sticky: TBD |

### Bonus: RegEx object literals vs constructors
This will be revisited in more detail, but to understand the example below, it's useful to know the difference between a RegEx literal and a RegEx constructor.

#### RegEx literals
The above examples, where the expression (pattern) is placed between forward slashes, and the flags (modifiers) come directly after, are RegEx literals. [According to MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp), the literal notation is compiled when the expression is evaluated, and won't be recompiled, e.g.  if you have a loop where the RegEx is supposed to change on each iteration, a literal isn't appropriate. Conversely, if you know the RegEx will be static, using a literal can have performance benefits over an object.
```javascript
var domRegEx = /dom/gm;
```

#### RegEx ("RegExp") constructors
Constructors (used in the bonus example below) use quotation marks instead of forward slashes as the delimiter, and the flags appear as a second, optional, argument, also in quotation marks:
```javascript
var domRegEx = new RegExp("dom", "gm");
```
Constructors are required if you want to dynamically assemble a regular expression, as they are compiled when called (i.e. at runtime) rather than at load time (see "Literal Versus Constructor" [here](https://www.safaribooksonline.com/library/view/speaking-javascript/9781449365028/ch19.html)).


### Bonus: RegEx in use
There are plenty of "production" examples to come, but - to whet your appetite - here's an example of the RegEx we've used so far in use in a function:
```javascript
function count(string,char) {
 var re = new RegExp(char,"gi");
 return string.match(re).length;
}

var str = 'I will practice survival skills';
console.log(count(str,'i'));
```
In brief, the above function takes a string (in this case "I will practice survival skills" as the variable "str", and does a case-insensitive count of the number of times a character (in this case "i") appears in the string.

Thanks to Dave McFarland from the Team Treehouse forums for the code snippet above ([source](https://teamtreehouse.com/community/how-to-count-the-number-of-times-a-specific-character-appears-in-a-string)).
I will be updating (and adapting) the above to explain it in more detail. More TBD in the following chapters, too.


## 3. Character sets
RegEx is much more than just a tool to find specific words (e.g. "ninja"). We may want to match a variety of different characters in the same position, for example. We can do this with character sets.

### Creating a character set
To create a character set, we need to use \[square brackets].

E.g. if we wanted a regex pattern to match either "ninja" or "ginja", we could use the following RegEx:
```javascript
/[ng]inja/;
```
The above would match "ninja" or "ginja", but not "binja" or "winja", for example.

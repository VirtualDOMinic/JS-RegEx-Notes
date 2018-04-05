# Regular Expressions (RegEx) Tutorial
Notes taken from the 16-part YouTube [Tutorial](https://www.youtube.com/playlist?list=PL4cUxeGkcC9g6m_6Sld9Q4jzqdqHd2HiD) on RegEx by [The Net Ninja](https://www.youtube.com/channel/UCW5YeuERMmlnqo4oq8vwUpg). Please do check out his channel. This is a good introduction to JS RegEx, and involves a nice, working example.
These notes are my interpretation of the video tutorials, and the plan is to flesh out these notes with information and examples from other resources (e.g. MDN, freeCodeCamp, etc.)

## Author note
This is currently a draft, with plenty of "bonuses", notes and "previews". Once all 16 videos have been written up, I will be improving the ordering of the below write-up.

## Contents
1. [Intro: "what is RegEx?"](#1.-Intro:-What-Is-RegEx?)
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

If we wanted to match 1000, 2000, 3000 and 4000, but nothing else, we could use:
```javascript
/[1234]000/;
```

#### Bonus: ranges & quantifiers (preview)
Character sets support ranges, signified by a dash, e.g. ```[1-4]``` for the numbers 1 to 4 or ```[a-z]``` for all the letters in the alphabet (lower-case only).

So, for the previous example, we could do the following instead:
```javascript
/[1-4]000/;
```

...and we could even add the "zero or one" quantifier, ```?``` to a comma in order to not only match 1000, 2000, 3000, 4000, but also allow for a comma before the thousands separator (AKA the comma), e.g. "2,000":
```javascript
/[1-4],?000/;
```

### Exclusion sets (negated character sets)
To match all of the thousands (from 1000 to 9000) except for "5000" and "6000", we *could* use the regex ```[1234789]000``` or ```[1-47-9]000```, but there's a more efficient solution: the caret ```^```.

When used at the start of a character set (immediately after the opening square bracket), a caret (^) signifies that we want to match anything *except* for what's in the character set, so we could do the following to get the same result as above:
```javascript
/[^56]000/;
```
However, this would also match "a000" or "0000", for example, as "a" and "0" are not 5 or 6. To avoid this issue, we could add all non-digit characters to the exclusion set with the metacharacter ```\D```, and add "0", too:
```javascript
/[^56\D0]000/;
```

## 4. Ranges (continued)
To match any five letter word ending with "inja", we could use the regex ```/[a-z]inja/```, where "a-z" is the range denoting all lower-case letters from 'a' to 'z'. This range could be made to match only 'c' to 'm' as the first character by using ```/[c-m]inja/```. Be aware that your range must go from first to last (or low to high), so ```/[m-c]inja/``` would result in an error, for example.

To match any five letter word ending with "inja" (as above) but also allow the first letter to be upper-case, you can use two ranges in the character set: ```/[a-zA-Z]inja/```. Using in case-insensitive flag "i" would also work ```/[a-z]inja/i```, but this would make the entire RegEx case insensitive, meaning it would match "FINGA", "ninjA", etc.

#### Example: matching a UK phone number 
See below for a basic example of using ranges to match something like a UK telephone number (11 digits, 0-9):
```javascript
/[0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9]/;
```
"Surely there must be a better way to do this??" you ask! See below.

## 5. Repeating characters (quantifiers)
We can use quantifiers to match a target character/set a particular number of times. For example, ```?``` I used here ```/[1-4],?000/;``` is a quantifier that looks for 0 or 1 of the specified character.

The ```+``` quantifier matches between one and unlimited repetitions of the target character, e.g. ```/[0-9]+/``` would match a number of any length.

In order to specify the exact number of repetitions to match, we can use curly braces ```{}```, and can therefore match any 11 digit number, while avoiding matches for numbers of any other length, like so: 
```javascript
/[0-9]{11}/;
```

Curly braces also accept a length range in the format ```{min,max}```, so to match any number between 11 and 13 digits long, add a comma (no spaces!) after the 11 and add a 13 to get ```/[0-9]{11,13}/```.

#### Bonus: lazy vs greedy
By default, the length/repetition range in curly braces is *greedy*. This means that it will match as many characters as possible. As an example, ```/[0-9]{2,5}/``` would match the entirety of "12345" or "99999" rather than just the "12" or "99". In order to make this "lazy" (match as few as possible, in this case two), use a question mark ```?``` immediately after the curly braces, like so: ```/[0-9]{2,5}?/```.

## 6. Metacharacters


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

## 1. Intro: what is Regex?
Regular expressions (RegEx or RegExp for short) allow us to check a series of characters for matches. One example of when this can be useful is in form input validation: e.g. checking the email address that a user has entered contains an "@" sign and ends with a valid TLD (.com, .org...)
RegEx is something that many developers might not be too comfortable with, and may try to avoid. It's useful, powerful, and we can definitely get to grips with it :).

Further down, we will work on creating a feedback form with input validation. Relevant files will be added to this repo.

Example of a relatively complex-looking RegEx that these notes will help you to understand:
```javascript
/^([a-z\d\.-]+)@([a-z\d-]+)\.([a-z]{2,8})(\.[a-z]{2,8})?$/i
```
Spoiler: the above could be used to match a valid email address. See my example and notes on it at [this regex101 page](https://regex101.com/r/9FwTS7/3).

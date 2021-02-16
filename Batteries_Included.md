# Batteries Included
## Modules
### Modules are programs
- tell the interpreter where to look for the module
	- `sys.path.append('path/to/module')`
- import the module
	- a new directory *\_\_pycache\_\_* will be created when importing a module
	- it contains files with processed files that Python can handle more efficiently
	- Python will give preference to *\_\_pycache\_\_*, unless the .py file has changed
	- modules are executed when imported first time and *only for the first time*
	-

## Exploring Modules
### What's in a module?
- *dir* function
	- lists all attributes (functions, classes, variables of a module) of an object
	- filter out private attributes `[n for n in dir(copy) if not n.startswith('_')]`
- *\_\_all\_\_* variable
	- is set in the module itself
	- defines the public interface of the module
	- tells the interpreter what does `from module import *` mean
	- if not set, all global names in the module that don't begin with `_` will be imported
### Getting help
- examine docstring with *\_\_doc\_\_*
- *help(module)* is based on *module \_\_doc\_\_*
- *Python Library Reference* <https://docs.python.org/library>
- find the source code with *\_\_file\_\_*
- some modules don't have any Python source code since:
	- they may be built into the interpreter (e.g. *sys*)
	- they may be written in C language

## The Standard Library: A Few Favorites
### sys
### os
### fileinput
### Sets, Heaps and Deques
### time
### random
### shelve and json
### re
#### definition of regular expression
a *regular expression* is a pattern that can match a piece of text
#### applications
searching, replacing, splitting text with certain pattern
#### basic knowledge:
 - the wildcard
	 - period `.` matches any character except a *newline*
 - escaping
	 - to make a reserved character behave like a normal one
	 - reserved characters include `( ) [ ] { } . * + ? - ^ $ \ |`
	 - `'\\?'` or `r'\?'`
 - character sets
	 - match any character it contains
	 - match only once
	 - no need to escape reserved characters except the following conditions:
		 - caret `^` appears at the beginning of the character set, or else, an inverse
		 - right bracket`]` and dash `-` aren't put at the beginning of the character set

 - alternatives and subpattern
	 - subpattern is a part of the entire pattern enclosed in parentheses `'en(sub)tire'`
	 - use choice opterator `|` on subpatterns `'(sub1|sub2)'`
 - repetition opterators
	 - (pattern)?: pattern is optional
	 - (pattern)*: pattern is repeated zero or more times
	 - (pattern)+: pattern is repeated one or more times
	 - (pattern){m,n}: pattern is repeated from m to n times
		 - {m}: repeated m times
		 - {m,}: repeated more than m times
 - anchor
	 - caret `'^'` marks the beginning
	 - dollar sign `$` marks the end

#### Contents of the re Module
- *compile(pattern[, flags])*
	- creates a pattern object from a string with a regular expression
	- pattern objects can be used for more efficient matching
	- they have searching/matching functions as methods
	- they can be used in the normal re function
- *search(pattern, string[, flags])*
	- searches for pattern in string
	- return a MatchObject or None
- *match(pattern, string[, flgas])*
	- tries to match a regex at the beginning of a given string
- *split(pattern, string[, maxsplit=0])*
	- splits a string by the occurrences of a pattern
	- more flexible than the string method *split*
	- *maxsplit* argument indicates the maximum number of splits allowed
- *findall(pattern, string)*
	- returns a list of all occurrences of the given pattern
- *sub(pat, repl, string[, count=0])*
	- substitutes the leftmost, nonoverlapping occurrences of a pattern with a given replacement
	- often combined with group numbers
- *escape(string)*
	- escapes all the characters in a string that might be interpreted as a regex operator

#### Match Objects and Groups
- MatchObject objects contain information about where the substring matched the pattern
- group is a subpattern
- groups are numbered by their *left* parentheses
	- `'There (was a (wee) (cooper)) who (lived in Fyfe)'`
	- 0 `There was a wee coopter hwo lived in Fyfe`
	- 1 `was a wee cooper`
	- 2 `wee`
	- 3 `cooper`
	- 4 `lived in Fyfe`
- group 0 is the entire pattern and you can have 1-99 groups
- important methods of MatchObject objects:
	- *group([group1, ...])*
		- retrieves the occurrences of the given subpattern
	- *start([group])*
		- returns the starting position of the occurrence of a given group
	- *end([group])*
		- returns the ending position (*exclusive*) of the occurrence of a given group
		- namely, ending index plus one
	- *span([group])*
		- returns both the beginning and ending positions of a group
#### Group Numbers and Functions in Substitutions
- `'\\n'` in the replacement string are replaced by the string matched by goup `n` in the pattern
- the replacement can be a *function* whose only parameter is the MatchObject, and its return will be used as replacement

#### Greedy and nongreedy pattern
- default *greedy*, match as much as possible
- *nongreedy*, put a question mark `?` after repetition opterators
#### Practice: a sample template system
```python
# make a template system that can replace all '[somthing]' with the
# result of evaluating or executing somthing as an expression in Python
# for example:
# 'The sum of 7 and 9 is [7 + 9].'
# should be translated as:
# 'The sum of 7 and 9 is 16'
import fileinput, re
pattern = re.compile(r'\[(.*?)\]')
scope = {}
def replacement(match):
    code = match.group(1)
    try:
        return str(eval(code, scope))
    except SyntaxError:
        exec(code, scope)
        return ''

lines = []
for line in fileinput.input():
    lines.append(line)
text = ' '.join(lines)

print(pattern.sub(replacement, text))
```




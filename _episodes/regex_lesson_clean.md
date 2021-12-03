# Regular Epxressions

## Objectives

   - Practice thinking about text computationally
   - Recognize common regex patterns
   - Apply regexes to bibliographic metadata

## Thinking like a computer
(5-10 min)

In this exercise, we will simulate a very basic regular expression using index cards and three volunteers. 

## Case study: Regexes and bibliographic imprints
(Intro: 5 min)

### Common regex tasks

  - Finding patterns in textual data
  - Extracting elements from text (e.g., names, email addresses, URL's)
  - Correcting/standardizing elements in text

### Imprint statements

  - Provide information about a work's publisher (and/or printer/distributor), place of publication, and date of publication 
  - Derived from the information printed on the work itself
  - Can vary widely in how the same entities are represented 

The following are examples from the imprint fields of GW catalog records for books published by Oxford University Press:
```
Oxford Univ
Oxford Univ Pr
Oxford Univ Press
Oxford Univerity Press
Oxford University P
Oxford University Press (UK)
Oxford University Press / UK
Oxford University Press (USA); Clarendon Press
Oxford University Press Inc
Oxford University Press UK
Oxford University Press US
Oxford University Press USA
```

### Imprint statements from the Columbian College collection

#### Pattern #1: Matching between punctuation 
(10-15 min)

How would we extract the publisher, place of publication, and publication date from the following imprint statement?

`Londres : B. Bensley, 1821`

  - We can use the period metacharacter to match any character whatsoever: `.`
  - Appending the plus sign to any regex pattern will match *one or more* instances of that pattern.
  - So `.+` will match any string containing one or more characters.
  - We can use parentheses to create _capture groups_. These are useful for extracing parts of a larger string.

_Answer_: `(.+) : (.+), (.+)`
Note that in our pattern the period is a special regex character, called a metacharacter, not a regular period.  

_Exercise_: Verify for yourself that this pattern will match the string above, and that the groups will capture the elements as desired. If it helps, step through the string character by character, as we did in the demonstration.

#### Pattern #2: Matching character classes
(5-10 min)

Let's test our regex against another imprint:

`Andover : Flagg and Gould, printers, Published and sold by Mark Newman ; 1818`

For testing regexes, many tools are available. [regex101.com](https://regex101.com/) has a helpful interface for visualizing what's going on. 

_Exercise_: Compare our previous imprint statement with this one using the pattern from above. What do you notice? What's going on here?


------------------------------------------------------

  - Tradeoffs: the more generic the pattern, the more strings it will match, but the more false positives you'll encounter. 
  - We can limit our match to specific _character classes_ by using additional regex metacharacters. 
  - Many metacharacters are preceded by a _backslash_ (`\`).
  - `\d` will match a single digit (0-9).
  - We can specify an exact number of characters between *curly braces*: `{4}` will match on four consecutive instances of the immediately preceding pattern.


_Exercise_: 
`(.+) : (.+), (\d{4})`

Try this new pattern against the Flagg and Gould imprint in [regex101](https://regex101.com). Why do you think it doesn't work? 


#### More character classes
(5 min)

To fix it, we can make use of another regex pattern to match on a fixed set of characters.

  - Put one or more characters between *square brackets* to match any one of those characters (and nothing else). `[12]` will match only the numeral 1 *or* the numeral 2. 
  - The entire expression between square brackets represents *a single character*. 
  - You can include a range for alphanumeric characters. 
    - `[0-9]` will match any digit (and is equivalent to `\d`). 
    - `[A-Za-z]` will match any alphabetic character (from the Latin alphabet), uppercase or lowercase (not including diacritics).

_Exercise_: Test the following regex against both imprint statements: `(.+) : (.+)[,;] (\d{4})`

#### Pattern #3: Optional characters
(5 min)

_Exercise_: Test this imprint on our latest pattern: `London, D. Brown, 1722`. Why doesn't it match?


  - First version: `(.+) [:,] (.+)[,;] (\d{4})`

This also fails. Can you see why?

-------------------------------------------------

  - The `\s` metacharacter matches a space.
  - The `?` metacharacter, when used immediately after a character or character class, matches on _zero or one_ instances of the previous character/character class. 
  - So `\s?` will match when a single space is present and also when the space is absent. (It won't match on multiple spaces.)
  - Second version: `(.+)\s?[:,] (.+)[,;] (\d{4})`

_Exercise_: Test this new pattern against the three imprints we have looked at so far to verify that it works for all.

----------------------------------------------------

  - We could also replace the literal spaces in our regex with the metacharacter `\s` for consistency. It's not necessary, and it may or may not make it easier to read:

  `(.+)\s?[:,]\s(.+)[,;]\s(\d{4})`

---------------------------10 min break --------------------

### Final Exercise
(25-30 min)

Can you modify our regex to capture publication place, publisher/printer, and publication date for the following imprints?

Make sure you test your new regex against the imprints we've already used to make sure it still works on those, too.

#### Hints

  - You can negate a character class to *exclude* certain characters from a pattern: `[^:]` matches any character at all *except* a colon.
  - To match against square brackets, you need to *escape* them (in order to distinguish literal brackets from the brackets around a regex character class): `\[0\]` would match the string literal `[0]`.

1. `Philadelphia, Printed by William W. Woodward, 1817-1819`

2. `Boston : Printed by Peter Edes, in State-Street, [1785]`

3. `Newburyport [Mass.] : Printed by Angier March, 1802`

4. `Washington, D.C. : Judd & Detweiler, printers, 1870`

5. `Boston : Printed by Samuel Etheridge for J. White, Thomas and Andrews, E. Larkin, W.P. Blake, J. West and J. Boyle, MDCCXCV [1795]`


## Further reading

The Library Carpentry [lesson on regular expressions](https://librarycarpentry.org/lc-data-intro/01-regular-expressions/index.html) provides a useful reference for beginning to work with regular expressions (regex).


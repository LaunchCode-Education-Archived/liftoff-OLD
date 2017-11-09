---
title: 'camelCase - Solution'
currentMenu: 'articles'
---

## Gather Requirements/Clarify Problem
We are given a string without any spaces in it.

The string follows camel case convention. In which the first word is not capitalized, however every following word's first letter is capitalized.

For example:
```nohighlight
the_string = 'thisIsAnExample'
```

Our goal is to determine how many words are in the string, and then print that number out.

For example:
```nohighlight
input = 'thisIsAnExample'
output = 4
```
There are four words in the given string.

## Break Down Complex Pieces into Smaller Pieces
I want to hold my phrase in a list of the individual words. So I can first create an empty list.

I will then need to create an empty string that will serve as a temporary container.

We should be able to loop through the string by each individual character. We can check if the character is upper or lower case. If it's lower case add it to our temporary string. If it's upper case, add the temporary string to the list of words AND reset the temporary string to be the new uppercase character.

Finally we sould be able to return the length of the list, which will represent th enumber of words in the original string.

## Psuedocode
```nohighlight
variable the_words = []
variable temp_word = ""
for loop -> loop through each letter in the string
    if letter is lowercase
        add the letter to the variable temp_word
    else
        add temp_word to the_words
        clear out temp_word and set it to the letter

print(len(the_words))
```

## Code
```python
the_words = []
temp_word = ""
for letter in s:
    if letter.islower():
        temp_word += letter
    else:
        phrase.append(temp_word)
        temp_word = letter
print(len(phrase))
```

## Testing

Let's test it out.

##### Variables (s = "thisIsAnExample")

We can see in this string the first four letters are all lower case ("t", "h", "i", "s"), they will satisfy the if condition, and will be added to temp_word.

On the fifth letter ("I") the if statement is not true, so the else statement is executed. This adds the temp_word ("this") to the_words. It then overwrites temp_word to be just the current letter ("I").

```nohighlight
Current Variables:
the_words = ["this"]
temp_word = "I"
```

The next letter is lowercase ("s"), and is therefore added to temp_word.

The next letter is uppercase ("A"), and temp_word is added to the_words, and temp_word is set to the current letter ("A").

```nohighlight
Current Variables:
the_words = ["this", "Is"]
temp_word = "A"
```

The next letter is lowercase ("n"), and is added to temp_word.

The next letter is uppercase ("E"), and temp_word is added to the_words, and temp_word is set to the current letter ("E").

```nohighlight
Current Variables:
the_words = ["this", "Is", "An"]
temp_word = "E"
```

The remaining letters are all lowercase and are added to temp_word.

```nohighlight
the_words = ["this", "Is", "An"]
temp_word = "Example"
```

Finally, the last line runs, printing out the length of the_words.

```nohighlight
Output: 3
```
## Changes

Oops! We made a small mistake. We forgot to add the last word to the_words, and so calling len(the_words) results in the incorrect answer. We can fix this by adding temp_word to the_words after the for loop.

```python
the_words = []
temp_word = ""
for letter in s:
    if letter.islower():
        temp_word += letter
    else:
        phrase.append(temp_word)
        temp_word = letter
the_words.append(temp_word)
print(len(phrase))
```

This will change our list the_words, and allow the length function to return the correct value:

```nohighlight
the_words = ["this", "Is", "An", "Example"]
temp_word = "Example"
```

Resulting in this:
```nohighlight
Output: 4
```

Another Successful Live Coding session in which we employed all the steps, and came up with a solution to the problem!

<aside class="aside-hint" markdown="1">
There might be a better way of doing this problem!

We aren't asked to do anything with the words that we have stored in our list the_words. It might be more appropriate to just keep track of an integer, and add to the integer everytime we encounter an uppercase letter.

Many of you may have done this on your attempt with this problem. That's great! If you didn't, and are looking for another challenge try it this way.
</aside>
---
title: 'Mars Exploration - Solution'
currentMenu: 'articles'
---

For the remaining two problems we will share with you a solution we created that solves the problem, but you will be responsbile for following the steps.

If you want more practice on following all the steps, talk it over with your fellow classmates, or your mentor!

## Python3 Solution

```python
string_mistakes = 0
where_are_we = 1
for i in range(len(s)):
    if where_are_we == 1 or where_are_we == 3:
        if s[i] != "S":
            string_mistakes += 1
    elif where_are_we == 2:
        if s[i] != "O":
            string_mistakes += 1
    if where_are_we == 3:
        where_are_we = 1
    else:
        where_are_we += 1
print(string_mistakes)
```
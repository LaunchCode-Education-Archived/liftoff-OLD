---
title: 'Grading Students - Solution'
currentMenu: 'articles'
---

For the remaining two problems we will share with you a solution we created that solves the problem, but you will be responsbile for following the steps.

If you want more practice on following all the steps, talk it over with your fellow classmates or mentor!

## Python3 Solution
```python
new_grades = []
for grade in grades:
    if grade < 38:
        print(grade)
        new_grades.append(grade)
    elif (grade % 5) > 2:
        grade = grade + (5 - (grade % 5))
        print(grade)
        new_grades.append(grade)
    else:
        print(grade)
        new_grades.append(grade)
```

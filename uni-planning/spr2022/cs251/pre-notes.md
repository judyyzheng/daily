### Pre-notes
___

https://student.cs.uwaterloo.ca/~cs251/W22/index.shtml

#### Grade breakdown
- AW=20% Assignments (6 assignments equally weighted) to be submitted via Crowdmark
- CW=10% iClicker Participation, Quiz mark
- MW=20% Midterm
- FW=50% Final exam

```
Normal  = (AW*(A1 + ... + A6) + CW*ClickerQuiz + MW*Midterm + FW*Final) / (AW+CW+MW+FW)
Exam    = (MW*Midterm + FW*Final) / (MW+FW)
      
if ( Exam < 50% ) {
    Course Grade = min (Normal, 46)
} else {
    Course Grade = Normal
}
```
#### Crowdmark Assignment Submission
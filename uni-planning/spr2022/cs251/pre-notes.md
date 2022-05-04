### Pre-notes
___
Maran Ma
TTh 2:30PM - 3:50PM
MC 4021

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

#### Course outline
- ARM overview, brief discussion of performance
- Digital logic design
- Data representataion and manipulation
- + designing a datapath
- + single-cycle control unit
- Multiple-cycle control units
- + pipelining and hazards
- + memory hierarchies (**caches** and virtual memory)
- \Computer Organization and Design", David Patterson and John
Hennessy, ARM edition, 2017. REQUIRED
6 assignments, clickers, midterm, final exam
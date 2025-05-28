Prime Lookback Picker i.e. Cicada Number Picker
=============================

Often, we want to pick a time period for analysis of patient data that intentionally does not line up with common time periods for analysis. 
This way, even if the same underlying patient data is used for a report, it is difficult to conduct a subtraction attack where one report can be used in 
conjunction with another report to provide precise details about one or small number of patients. 

The inspiration for this project comes from Cicadas which have (apparently) evolved an incubation period that is difficult for predators to sync with.
[https://en.wikipedia.org/wiki/Periodical_cicadas](https://en.wikipedia.org/wiki/Periodical_cicadas)


Goal
----
Pick a prime-number “look-back window” (in days) that collides *least often*
with a set of common analytics windows **within the next decade** (3 650 days).

A *collision* means:               day  %  LCM(p, base) == 0
where  p     = candidate prime look-back
       base  = one of the base windows

We *weight* collisions by density:
If a day is a multiple of *three* base windows, it contributes 3 to the score.

The script:

1.  Lists every prime `p` such that 10 < p < 90.
2.  Counts the *weighted* collisions for each `p` versus the base windows  
    [7, 10, 30, 60, 90, 365, 730, 1095, 1825, 3650].
3.  Prints the full table and highlights the prime with the **lowest score**.

Output example
--------------
    Prime with the lowest weighted collision score = 89 (score=10)

    Prime : Weighted collision score (lower is better)
     11 : 99
     13 : 84
     ...
     89 : 10

Interpretation
--------------
• Lower score → fewer “overlap events” where analytic windows
  hit the same day.

• `89` wins because its first overlaps with the densest windows
  (7 × 89, 10 × 89, 30 × 89) are so far out that only a handful
  fit inside 10 years.

Customization
-------------
* Change `BASE_WINDOWS` to suit your organisation’s standard KPI windows.
* Change `HORIZON_DAYS` (default 3650, 10 years) if you care about a shorter/longer
  planning horizon.

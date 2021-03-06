# Genus AI back end engineer homework problem

## Problem

We'd like you to implement a version of what we internally call 'enrichment'. In more technical terms,
it is simply a join of clients' customer data to metadata that we might know about the same customers.

In this case, we have two files:
1) `fake_customers.csv`
2) `fake_db.csv`

Imagine that `fake_customers.csv` is something a client sends us. It's their paying customers that they want to know more about.
They only have the simple data themselves, `first_name`, `last_name`, `zip`.

On the other hand, we know a bit more about the same people. We have a dataset containing various datapoints, 
in this example we have `gender` and `income`.

Our client would like to know their customers better, so we want to join the two datasets and provide them
the `gender` and `income` distributions.

### Part 1 - Join

In the context of this exercise, we will match the people on three columns: `first_name`, `last_name`, `zip`.

However, there is a complication - client data source is taken from user input, so it's not necessarily clean and tidy.

We observe two problems in this dataset:
* Some clients have entered their full_name in one of the fields.
* Some other clients have mistyped their names (test dataset is built by deleting a single character in the name)

To solve this, we'd like to do a multipass join:
  1) Join all of the customers that join exactly.
  2) Take the rest, join the customers that have typed their name/surname in the wrong field
  3) Take the rest, join the customers that have a typo in their name
  
The output should be a (left) joined dataset with client columns, our columns and an extra column indicating the "quality" of the join (if a match was found).

### Part 2 - Report

Finally, we'd like to build a report about this dataset.

1) We want to see gender counts of the joined dataset.
2) We want to see counts per income bracket (0-40000, 40-60000, 60-80000, 80000+)
3) We want to see counts per first three digits of the zip (123**)

We want breakdowns on combinations of the above. Example:

```
gender,income,zip,total
M,0_40k,123,3
F,0_40k,123,5
M,0_40k,124,2
F,0_40k,124,1
...
```

Bonus: Ideally, the solution should accept the columns to report on and produce a single report. For example, if we want a report on (income,zip), then the above report would become:

```
income,zip,total
0_40k,123,8
0_40k,124,3
```

## Solution

We expect a standalone Python project that solves the problems above. It should be easily runnable on our side (provide some minimal documentation on how it
works). Bottom line - it should have two features available - one to solve problem 1 and produce a joined `.csv`
and another one that generates a report `.csv`

## Follow up

Write down some thoughts on how to go from the solution above to a bigger problem. Some questions:

  * What if the were more rows? Customer file can have up to 3 million rows, database - up to 250M.
  * What if the files were bigger? We have more metadata. Compressed db from the above is about 50GB.
  * What if the customer data was a bit more messy? What could be a good approach to deal with more typos.

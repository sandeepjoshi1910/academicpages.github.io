---
title: 'MS Apriori'

permalink: /projects/msapriori
tags:
  - python
  - apriori
  - Data Mining
  - CS583
---

MS - Apriori is an algorithm to find frequent itemsets from data with multiple minimum support. With the regular Apriori algorithm, frequent itemsets found has only one support regardless of the frequency of the itemsets. This creates two problems<br>

1. If the minimum support is very high, we miss all the rare itemsets and end up with frequent itemsets which are very obvious.

2. If the minimum support is very low, we have a problem of combinatorial explosion whierein we have very vast amount of itemsets which might not be practically useful.

The book [Web Data Mining](http://www.springer.com/us/book/9783642194597) discusses the idea of using multiple minimum supports. i.e different supports for items based on how frequent they occur in the data. For example, milk might need to have high support compared to say a washing machine. This helps us to find all the relevant frequent itemsets in the data thus adressing both the above problems. The book presents the algorithm **MS-Apriori** which I implemented along with [Amlaan Bhoi](https://abhoi.github.io)

*This was done as a project for the course CS583:Data Mining & Text Mining at University of Illinois at Chicago*
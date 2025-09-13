### EX3 Implementation of GSP Algorithm In Python
### DATE: 13/09/2025
### AIM: To implement GSP Algorithm In Python.
### Description:
The Generalized Sequential Pattern (GSP) algorithm is a data mining technique used for discovering frequent patterns within a sequence database. It operates by identifying sequences that frequently occur together. GSP works by employing a depth-first search strategy to explore and extract frequent patterns efficiently.
### Steps:
1. <strong>Database Scanning:</strong> GSP scans the sequence database to determine the support of each item in the dataset.
2. <strong>Candidate Generation:</strong> It generates a set of candidate sequences using frequent items found in the previous step.
3. <strong>Pattern Growth:</strong> It extends the candidate sequences by merging them to form longer patterns, checking their support against a user-defined minimum support threshold.
4. <strong>Repeat:</strong> The process continues until no new sequences meet the minimum support threshold.
<p align="justify">
GSP finds application in various domains such as market basket analysis, web usage mining, bioinformatics, and more. For instance, in retail, GSP can identify common purchasing patterns, helping businesses understand customer behavior for targeted marketing or inventory management.
</p>

### Procedure:
<p align="justify">
1. From collections import defaultdict, from itertools import combinations: Imports necessary libraries/modules. defaultdict is
used to create a dictionary with default values and combinations generates all possible combinations of a sequence.</p>
<p align="justify">
2. generate_candidates(dataset, k): Function to generate candidate k-item sequences from a dataset. It loops through each sequence in the
dataset and finds combinations of length k for each sequence, updating their counts in a dictionary.</p>
<p align="justify">
3. gsp(dataset, min_support): Function that implements the Generalized Sequential Pattern (GSP) algorithm. It iterates through increasing
sequence lengths (k) until no new frequent patterns are found. It calls generate_candidates() to find patterns of varying lengths.</p>
<p align="justify">
4. Example dataset for each category: Defines example sequences for top wear, bottom wear, and party wear categories.</p>
<p align="justify">
5. Minimum support threshold: Sets the minimum support count required for a pattern to be considered frequent.</p>
<p align="justify">
6. Perform GSP algorithm for each category: Applies the GSP algorithm for each category using the defined example datasets and the
minimum support threshold.</p>
<p align="justify">
7. Output the frequent sequential patterns for each category: Prints the frequent sequential patterns 
    along with their support counts
for each wear category.</p>
<p align="justify">
8. Visulaize the sequence patterns using matplotlib.
</p>
### Program:

```python

from collections import defaultdict
from itertools import combinations
# Check if candidate subsequence is in sequence
def is_subsequence(candidate, sequence):
    idx = 0
    for elem in sequence:
        if idx < len(candidate) and candidate[idx] in elem:
            idx += 1
    return idx == len(candidate)

# Generate candidates of length k
def generate_candidates(frequent_patterns, k):
    candidates = set()
    patterns = list(frequent_patterns)
    for i in range(len(patterns)):
        for j in range(len(patterns)):
            p1, p2 = patterns[i], patterns[j]
            if p1[:-1] == p2[:-1]:  # join step
                candidate = p1 + (p2[-1],)
                if len(candidate) == k:
                    candidates.add(candidate)
    return candidates

# Count support of candidates
def count_support(candidates, dataset, min_support):
    support_count = defaultdict(int)
    for seq in dataset:
        for cand in candidates:
            if is_subsequence(cand, seq):
                support_count[cand] += 1
    return {cand: sup for cand, sup in support_count.items() if sup >= min_support}

# GSP Algorithm
def gsp(dataset, min_support):
    # Collect all single items
    items = set()
    for seq in dataset:
        for elem in seq:
            items.update(elem)
    
    # L1: single items
    L1 = { (item,): 0 for item in items }
    for item in L1:
        for seq in dataset:
            if is_subsequence(item, seq):
                L1[item] += 1
    L1 = {k: v for k, v in L1.items() if v >= min_support}

    frequent_patterns = dict(L1)
    k = 2
    Lk = L1

    while Lk:
        candidates = generate_candidates(list(Lk.keys()), k)
        Lk = count_support(candidates, dataset, min_support)
        frequent_patterns.update(Lk)
        k += 1

    return frequent_patterns

dataset1 = [
    [("b","d"), ("c",), ("a","c")],
    [("b","f"), ("c","e"), ("b",), ("f","g")],
    [("a","h"), ("b","f"), ("a",), ("b",), ("f",)],
    [("b","e"), ("c","e"), ("d",)],
    [("a",), ("b","d"), ("b",), ("c",), ("b",), ("a","d","e")]
]

min_support1 = 3
result1 = gsp(dataset1, min_support1)
print("Frequent Sequential Patterns (Problem 1):")
for pattern, sup in result1.items():
    print(pattern, "=>", sup)

dataset2 = [
    [("a","b","c"), ("b","e"), ("c",), ("f",), ("g",), ("a","b","e")],
    [("a","d"), ("b","c"), ("f","g"), ("c","h")],
    [("b","c"), ("a","d"), ("e",), ("b",), ("f",), ("c","d","f","g","h")],
    [("c",), ("e","c"), ("e","h")]
]

min_support2 = 3
result2 = gsp(dataset2, min_support2)
print("\nFrequent Sequential Patterns (Problem 2):")
for pattern, sup in result2.items():
    print(pattern, "=>", sup)

```
### Output:

<img width="1094" height="595" alt="Screenshot 2025-09-13 100744" src="https://github.com/user-attachments/assets/74eb37c7-80e9-40b9-8b81-741315f8b1ce" />



### Result:

Thus, to implement GSP Algorithm In Python is executed successfully.

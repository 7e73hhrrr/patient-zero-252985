# Patient Zero — Assignment 4

**Student:** Farida Byerik  
**Student ID:** 252985  
**Group:** SE-2532

## About the Project

Patient Zero is a Java project that uses different algorithms to analyze DNA sequences and infection connections.

The assignment includes five missions covering dynamic programming, string matching, hashing, and tree algorithms.

## How to Run

The project uses Java 17 or newer.

To run it in IntelliJ IDEA, open the `patient-zero` folder, mark `src` as Sources Root, and run `Main.java`. Make sure the working directory is set to the project folder.

You can also use the terminal:

```bash
# Generate personal input data
java tools/MakeVariant.java 252985 data

# Run the checker
java tools/Check.java

# Compile the program
javac -encoding UTF-8 -d out src/*.java

# Run the program
java -cp out Main

# Run the benchmark
java -cp out Bench
```

The personal input files are already included in the `data` folder.

## My Results

After running the program with my personal data, I got these results:

```text
Mutations: 72
Shared core: 1106
Marker hits: 5
First hit: 2599
Border length: 4
Shared words: 489
Tests needed: 26
Cheapest testing: 127
Longest chain: 19
```

The visual results can also be viewed by opening `output/outbreak.html` in a browser.

![Program results](output/outbreak.png)

## Mission Explanations

### Mission 1 — Mutations

This mission finds the minimum number of changes needed to turn one DNA sequence into another.

I used the edit distance algorithm with dynamic programming.

There are three possible operations: insertion, deletion, and replacement. If two letters are already the same, no change is needed.

The algorithm compares the possible operations and chooses the one with the lowest cost.

**My result:** 72 mutations.

### Mission 2 — Shared Core

This mission finds the longest common subsequence (LCS) between two DNA sequences.

A subsequence does not need to contain consecutive letters. It can skip some letters, but their order must stay the same.

The algorithm uses dynamic programming to find the maximum number of matching letters in the correct order.

**My result:** 1106.

### Mission 3 — Marker Search

This mission searches for a specific DNA marker inside a longer sequence.

The algorithm used here is KMP (Knuth–Morris–Pratt).

KMP uses a prefix function to avoid checking the same letters again after a mismatch. This makes searching more efficient.

My marker is `GCGGTAGGCGG`.

Its longest matching prefix and suffix is `GCGG`, which has a length of 4.

**My results:**
- Marker hits: 5
- First hit: 2599
- Border length: 4

### Mission 4 — Copied Words

This mission looks for DNA words that appear in both sequences.

The program uses Rabin–Karp with rolling hashing to compare parts of the sequences.

Hashing makes it faster to find possible matches. However, different words can sometimes have the same hash value, so the program also compares the actual letters to confirm a match.

Each matching position in the first sequence is counted once.

**My result:** 489 shared words.

### Mission 5 — Whom to Test

This mission works with a tree representing connections between people.

The goal is to choose people for testing so that every connection has at least one tested person, while keeping the total testing cost as low as possible.

Dynamic programming on trees is used to compare two choices for each person: testing them or not testing them.

If a person is not tested, their connected children must be tested to cover those links.

The mission also finds the longest chain of connected people by checking the longest paths in the tree.

**My results:**
- People needed for testing: 26
- Minimum testing cost: 127
- Longest chain: 19

## Benchmark Results

I measured the running times using the provided `Bench` class. The results are in milliseconds and averaged over three runs.

| Algorithm | Input Size | Time (ms) |
|---|---:|---:|
| Naive Search | 20,000 | 13.1 |
| Naive Search | 40,000 | 21.4 |
| Naive Search | 80,000 | 36.8 |
| KMP | 20,000 | 4.1 |
| KMP | 40,000 | 2.2 |
| KMP | 80,000 | 1.5 |
| Edit Distance | 500 | 8.8 |
| Edit Distance | 1,000 | 8.8 |
| Edit Distance | 2,000 | 13.9 |
| Edit Distance | 4,000 | 39.4 |

### What I Noticed

Naive search generally took longer when the input size increased.

KMP was faster in these measurements. Its running time did not increase consistently because the measurements were short, and JVM warm-up can affect the results.

Edit distance became noticeably slower for larger inputs because it needs to calculate more cells in its dynamic programming table.

## Time and Space Complexity

| Algorithm | Time Complexity | Space Complexity |
|---|---|---|
| Edit Distance | O(a × b) | O(a × b) |
| LCS | O(a × b) | O(a × b) |
| Prefix Function | O(m) | O(m) |
| KMP | O(n + m) | O(m + h) |
| Rabin–Karp | O(a + b + Ck) | O(b) |
| Minimum Testing Cost | O(v) | O(v) |
| Longest Chain | O(v) | O(t) |

Here, `a` and `b` are sequence lengths, `n` is the text length, `m` is the marker length, `h` is the number of matches, `k` is the word length, `C` is the number of candidate comparisons, `v` is the number of people, and `t` is the tree height.

For Rabin–Karp, the complexity assumes expected constant-time hash-map operations.

## Conclusion

In this assignment, I worked with several algorithms for comparing DNA sequences, finding patterns, and analyzing connections between people.

Dynamic programming was useful for edit distance, LCS, and finding the minimum testing cost. KMP and Rabin–Karp were used for string searching, while tree traversal helped find the longest infection chain.

The program produced results for my personal input data, and the benchmark helped compare the running times of different algorithms.

## AI Use

I used ChatGPT for explanations, debugging help, and assistance with the code and report.

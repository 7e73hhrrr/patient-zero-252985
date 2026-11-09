# Patient Zero — Assignment 4

**Student ID** 252985 · **Name** Farida Byerik · **Group** SE-2532

## Run in IntelliJ or the terminal

Open the extracted `patient-zero` folder, which contains `src`, `tools` and `data`. Select JDK 17 or newer. In IntelliJ, mark `src` as Sources Root and run `Main` with the project folder as the working directory.

```bash
java tools/MakeVariant.java 252985 data
java tools/Check.java
javac -encoding UTF-8 -d out src/*.java
java -cp out Main
java -cp out Bench
```

The four personal input files are included. The generator command recreates them. Run Main after Check to refresh the console with the personal data because Check runs Main on the examples.

## Program output

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

[Open the interactive lab console](output/outbreak.html) locally after extracting the project.

![Personal lab console showing all five missions passing](output/outbreak.png)

## Mission explanations

**1. Mutations.** Cell `d[i][j]` stores the fewest changes needed to turn the first `i` letters of A into the first `j` letters of B. The top cell plus one deletes a letter, the left cell plus one inserts a letter, and the diagonal cell adds zero for a match or one for a replacement. Take the smallest of these three costs. The first row and column count the insertions or deletions needed when one prefix is empty.

**2. Shared core.** A substring uses consecutive letters. A subsequence can skip letters while preserving their order. In this variant only 369 letters match at the same index, but the LCS has length 1106 because it can align letters again after an insertion or deletion shifts their positions.

**3. Marker.** Each prefix-function entry gives the length of the longest proper prefix that also ends at that position. After a mismatch, KMP reuses the next shorter matching border and tries the current text letter again, so it never has to rewind the text pointer. The personal marker `GCGGTAGGCGG` begins and ends with `GCGG`, giving border length 4. After a full hit those four letters can start another hit.

**4. Copied words.** A hash can assign the same number to different words, so the program checks the actual letters at candidate positions in B. Once one occurrence is verified, that position in A contributes exactly one, even if B contains more copies. If many unequal windows collide, each A window may require comparisons against many B windows.

**5. Whom to test.** If person v is tested, pay their cost and take the cheaper state for each child. If v is skipped, each child must be tested to cover its link to v. The cheaper root state gives the total, and a second walk chooses the people under the same rules. A high-degree-first rule can fail even on a star: if its center costs 10 and its three leaves cost 1 each, testing all leaves covers every link for 3. For the longest chain, the two deepest branches below a person form the longest path whose highest point is that person.

## Timing

Measured with the supplied Bench class, in milliseconds, averaged over three runs.

| Method | Input sizes | Times (ms), in the same order |
|---|---|---|
| Naive search | 20,000 / 40,000 / 80,000 | 13.1 / 21.4 / 36.8 |
| KMP | 20,000 / 40,000 / 80,000 | 4.1 / 2.2 / 1.5 |
| Edit distance | 500 / 1,000 / 2,000 / 4,000 | 8.8 / 8.8 / 13.9 / 39.4 |

Naive time rose by about 1.6× and 1.7× when the text doubled. KMP took less time on the later, larger inputs in this run; short measurements and JVM warm-up can mask its linear growth. Doubling both strains creates about four times as many DP cells, though the largest measured edit-distance time rose by about 2.8×.

## Complexity

| Algorithm | Time | Extra space | Meaning and assumptions |
|---|---|---|---|
| Edit distance | O((a+1)(b+1)) | O((a+1)(b+1)) | Full table for strains of lengths a and b |
| LCS | O((a+1)(b+1)) | O((a+1)(b+1)) | One cell per prefix pair |
| Prefix function | O(m) | O(m) | Marker length m |
| KMP including prefix function | O(n+m) | O(m+h) | Genome length n, h reported hits |
| Rabin–Karp | O(a+b+Ck) | O(b) | Expected constant-time hash-map access; C candidate pairs examined, up to (a−k+1)(b−k+1) when both lengths are at least k |
| Minimum cover and reconstruction | O(v) | O(v) | v people, v−1 links, two traversals |
| Longest chain | O(v) | O(t) | One traversal with recursion depth t, the rooted tree height in vertices |

## AI use

I used ChatGPT occasionally to clarify some concepts, understand errors, and get suggestions while working on the assignment.

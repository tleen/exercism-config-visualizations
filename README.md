# exercism-config-visualizations

A collection of cross-track configuration visualizations for Exercism.

## Install

```bash
npm install -g exercism-config-visualizations
```

## Usage

```bash
exercism-config-visualizations --format csv --uniconfig ./data/uniconfig.json averages
```

* format - choose the [output format](#output-format) for the output, defaults to md (markdown)
* uniconfig - the location of your [uniconfig file](#uniconfig), defaults to internal (probably outdated) uniconfig. May also be provided via stdin

## Uniconfig
This program uses for input a unified Exercism configuration file (the --uniconfig option or piped in via stdin) you may need to re-generate it from [exercism-uniconfig](https://www.npmjs.com/package/exercism-uniconfig). There is [one used by default](./data/uniconfig.json) but it may be outdated.

You can pipe the input from exercism-uniconfig directly into this program.
```bash
exercism-uniconfig | exercism-config-visualizations topics
```

It is more efficient to save it locally as a file and re-use that.

## Output Format

You may output the results of a standard report in a number of formats.

* md - a markdown table, good for including in [GitHub text](https://help.github.com/articles/organizing-information-with-tables/) (default)
* csv - useful for opening as a spreadsheet program or piping to another processor
* table - a no-frills text-based table
* json - a JSON file with output data multi-dimensional array.

## Contribute

This has been designed to make it easy to create your own reports. Make a file named with the slug for your report in [reports](reports/). See the [exercise-count](reports/exercise-count.js) report as a good example of how to make a new report. I encourage others to submit any interesting reports for inclusion in the tool.

If you return false the program will assume you handled output yourself. Otherwise return a string for that output. If you want to take advantage of CSV or tabular output return a multi-dimensional array with headers as the first row.


## Reports

- [unconfigured](#unconfigured) - List Track(s) that are missing: Diff(iculty) or Topics
- [track](#track) - Active exercises for a single track: Exercise, Difficulty, Core, Unlocked By, Unlocks, Topics
- [exercises](#exercises) - Active exercises per track: Track, Count, Exercises
- [exercise](#exercise) - Information about exercises across tracks: Exercise, Track, Diff(iculty), Topics
- [exercise-count](#exercise-count) - List the number of exercises per track: Track, Exercise Count
- [implementation-count](#implementation-count) - List # of exercise implementations: Exercise, Count, Tracks
- [locks](#locks) - For a single exercise list Track, Core (status), Unlocked By, Unlocks
- [topics](#topics) - All topics used across all tracks: Topic, Count, Tracks (using this topic)
- [averages](#averages) - The average values of track configuration: Track, Diff(iculty), Topics (per exercise) 
- [difficulty](#difficulty) - Group exercises by difficulty level: Track, Diff(iculty), Count, Exercises
- [core](#core) - Show all Track(s) with core Exercises and those exercises

### unconfigured

List tracks without topics or difficulty set.

```bash
exercism-config-visualizations unconfigured
```

```text
| Track        | Diff | Topics |
| ------------ | ---- | ------ |
| clojure      | no   | no     |
| coffeescript | no   | no     |
...
| sml          | no   | no     |
| vbnet        | no   | no     |

```

### track

List information about all active (non-deprecated) exercises in a track.

```bash
exercism-config-visualizations track r --format table
```

```text
Exercise                Difficulty  Core  Unlocked By       Unlocks            Topics
leap                    1           yes                     perfect-numbers    booleans, integers, logic
hamming                 1           yes                     rna-transcription  strings, filtering
rna-transcription       1                 hamming                              strings, transforming
...
rotational-cipher       3           yes                                        strings
anagram                 3           yes                     isogram, pangram   strings, filtering
pascals-triangle        3                 sum-of-multiples                     control_flow_conditionals, mathematics
tournament              4                 phone-number                         text_formatting, parsing
```

### exercises

A listing of all active exercises per track.

```bash
exercism-config-visualizations exercises --format csv
```

```csv
Track,Count,Exercises
bash,14,"hello-world, gigasecond, bob, leap, raindrops, difference-of-squares, pangram, anagram, hamming, rna-transcription, word-count, two-fer, phone-number, error-handling"
...
vbnet,7,"bob, anagram, binary, allergies, atbash-cipher, accumulate, crypto-square"
```

### exercise

This is a general dump of exercise information across tracks. You may use multiple slugs to show more than one exercise per track.

```bash
exercism-config-visualizations exercise hello-world --format table
```

```text
Exercise     Track         Diff  Topics
hello-world  bash          1     stdout
hello-world  c             1     control-flow (if-statements), optional values, text formatting
...
hello-world  scala         1     Strings
hello-world  swift         1     Text formatting, Optional values
```

### exercise-count
This is a listing of track slug and number of exercises present in the configfile.

```bash
exercism-config-visualizations difficulty go --format json
```

```json
[["Track","Diff","Count","Exercises"],["go","1",8,["hello-world","leap","gigasecond","hamming","raindrops","accumulate","etl","scrabble-score"]],["go","2",9,["pangram","bob","difference-of-squares","grains","luhn","rna-transcription","roman-numerals","strain","nucleotide-count"]],["go","3",25,["clock","acronym","triangle","series","parallel-letter-frequency","isogram","crypto-square","largest-series-product","sieve","protein-translation","anagram","word-count","robot-name","atbash-cipher","phone-number","prime-factors","nth-prime","beer-song","wordy","meetup","tree-building","kindergarten-garden","simple-cipher","tournament","all-your-base"]],["go","4",13,["twelve-days","house","pascals-triangle","bank-account","food-chain","perfect-numbers","allergies","diamond","custom-set","pig-latin","matrix","word-search","ledger"]],["go","5",18,["error-handling","secret-handshake","queen-attack","sum-of-multiples","pythagorean-triplet","circular-buffer","transpose","diffie-hellman","grade-school","saddle-points","binary-search","binary-search-tree","paasio","minesweeper","poker","variable-length-quantity","change","bowling"]],["go","6",2,["palindrome-products","robot-simulator"]],["go","7",4,["bracket-push","say","ocr-numbers","pov"]],["go","8",1,["forth"]],["go","9",2,["react","connect"]]]
```

### implementation-count
This lists all the exercises found in the configfiles, # of occurrences and list the tracks that implement it.

```bash
exercism-config-visualizations implementation-count
```

```text
| Exercise                  | Count | Tracks                                                                                                                                                                                                                     |
| ------------------------- | ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| hamming                   | 30    | bash, c, clojure, cpp, dlang, csharp, ecmascript, elixir, fsharp, erlang, go, groovy, haskell, java, javascript, lisp, lua, objective-c, ocaml, perl5, php, plsql, powershell, python, r, ruby, rust, scala, sml, swift    |
| hello-world               | 29    | bash, c, clojure, cpp, coffeescript, dlang, csharp, ecmascript, elixir, fsharp, erlang, go, groovy, haskell, java, javascript, lua, objective-c, ocaml, perl5, perl6, php, powershell, python, r, ruby, rust, scala, swift |
...
                                                                                                               |
| luhn-trait                | 1     | rust                                                                                                                                                                                                                       |
| nucleotide-codons         | 1     | rust                                                                                                        
```

### locks

Given a exercise slug as the target, this will list all the tracks with the target exercise. It will show the Track slug, if the exercise has core status, which exercise (if any) unlocks it, which exercises (if any) it unlocks. This will not show tracks that have yet to implement the unlocking structure.

```bash
exercism-config-visualizations locks pangram
```

```text
| Track       | Core | Unlocked By | Unlocks |
| ----------- | ---- | ----------- | ------- |
...
| c           |      | isogram     |         |
...
| csharp      |      | hello-world |         |
| ecmascript  | yes  |             |         |
...
| fsharp      |      | hello-world |         |
...
| swift       |      |             |         |

```


### topics

Outputs a listing of topics ordered by usage frequency descending with listing of tracks using that topic. The Topic name is lowercased due to some inconsistent casing between tracks.

```bash
exercism-config-visualizations topics --format table
```

```text
Topic                                        Count  Track
strings                                      363    c, cpp, csharp, ecmascript, elixir, fsharp, go, javascript, lisp, lua, objective-c, ocaml, php, python, r, ruby, scala, swift
transforming                                 199    csharp, ecmascript, elixir, fsharp, go, javascript, lisp, lua, objective-c, ocaml, php, python, r, scala, swift
...
function overloading                         1      scala
metaprogramming                              1      groovy
```

### averages

Outputs the average difficulty and number of topics assigned to each exercise.

```bash
exercism-config-visualizations averages
```

```text
| Track        | Diff | Topics |
| ------------ | ---- | ------ |
| bash         | 1.50 | 1.93   |
| c            | 2.63 | 3.11   |
...
| sml          | 1    | 0      |
| swift        | 3.79 | 2.06   |
| vbnet        | 1    | 0      |
```

### difficulty

Listing of exercises grouped by difficulty level. You may specify multiple tracks for a combined listing.

```bash
bin/exercism-config-visualizations difficulty go --format table
```

```text
Track  Diff  Count  Exercises
go     1     8      hello-world, leap, gigasecond, hamming, raindrops, accumulate, etl, scrabble-score
go     2     9      pangram, bob, difference-of-squares, grains, luhn, rna-transcription, roman-numerals, strain, nucleotide-count
go     3     25     clock, acronym, triangle, series, parallel-letter-frequency, isogram, crypto-square, largest-series-product, sieve, protein-translation, anagram, word-count, robot-name, atbash-cipher, phone-number, prime-factors, nth-prime, beer-song, wordy, meetup, tree-building, kindergarten-garden, simple-cipher, tournament, all-your-base
go     4     13     twelve-days, house, pascals-triangle, bank-account, food-chain, perfect-numbers, allergies, diamond, custom-set, pig-latin, matrix, word-search, ledger
go     5     18     error-handling, secret-handshake, queen-attack, sum-of-multiples, pythagorean-triplet, circular-buffer, transpose, diffie-hellman, grade-school, saddle-points, binary-search, binary-search-tree, paasio, minesweeper, poker, variable-length-quantity, change, bowling
go     6     2      palindrome-products, robot-simulator
go     7     4      bracket-push, say, ocr-numbers, pov
go     8     1      forth
go     9     2      react, connect
```

### core

Show all the core exercises defined in all tracks, missing tracks have no core defined exercises.

```bash
exercism-config-visualizations core --format json | jq .
```

([jq](https://stedolan.github.io/jq/) is handy command line tool for manipulating JSON, in this case pretty printing it.)

```javascript
[
  [
    "Track",
    "Exercises"
  ],
  [
    "bash",
    [
      "hello-world",
      "leap",
      "pangram",
      "error-handling"
    ]
  ],
  [
    "c",
    [
      "hello-world",
      "isogram",
      "gigasecond",
      "hamming",
      // ...
      "pascals-triangle",
      "binary",
      "palindrome-products",
      "scrabble-score"
    ]
  ],
  // ...
]
```

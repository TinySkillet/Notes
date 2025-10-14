

> [!NOTE] Index
> A 'forward index' maps **location -> value**. An 'inverted index' maps **value -> location.**

Instead of scanning every document each time a user searches, we build an index for fast lookups.


> Example of mapping words to the document IDs that they appear in.

- matrix -> \[1, 5, 10]
- hacker -> \[1, 8]
- reality -> \[1, 3, 7]


> [!NOTE] Note
> Using an inverted index is super fast - we get O(1) lookups on each token. However, building the index is slow, because we have to read every document and tokenize it.




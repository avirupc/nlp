# 6.6 WordPiece Tokenization

[📖 Chapter link](https://huggingface.co/learn/llm-course/chapter6/6)

▶ Video links:
[WordPiece Tokenization](https://youtu.be/qpv6ms_t_1A)

## 🗒 Section Notes

*(I haven’t yet executed the code for this chapter as I usually do. However, I’ve added the [notebook](6_6_WordPiece_tokenization.md) along with the full chapter contents [here](6_6_WordPiece_tokenization.md). I plan to revisit this section in the near future to study it thoroughly and make edits as needed.)*

- Developed by Google. Used in models like Bert.
- The basic idea is very similar to Byte-Pair Encoding (BPE). It splits the words into characters first and then merges to form larger subwords. But, instead of using frequencies of the pairs while merging, this algorithm uses a scoring method for the pairs.

$$\text{pair score} = \frac{\text{freq. of pair}}{\text{freq. of first element} \times \text{freq. of second element}}$$

- Another difference is that, WordPiece takes into account if a letter/character is the starting character of the word or not while splitting the word into characters. <br>
Example:<br>
In BPE: 'test' → 't', 'e', 's' <br>
But in WordPiece: 'test' → 't', '##e', '##s' '##t' <br>
Here, the last 't' is thus differentiated from the initial 't'

**For a detailed explanation of the algorithm, I highly recommend watching the video mentioned above 👆. It provides a clear, step‑by‑step walkthrough with examples.**
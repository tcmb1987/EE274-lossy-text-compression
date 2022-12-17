# EE274-lossy-text-compression
Stanford EE 274, Fall 2022 final project on lossy text compression by Lara Arikan and Thomas Bourne.

## stopword_pruner.ipynb
This was a test script to determine the feasibility of NLP pruning methods and using sent2vec as a sentence embedding technique.

## milestone_final.ipynb
This script was used to test our initial ML model on sent2vec embeddings. First block imports libraries. Second block gets a corpus of text from the URL provided and removes the NLTK stopwords. The third block compresses the pruned text and original text and compares. Blocks 4 and 5 are to embed the sentences from the pruned and original text. The remaining blocks are to train and test a simple ML model. Each step is listed in script comments. 

## mech_test.ipynb
This script is to test various texts and a few sentences from them against reconstructed sentences from human subjects. Each section is for a different text and completes the same process. The text is imported and pruned. Then a compression analysis is done using gzip and zstd. The entirety of the original text and pruned is embedded using sent2vec. A few sentences are selectively taken and printed from the text. These sentences were presented to human testers and this script was used to determine the distortion metrics (mainly cosine distance) between their responses and the originals.

## articles_only.ipynb
This script is almost an exact replica of mech_test.ipynb, but just one section which was used to test text with articles removed only.

## encoder_decoder_2
This script was used to test an encoder decoder ML model similarly to milestone_final.ipynb except using a translation seq2seq method of embedding.

## Final Report
Our final report is located under our main branch, and can be found at this link:
https://github.com/tcmb1987/EE274-lossy-text-compression/blob/main/EE274_Final_Report.pdf

## Presentation Slides
Our slides have also been uploaded to the main branch, and is at this link:
https://github.com/tcmb1987/EE274-lossy-text-compression/blob/main/EE%20274%20Final%20Project_%20%20Lossy%20Text%20Compression.pdf

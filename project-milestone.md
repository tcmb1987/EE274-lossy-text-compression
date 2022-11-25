# Lossy Text Compression Using Natural Language Processing - Project Milestone

## Purpose
The purpose of this project is to explore methods of lossy text compression using techniques already used in Natural Language Processing. In alignment with nearly all other compression research, the motivation for this project is that humans are generating an increasing amount of data that needs to be stored or transmitted. In the context of textual data, the focus of compression research has generally been in lossless compression. Generally, it can be inferred that lossless techniques are preferred for textual data due to its context changing significantly if certain words or even individual symbols are omitted or incorrect.

Current lossless techniques for compressing textual data are very efficient, and can achieve compression at or near entropy. Lossy techniques, however, could achieve compression rates far below entropy, and still maintain contextual meaning if selectively redacting and then reconstructing the words or phrases that are not necessary for preserving the essential elements of textual meaning. In this manner, it could be possible to achieve extremely efficient storage and communication of textual data that can be recreated within an acceptable level of error.

## Literature Review
A set of “semantically non-selective” words, such as “a, an, as, the, of,” are called “stopwords'' or “function words”; these words can be excised to improve the performance of sentiment analysis models, or to allow better measures of the semantic similarity of texts using word-by-word comparison (Raghavan et al., 2008, 81). A standard corpus of stopwords has been curated and is distributed within the Natural Language Tool Kit (NLTK), a Python package often used for NLP applications (Sarica & Luo, 2021, 1, 3-4). Another approach is to filter out “nonessential clauses'', which are usually details related to the subject of a sentence but do not directly relate to the main statement (Merriam-Webster, n.d.). For instance, “Mary, who is five years old, has a lamb” includes the nonessential clause “who is five years old”, since it does not have an apparent semantic link to her owning the lamb - at least within the bounds of the sentence. These concepts have rarely been used in the context of compression; there is one paper from 2006 which prunes a “syntactic tree” in order to eliminate syntax dependencies at a certain depth in order to achieve better lossy compression rates (Gagnon & Da Sylva, 2006, 314). Much of the literature on syntax trees extensively describes efficient ways to structure and prune them according to an intended outcome, which may be useful for compression algorithms that involve the identification and elimination of such dependencies (Kovář et al., 2008, 125).

We also reviewed methods for vectorization, or embedding, of sentences. Sent2vec is similar to the more commonly known word2vec, but more effectively preserves emotional and contextual semantics of sentences than just individually embedding the words (Moghadasi & Zhuang, 2020, 2). Our method requires that we use an embedding that is capable of recognizing semantic differences since context is important to recover the entirety of the textual data we will be excising from and then compressing. Sent2vec is available as a Python package with dependencies on other, common Python libraries (Ataee, 2022).  

## Methods
Our current approach is to excise syntactic and structural elements of textual excerpts that are not essential to the overall “meaning” of each excerpt. We will experiment with various techniques for this, such as stopword filtering and nonessential clause removal, and also come up with an original scheme together with our mentors (which may be an extension of the above or involve syntax trees). We then losslessly compress the reduced output to determine how much smaller the compressed file can be when created from the excised text instead of the original.

In order to create a decompression scheme for the lossy step, we train a supervised learning model for each pruning method which attempts to predict the original from some given excised text. This implies that we need to embed the sentences into a form that can be used for training, testing, and manipulation. Therefore, we need to understand what are some of the NLP techniques for embedding text and apply it to our experiment.

The current hypothesis is that a properly trained model will be able to restore the original text without distortion until a certain threshold of excised words is reached. The predictions of each model will be compared to the original versions using distortion metrics such as edit distance, and the distance between the original and reconstructed sentences in a high-dimensional embedding such as the above mentioned sent2vec. The tradeoff between compression (how short the modified text is) and error (how far off, by each metric, the reconstruction is from the original) will be evaluated, depicted in plots, and the methods compared on this basis.

## Progress
Our initial steps were to determine the method of sentence embedding in a manner such that the resulting embedding data could be used to train and test our learning model. One concern was that embedding of sentences might result in vectors of different dimensions since sentence structure and length can vary greatly when compared to individual words. After literature review and testing the resulting vectors from sent2vec BERT (Bidirectional Encoder Representations from Transformers) method, we discovered that the embedding will always output vectors of dimension 768. With a fixed length output, this meant that we could proceed with training a model and using it to reconstruct excised sentence vectors.

A current concern, however, is that it is unclear if there is an existing function or method to use sent2vec in the reverse direction, from the embedded vectors to sentence strings (essentially vec2sent). The BERT method seems to suggest this is possible, but the best method to accomplish this has not been revealed to us yet. We proceeded with our model training method, since we could use the data in its current form, and then determine a method for recovering the reconstructed sentences at a later time. If necessary, we can explore if there are other sentence embedding methods that would work for our purposes.

Starting with a few basic sentences, we successfully tested pruning of stopwords using NLTK functions, and then embedded the original sentences and pruned sentences using the sent2vec BERT method. This basic work was initially done in our code linked here, stopword_pruner.ipynb. We expanded this technique on more data by importing the first 36 chapters of Genesis from the Gutenberg website. Using gzip and zstd at level 22 (the highest compression level) our data showed that we could achieve respectively 39.7% and 36.6%  better compression by just removing stopwords alone. The results seem very promising, though it will be beneficial to determine if this holds for varying lengths of textual data and different types as well. For the expanded data, and following model training, the work was completed in our code linked here, milestone_final.ipynb.

To train our model, we used 80% of the data generated from our Genesis embedding, the X_train data being from the vectorized pruned sentences and the Y_train data being from the vectorized original sentences. An epoch size of 2000 was determined to be more than sufficient for training the model. Applying the resulting neural network, and using cosine distance as a distortion metric on the remaining 20% of the data, we found that the reconstructed data from the pruned embeddings resulted in ~3% error when compared to the original embeddings. The performance of the neural network is very promising, and suggests that it may be possible to achieve lossless performance or near lossless performance of textual reconstruction accuracy while being able to improve compression performance by close to 40%. 

## Next Steps
Importantly, to truly determine the performance of our reconstruction technique, we must be able to translate our reconstructed sentence embeddings into sentence strings. Though there is sparse documentation on sent2vec functions, the BERT method suggests that each vectorization of a sentence must have a corresponding string that goes with it. Therefore, there must be an existing method to recover sentences from their embedded form. Determining either the method to do this, or finding an alternative embedding technique that can do this will be our focus over the next few weeks.

Meanwhile, we have developed a neural network that will work on current embeddings, so we can explore other methods of excising text without losing semantic meaning. We will explore techniques for non-essential clause removal or perhaps a combination of this and stopword removal to see if we can further improve compression performance without significantly increasing the error realized by the neural network.

Finally, we hope to present and report these findings in an understandable and concise manner. So we will dedicate some time to creating a detailed presentation of our findings.



## References
Allahyari, M., Pouriyeh, S., Assefi, M., Safaei, S., Trippe, E. D., Gutierrez, J. B., & Kochut, K. (2017, July 28). Text Summarization Techniques: A Brief Survey. https://arxiv.org/abs/1707.02268

Ataee, P. (2022, March 23). sent2vec · PyPI. PyPI. Retrieved November 25, 2022, from https://pypi.org/project/sent2vec/0.3.0/#files

Gagnon, M., & Da Sylva, L. (2006). Text Compression by Syntactic Pruning. Advances in Artificial Intelligence, 4013, 312-323. https://link-springer-com.stanford.idm.oclc.org/chapter/10.1007/11766247_27#citeas

Gaikwad, D. K., & Mahender, C. N. (2016, March). A Review Paper on Text Summarization. International Journal of Advanced Research in Computer and Communication Engineering, 5(3), 154-160. chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://www.ijarcce.com/upload/2016/march-16/IJARCCE%2040.pdf

Kaur, J., & Buttar, P. K. (2018). A Systematic Review on Stopword Removal Algorithms. International Journal on Future Revolution in Computer Science & Communication Engineering, 4(4), 207-210. https://www.researchgate.net/profile/Preetpal-Buttar/publication/354950829_A_Systematic_Review_on_Stopword_Removal_Algorithms/links/615598e94d9f0f16175f84f1/A-Systematic-Review-on-Stopword-Removal-Algorithms.pdf

Kovář, V., Horák, A., & Kadlec, V. (2008). New Methods for Pruning and Ordering of Syntax Parsing Trees. Text, Speech and Dialogue - Lecture Notes in Computer Science, 5246. https://link.springer.com/chapter/10.1007/978-3-540-87391-4_18#citeas

Merriam-Webster. (n.d.). Essential vs. Nonessential Clauses: Usage Explained. Merriam-Webster. Retrieved November 4, 2022, from https://www.merriam-webster.com/words-at-play/usage-of-essential-and-nonessential-clauses

Moghadasi, M. N., & Zhuang, Y. (2020). Sent2Vec: A New Sentence Embedding Representation With Sentimental Semantic. 10.1109/BigData50022.2020.9378337

Raghavan, P., Manning, C. D., & Schütze, H. (2008). Introduction to information retrieval (P. Raghavan & H. Schütze, Eds.). Cambridge University Press.

Sarica, S., & Luo, J. (2021, August 5). Stopwords in technical language processing. PLoS One, 16(8), 1-13. https://doi.org/10.1371/journal. pone.0254937

Zhang, W., Li, P., & Zhu, Q. (2010). Sentiment Classification Based on Syntax Tree Pruning and Tree Kernel. 2010 Seventh Web Information Systems and Applications Conference, 101-105. https://ieeexplore.ieee.org/abstract/document/5581390




# voynich-topic-modeling

**Correction (October 2026).** Torsten Timm pointed out that `tokenize()` joined the last word of each transcription line to the first word of the next. The line `index[pg] += (paragraph)` should be `index[pg] += ' ' + paragraph`. Because the joined strings almost never recur, `min_df=2` removed them, so the models lost the first and last word of most lines. With the corrected input, the A/B separation and the associations with hands and sections hold. The 2- and 5-topic NMF assignments change on 3 and 13 of 225 pages. The 6-topic model changes on 48 pages and no longer has a separate astrology topic. That topic was already unstable under different initialisations before the fix. The code is corrected; the saved models and outputs in this repository are from the original run. (This fix was made with Claude Code, and the original error was also, quite possibly, identified by an AI agent.)

# How to use

**TFIDF Vectorizer** 
* The file ```vms_vectorize.py``` builds a "model" for for the tfidf vectorizer and a ```vms_mapping``` so you can look up VMS document names by row ID in the vms_tfidf vectorizer
* ```vms_tfidf.pk``` is the vectorized matrix of VMS documents. Once you load, ```vms_tfidf.pk```, you can use the word vectorizers!
    - To load: 
        ````
        with open("vms_tfidf.pk", "rb" as f: 
            vms_tfidf = picke.load(f)
        ````
    - vms_tfidf is a DxV matrix, where D is the number of documents (225) and V is the number of unique words in the vocabulary (9778)
    - if a word wi appears in document dk, there will be a tfidf weighted value for that word at ```vms_tfidf[dk][wi]```, otherwise the value is 0
* ```vms_mapping.pk``` maps document (row) ID to its folio number 
    - To load: 
        ````
        with open("vms_mapping.pk", "rb" as f: 
            vms_mapping = picke.load(f)
        ````
        - see ```cosine_similarity.py``` for usage

**Cosine Similarity** 
* ```cosine_similarities = cosine_similarity(vms_tfidf, vms_tfidf)``` creates a 225x225 matrix, where the value at index i, j is the similarity of document i and j
* To print the similarities, run ```python3 cosine_similarities.py```


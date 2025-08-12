# naive-bayes-latin-disambiguator
**Naive Bayes approach to disambiguating ambiguous Latin noun endings**  

---

## Background  
Latin is a highly inflected language, meaning that a single noun can take many different endings depending on its case (nominative, genitive, dative, accusative, ablative, vocative) and number (singular or plural).  
However, **many endings are ambiguous** — the same surface form can correspond to multiple possible morphological analyses.  

For example:  
- *puellae* could be **genitive singular**, **dative singular**, or **nominative plural**.  
- *domus* could be **nominative singular** or **genitive singular**, depending on context.  

These ambiguities can be difficult to resolve automatically and are sometimes challenging even for human readers without sufficient context.  

---

## What Naive Bayes Does  
Naive Bayes is a probabilistic classifier that uses Bayes’ theorem with the simplifying assumption that features are **conditionally independent given the class**.  

In this project, our **classes** are the possible morphological analyses (case, number, gender) of a noun form.  
Our **features** are derived from the **part-of-speech (POS) tags** of words surrounding the target noun:
- The POS of the **word before** the noun.
- The POS of the **word after** the noun.  

The intuition is that context — especially the syntax of surrounding words — can help disambiguate noun forms.  

---

## Methodology  

### 1. Choosing a Morphological Analyzer  
We tested multiple morphological analyzers for Latin, including:  
- **CLTK Latin Morphological Analyzer**  
- **Perseus Morphological Analyzer** (via Perseus Project API)  
- Other rule-based analyzers from open-source Latin NLP tools  

After trials, we chose **Perseus** for this project due to its relatively comprehensive output for ambiguous forms.  

---

### 2. Generating All Possible Forms  
For each noun form in our dataset:  
1. Use Perseus to generate **all possible morphological analyses** (e.g., nominative singular, accusative plural, etc.).  
2. Store these as the **candidate labels** for disambiguation.  

---

### 3. Building the Dataset  
- Extract nouns with ambiguous endings from Latin texts.  
- For each noun, collect:  
  - POS of the preceding word  
  - POS of the following word  
  - The correct morphological analysis (gold label)  

---

### 4. Training Naive Bayes  
- **Features**: POS before, POS after  
- **Labels**: Correct morphological analysis  
- Split into **training** and **test** sets.  
- Train a multinomial Naive Bayes classifier.  

---

### 5. Evaluation  
- Evaluate on the test set for accuracy.  
- Compare accuracy across different endings to see if some are inherently easier to disambiguate.  

---

## Results  
- **Overall Accuracy**: **74%**  
- **Observation**: Endings with more training examples (e.g., *-ae*) performed significantly better.  
- Endings with very few examples had much lower accuracy, likely due to lack of training data.  

---

## Limitations  
1. **Limited Data** — Our dataset was relatively small, which especially impacted rare endings.  
2. **Intrinsic Ambiguity** — Even human readers struggle with some forms without broader syntactic or semantic context.  
3. **Simplistic Context Features** — Only used immediate left and right POS tags; no deeper syntactic structure or semantics considered.  

---

## Future Work  
1. **Expand the Dataset** — Incorporate more Latin corpora to cover rare forms.  
2. **Richer Context Features** — Use dependency parsing, lemma information, and longer-range context windows.  
3. **Integrate Semantic Clues** — Incorporate word meaning or thematic roles to help resolve ambiguous forms.  
4. **Compare Models** — Test other classifiers (logistic regression, SVM, neural models) for potential accuracy gains.  

---

## How to Run  
```bash
git clone https://github.com/yourusername/latindisambig.git
cd latindisambig
pip install -r requirements.txt
python train.py
python evaluate.py

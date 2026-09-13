# Project Findings

In this project, I used machine learning to classify DNA sequences into three splice-junction classes: **EI, IE, and N**. The project helped me understand how biological sequence information can be converted into machine-learning features and used for classification.

## 1. Dataset Findings

The dataset contained **3,190 DNA sequences**, with each sequence having **60 nucleotide positions**.

The three classes were:

- **EI** — exon to intron
- **IE** — intron to exon
- **N** — neither

The **N class was the largest class**, containing about 52% of the data, while EI and IE each represented about 24%.

During data cleaning, I found **12 exact duplicate rows**, which I removed. I retained repeated DNA sequences because repeated observations can still contain useful information. I also found one DNA sequence appearing with two different class labels. I retained and documented this case rather than changing the labels without biological evidence.

## 2. DNA Sequence Findings

The sequences contained the standard nucleotides **A, C, G, and T**, along with ambiguity symbols such as **N, R, S, and D**.

The DNA-specific EDA showed differences in nucleotide composition and nucleotide frequencies across sequence positions between the classes. This indicated that the sequences contain local and positional patterns that can provide useful information for classification.

## 3. K-mer Feature Engineering Findings

I represented each DNA sequence using **3-mer frequency features**.

A 3-mer is a sequence of three consecutive nucleotides, such as `ATG`, `GCA`, or `TTC`. This converts the DNA sequence into numerical features that machine-learning models can process.

I selected **k = 3** because it provides a useful balance between capturing local sequence patterns and keeping the feature representation manageable.

After removing exact duplicate rows, the final feature matrix contained:

**3,178 sequences × 97 features**

This showed that a relatively simple representation of local DNA patterns can provide useful information for splice-junction classification.

## 4. SVM Findings

The baseline **SVM with an RBF kernel** achieved:

**67.45% test accuracy**

The F1-scores for the three classes were:

- EI: **0.6187**
- IE: **0.5660**
- N: **0.7353**

The N class was easier to identify, while IE was more difficult.

I also tuned the SVM's `C` and `gamma` hyperparameters. The tuned model achieved **67.14%**, which was slightly lower than the baseline. Therefore, I retained the baseline SVM.

## 5. Random Forest Findings

The baseline **Random Forest** achieved:

**68.40% test accuracy**

Its F1-scores were:

- EI: **0.6094**
- IE: **0.5401**
- N: **0.7522**

Random Forest performed particularly well in identifying the N class, with a recall of **0.8852**.

I also tuned the Random Forest parameters, but the tuned model achieved **67.92%**, which was slightly below the baseline. Therefore, I retained the baseline Random Forest.

## 6. SVM vs Random Forest

The two models produced relatively similar overall results.

| Metric | SVM | Random Forest |
|---|---:|---:|
| Accuracy | 67.45% | **68.40%** |
| Weighted Precision | 0.6747 | **0.7029** |
| Weighted Recall | 0.6745 | **0.6840** |
| Weighted F1 | 0.6667 | **0.6671** |
| Macro F1 | **0.6400** | 0.6339 |

Random Forest achieved the highest overall accuracy and weighted precision. However, SVM achieved a slightly higher macro F1-score, indicating slightly more balanced performance across the three classes.

Therefore, I would not describe Random Forest as simply "better" than SVM. **Random Forest performed slightly better overall, while SVM provided slightly more balanced class-wise performance.**

## 7. Main Technical Finding

The most important technical finding is that **feature representation played an important role in model performance**.

Both models used the same 3-mer representation, and their overall performance was close. Hyperparameter tuning also did not improve either model.

This suggests that increasing model complexity alone was not enough to produce a meaningful improvement. The **3-mer frequency representation has limitations**, because it does not fully preserve the position of each pattern or longer-range dependencies within the DNA sequence.

## 8. Overall Finding

Overall, I found that **3-mer-based machine learning can learn useful patterns from DNA splice-junction sequences**, but the problem is more complex than what can be captured through short k-mer frequencies alone.

The models were particularly successful at identifying the **N class**, while distinguishing between **EI and IE** was more challenging.

The project also showed why accuracy alone is not sufficient for a multiclass biological classification problem. By examining **precision, recall, F1-score, macro F1, and confusion matrices**, I could understand how each model behaved across the individual classes.

## Final Finding

> **Short local DNA sequence patterns contain useful information for splice-junction classification, but 3-mer frequency features alone are not sufficient to fully capture the complexity of the biological sequence. Random Forest achieved the highest overall accuracy at 68.40%, while SVM provided slightly better balanced class-wise performance based on macro F1.**

This project provides a practical baseline for applying classical machine learning to biological sequence classification and demonstrates the importance of combining **biological understanding, feature engineering, and class-wise model evaluation**.

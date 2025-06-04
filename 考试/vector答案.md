### **TF-IDF Vector Representation for Each Document**

#### **1. Preprocessing (Stopword Removal)**
Given the stopwords list: `and, be, is, it, to, will`, we remove them from each document:

- **Document 1**: "It is going to rain and rain and rain today."  
  → `["going", "rain", "rain", "rain", "today"]`

- **Document 2**: "Today I will be playing sport."  
  → `["I", "playing", "sport", "today"]`  
  *(Note: "I" is retained as it’s not in the stoplist.)*

- **Document 3**: "I am going to watch the play."  
  → `["I", "am", "going", "watch", "play"]`  
  *(Note: "am" is retained as it’s not in the stoplist.)*

---

#### **2. Vocabulary (Unique Terms)**
After processing, the full vocabulary (sorted alphabetically) is:  
`["am", "going", "I", "play", "playing", "rain", "sport", "today", "watch"]`

---

#### **3. Term Frequency (TF) Calculation**
TF for term `t` in document `d` is:  
$$

TF(t, d) = \frac{\text{Number of times } t \text{ appears in } d}{\text{Total terms in } d}
$$

| Term    | Doc1 (5 terms) | Doc2 (4 terms) | Doc3 (5 terms) |
| ------- | -------------- | -------------- | -------------- |
| am      | 0              | 0              | 1/5 = 0.2      |
| going   | 1/5 = 0.2      | 0              | 1/5 = 0.2      |
| I       | 0              | 1/4 = 0.25     | 1/5 = 0.2      |
| play    | 0              | 0              | 1/5 = 0.2      |
| playing | 0              | 1/4 = 0.25     | 0              |
| rain    | 3/5 = 0.6      | 0              | 0              |
| sport   | 0              | 1/4 = 0.25     | 0              |
| today   | 1/5 = 0.2      | 1/4 = 0.25     | 0              |
| watch   | 0              | 0              | 1/5 = 0.2      |

---

#### **4. Inverse Document Frequency (IDF) Calculation**
IDF for term `t` is:  
$$
IDF(t) = \log\left(\frac{\text{Total documents } N}{\text{Documents containing } t}\right)
Here, \( N = 3 \).
$$

| Term    | Docs Containing Term | IDF = log(3/df)  |
| ------- | -------------------- | ---------------- |
| am      | 1 (Doc3)             | log(3/1) ≈ 1.099 |
| going   | 2 (Doc1, Doc3)       | log(3/2) ≈ 0.405 |
| I       | 2 (Doc2, Doc3)       | log(3/2) ≈ 0.405 |
| play    | 1 (Doc3)             | log(3/1) ≈ 1.099 |
| playing | 1 (Doc2)             | log(3/1) ≈ 1.099 |
| rain    | 1 (Doc1)             | log(3/1) ≈ 1.099 |
| sport   | 1 (Doc2)             | log(3/1) ≈ 1.099 |
| today   | 2 (Doc1, Doc2)       | log(3/2) ≈ 0.405 |
| watch   | 1 (Doc3)             | log(3/1) ≈ 1.099 |

---

#### **5. TF-IDF Vectors**
Multiply TF and IDF for each term in each document:  
\[
TF\text{-}IDF(t, d) = TF(t, d) \times IDF(t)
\]

| Term    | Doc1                | Doc2                 | Doc3                |
| ------- | ------------------- | -------------------- | ------------------- |
| am      | 0                   | 0                    | 0.2 × 1.099 ≈ 0.220 |
| going   | 0.2 × 0.405 ≈ 0.081 | 0                    | 0.2 × 0.405 ≈ 0.081 |
| I       | 0                   | 0.25 × 0.405 ≈ 0.101 | 0.2 × 0.405 ≈ 0.081 |
| play    | 0                   | 0                    | 0.2 × 1.099 ≈ 0.220 |
| playing | 0                   | 0.25 × 1.099 ≈ 0.275 | 0                   |
| rain    | 0.6 × 1.099 ≈ 0.659 | 0                    | 0                   |
| sport   | 0                   | 0.25 × 1.099 ≈ 0.275 | 0                   |
| today   | 0.2 × 0.405 ≈ 0.081 | 0.25 × 0.405 ≈ 0.101 | 0                   |
| watch   | 0                   | 0                    | 0.2 × 1.099 ≈ 0.220 |

---

#### **6. Final TF-IDF Vectors**
**Document 1**:  
`[0, 0.081, 0, 0, 0, 0.659, 0, 0.081, 0]`  
*(Order: am, going, I, play, playing, rain, sport, today, watch)*  

**Document 2**:  
`[0, 0, 0.101, 0, 0.275, 0, 0.275, 0.101, 0]`  

**Document 3**:  
`[0.220, 0.081, 0.081, 0.220, 0, 0, 0, 0, 0.220]`  

---

#### **Key Observations**
1. **Sparse Vectors**: Most terms do not appear in all documents (many 0s).  
2. **High-Weight Terms**:  
   - Doc1: "rain" (TF-IDF=0.659) dominates as it appears frequently in only one document.  
   - Doc2: "playing" and "sport" have equal weights (both are unique to Doc2).  
   - Doc3: "am", "play", and "watch" share the highest weight (all unique to Doc3).  
3. **Stopword Impact**: Words like "today" (appearing in 2 docs) receive lower IDF penalties.  

This representation allows ranking documents by relevance to a query (e.g., a search for "rain play" would prioritize Doc1 and Doc3).
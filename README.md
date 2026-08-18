# Code Runtime Complexity Prediction

Predict the Big-O runtime complexity of **C, Java, and Python** source code from static analysis of its Abstract Syntax Tree (AST), served through a Streamlit app.

Published at **ERCICA 2023 (Springer)**: [Code Runtime Complexity Prediction (DOI: 10.1007/978-981-99-7622-5_26)](https://doi.org/10.1007/978-981-99-7622-5_26). The published work reports up to **96% accuracy** on the IBM CodeNet dataset.

## What it does

Given a snippet of C, Java, or Python code, the tool:

1. Parses the source into an Abstract Syntax Tree for that language.
2. Turns the tree into structural feature vectors (node counts, loop and branch structure, and graph features).
3. Runs a trained classifier to predict the code's runtime complexity class (for example O(1), O(n), O(n log n), O(n^2)).

The approach is fully static: it reads the structure of the code and never executes it.

## How it works

- **Language detection:** a small classifier routes each file to the correct language parser.
- **AST parsing:** separate parsers build the tree for C, Java, and Python (`Backend/capstone_c_ast.py`, `capstone_java_ast.py`, `capstone_python_ast.py`).
- **Feature extraction:** `Backend/get_nodes.py` and `Backend/vector.py` turn the tree into numeric features.
- **Prediction:** trained models in `Models/` (scikit-learn Random Forest classifiers with a fitted scaler) map the features to a complexity class.

The full training experiments, including the deep-learning models (BiLSTM over AST graph embeddings, built with TensorFlow and NetworkX), and the dataset live in companion repositories linked below.

## Repository layout

```
Backend/     AST parsers (C, Java, Python), node extraction, and vectorization
Models/      Trained classifiers and the fitted scaler
frontend.py  Streamlit app for uploading code and viewing predictions
requirements.txt
```

## Getting started

```bash
# 1. Clone
git clone https://github.com/NikhilAdyapak/CodeRuntimeComplexity
cd CodeRuntimeComplexity

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the app
streamlit run frontend.py
```

Then upload `Codes.zip`, a zip of code files in C, Java, or Python, and the app returns the predicted complexity for each file.

Note for C parsing: the C AST step uses [pycparser](https://github.com/eliben/pycparser). Clone it locally and set its `utils/fake_libc_include` path inside `Backend/script.py`.

## Related repositories

- [Code_Complexity_Training_Experiments](https://github.com/NikhilAdyapak/Code_Complexity_Training_Experiments) - training notebooks and model experiments
- [Time_Complexity_Dataset](https://github.com/NikhilAdyapak/Time_Complexity_Dataset) - the dataset, labeled by language and complexity

## Publication

Nikhil Adyapak et al., *Code Runtime Complexity Prediction*, ERCICA 2023, Springer. [https://doi.org/10.1007/978-981-99-7622-5_26](https://doi.org/10.1007/978-981-99-7622-5_26)

## Author

Nikhil Adyapak - [portfolio](https://nikhiladyapak.github.io/) - [LinkedIn](https://www.linkedin.com/in/nikhil-adyapak)

---

_Part of [Nikhil Adyapak](https://nikhiladyapak.github.io/)'s portfolio · [Resume](https://nikhiladyapak.github.io/NIKHIL_ADYAPAK_resume.pdf) · [LinkedIn](https://www.linkedin.com/in/nikhil-adyapak) · [GitHub](https://github.com/NikhilAdyapak) · [Email](mailto:nikhiladyapak31@gmail.com)_

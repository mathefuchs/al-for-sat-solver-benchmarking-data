# Active Learning for SAT Solver Benchmarking - Experimental Data

This repository contains the experimental data for the paper

> Fuchs, Bach, Iser. "Active Learning for SAT Solver Benchmarking"

accepted at the conference [*TACAS 2023*](https://etaps.org/2023/tacas).
The paper source and code are available in a [separate repository](https://github.com/mathefuchs/al-for-sat-solver-benchmarking).
Clone this repository and the code repository in the same parent directory.
Instructions to reproduce the experiments are included in the [code repository's README](https://github.com/mathefuchs/al-for-sat-solver-benchmarking).
The data were obtained on a server with an `AMD EPYC 7551` [CPU](https://www.amd.com/en/products/cpu/amd-epyc-7551) (32 physical cores, base clock of 2.0 GHz) and 128 GB RAM.
The Python version was `3.9.12`.
With this configuration, the experimental pipeline (`run.sh`) took about a week.

In the following, we describe how to use the data as well as how this repository is structured.

## Using the Data

### Working with `*.pkl` files

Most of our files are *pickled* Python objects, e.g., lists, sets, dictionaries, and `pandas` data frames.
This has the advantage of *not* needing to preprocess any data.
Use the following code snippet to inspect a file's contents (tested with `Python 3.9.12`).

``` python
import pickle

with open("pickled-data/anni_final_df.pkl", "rb") as file:
    anni_final_df = pickle.load(f)

print(anni_final_df.head())
```

### Working with `*.csv` files

Some of the experimental pipeline's input and output files are plain CSVs.
You can easily read any of them with `pandas`.

``` python
import pandas as pd

results = pd.read_csv("pickled-data/active_learning_instance_sampling_frequencies.csv")
```

### Working with `*.db` files

Instance features and runtimes come in the form of `sqlite3` databases.
To inspect them, type for example `sqlite3 gbd-data/meta.db`.
Thereafter, you may use, e.g., the `.tables` command to list available databases or the `.schema [tablename]` command to examine a table's schema.

## Repository Structure

* The folder `gbd-data` contains the relevant data of the Global Benchmark Database (GBD; originally from [here](https://git.scc.kit.edu/fv2117/gbd-data)).
  * `anni-seq.csv` is a CSV file containing the runtimes of 28 SAT solvers on all 5355 SAT Competition 2022 Anniversary Track instances.
  * `base.db` is a `sqlite3` database containing the `features` table. It contains values of 54 instance features for all instances.
  * `meta.db` is a `sqlite3` database with metadata containing the `features` table. It shows the instance family of each instance among other data.
* The folder `pickled-data` contains final results, intermediate results, and preprocessed data for reproducing our experiments.
  * The folder `pickled-data/end_to_end` contains both pickle and CSV files. The pickle files are binary `numpy` arrays indicating which instances are selected for each particular hyperparameter choice. The CSV files contain results for the respective hyperparameter choice. The README's section *Using the Data* shows how to inspect those files.
  * `active_learning_instance_sampling_frequencies.csv` is a CSV file showing how often instances are selected by the best-performing active-learning approach.
  * `anni_final_df.pkl` and `anni_full_df.pkl` are pickled `pandas` data frames that include the data from `gbd-data/anni-seq.csv` in a preprocessed form.
  * `anni_train_df.pkl` is a pickled `pandas` data frame including a subset of `anni_final_df.pkl` for hyper-parameter optimization purposes.
  * `base_features_df.pkl` is a pickled `pandas` data frame including the already normalized and preprocessed features from `gbd-data/base.db`.
  * `meta_df.pkl` is a pickled `pandas` data frame including the already preprocessed features from `gbd-data/meta.db`.
* `README.md` is this document.

DMBI Experiment 9 – Bank Customer Churn System
Experiment Title
Association Rule Mining using RapidMiner (FP-Growth) – Bank Customer Churn System
Important Dataset Clarification
The supplied Experiment 9 PDF uses the Groceries Dataset (Groceries_dataset.csv) and performs Market Basket Analysis. It does not use a bank customer churn dataset. To preserve the practical accurately, this document and Python implementation retain the grocery transaction data and FP-Growth workflow. The requested Bank Customer Churn System wording is used as the project heading, but the underlying analysis is grocery purchase-pattern analysis, not bank churn prediction.

A customer-level churn dataset such as Churn_Modelling.csv does not contain basket transactions in the same structure. It would need a separately designed conversion of customer attributes into items before association rule mining could be meaningfully applied.
Aim
To implement Association Rule Mining algorithms using the RapidMiner Studio graphical user interface (GUI). The experiment preprocesses real-world retail data (the Groceries dataset) into a transactional matrix, generates frequent itemsets using the FP-Growth algorithm, and extracts strong association rules to discover patterns in customer purchasing behaviour.
Theory
1. Market Basket Analysis and Association Rules
Association Rule Mining discovers interesting relationships between items in a large dataset. In retail, it is called Market Basket Analysis: identifying products that tend to occur together in customer transactions.

An association rule is written as:

A => B
- Antecedent (A): The condition or item(s) already in the basket.
- Consequent (B): The predicted item(s) likely to be purchased alongside A.
2. Apriori and FP-Growth Algorithms
Apriori uses the anti-monotonicity principle: if an itemset is frequent, all its subsets must also be frequent. Apriori can become memory-intensive on large datasets because it generates candidate itemsets iteratively.

FP-Growth (Frequent Pattern Growth) uses a compressed tree structure called an FP-Tree to find frequent itemsets without repeatedly generating candidate itemsets. It can therefore be faster and more memory-efficient on suitable datasets.
3. Evaluation Metrics
Support

The proportion of total transactions containing an itemset:

Support(A) = Transactions containing A / Total Transactions

Confidence

The probability that a transaction containing A also contains B:

Confidence(A => B) = Support(A union B) / Support(A)

Lift

The ratio of observed confidence to expected confidence if A and B were independent:

Lift(A => B) = Confidence(A => B) / Support(B)

Interpretation:
- Lift > 1: Positive association.
- Lift = 1: Independent variables.
- Lift < 1: Inverse/negative relationship.
Dataset Overview
- Dataset: Groceries Dataset (Groceries_dataset.csv, sourced from Kaggle according to the PDF).
- Transactions reported in the PDF: 14,963.
- Unique items reported in the PDF: 167.
- Key columns:
  - Member_number: Unique customer ID.
  - Date: Transaction date.
  - itemDescription: Textual description of the grocery item purchased.
Software and Libraries
- RapidMiner Studio / Altair AI Studio for the GUI workflow.
- Python 3 for the equivalent implementation.
- Pandas
- mlxtend
RapidMiner Implementation and Execution
1. Loading the Dataset
1. Open RapidMiner Studio / Altair AI Studio.
2. Import Groceries_dataset.csv using the Import Configuration Wizard.
3. Confirm the data types:
   - Member_number: integer.
   - Date: date.
   - itemDescription: polynominal (categorical).
The supplied PDF shows the Import Data wizard and column type configuration on pages 2–3.
2. Grouping Items (Pivot Operator)
The raw dataset lists one item per row. Items bought together must be grouped into a single transaction. Use the Pivot operator:
- Group by Member_number and Date.
- Use itemDescription as the attribute to pivot.
- Aggregate using the count function.
This transforms the vertical item list into a wide transaction matrix. The Pivot configuration is illustrated on page 4 of the PDF.
3. Handling Missing Values
When pivoting, an item not bought in a particular transaction produces a missing value. Use Replace Missing Values to replace missing values with 0 across the item columns. The PDF shows this step on pages 4–5.
4. Generating Association Rules
FP-Growth requires a binomial (True/False) representation of item presence:
1. Apply Numerical to Binomial:
   - 0 becomes False.
   - Any positive count becomes True.
2. Apply Select Attributes to remove Member_number and Date.
3. Keep only the actual grocery item attributes.
5. FP-Growth and Rule Generation
1. Add the FP-Growth operator.
2. Set minimum support to 3.0E-4 (0.0003).
3. Connect the frequent itemsets output to Create Association Rules.
4. Configure the minimum confidence according to the required process setting and run the workflow.
The FP-Growth settings and final RapidMiner process pipeline are shown on page 6 of the PDF. The complete pipeline is:

Read CSV → Select Attributes → Pivot → Replace Missing Values → Numerical to Binominal → Select Attributes → FP-Growth → Create Association Rules
Python Equivalent Implementation
The supplied bank_customer_churn_system_exp9.py implements the same core data-mining sequence using Python:
1. Load the groceries CSV.
2. Group by Member_number and Date using a pivot table.
3. Replace missing values with zero.
4. Convert positive counts to True and zero to False.
5. Apply FP-Growth with minimum support 0.0003.
6. Generate association rules and sort by Lift.
7. Save the transaction matrix, frequent itemsets and rules.
Install dependencies
pip install pandas mlxtend
Run locally
1. Keep Groceries_dataset.csv in the same folder as bank_customer_churn_system_exp9.py.
2. Run:
python bank_customer_churn_system_exp9.py
Run in Google Colab
Run the script in a Colab notebook. It detects Google Colab and prompts you to upload Groceries_dataset.csv.
Generated Output Files
- bank_customer_churn_system_transaction_matrix.csv – Boolean transaction matrix.
- bank_customer_churn_system_fp_growth_itemsets.csv – Frequent itemsets and their support.
- bank_customer_churn_system_fp_growth_rules.csv – Association rules with support, confidence and lift.
Results and Interpretation
The supplied PDF states that the process was run on 14,963 grocery transactions. It describes the data preparation and FP-Growth workflow, but does not provide a complete numerical table of generated association rules or a verified count of frequent itemsets/rules. The screenshot on page 6 shows a frequent-itemset result panel, but it is not sufficient to infer a complete rules table.

Therefore, do not invent exact rule counts, support values, confidence values or lift values for this experiment. Run the RapidMiner process or Python script and record the actual output.

The PDF's result section on pages 6–7 states that FP-Growth processed the grocery transaction data with minimum support of 0.03%, followed by association-rule generation. Its inference describes high-lift associations between grocery items, such as baking ingredients and beverages with snacks, but does not provide a complete numerical rule listing.
Result
The Association Rule Mining workflow for the supplied Groceries dataset is implemented using RapidMiner's Pivot, Replace Missing Values, Numerical to Binomial, Select Attributes, FP-Growth and Create Association Rules operators. The equivalent Python implementation creates a transaction matrix, applies FP-Growth and generates association rules. Actual itemsets and rule metrics should be taken from the execution output.
Conclusion
FP-Growth is an efficient frequent-pattern mining algorithm that uses a compressed FP-Tree rather than repeatedly generating candidate itemsets. In this experiment, the grocery data is transformed from single-item rows into a transaction matrix, cleaned, converted to binomial values and processed using FP-Growth. Support, Confidence and Lift help evaluate the generated association rules. The discovered patterns can support market basket analysis, store-layout planning and cross-promotional retail strategies.
Note
Although the requested project heading says Bank Customer Churn System, this Experiment 9 implementation follows the Groceries dataset and Market Basket Analysis described in the supplied PDF. It must not be described as a bank churn prediction model or as analysis performed on Churn_Modelling.csv.

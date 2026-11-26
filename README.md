# MSCS 634 – Lab 6: Association Rule Mining with Apriori and FP-Growth

**Student:** Sandeep Reddy Polareddy  
**Course:** Advanced Big Data and Data Mining (MSCS-634-B01)  
**Dataset:** UCI Online Retail

## Purpose of the lab

The purpose of this lab was to explore association-rule mining using the Apriori and FP-Growth algorithms. The analysis used the UCI Online Retail transactional dataset to identify frequently purchased products and product combinations, generate association rules, and interpret those rules using support, confidence, and lift. Seaborn visualizations were used to explore item frequency, product co-occurrence, frequent itemsets, and the relationship between confidence and lift.

## Dataset preparation and key results

The original dataset contained **541,909 rows**. After cleaning, the analysis retained **519,865 item-level records**, representing **20,136 unique invoices** and **4,060 unique products**. The basket matrix contained 20,136 transactions and 4,060 product columns, with an average of **25.82 distinct products per invoice**.

The cleaning process removed records with missing invoice numbers or descriptions, cancelled invoices (invoice numbers beginning with `C`), non-positive quantities, and duplicate invoice-product entries. Product descriptions were trimmed and converted to uppercase. Each invoice was treated as one transaction, and each product was represented as present or absent in that transaction.

The most frequently purchased products by distinct invoice count were:

1. **WHITE HANGING HEART T-LIGHT HOLDER** – 2,260 invoices.
2. **JUMBO BAG RED RETROSPOT** – 2,092 invoices.
3. **REGENCY CAKESTAND 3 TIER** – 1,989 invoices.
4. **PARTY BUNTING** – 1,686 invoices.
5. **LUNCH BAG RED RETROSPOT** – 1,564 invoices.

These results indicate strong representation of decorative household items, bags, and cake-related products among the most common purchases.

## Apriori and FP-Growth comparison

Both algorithms were applied to the same transaction-item matrix with a minimum support threshold of **0.02 (2%)**.

| Measure | Apriori | FP-Growth |
|---|---:|---:|
| Frequent itemsets | 376 | 376 |
| Recorded runtime | 1.602 seconds | 42.981 seconds |

The algorithms returned identical itemset collections: there were no itemsets found exclusively by either algorithm, and the maximum absolute difference between corresponding support values was zero. In this recorded execution, **Apriori was faster**. FP-Growth took approximately 26.8 times as long. Although FP-Growth can be efficient on large or dense datasets because it avoids repeated candidate generation, performance depends on the dataset, implementation, hardware, and execution environment. The observed timing is specific to this run.

## Association rules and key insights

Association rules were generated using a minimum confidence threshold of **0.30 (30%)**. The analysis produced **151 rules**. Support measures how often the item combination appears in all transactions; confidence measures how often the consequent appears when the antecedent is present; and lift compares the rule's confidence with the consequent's baseline support. A lift greater than 1 indicates a positive association.

Three notable rules were:

| Association rule | Support | Confidence | Lift |
|---|---:|---:|---:|
| Pink Regency Teacup and Saucer → Green Regency Teacup and Saucer + Roses Regency Teacup and Saucer | 2.69% | 70.76% | 18.55 |
| Pink Regency Teacup and Saucer + Roses Regency Teacup and Saucer → Green Regency Teacup and Saucer | 2.69% | 90.48% | 17.95 |
| Gardeners Kneeling Pad Keep Calm → Gardeners Kneeling Pad Cup of Tea | 2.71% | 59.80% | 15.89 |

The Regency teacup rules show a strong relationship among the pink, green, and roses designs. In particular, 90.48% of transactions containing both the pink and roses designs also contained the green design. The gardening-pad rule suggests that customers who purchased the Keep Calm design often also purchased the Cup of Tea design. These associations could inform related-product recommendations, cross-selling, coordinated product displays, or bundled promotions.

Although the lift values are high, the support values are approximately 2.7%, meaning these combinations occurred in a relatively small proportion of all invoices. The rules identify co-occurrence, not causation.

## Challenges and decisions

A major challenge was preparing the raw retail data for association mining. The dataset contained missing descriptions, cancelled invoices, returns or other non-positive quantities, and duplicate product lines. These issues were addressed by cleaning the records before creating the transaction-item matrix. Invoice number was selected as the transaction identifier, and product description was used as the item label.

Threshold selection was another important decision. A 2% minimum support threshold was selected to identify recurring patterns without including every rare combination. A 30% confidence threshold was used to filter the generated rules. Thresholds that are too high may exclude potentially useful combinations, while thresholds that are too low can produce an excessive number of patterns and rules.

A further observation was that Apriori ran faster than FP-Growth in this particular execution, even though FP-Growth is often expected to perform well on large datasets. This reinforced the importance of measuring algorithm performance on the selected dataset rather than assuming one method will always be faster.

## Conclusion

The lab demonstrated how association-rule mining can reveal useful purchasing patterns in retail transaction data. Apriori and FP-Growth produced the same 376 frequent itemsets, while Apriori had the shorter recorded runtime. The 151 association rules highlighted strong co-purchasing relationships, especially among coordinated Regency teacup designs and themed gardening products. These findings can support product recommendations and cross-selling decisions, provided that support, confidence, and lift are considered together and that associations are not mistaken for causal relationships.

## Dataset source

UCI Machine Learning Repository. [Online Retail](https://archive.ics.uci.edu/dataset/352/online+retail).

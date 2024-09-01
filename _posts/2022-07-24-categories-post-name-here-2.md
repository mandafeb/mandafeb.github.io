---
title: "Drug Discovery on entamoeba histolytica"
excerpt: ""

categories:
  - Work
tags:
  - [work, bioinformatics]

permalink: /Work/Entamoeba/

toc: true
toc_sticky: true

date: 2022-12-10
last_modified_at: 2022-12-10
---

🦥

### Data Preprocessing
The data was required from ChEMBL Database. A chemical database of bioactive molecule with drug like properties.

<table class="table-wrapper" markdown="block" style='font-size:14px;display: flex; align-items: center; justify-content: center;'>
  <tr>
    <th>assay_chembl_id</th>
    <th>assay_type</th>
    <th>target_organism</th>
    <th>type</th>
    <th>units</th>
    <th>value</th>
  </tr>
  <tr>
    <td>CHEMBL676675</td>
    <td>F</td>
    <td>Entamoeba histolytica</td>
    <td>IC50</td>
    <td>uM</td>
    <td>0.069</td>
  </tr>
  <tr>
    <td>CHEMBL676675</td>
    <td>F</td>
    <td>Entamoeba histolytica</td>
    <td>IC50</td>
    <td>uM</td>
    <td>0.022</td>
  </tr>
  <tr>
    <td>CHEMBL676675</td>
    <td>F</td>
    <td>Entamoeba histolytica</td>
    <td>IC50</td>
    <td>uM</td>
    <td>0.35</td>
  </tr>
  <tr>
    <td>CHEMBL676675</td>
    <td>F</td>
    <td>Entamoeba histolytica</td>
    <td>IC50</td>
    <td>uM</td>
    <td>0.046</td>
  </tr>
</table>

after requiring the necessary data, we could explore the data using Lipinski Descriptor. Christopher Lipinski, a scientist at Pfizer, came up with a set of rule-of-thumb for evaluating the druglikeness of compounds. Such druglikeness is based on the Absorption, Distribution, Metabolism and Excretion (ADME) that is also known as the pharmacokinetic profile. Lipinski analyzed all orally active FDA-approved drugs in the formulation of what is to be known as the Rule-of-Five or Lipinski's Rule.
<br>
<br>
The Lipinski's Rule stated the following:
- Molecular weight < 500 Dalton
- Octanol-water partition coefficient (LogP) < 5
- Hydrogen bond donors < 5
- Hydrogen bond acceptors < 10
<br>
we also need to convert the $IC^{50}$ to $pIC{50}$, to allow $IC^{50}$ data to be more uniformly distributed, we will convert $IC^{50}$ to the negative logarithmic scale which is essentially $-log10(IC^{50})$. <br>
The conversion process is the following:

> * Take the $IC^{50}$ values from the standard_value column and converts it from nM to M by multiplying the value by $10^{-9}$
> * Take the molar value and apply $-log10$
> * Delete the **standard_value** column and create a new $pIC{50}$ column

Lastly, we need to clean the *salt* from the **Canonical SMILES**. 
> Canonical SMILES represents chemical information that partain to the chemical structure

The Canonical SMILES were pre-processed by applying sequential filters to remove stereochemistry, salts, and molecules with undesirable atoms or groups. SMILES strings $>100$ symbols in length were removed, as $∼97%$ of the dataset consists of SMILES strings with $<100$ symbols. The RDKit library in Python was used for this pre-processing. 

From the preprocessing, we get this data
<div class="table-wrapper" markdown="block" style='font-size:14px;display: flex; align-items: center; justify-content: center;'>

| molecule_chembl_id |               canonical_smiles | bioactivity_class |      MW |    LogP | NumHDonors | NumHAcceptors |    pIC50 |
|:------------------:|:------------------------------:|:-----------------:|:-------:|:-------:|:----------:|:-------------:|:--------:|
|        `CHEMBL55641` |     `FC(F)(F)c1nc2ccccc2[nH]1` |            active | 186.136 | 2.58170 |        1.0 |           1.0 | 7.161151 |
|        `CHEMBL53788` | `FC(F)(F)c1nc2cc(Cl)ccc2[nH]1` |            active | 220.581 | 3.23510 |        1.0 |           1.0 | 7.657577 |
|          `CHEMBL137` |    `Cc1ncc([N+](=O)[O-])n1CCO` |            active | 171.156 | 0.09202 |        1.0 |           5.0 | 6.455932 |
|        `CHEMBL56473` | `Cn1c(C(F)(F)F)nc2cc(Cl)ccc21` |            active | 234.608 | 3.24550 |        0.0 |           2.0 | 7.337242 |
|       `CHEMBL293520` |      `Cn1c(C(F)(F)F)nc2ccccc21`|            active | 200.163 | 2.59210 |        0.0 |           2.0 | 7.397940 |

</div>

### Model
The pre-processed **Canonical SMILES** are processed using PaDEL Descriptor to get the fingerprints which are later used for the modeling. The fingerprints acquired has 882 columns so to maximize the result, the low variance are removed using the **Variance Threshold** from the sklearn feature selection.

<div class="table-wrapper" markdown="block" style='font-size:14px;display: flex; align-items: center; justify-content: center;'>

|   | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | ... | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 |
|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|--:|----:|----:|----:|----:|----:|----:|----:|----:|----:|----:|----:|
| 0 | 1 | 1 | 1 | 0 | 1 | 0 | 1 | 1 | 1 | 1 | ... |   1 |   1 |   1 |   1 |   1 |   1 |   0 |   1 |   1 |   0 |
| 1 | 1 | 1 | 1 | 1 | 0 | 0 | 1 | 1 | 1 | 1 | ... |   1 |   1 |   1 |   1 |   0 |   0 |   0 |   0 |   0 |   0 |
| 2 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | ... |   1 |   1 |   1 |   0 |   1 |   0 |   0 |   1 |   0 |   0 |
| 3 | 1 | 1 | 1 | 0 | 1 | 0 | 1 | 1 | 1 | 0 | ... |   1 |   1 |   1 |   0 |   1 |   1 |   0 |   1 |   1 |   0 |
| 4 | 1 | 1 | 1 | 1 | 0 | 0 | 1 | 1 | 1 | 1 | ... |   1 |   1 |   1 |   0 |   0 |   0 |   0 |   0 |   0 |   0 |

</div>

After applying 3 different algorithm (CatBoost, Forest Regressor, Gradient Boost) and comparing the results, it is concluded that **Gradient Boost** with **Grid CV** is the best algorithm to use in this case with the $R^2 = 8.38$.

<!-- <img src="/assets/entamoeba/test.png" style='margin:10px;width:75%;margin-left:100px;'/> -->

<button class="button button1" onclick="window.open('https://github.com/mandafeb/Entamoeba','_blank')" type="button"><b>Code</b></button> 

<br />
<br />

<button class="button button1" type="button" style='margin:10px;'><b>Demo</b></button> 
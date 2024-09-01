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

## 🦥

<b>Data preprocessing</b><br>
The data was required from ChEMBL Database. A chemical database of bioactive molecule with drug like properties.

<table>
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
The Lipinski's Rule stated the following:<br>
- Molecular weight < 500 Dalton
- Octanol-water partition coefficient (LogP) < 5
- Hydrogen bond donors < 5
- Hydrogen bond acceptors < 10
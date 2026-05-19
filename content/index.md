---
title: Journal Club
---

# Computational Biology Journal Club

A biweekly reading list tracking **antibody design** and **targeted protein degradation** through a computational lens — with emphasis on methods that have been experimentally validated and datasets that move us closer to clinical outcomes.

Inspired by the [CeTPD Journal Club](https://sites.dundee.ac.uk/alessio-ciulli/) (Ciulli Lab, Dundee) and [NaturalAntibody](https://www.linkedin.com/company/naturalantibody/)'s weekly highlights. The goal is not a list — it's applied DIKW: turning papers into information, and information into knowledge by connecting the dots.

---

## Recent Issues

```dataview
TABLE period AS "Period", length(file.tags) AS "Papers"
FROM "issues"
SORT date DESC
LIMIT 6
```

---

## Topics

- [[topics/antibody-design|Antibody Design & Engineering]]
- [[topics/protein-degradation|Targeted Protein Degradation]]
- [[topics/ml-clinical-translation|ML for Clinical Translation]]

---

## What we look for

| Priority | Criteria |
|----------|----------|
| ★★★ | Computational method + experimental validation in the same paper |
| ★★ | Computational method benchmarked on real-world data |
| ★★ | Novel dataset enabling ML toward clinical outcomes |
| ★ | Strong preprint from leading groups |

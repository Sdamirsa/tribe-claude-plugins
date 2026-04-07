---
name: crystalit-labeler
description: "Labels each research paper against a thematic ontology, producing structured JSON files as part of the CrystaLit pipeline. Use this skill when the user says 'label my papers,' 'code papers against the ontology,' 'create JSON labels,' 'apply the taxonomy to papers,' or when the crystalit orchestrator dispatches Phase 3. Reads each paper's note and PDF, maps concepts from the YAML ontology, and outputs one JSON per paper."
---

# CrystaLit Labeler

You are a meticulous research coder who reads papers and assigns labels from a predefined ontology. Your labels become the data layer for all downstream analysis, so precision matters more than speed. A wrongly applied label propagates through figures and reports; a missing label means a paper's contribution is invisible.

## The Cardinal Rule (inherited from Noter)

Label based on **methods and results only**. If a paper mentions U-Net in its introduction as related work but actually uses a Random Forest, label it as Random Forest, not U-Net. If a paper discusses potential clinical applications in the discussion but only validated on phantoms, label the validation type as phantom, not clinical.

## Input

1. The finalized YAML ontology (from crystalit-ontologist)
2. The markdown note for each paper (from crystalit-noter)
3. Access to the original PDF when the note is ambiguous

## Output Format

One JSON file per paper, named to match the paper's filename:

```json
{
    "paper_id": "AuthorLastName_Year",
    "title": "Full title of the paper",
    "T1_Theme_Name": {
        "T1-S1_Subtheme_Name": [
            "Concept_One",
            "Concept_Two"
        ],
        "T1-S2_Another_Subtheme": [
            "Concept_Three"
        ]
    },
    "T2_Another_Theme": {
        "T2-S1_Subtheme": []
    }
}
```

Every subtheme must appear in the JSON, even if the paper has no applicable concepts for it (use an empty array `[]`). This ensures consistent structure across all JSON files and simplifies aggregation.

## Labeling Decision Rules

Read the reference file `references/labeling-rules.md` for detailed decision rules. The core principles are:

### Inclusion Threshold

Apply a concept label if the paper **uses, evaluates, or directly contributes to** that concept in its methods or results. Do not apply a label if the paper merely mentions the concept in passing, cites another paper that uses the concept, or discusses the concept as future work.

### Multi-label Is Expected

Papers typically map to multiple concepts per subtheme. A paper using U-Net with data augmentation on contrast-enhanced CTA for left ventricle segmentation should be labeled under Algorithm (U-Net), Preprocessing (Data Augmentation), Data Modality (Contrast Enhanced CTA), and Anatomy (Left Ventricle), among others.

### Ambiguity Resolution

When a paper's methodology is ambiguous (e.g., unclear whether gating was prospective or retrospective), check the original PDF. If still ambiguous, apply the most conservative label and add a comment field:

```json
"_comments": ["Gating type unclear from text; labeled as ECG_Gated only"]
```

### Hierarchy Respect

Label at the concept level, not the group or subtheme level. The hierarchy exists for organization, but the atomic unit of labeling is the concept.

## Process for Each Paper

1. Open the markdown note
2. Read through the Data Extract section, mapping each detail to ontology concepts
3. Check the Theme Summary section (from the noter) as a cross-reference
4. For each theme and subtheme in the ontology, decide which concepts apply
5. If uncertain, consult the original PDF
6. Write the JSON file with all subthemes represented

## Aggregation

After all papers are labeled, aggregate all JSON files into a single `all_papers.json` that contains the complete ontology structure plus the full set of labeled papers. This aggregated file is the input for Phase 4 (visualization).

The aggregation should also produce summary statistics: how many papers map to each concept, which concepts are unused (potential over-specification in the ontology), and which papers have unusually few labels (potential under-coding).

## Quality Checks

After labeling the full set, run these checks:

1. **Completeness:** Every paper should have at least one concept in every theme (unless the theme is genuinely not applicable, e.g., a paper on calcium scoring has no anatomy segmentation labels).
2. **Distribution:** If any single concept has more than 80% of papers, it may be too broad. If a concept has zero papers, it may be too narrow or misplaced.
3. **Consistency:** Papers with similar methods should have similar label patterns. Spot-check 2-3 pairs of similar papers to verify.

## What to Hand Off

The collection of JSON files and the aggregated `all_papers.json` go to the crystalit-vizmaker for Phase 4. The labeling rules and any edge-case decisions documented in `_comments` fields should be preserved for the user's review.

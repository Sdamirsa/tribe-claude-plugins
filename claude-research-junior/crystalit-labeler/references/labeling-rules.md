# CrystaLit Labeling Decision Rules

This reference document encodes the decision rules for labeling papers against the thematic ontology. It addresses the ambiguous cases that arise in practice and provides consistent resolution strategies.

## General Principles

### Source Sections

Label from methods and results sections only. Introduction and discussion sections contain claims about other work and speculation that should not be attributed to the current paper.

**Exception:** The Gaps section in the noter output may capture author-acknowledged limitations from the discussion. These can inform whether to label concepts related to validation scope or study design, but should be treated as self-reported rather than independently verified.

### Granularity

Always label at the most specific applicable concept level. If a paper uses nnU-Net, label it as `nnU-Net`, not just as `Deep_Learning` or `3D_CNN`. The group and theme hierarchy is for organization, not for labeling.

### Absence vs. Non-Applicability

An empty array `[]` for a subtheme means "this paper does not use or contribute to any concept in this subtheme." This is different from "we could not determine the label." If you genuinely cannot determine a label, use the `_comments` field to flag the ambiguity.

## Domain-Specific Rules

### Algorithm Labels

Label the **index model** (the main contribution) and any **comparison models** evaluated in the paper. If a paper proposes a new architecture and compares it against U-Net and nnU-Net as baselines, all three should be labeled.

If a paper uses an existing architecture without modification, label the specific architecture name. If it modifies an architecture (e.g., "attention U-Net with squeeze-and-excitation blocks"), label both the base architecture and the modification.

### Data Labels

**Population size categories:** Use the thresholds defined in the ontology (e.g., Small < 200, Medium 200-1000, Large > 1000). If the paper reports separate training and test sets, use the total number of unique subjects.

**Multi-center vs. single-center:** Label based on the data used, not on where the authors are from. A paper from three institutions that uses data from only one center is single-center.

**Imaging modality:** Label the actual modality used in the study. If a paper uses contrast-enhanced CTA but also mentions non-contrast CT as context, label only CTA.

### Evaluation Labels

**Reference standard:** Label what the paper actually compared against. If manual segmentation by an expert was the reference, label it as such, even if the paper also mentions that echocardiography could serve as a clinical reference.

**Validation type:** Internal test set means a held-out split from the same institution. External validation means data from a different institution not seen during training. Cross-validation is its own category.

### Clinical Application Labels

Label the **demonstrated** clinical application, not the intended one. If a paper proposes a model for cardiac function assessment but only validates segmentation accuracy (Dice scores), label the task as segmentation, not as clinical function assessment. You may note the intended application in the `_comments` field.

### Anatomy Labels

Label all anatomical structures that the paper's method **explicitly segments, quantifies, or analyzes**. If a whole-heart segmentation method segments 7 structures, list all 7.

## Edge Cases

### Papers Using Foundation Models

If a paper applies TotalSegmentator or another foundation model without modification, label the foundation model name. If it fine-tunes the foundation model, label both the foundation model and the fine-tuning strategy.

### Review Papers in the Collection

If the collection includes review papers (unusual for CrystaLit, which targets originals), label them only for their methodological contribution (e.g., a novel taxonomy they propose), not for the papers they review.

### Multi-Task Papers

Some papers address multiple tasks (e.g., segmentation + classification + quantification). Label all tasks that have methods and results. Do not label tasks that are mentioned only as potential extensions.

## Version Control

If the ontology is updated after labeling has begun, re-label any papers that are affected by the changes. The orchestrator should flag which concepts were added, removed, or moved, and identify which papers need re-labeling.

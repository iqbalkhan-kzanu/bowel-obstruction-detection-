Automated CT-Based Assessment of Bowel Obstruction

A system that takes a single abdominal CT scan and automatically checks for small bowel obstruction, using a fine-tuned segmentation model and a set of measurements drawn from published radiology literature.

Built as part of an M.Tech project at IIT Mandi.

What it does
Given a raw CT scan, the pipeline:

Segments the liver, small bowel, and colon using a TotalSegmentator model fine-tuned on the AbdomenAtlas dataset
Measures small bowel diameter and flags obstruction if two or more distinct loops exceed 2.5 cm (Fukuya et al., 1992)
If obstruction is flagged, computes a bowel-to-liver attenuation ratio as an ischemia signal (adapted from Fujishima et al., 2025) and estimates free fluid volume (Matsushima et al., 2016)
Outputs one structured JSON report and one annotated image per scan
Why
Diagnosing bowel obstruction on CT currently depends on a radiologist manually weighing several signs together — dilatation, wall enhancement, free fluid. It's not fast and it's not perfectly consistent between readers. This project tries to automate the parts of that process that already have published, validated thresholds behind them, and be upfront about the parts that don't.

Results

Segmentation Dice score improved from 0.707 to 0.781 after fine-tuning (n=24 test cases). IoU improved from 0.578 to 0.671.
Diameter measurement was checked against a synthetic phantom of known size (30mm) — 0.70% error.
Tested on 4 cases (3 confirmed normal, 1 confirmed obstruction) — all correctly classified. Small sample, stated honestly, not a claim of clinical validation.
Ischemia ratio and free fluid detection were checked against known-negative cases (healthy patients, solid liver tissue) and behaved as expected.
Full numbers and methodology are in docs/results.md.

What this isn't
This is not a clinically validated diagnostic tool. Two things in particular need more work before it could be:

No public dataset exists with confirmed bowel obstruction diagnoses attached to CT volumes, so detection accuracy has only been checked on a handful of cases, not a proper labeled cohort.
The ischemia ratio is adapted from a manually-measured biomarker to automated whole-mask averaging, and hasn't been independently re-validated. It also showed moderate sensitivity to small changes in the segmented mask boundary during stress testing (CV ~26%).
Both are documented honestly in the results write-up rather than glossed over.

Setup

bash
conda create -n bowel_env python=3.10
conda activate bowel_env
pip install nnunetv2 SimpleITK nibabel matplotlib scipy scikit-image
You'll also need a trained nnU-Net model (fine-tuning instructions in docs/finetuning.md) and to set the standard nnU-Net environment variables:

bash
export nnUNet_raw=/path/to/nnunet_raw
export nnUNet_preprocessed=/path/to/nnunet_preprocessed
export nnUNet_results=/path/to/nnunet_results
Usage
python
from pipeline_core import analyze_new_ct

report = analyze_new_ct("path/to/scan.nii.gz", work_dir="outputs/my_case")
This runs segmentation, computes all measurements, and saves report.json plus an annotated result image to outputs/my_case/.

Repository structure

pipeline_core.py       core segmentation + measurement logic
validation/            phantom test, outcome table, sanity checks
docs/
  results.md           full metrics and figures
  finetuning.md         how the segmentation model was fine-tuned
  limitations.md        known gaps, stated directly  

References
Fukuya, T., et al. (1992). CT diagnosis of small-bowel obstruction: efficacy in 60 patients. AJR 158(4), 765-769.
Fujishima, H., et al. (2025). Bowel to liver attenuation ratio as a predictor of resection in strangulated small bowel obstruction. Emergency Radiology 32.
Matsushima, K., et al. (2016). Free fluid on CT as a predictor of surgical management in bowel obstruction. J Gastrointest Surg 20(11), 1861-1866.
Isensee, F., et al. (2021). nnU-Net: a self-configuring method for deep learning-based biomedical image segmentation. Nature Methods 18(2), 203-211.
Wasserthal, J., et al. (2023). TotalSegmentator: robust segmentation of 104 anatomic structures in CT images. Radiology: AI.
Qu, C., et al. AbdomenAtlas: a large-scale, multi-institution CT dataset for abdominal organ segmentation.
 
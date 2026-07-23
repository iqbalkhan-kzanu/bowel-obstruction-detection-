Automated CT-Based Assessment of Bowel Obstruction
An automated system that detects small bowel obstruction from a single abdominal CT scan. It segments the liver, small bowel, and colon using a fine-tuned model, then measures bowel diameter and checks it against a validated clinical threshold. If obstruction is flagged, it also estimates an ischemia risk ratio and free fluid volume.

Built as part of an M.Tech research project.

Output
For each CT scan, the system produces:

Status — normal or obstruction detected
Small bowel dilatation — maximum diameter, number of distinct dilated loops, and whether it crosses the 2.5cm clinical threshold
Colon diameter — reported for reference, not used in detection
Ischemia ratio — bowel-to-liver CT attenuation ratio, computed only if obstruction is flagged
Free fluid — estimated volume and grade, also computed only if obstruction is flagged
One annotated image showing the segmented CT slice alongside all of the above, plus a structured JSON report
Results
Segmentation was fine-tuned on an independent, multi-institution dataset (AbdomenAtlas), improving accuracy over the base model:

Dice: 0.707 → 0.781
IoU: 0.578 → 0.671
(n = 24 held-out test cases)
Diameter measurement was checked against a synthetic phantom of known size, giving 0.70% error.

The system correctly classified all 5 tested cases — 1 confirmed obstruction and 4 confirmed normal.

Tested on cases from the AbdomenAtlas dataset (1 obstruction case) and the TCIA Pancreas-CT collection (3 healthy controls) — both real, de-identified patient CT scans. One additional real CT scan, obtained informally with consent, was also tested and correctly showed no obstruction. One result was independently reviewed by a radiologist, who found it consistent with the CT findings.

Both the classification results (normal vs. obstruction cases) and the segmentation Dice/IoU scores, before and after fine-tuning, are provided below.

            
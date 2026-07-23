Automated CT-Based Assessment of Bowel Obstruction
An automated system that detects small bowel obstruction from a single abdominal CT scan. It segments the liver, small bowel, and colon using a fine-tuned model, then measures bowel diameter and checks it against a validated clinical threshold. If obstruction is flagged, it also estimates an ischemia risk ratio and free fluid volume.

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

The system correctly classified all 4 tested cases — 3 confirmed normal, 1 confirmed obstruction.


   
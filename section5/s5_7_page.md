---
layout: default
title: "Step Counting"
nav_order: 7
parent: "Dataset Research Studies"
---

# Step Counting

This study evaluates the ability of classical signal processing techniques to count steps using accelerometer data. The results obtained are placed in the context of the current state of the scientific literature in this field.

### Methodology

To use a dataset labelled with ground-truth values for step counting, one structured activity (treadmill) and several (9) semi-structured activities were considered:  walking at usual speed, walking with shopping, walking with a mobile phone or book, walking in a zigzag pattern, standing whilst moving books, standing whilst folding towels, standing whilst sweeping, jogging, and going up and down stairs.

We developed 8 different step-counting methods:
1.	Model_cog_walking_events (based on hip accelerometry).
2.	Model_thigh_walking_events (based on thigh accelerometry).
3.	Model_step_count_wrist (based on wrist accelerometry).
4.	Model_step_count_thigh (based on thigh accelerometry).
5.	Model_step_count_hip (based on hip accelerometry).
6.	Model_step_detection_threshold_thigh (based on thigh accelerometry).
7.	Model_step_detection_threshold_hip (based on hip accelerometry).
8.	Model_verisense_thigh (based on thigh accelerometry). This algorithm is currently in the development and debugging phase.


### Key Findings

- Using Algorithm 1, the median MAPE value for the various activities is around 5%; in most cases, it is lower than this.

- The compilation of statistics on the results of the other algorithms is currently in progress.


### Key Insights

- There are outliers in the MAPE distribution for certain activities (“treadmill” and “walking with a mobile phone or book”). This may be due to difficulties in manually counting steps caused by obstructions in the camera. The trend observed is that the number of steps recorded as ground truth for the “walking with a mobile phone or book” activity is around 200 for all subjects.

- A step will be counted each time one of the feet moves and makes contact with the ground under load, resulting in a visible forward movement of the torso in the direction of travel. The step will be recorded at the moment of initial contact (IC) of the supporting foot, whether heel or toe, as visually identified.


### Reference

- Lopez AM, Alvarez D, Gonzalez RC, Alvarez JC. Validity of four gait models to estimate walked distance from vertical COG acceleration. J Appl Biomech. 2008 Nov;24(4):360-7. PMID: 19075305. | [Paper's abstract available from this link 🔬](https://pubmed.ncbi.nlm.nih.gov/19075305/) |

- Alvarez, J.C.; Álvarez, D.; López, A.M. Accelerometry-Based Distance Estimation for Ambulatory Human Motion Analysis. Sensors 2018, 18, 4441. https://doi.org/10.3390/s18124441. | [Paper available from this link 🔬](https://www.mdpi.com/1424-8220/18/12/4441) |

- Koffman, L., & Muschelli, J., III. (2024). Evaluating Step Counting Algorithms on Subsecond Wrist-Worn Accelerometry: A Comparison Using Publicly Available Data Sets. Journal for the Measurement of Physical Behaviour, 7(1), Article jmpb.2024-0008, jmpb.2024-0008. Retrieved Apr 18, 2026, from https://doi.org/10.1123/jmpb.2024-0008. | [Paper's abstract available from this link 🔬](https://journals.humankinetics.com/view/journals/jmpb/7/1/article-jmpb.2024-0008.xml) |

- **The publication of a scientific article is planned for the future.**


### Report

<figure>
  <iframe src="./assets/files/Regression_MET_draft.pdf" width="320" height="240" style="width:320px;max-width:100%;height:240px;" title="Step Counting">
      This browser does not support PDFs. Please download the PDF to view it: <a href="./assets/files/cuenta_pasos.pdf">Download PDF</a>
  </iframe>
</figure>



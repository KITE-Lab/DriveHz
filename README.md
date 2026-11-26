# DriveHz

**More Camera Frames, Better Driving? Within-Scene Information Scaling in Autonomous Driving**

Yumao Liu<sup>1,∗</sup>, Rui Qian<sup>2,∗</sup>, Qiang Huang<sup>2</sup>, Jiahao Wei<sup>2</sup>, Tao Liu<sup>1</sup>, Xiangyu Li<sup>1</sup>, Ke Ma<sup>1,✉</sup>

<sup>1</sup> The Hong Kong University of Science and Technology (Guangzhou) · <sup>2</sup> Fudan University<br>
<sup>∗</sup> Equal contribution · <sup>✉</sup> Corresponding author: [Ke Ma](mailto:kema@hkust-gz.edu.cn)

[Read the paper (PDF)](https://openreview.net/pdf?id=clDpaFHaep) · **Code coming soon**

![Overview of within-scene information scaling. Sparse sampling can miss driving-relevant information. Denser sampling exposes more of the scene and increases driving-irrelevant exposure. The model predicts that the preferred sampling frequency rises with capacity.](assets/within-scene-information-scaling.png)

How densely should a model sample camera frames from the same driving scenes during training? We vary the density of training timestamps while keeping each model’s input format and prediction target fixed. Within the E2EDriver family, increasing model width raises the preferred temporal sampling frequency for trajectory prediction (ADE/FDE). At fixed width, adding driving-irrelevant noise shifts that preference downward.

## How does trajectory error change with sampling frequency?

![Three panels show average displacement error against temporal sampling frequency on Waymo, nuScenes, and PAVE. AutoVLA reaches its lowest reported error at the highest tested frequency on each dataset. Tiny-SSR achieves low error even at sparse frequencies.](assets/frequency-response-curves.png)

On all three datasets, Tiny-SSR at 2 Hz already achieves lower mean average displacement error (ADE) than AutoVLA at its highest tested frequency. Yet AutoVLA gains more from sampling densely. The curves show why we evaluate both driving performance under sparse temporal exposure and gains from denser temporal exposure.

## Does longer training on sparse data recover the gain?

We match optimization updates within each model on NAVTEST by training longer on the 2 Hz anchors. The table shows the driving score (EPDMS) from Table 5 of the paper. Values are means ± sample standard deviation over three seeds. Higher is better.

| Model | Epochs at 2 Hz / 10 Hz | EPDMS at 2 Hz (%) | EPDMS at 10 Hz (%) |
|---|---:|---:|---:|
| AutoVLA (answer-only SFT) | 25 / 5 | 77.881 ± 0.055 | 81.135 ± 0.085 |
| DriveVLA-W0 (Emu3Pi0) | 20 / 4 | 86.787 ± 0.051 | 87.366 ± 0.093 |

AutoVLA retains a 3.25-point EPDMS advantage at 10 Hz after matching updates. DriveVLA-W0 scores 85.673 at 2 Hz under its original four-epoch budget (Table 4). Training longer on the same sparse anchors brings it within 0.58 points of its 10 Hz score.

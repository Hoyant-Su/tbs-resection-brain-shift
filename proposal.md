## Name

Haoyang Su

## Email

hoyantsu@gmail.com

## Role

PhD Student in AI4Medicine

## Affiliation

Fudan University

## Professional Profile

https://hoyant-su.github.io/

## Academic Profile

https://orcid.org/0000-0003-3378-1440

## GitHub Username

Hoyant-Su

## Academic Work & Publications

My publications in medical imaging include:

1. **A Multi-phase CT Dataset for Automated Differential Diagnosis of Liver Tumors.** Scientific Data 13, 31 (2026). Joint first author. This work presents a public CT dataset and a benchmark for liver tumor differential diagnosis across contrast phases. https://www.nature.com/articles/s41597-025-06343-4

2. **CTSL: Codebook-Based Temporal-Spatial Learning for Accurate Non-contrast Cardiac Risk Prediction Using Cine MRIs.** MICCAI 2025. First author. This work develops representations of cardiac motion from cine MRI for cardiovascular risk prediction. https://doi.org/10.1007/978-3-032-05182-0_15

3. **PRISM: A Framework Harnessing Unsupervised Visual Representations and Textual Prompts for Explainable MACE Survival Prediction from Cardiac Cine MRI.** arXiv preprint (2025, revised 2026). First author. This work combines cardiac MRI representations and clinical information for survival prediction and interpretation. https://arxiv.org/abs/2508.19325

Academic homepage: https://hoyant-su.github.io/

## Relevant Experience

I am a PhD student at Fudan University working on medical image analysis and medical AI. My research includes multiphase contrast CT, cardiac cine MRI, representation learning, and clinical outcome prediction. In CTSL and PRISM, I developed methods for extracting cardiac motion and imaging representations and evaluating their relationship to cardiovascular outcomes.

For multiphase liver CT, I developed the image preprocessing and registration pipeline, established a benchmark for liver tumor differential diagnosis, and evaluated the contribution of individual contrast phases and their combinations to diagnostic performance. I also prepared and released the dataset and analysis code accompanying our Scientific Data publication.

## Proposal Title

Quantifying Brain Displacement Across Stages of Glioma Resection from Intraoperative Ultrasound

## Scientific Domain

Life Sciences

## Field

Medicine

## Subfield

Medical Imaging and Image Guided Surgery

## Scientific Problem

We examine how much anatomical displacement continues between an intermediate ultrasound acquisition and the end of glioma resection. The result is a set of anatomical trajectories across three acquired ultrasound volumes and a quantitative estimate of the additional displacement after the intermediate acquisition.

Opening the dura, cerebrospinal fluid loss and tissue removal alter brain anatomy during surgery. Coordinates established before resection can therefore become inaccurate as the operation progresses. Repeated ultrasound provides an inexpensive way to observe these changes, but the resection cavity, altered acoustic appearance and incomplete correspondence make intensity alignment unreliable. Recovering the same anatomical structures across acquisitions is necessary before interpreting the displacement as a physiological change.

This is an established workflow in research on correction of brain shift using intraoperative ultrasound. The task uses one real operation from RESECT and independently annotated anatomical correspondences. It asks for patient-specific motion quantification without extrapolating the result to treatment efficacy or other patients.

## Workflow Details

Inputs are three acquired 3D ultrasound volumes obtained before, during and after resection, two lists of anatomical coordinates in the initial image, and the identities of twelve positions shared by both lists. The images have been resampled to a common 0.5 mm grid with their physical geometry preserved.

The agent inspects acquisition geometry and ultrasound appearance, develops a method for establishing anatomical correspondence despite tissue removal, and estimates each landmark position in the appropriate later volume. It then reconstructs the trajectories of the twelve shared positions and calculates median displacement during the early interval, over the whole operation, and during the interval between the intermediate and final acquisitions.

Outputs consist of the estimated anatomical coordinates and a numerical summary of displacement. No registration code, target coordinates or deformation field is supplied to the agent. The method may use established registration libraries, local anatomical matching or a combination of approaches.

## Requirements

The reference implementation uses Python 3.12, NumPy 2.2.6 and SimpleITK 2.5.6. The independent verifier requires NumPy, with pytest 8.4.1 and pytest-json-ctrf 0.3.5 for the benchmark test interface.

A Linux workstation with 4 CPU cores, 4 GB RAM and 4 GB free storage is recommended. No GPU or model weights are required. The reference performs two multiscale registrations in 51.5 seconds on four CPU threads, with approximately 790 MB peak resident memory. This is execution time after implementation. The task can be developed and evaluated directly in Python without Docker.

## Dataset

The task uses the before, during and after resection ultrasound acquisitions from Case 1 of RESECT, a public clinical database of glioma surgery. The complete task payload, including evaluation annotations, occupies approximately 21.75 MB on disk and 37.47 MB when compressed scientific arrays are expanded. Ultrasound intensities were resampled to a common 0.5 mm grid after mild antialias filtering. The data are acquired patient images, with no simulated anatomy or imposed deformation.

The agent receives 34 initial positions to locate during resection and 13 initial positions to locate after resection. Twelve positions are shared across both lists. Independent target positions are kept separately for verification. In the original study, one rater defined the initial reference positions. Two raters then independently located the corresponding targets twice, and the four target observations were averaged. This provides anatomical reference measurements independent of the registration method.

The dataset is distributed under Creative Commons Attribution 4.0. The release is available at https://doi.org/10.11582/2017.00004. Individual files can be obtained from the archive without downloading the complete dataset. The compact task inputs are included with the implementation.

## Evaluation Strategy

The verifier compares every submitted coordinate with the independent manual target in physical millimetres. The during acquisition must have mean target registration error at most 2.0 mm and 90th percentile error at most 4.0 mm. The after acquisition must have mean error at most 2.5 mm and 90th percentile error at most 4.0 mm. The median additional displacement from the intermediate to the final acquisition must be within 1.0 mm of the value obtained from the twelve paired manual trajectories.

The verifier also checks coordinate shapes, finite values and consistency of the reported displacement summaries with the submitted coordinates. It accepts any method that satisfies the anatomical and quantitative criteria. It does not compare a dense deformation field with an assumed physical truth or require a particular interpolation convention.

The reference obtains mean errors of 1.70 mm during resection and 1.97 mm after resection. Its estimate of median additional displacement is 4.89 mm, compared with 4.21 mm from independent annotations. Keeping the original coordinates gives mean errors of 2.32 mm and 5.80 mm and fails. A tested rigid intensity registration gives a mean after error of 3.20 mm and also fails that criterion. These are measured results for this case, not a claim that every possible rigid method must fail.

The tolerances are research targets at a millimetre scale. They are above the approximately 0.27 mm mean inter-rater distance reported across the study for the two ultrasound annotation settings and are compatible with the scale of errors reported in published RESECT registration research. They do not establish clinical safety.

## Complexity

A medical imaging researcher familiar with registration is estimated to need about three hours to inspect the acquisitions, implement and check a correspondence workflow, and produce the displacement analysis. This development estimate has not been timed in a controlled expert study.

The agent must distinguish physical coordinates from array indices, handle ultrasound intensity changes and tissue removal, choose a registration strategy and its regularization, and compute trajectories across acquisitions. A standard multiscale deformable registration is sufficient when configured appropriately. One independent Codex run configured with gpt-6-astra and high reasoning effort passed the task in 35.4 minutes. It used multiscale registration, local image matching and visual anatomical checks, without access to the reference solution or target annotations. One run does not establish a population success rate. The task does not require a new registration algorithm. The substantive result is an anatomically verified estimate of ongoing brain displacement in a real operation.

## References & Resources

Xiao Y et al. REtroSpective Evaluation of Cerebral Tumors (RESECT): A clinical database of pre-operative MRI and intra-operative ultrasound in low-grade glioma surgeries. Medical Physics, 2017.
https://doi.org/10.1002/mp.12268

RESECT dataset, Creative Commons Attribution 4.0.
https://doi.org/10.11582/2017.00004

Drobny D et al. Landmark-based evaluation of a block-matching registration framework on the RESECT pre- and intra-operative brain image data set. MICCAI workshops, 2019.
https://doi.org/10.1007/978-3-030-33642-4_15

## Supporting Files

The complete implementation, compact inputs, reference solution, separate evaluation annotations, validation results and independently generated agent outputs are available at https://github.com/Hoyant-Su/tbs-resection-brain-shift.

## Additional Information

The implementation and validation were developed with AI assistance. Validation was performed in native Python on CPU. Container definitions are included for integration, but Docker execution has not been tested on the available host. The original dataset and annotations are public. The instructions prohibit retrieving exact target annotations. Execution traces require review because numerical verification cannot establish output provenance or exclude training contamination.

## Commercial Affiliation & Conflicts of Interest

I participate in other community research and evaluation projects, including RLE-Bench, AutoConference, OpenRSI and RSI-Exam. This proposal is submitted in my academic capacity at Fudan University.

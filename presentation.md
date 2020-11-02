class: center middle

# BIDS Applications and Derivatives
### Christopher J. Markiewicz
#### Center for Reproducible Neuroscience
#### Stanford University

###### [effigies.github.io/bids-derivatives](https://effigies.github.io/bids-derivatives)

---
name: footer
layout: true

<div class="slide-slug">Open and Reproducible Neuroimaging - Oldenburg - Nov 2020</div>

---
layout: true
template: footer
name: BIDS

# BIDS: Brain Imaging Data Structure

---
layout: true
template: BIDS

.pull-left[![Bids Layout](assets/bids.png)]

---

.pull-right[
* BIDS is a directory structure, not a binary format

* Builds on existing standards (NIfTI, JSON, TSV)

* Intended for human *and* machine legibility

* The [BIDS Validator](https://bids-standard.github.io/bids-validator)
  makes compliance easy to verify

* The [specification](https://bids-specification.readthedocs.io/en/stable/)
  is a searchable HTML document
]

---

.pull-right[

* Basic metadata in the file names

  * Subject, session, imaging modality, etc.
  * Generally just enough to assign unique names

]

---
count: false

.pull-right[
* Basic metadata in the file names
  * Subject, session, imaging modality, etc.
  * Generally just enough to assign unique names

* NIfTI headers and JSON sidecars contain detailed,
  image-related metadata

]

---
count: false

.pull-right[
* Basic metadata in the file names
  * Subject, session, imaging modality, etc.
  * Generally just enough to assign unique names

* NIfTI headers and JSON sidecars contain detailed,
  image-related metadata

* [`dataset_description.json`](https://bids-specification.readthedocs.io/en/stable/03-modality-agnostic-files.html#dataset_descriptionjson),
  [`participants.tsv`](https://bids-specification.readthedocs.io/en/stable/03-modality-agnostic-files.html#participants-file),
  [`sessions.tsv`](https://bids-specification.readthedocs.io/en/stable/06-longitudinal-and-multi-site-studies.html#sessions-file),
  and [`scans.tsv`](https://bids-specification.readthedocs.io/en/stable/03-modality-agnostic-files.html#scans-file)
  record study-level metadata that may not be associated
  with specific images
]

---
layout: true
template: footer

---
layout: true
template: footer
name: Apps

# BIDS Applications

---

.install-cmd[
```Bash
pip install pybids
```
]

A common specification of neuroimaging datasets affords queries for and
adaptation to the available data.

For example, [PyBIDS](https://github.com/bids-standard/pybids/) allows:

```Python
>>> layout = BIDSLayout('/data/bids/ds000003')
>>> bold = layout.get(subject='01', suffix='bold')
>>> bold[0].filename                                                                           
'sub-01_task-rhymejudgment_bold.nii.gz'
>>> bold[0].get_metadata()
{'RepetitionTime': 2.0, 'TaskName': 'rhyme judgment'}
```

--

&nbsp;

Queryable (meta)data allows a very simple protocol for a
[BIDS App](https://bids-apps.neuroimaging.io/apps/):

```Bash
bids-app /bids-directory /output-directory participant [OPTIONS]
```

.footnote[
\* Note that `participant` is an analysis level. Apps may also operate
at the `run`, `session` or `dataset` levels.]

---

.pull-left[
### Many application types are possible
]
.pull-right[
[![:img BIDS App list, 100%](assets/bids-apps.png)](https://bids-apps.neuroimaging.io/apps/)
]

---
count: false

.pull-left[
### Many application types are possible

* Quality control

]
.pull-right[
[![:img BIDS QC Apps, 100%](assets/bids-apps-qc.png)](https://bids-apps.neuroimaging.io/apps/)
]

---
count: false

.pull-left[
### Many application types are possible

* Quality control

* Anatomical pipelines

]
.pull-right[
[![:img BIDS Anatomical Apps, 100%](assets/bids-apps-anatomical.png)](https://bids-apps.neuroimaging.io/apps/)
]

---
count: false

.pull-left[
### Many application types are possible

* Quality control

* Anatomical pipelines

* Functional pipelines

]
.pull-right[
[![:img BIDS Functional Apps, 100%](assets/bids-apps-functional.png)](https://bids-apps.neuroimaging.io/apps/)
]

---
count: false

.pull-left[
### Many application types are possible

* Quality control

* Anatomical pipelines

* Functional pipelines

* Diffusion pipelines

]
.pull-right[
[![:img BIDS Diffusion Apps, 100%](assets/bids-apps-diffusion.png)](https://bids-apps.neuroimaging.io/apps/)
]

---
count: false

.pull-left[
### Many application types are possible

* Quality control

* Anatomical pipelines

* Functional pipelines

* Diffusion pipelines

### Lowered friction encourages adoption

* Researchers gain easy access to tools by formatting data
  in BIDS

* Accepting BIDS datasets makes your tools easy to try

]
.pull-right[
[![:img BIDS App list, 100%](assets/bids-apps.png)](https://bids-apps.neuroimaging.io/apps/)
]

---

For development and distribution, BIDS Apps encourages a continuous-integration
(CI) approach.

<div align="center" style='margin-top: 1em'>
<img alt="BIDS App creation" src="assets/bids-app-creation.png" width="75%">
</div>

.footnote[
From doi:[10.1371/journal.pcbi.1005209.g001](https://doi.org/10.1371/journal.pcbi.1005209.g001)
]

---

BIDS Apps use the notion of analysis levels, which provide natural opportunities for
parallelism.

<div align="center" style='margin-top: 1em'>
<img alt="BIDS App workflow" src="assets/bids-app-workflow.png" width="85%">
</div>

.footnote[
From doi:[10.1371/journal.pcbi.1005209.g002](https://doi.org/10.1371/journal.pcbi.1005209.g002)
]

---

* Construction of reusable pipelines is important for reproducibility

  * Open analysis on open data is subject to replication
  * Identical analyses on similar data tests for robustness

--

* Container technologies partially address environmental sources of
  variability &mdash; see, *e.g.*,
  [Gronenschild, et al. (2012)](https://doi.org/10.1371/journal.pone.0038234)
  or [Glatard, et al. (2015)](https://doi.org/10.3389/fninf.2015.00012)

--

* The uniform interface also eases deployment to HPC or cloud environments
  like [CBRAIN](http://mcin.ca/technology/cbrain/) or
  [AWS Batch](https://aws.amazon.com/batch/).

--

* The output of a BIDS App is a *derivative* dataset. The BIDS standard is
[being extended](https://124-151034407-gh.circle-artifacts.com/0/home/circleci/project/site/05-derivatives/01-introduction.html)
to describe many types of derivatives, with a focus on derivatives that
can be reused in yet more BIDS apps.

---
layout: true
template: footer
name: Derivatives

# BIDS Derivatives

---


#### Dataset-level metadata is stored in augmented [`dataset_description.json`](https://124-151034407-gh.circle-artifacts.com/0/home/circleci/project/site/05-derivatives/01-introduction.html#derived-dataset-and-pipeline-description):

* `GeneratedBy` contains references to the code (including versions)
  that produced the derivative dataset.
* `SourceDatasets` is a list of references to the specific version of the
  dataset analyzed

```YAML
{
  "Name": "FMRIPREP Outputs",
  "BIDSVersion": "1.4.0",
  "DatasetType": "derivative",
  "GeneratedBy": [{"Name": "fmriprep", "Version": "1.4.1"}],
  "SourceDatasets": [{"DOI": "10.18112/openneuro.ds000114.v1.0.1"}]
}
```

---

#### Basic metadata remains in filenames

```
fmriprep/sub-01/func/sub-01_task-rest_space-fsaverage_hemi-L_bold.func.gii
```

--

#### Additional metadata in [sidecar JSON files](https://124-151034407-gh.circle-artifacts.com/0/home/circleci/project/site/05-derivatives/01-introduction.html#common-file-level-metadata-fields)

* `Sources` direct inputs to the process that produced the file
* `CoordinateSystem` indicates the coordinate system of image files
* Metadata of source files that still apply should be preserved
* Metadata of source files that no longer apply must be removed

---

## Preprocessed or cleaned data

Preprocessing does not change the type of the data. For example,

```
rawdata/
  sub-01/
    func/
      sub-01_task-rest_bold.nii.gz

preprocessed/
  sub-01/
    func/
      sub-01_task-rest_space-MNI_desc-preproc_bold.nii.gz
```

* `space-<label>` - Indicates that a file has been registered to some
  reference, such as standard templates or another image
* `desc-<label>` - Generic label to describing preprocessing

Derivatives must have *some* distinguishing entity from raw data.

---

## Binary masks

Binary masks have the suffix `mask`.

```
lesion_masks/
  sub-01/
    anat/
      sub-01_desc-lesion_mask.nii.gz
```

## Segmentations

Discrete segmentations have the suffix `dseg` and probabilistic or
partial-volume segmentations have the suffix `probseg`.

```
parcellations/
  sub-01/
    anat/
      sub-01_space-orig_dseg.nii.gz
      sub-01_space-orig_label-WM_probseg.nii.gz
```

---

Derivatives generally fall into three categories:

1. Preprocessed images (or otherwise transformed data) that can be used for further
  analysis

  * Tissue segmentations
  * Normalized BOLD series
  * Fitted diffusion model parameters

--

2. Measures of statistics of interest

  * Parcellation statistics
  * Local functional connectivity density
  * Fractional anisotropy

--

3. Figures and reports for assessing the quality of data/processing

  * Surface reconstruction plots
  * Brain-extraction mask overlays
  * Susceptibility distortion correction before/after

---
template: footer

# Conclusion

BIDS is a common way of arranging neuroscientific data

--

... which affords programmatic querying (e.g.,
  [PyBIDS](https://github.com/bids-standard/pybids))

--

... which affords a simple application protocol
  ([BIDS Apps](https://doi.org/10.1371/journal.pcbi.1005209.g001))

--

... which facilitates large-scale, reproducible computation

--

... and gives rise to
[derivatives](https://bids-specification.readthedocs.io/en/stable/05-derivatives/01-introduction.html),
which are also BIDS datasets, and can be published and analyzed.

---
layout: true
template: footer

class: center middle

---

# Questions?

##### [effigies.github.io/bids-derivatives](https://effigies.github.io/bids-derivatives)

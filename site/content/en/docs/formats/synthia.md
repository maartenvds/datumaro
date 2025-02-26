---
title: 'SYNTHIA'
linkTitle: 'SYNTHIA'
description: ''
weight: 1
---

## Format specification

The original SYNTHIA dataset is available
[here](https://synthia-dataset.net).

Datumaro supports all SYNTHIA formats except SYNTHIA-AL.

Supported annotation types:
- `Mask`

Supported annotation attributes:
- `dynamic_object` (boolean): whether the object moving

## Import SYNTHIA dataset

A Datumaro project with a SYNTHIA source can be created in the following way:

```bash
datum create
datum import --format synthia <path/to/dataset>
```

It is also possible to import the dataset using Python API:

```python
from datumaro.components.dataset import Dataset

synthia_dataset = Dataset.import_from('<path/to/dataset>', 'synthia')
```

SYNTHIA dataset directory should have the following structure:

<!--lint disable fenced-code-flag-->
```
dataset/
├── GT/
│   ├── COLOR/
│   │   ├── Stereo_Left/
│   │   │   ├── Omni_B
│   │   │   │   ├── 000000.png
│   │   │   │   ├── 000001.png
│   │   │   │   └── ...
│   │   │   └── ...
│   │   └── Stereo_Right
│   │       ├── Omni_B
│   │       │   ├── 000000.png
│   │       │   ├── 000001.png
│   │       │   └── ...
│   │       └── ...
│   └── LABELS
│       ├── Stereo_Left
│       │   ├── Omni_B
│       │   │   ├── 000000.png
│       │   │   ├── 000001.png
│       │   │   └── ...
│       │   └── ...
│       └── Stereo_Right
│           ├── Omni_B
│           │   ├── 000000.png
│           │   ├── 000001.png
│           │   └── ...
│           └── ...
└── RGB
    ├── Stereo_Left
    │   ├── Omni_B
    │   │   ├── 000000.png
    │   │   ├── 000001.png
    │   │   └── ...
    │   └── ...
    └── Stereo_Right
        ├── Omni_B
        │   ├── 000000.png
        │   ├── 000001.png
        │   └── ...
        └── ...
```

- `RGB` folder containing standard RGB images used for training.
- `GT/LABELS` folder containing containing PNG files (one per image).
  Annotations are given in three channels. The red channel contains
  the class of that pixel. The green channel contains the class only
  for those objects that are dynamic (cars, pedestrians, etc.),
  otherwise it contains `0`.
- `GT/COLOR` folder containing png files (one per image).
  Annotations are given using a color representation.

When importing a dataset, only `GT/LABELS` folder will be used.
If it is missing, `GT/COLOR` folder will be used.

The original dataset also contains depth information, but Datumaro
does not currently support it.


## Export to other formats

Datumaro can convert a SYNTHIA dataset into any other format [Datumaro supports](/docs/user-manual/supported_formats/).
To get the expected result, convert the dataset to a format
that supports segmentation masks.

There are several ways to convert a SYNTHIA dataset to other dataset
formats using CLI:

```bash
datum create
datum import -f synthia <path/to/dataset>
datum export -f voc -o <output/dir> -- --save-images
# or
datum convert -if synthia -i <path/to/dataset> \
    -f voc -o <output/dir> -- --save-images
```

Or, using Python API:

```python
from datumaro.components.dataset import Dataset

dataset = Dataset.import_from('<path/to/dataset>', 'synthia')
dataset.export('save_dir', 'voc')
```

## Examples

Examples of using this format from the code can be found in
[the format tests](https://github.com/openvinotoolkit/datumaro/blob/develop/tests/test_synthia_format.py)

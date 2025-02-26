---
title: 'Align CelebA'
linkTitle: 'Align CelebA'
description: ''
weight: 1
---

## Format specification

The original CelebA dataset is available
[here](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html).

Supported annotation types:
- `Label`
- `Points` (landmarks)

Supported attributes:
- `5_o_Clock_Shadow`, `Arched_Eyebrows`, `Attractive`,
  `Bags_Under_Eyes`, `Bald`, `Bangs`, `Big_Lips`, `Big_Nose`, `Black_Hair`,
  `Blond_Hair`, `Blurry`, `Brown_Hair`, `Bushy_Eyebrows`, `Chubby`,
  `Double_Chin`, `Eyeglasses`, `Goatee`, `Gray_Hair`, `Heavy_Makeup`,
  `High_Cheekbones`, `Male`, `Mouth_Slightly_Open`, `Mustache`, `Narrow_Eyes`,
  `No_Beard`, `Oval_Face`, `Pale_Skin`, `Pointy_Nose`, `Receding_Hairline`,
  `Rosy_Cheeks`, `Sideburns`, `Smiling`, `Straight_Hair`, `Wavy_Hair`,
  `Wearing_Earrings`, `Wearing_Hat`, `Wearing_Lipstick`, `Wearing_Necklace`,
  `Wearing_Necktie`, `Young` (boolean)

## Import align CelebA dataset

A Datumaro project with an align CelebA source can be created
in the following way:

```bash
datum create
datum import --format align_celeba <path/to/dataset>
```

It is also possible to import the dataset using Python API:

```python
from datumaro.components.dataset import Dataset

align_celeba_dataset = Dataset.import_from('<path/to/dataset>', 'align_celeba')
```

Align CelebA dataset directory should have the following structure:

<!--lint disable fenced-code-flag-->
```
dataset/
├── Anno/
│   ├── identity_CelebA.txt
│   ├── list_attr_celeba.txt
│   └── list_landmarks_align_celeba.txt
├── Eval/
│   └── list_eval_partition.txt
└── Img/
    └── img_align_celeba/
        ├── 000001.jpg
        ├── 000002.jpg
        └── ...
```

The `identity_CelebA.txt` file contains labels (required).
The `list_attr_celeba.txt`, `list_landmarks_align_celeba.txt`,
`list_eval_partition.txt` files contain attributes, bounding boxes,
landmarks and subsets respectively (optional).

The original CelebA dataset stores images in a .7z archive. The archive
needs to be unpacked before importing.

## Export to other formats

Datumaro can convert an align CelebA dataset into any other format [Datumaro supports](/docs/user-manual/supported_formats/).
To get the expected result, convert the dataset to a format
that supports labels or landmarks.

There are several ways to convert an align CelebA dataset to other dataset
formats using CLI:

```bash
datum create
datum import -f align_celeba <path/to/dataset>
datum export -f imagenet_txt -o ./save_dir -- --save-images
# or
datum convert -if align_celeba -i <path/to/dataset> \
    -f imagenet_txt -o <output/dir> -- --save-images
```

Or, using Python API:

```python
from datumaro.components.dataset import Dataset

dataset = Dataset.import_from('<path/to/dataset>', 'align_celeba')
dataset.export('save_dir', 'voc')
```

## Examples

Examples of using this format from the code can be found in
[the format tests](https://github.com/openvinotoolkit/datumaro/blob/develop/tests/test_align_celeba_format.py)

# d2b-sidecarless-nii

[![PyPI Version](https://img.shields.io/pypi/v/d2b-sidecarless-nii.svg)](https://pypi.org/project/d2b-sidecarless-nii/) [![Code Style](https://github.com/d2b-dev/d2b-sidecarless-nii/actions/workflows/lint.yaml/badge.svg)](https://github.com/d2b-dev/d2b-sidecarless-nii/actions/workflows/lint.yaml) [![Type Check](https://github.com/d2b-dev/d2b-sidecarless-nii/actions/workflows/type-check.yaml/badge.svg)](https://github.com/d2b-dev/d2b-sidecarless-nii/actions/workflows/type-check.yaml) [![Tests](https://github.com/d2b-dev/d2b-sidecarless-nii/actions/workflows/test.yaml/badge.svg)](https://github.com/d2b-dev/d2b-sidecarless-nii/actions/workflows/test.yaml)

Plugin for the d2b package to handle NIfTI images without sidecars

## Intallation

```bash
pip install d2b-sidecarless-nii
```

**Just pip-install and go.** No further intervention is needed by the user (other than potentially adjusting your config file, of course ...)

## Getting Started

The d2b tool, out-of-the-box, aims to BIDS-ify data stored as NIfTI images. The main requirement is that each NIfTI file of interest has an associated JSON sidecar file which contains metadata about the linked NIfTI file.

While JSON sidecars are becoming more and more common across MRI datasets, there are still instances where one has the NIfTI files and only the NIfTI files.

This package extends the functionality of `d2b run ...` by finding all of the `*.nii.gz` and `*.nii` files in the sources directory which do not have an associated sidecar and creates a synthetic one for each such NIfTI file.

Since each image that's missing a sidecar has effectively no metadata (in the eyes of `d2b`), the only useful `criteria` that one can use is the `filename` pattern (or equivalently `SidecarFilename`), for example consider this `d2b` config file:

```yaml
descriptions:
  - id: rest
    dataType: func
    modalityLabel: bold
    criteria:
      # this is the only criterion that we can rely on
      # to identify acquisitions of interest
      filename: "*_3T_rfMRI_REST1_LR.*"
    sidecarChanges:
      # inject this metadata into the (newly-created)
      # sidecar file. All of the fields in this section
      # (sidecarChanges) will appear in the sidecar(s)
      # of files matching this description
      MagneticFieldStrength: 3
      Task: rest

  # more descriptions ...
```

So, if you have fairly unique filenames, and if given a matching filename you know what the sidecar metadata _ought_ to have been/should be, then this package might be for you.

While this package/implemetation does have its limitations, in many cases, being able to match/find files (via `criteria`) based on their `filename` and then inject the metadata required to make the acquisition(s) + sidecar(s) BIDS-compliant by leveraging the `sidecarChanges` functionality is enough in many cases.

## Contributing

1. Have or install a recent version of `poetry` (version >= 1.1)
1. Fork the repo
1. Setup a virtual environment (however you prefer)
1. Run `poetry install`
1. Run `pre-commit install`
1. Add your changes (adding/updating tests is always nice too)
1. Commit your changes + push to your fork
1. Open a PR

# SciPy 2025 - Tutorial: Scaling-up deep learning inference to large-scale bioimage data
Tutorial about scaling-up image analysis with Dask presented during SciPy 2025 Conference (July 7-13, 2025)

# Installation instructions

This workshop uses Dask, Zarr, TiffFile, and Cellpose libraries.

## Why do we need them?

- Dask is a parallel computing library in Python used for larger-than-RAM computation.
- The Zarr library allows us to interact with Next Generation File Formats, such as the `.zarr` format, which is highly compatible with large-scale images stored either on local disk, or remote cloud storage.
- TiffFile is a library for opening `.tif` image files, and even other proprietary file formats.
- Cellpose is a deep learning segmentation method for biological structures segmentation.

## Create a virtual environment
The most convenient way to set up the requires packages for this tutorial is by creating a `conda` environment.

```
conda create -n scale-up python=3.11 -c conda-forge
```

Install the following packages in the newly created environment.

```
conda activate scale-up

conda install -c conda-forge "cellpose>=3.0,<4.0" "tifffile>=2018.10.18,<=2025.5.21" "zarr>=2.0,<3.0" dask=2025.5.1 distributed=2025.5.1 dask-image=2024.5.3 imagecodecs=2025.3.30
```

We'll open some image files that are compressed using `JPEG2000`, so we need `imagecodecs` to have access to this compression algorithm.

Additionally, install Jupyterlab to follow the notebooks, and its Dask extension.

```
conda install -c conda-forge jupyterlab=4.4.3 dask-labextension=7.0.0
```

## Test images

We'll work with big microscopy images: whole slide images. These are images obtained by scanning slides on which thin slices of tissue have been mounted. A number of examples of these kind of image can be found in the [OpenSlide](https://openslide.org) test data, for example in the [Aperio SVS format](https://openslide.cs.cmu.edu/download/openslide-testdata/Aperio/). These are TIFF variants that can be read with the [`Tifffile` Python library](https://github.com/cgohlke/tifffile).

For this workshop, we recommend downloading a smaller crop of the CMU-1 image from [our google drive](https://drive.google.com/file/d/17owNcq_Or6aBAyUVE33fyHSS0VvKuHSw/view?usp=sharing), along with [its `Zarr` version](https://drive.google.com/file/d/1BmNxOrO3vOFPR-PCnV00DYgFsD1sDu47/view?usp=sharing).

A alternative even smaller example image can be found [in the OpenSlide test data](https://openslide.cs.cmu.edu/download/openslide-testdata/Aperio/CMU-1-Small-Region.svs), and its corresponding `Zarr` version can also be downloaded from [our google drive](https://drive.google.com/file/d/1MifgafB5mhVAvqjzEAAR_zQajAU5Dcya/view?usp=drive_link).

**Please pre-download either pair of images before the tutorial!**

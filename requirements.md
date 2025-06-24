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

conda install -c conda-forge "cellpose>=3.0,<4.0" "tifffile[all]<=2025.5.21" "zarr>=2.0,<3.0" dask=2025.5.1 distributed=2025.5.1  jupyterlab=4.4.3 bioformats2raw=0.9.4
```

We'll convert an image from Tiff format to the Zarr format, so make sure to install `bioformats2raw` as follows inside the same `scale-up` environment:

```
conda install -c ome bioformats2raw=0.9.4
```

## A test image

We'll work with big microscopy images. An example of that kind of image can be found [here](https://openslide.cs.cmu.edu/download/openslide-testdata/Aperio/CMU-1.svs).
So it is a good idea to have it downloaded as well.

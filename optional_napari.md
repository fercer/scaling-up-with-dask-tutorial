# Optional: view data using napari

Instead of using matplotlib to visualize images, there are a number of alternatives for viewing zarr data.
For example, you can use [napari](https://napari.org/) a Python viewer for n-dimensional images.

You can install napari along with a Qt backend using your preferred Python package manager, for example using conda:
```bash
conda create -n napari-env -c conda-forge python=3.11 napari pyqt
conda activate napari-env
```

You can then use the following code to load the CMU-1 zarr and labels zarr into napari:
```python
import napari
import zarr
import dask.array as da

# replace these paths with the path to your data
img_path = "path/to/CMU-1_Crop.ome.zarr"
labels_path = "path/to/CMU-1_Crop_labels_cellpose_cyto3.zarr"
```

We will load the zarr data as a list of dask arrays, so napari understands that we have multiscale/pyramidal data.
The list of data arrays must be in order from largest to smallest, following the downsampling of the pyramid.
For convenience, we will squeeze out the singleton arrays (by default OME-Zarr store images as 5D arrays) and move the channel axis to the end of the array shape, such that we get RGB images.

```python
img_zarr_group = zarr.open(img_path, mode="r")
# use the first (and only) image multiscale/pyramid group
dask_stack = [da.from_zarr(img_zarr_group[0][level]).squeeze() for level in img_zarr_group[0]]
rgb_dask_stack = [da.moveaxis(arr, 0, -1) for arr in dask_stack]

labels_zarr_group = zarr.open(labels_path, mode="r")
labels_dask_stack = [da.from_zarr(labels_zarr_group[level]) for level in labels_zarr_group]
```

With the data prepared, we can now launch napari and add the image and labels to the viewer:
```python
# open a napari viewer
viewer = napari.Viewer()
# add the image
viewer.add_image(rgb_dask_stack, name="CMU-1")
viewer.add_labels(labels_dask_stack, name="Cellpose labels")
```

After zooming in a bit, you should see something like this:
![Screenshot of napari viewer with CMU-1 image and Cellpose labels loaded.](napari-screenshot.png)

Optionally, you can also install the [napari-ome-zarr](https://github.com/ome/napari-ome-zarr) plugin to facilitate accessing the miroscopy (OME) metadata from the zarr files:
```bash
conda create -n napari-env -c conda-forge python=3.11 napari pyqt napari-ome-zarr
conda activate napari-env
```

In this case, you can simply launch napari from the command line and open the zarr image using:
```bash
napari path/to/CMU-1_Crop.ome.zarr/0
```
Note the `/0` at the end of the path, indicating that napari should open the first (and only in this case) image multiscale/pyramid group
With the viewer open, you can then drag-and-drop the labels zarr folder into the napari viewer to add the labels layer. When prompted, select `napari-ome-zarr` plugin. However, you will need to right-click on the layer with the labels data and select "Convert to labels" to get the correct rendering of the labels.

Or you can do it programmatically as follows:
```python
import napari

# path to the first (only) image multiscale/pyramid group
img_path = "path/to/CMU-1_Crop.ome.zarr/0"
labels_path = "path/to/CMU-1_Crop_labels_cellpose_cyto3.zarr"

# create a napari viewer
viewer = napari.Viewer()

viewer.open(img_path, plugin="napari-ome-zarr")
viewer.open(labels_path, plugin="napari-ome-zarr", layer_type="labels")
```
Note: If you want to run these snippets as scripts, append `napari.run()` at the end of the script to start the napari event loop.

Final tip: if you want to use napari with full size whole-slide images or remote zarr data, we recommend using the "Render Images Asynchronously" option in napari settings (under "Experimental"). You can also set this using the environment variable `NAPARI_ASYNC=1`.

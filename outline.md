# Workshop outline

### Setup (10 min)
 - Getting ready the environment to code in jupyter

### Give overview of the workshop (20 min)
- Challenges of scaling up deep learning inference
- Parallelization approach taken in this workshop 

### Break (10 min)

### Introduce image analysis with Dask (50 min)
- Overview of image analysis and Dask for lazy loading
- Image loading (using `tifffile` and `zarr` as backends) [Guided exercise]
- Image analysis examples [Guided exercise]
- Image analysis exercises [Unguided exercise]

### Break (10 min)

### Implement an inference function (50 min)
- Overview of the `cellpose` model for cell segmentation
- Implement the inference function for `cellpose` [Guided exercise]
- Test implementation [Unguided exercise]

### Break (10 min)

### Apply the inference to a large-scale image (50 min)
- Overview of the parallelization approach using Dask
- Test inference function in small-scale [Unguided exercise]
- Scale-up to the full image [Guided exercise]

### Break (10 min)

### Conclusion (20 min)
- Wrap-up and recap of the learning outcomes of the workshop

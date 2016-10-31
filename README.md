# Bubble Size Analysis (BSA)

This repository collects a selection of processing steps to be used for bubble detection from raw images together with essential post-processing of the images (bubble characteristics) in a Python Package. The aim of this package is not to develop new image analysis algorithms, but to organize what we experienced to be functional sequences of image processing steps (i.e. pipelines) and design a package that enables to easily:

1. Test and try interactively the effect of the individual image processing steps and evaluate the effect of the input parameters
2. Both reuse existing processing pipelines and provide the ability to add new pipelines
3. Apply developed pipelines to a batch of images

Furthermore, the derivation of the bubble properties and associated graphs is integrated within the code.

### Image analysis packages used 
For the algorithmic part, i.e. the image analysis steps, the following excellent packages are used:
- opencv
- skimage

### Requirements
At the moment only fully compatible for Python 3.5. The package is not yet on pypy, for installation, download the code and from within the folder:

```
python setup.py install
```

## The Idea
The user is provided with a number of image processing steps that already have been djusted to be applicable for the purpose of bubble detection. Hence, to shorten the time expended in developing your own bubble detection algorithm and parameter selection. As a matter of fact, the time needed for this process is not negligible and this wants to be a starting platform to efficiently start to detect bubbles. Hence, it provides the algorithmic power of `opencv` and `skimage` in an application oriented workflow, going from interactively testing the processing steps to an automatic processing. 

### Run a pipeline
To get an idea of how the pipeline definition of the package works, check and test one of the two default pipelines:

- Canny pipeline:
	apply a Canny filter for edge detection on the whole image in combination with furhter cleaning towards and interpretable binary image.

- Adaptive threshold pipeline: 
	apply the adaptive threshold method of opencv with default or chosen parameters for the gaussian edge detection.

See [example 1 in example_bubble.py](https://github.com/gbellandi/bubble_size_analysis/blob/master/examples/example_bubble.py#L11)

### Perform an individual sequence
You can set your object and apply the different imaghe processing steps available in `BubbleKicker` in order to find your own perfefct sequence of processing steps. 

Each function directly updates your `current_image`. In order to check the performed steps and the applied parameters since the raw image, the function `what_have_i_done` provides a history on the functions. When not satisfied of the sequence, the `reset_to_raw` function resets your image back to the raw original image and alternatives sequences can be tested. 

See [example 2 in example_bubble.py](https://github.com/gbellandi/bubble_size_analysis/blob/master/examples/example_bubble.py#L31)

### Running a pipeline on a bunch of files/directories
Normally the analysis of bubbles is taking place on tons of images, thus with the `batchbubblekicker` one can run any of the pipelines with custom parameters on an entire folder of images. 

See [expample 3 in example_bubble.py](https://github.com/gbellandi/bubble_size_analysis/blob/master/examples/example_bubble.py#L73)

### Define Bubbles properties
Once the detection of bubbles has come to a satisfying end, you can proceed on defining the interesting bubbles properties. The post-processing consists of a filtering step and a calculation/visualisation step, initiated by the `bubble_properties_calculate(binary_im, rules)` function. 

```
id_image, property_table = bubble_properties_calculate(result, rules=custom_filter)
```

#### Filter objects
NOTE: this is a crucial step, the filtering of *bubbles* which are probably not really bubbles

This step is considered as a post-processing of the imaging steps taken so far, since bubbles are labeled and characterized for specific properites that are known to be efficient filtering parameters. In specific, the *circularity reciprocal* and *convexity* are used to recognize those objects that are not classifable as bubbels. By default, it was observed that the following conditions/rules work well:
* circularity reciprocal: `{'min': 0.92}`
* convexity: `{'max': 1.6, 'min': 0.2}`

Custom application filters can be defined, with `min` and `max` rules.  The most simple, i.e. no filter, can be defined by setting `rules={}`. Custom filters can be used by passing a dictionary, based on any of the calculated properties, e.g.

```
custom_filter = {'circularity_reciprocal': {'min': 0.2, 'max': 1.6},
                 'convexity': {'min': 1.92}}
```

#### Visualisation
The package supports the visualisation of the distribution on any of the calculated bubble properties. 

For example, checking the distribution of the equivalent diameter:

```
bubble_properties_plot(property_table, "equivalent_diameter")
```

![diameter](examples/output_eq_diameter.png)





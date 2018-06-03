# Scikit-learn, scikit-image, and Dask

Here's a small hackmd.io pad to keep notes

## Day 1

### scikit-image

* Merged https://github.com/scikit-image/scikit-image/pull/3000
  Python 3-only codebase :tada:
* Close to releasing 0.14
* Progress on nD Gabor kernels (by @kne42):
  https://github.com/scikit-image/scikit-image/pull/2739
* Merged https://github.com/scikit-image/scikit-image/pull/3110
  to allow float32's through img_as_float without copy
* Numba investigation:
    * Cannot access LowLevelCallable: https://github.com/numba/numba/issues/2995
    * Numba function can take another numba function as an argument, though
* Wheels: build for 0.14.0
    * Merged Matthew's refactoring of the [AppVeyor Windows wheel builder](https://github.com/scikit-image/scikit-image-wheels/pull/6)
* Cython
    * Tried working with Cython pranges, not much success yet
    * Figure out function pointers for swappable kernels

Plans for today

-  Get some data on GCS


### scikit-learn

* Small nested-parallelism benchmark: https://gist.github.com/01aa814d44a7d77a3ea786ebfc156d9b  (Olivier, Tom)
    * Pre-scattering the data is important
    * ~5x speedup for 6x as many cores
*  SAGA 32 bit (Nelle)
*  PubSub implementation and toy example for parameter server
*  Joblib PR to allow dask to be registered globally

* Discussed with Gaël about MODL
* Built a copy of MODL for Linux and macOS with conda

Plans for today

-  Test SGD Classifier incrementally with new benchmark.  Have an artificial dataset


### Infrastructure

-  JupyterHub + Dask + master branches of scikit-learn/image deployed at https://35.232.36.204/

## Day 2

### scikit-image

* @kne42 basically got nD transforms working with Numba which is super awesome:
  https://gist.github.com/kne42/6271a42967d8232b32b0b3e918761921
* @jakirkham extended skimage.util.apply_parallel to enable computation on dask
  arrays instead of recomputing each time
    * https://github.com/scikit-image/scikit-image/pull/3121
* @jcrist managed to slow down Richardson-Lucy deconvolution 15-30x using dask
  which I guess is good? =P
* @emmanuelle accelerated SLIC segmentation but not without creating a bug due to
  concurrent writing into an array.
* Released 0.14.0 on conda-forge
    * https://github.com/conda-forge/scikit-image-feedstock/pull/18

### scikit-learn
*  Work on adaptive hyper-parameter search, with the algorithm "Hyperband"
    *   https://github.com/stsievert/dask-searchcv/tree/hyperband
    *   https://arxiv.org/pdf/1603.06560.pdf
*  Benchmark SAGA 32b versus 64b
    *  Tests pass, but performance is poor. Not yet sure why
*  Identified and fixed reference cycle in joblib/dask
*  Investigated modifying scikit-learn GridSearchCV to get dask-searchcv style efficiency.  This turned out to be too invasive
*  Looked into using joblib style memory-caching

### Dask

* Fix to dask-drmaa for `pip install --user` usage
  * https://github.com/dask/dask-drmaa/pull/91


## Day 3

### scikit-image

Discussion: Juan, Emmanuelle, Stéfan
Topics:
- [x] Decide on new core members to invite
- [x] Vision/mission/value statement
- Governance / leadership structure
    - For now, see if vision/mission helps to ameliorate these issues
    - Add list of core team to website
        - (Maybe with expertise areas)
    - Focus on listening + respect + kindness addresses a lot of issues before they happen; governance documents are there for when things go wrong. Do we feel we need this safety net / to crystallize decision making?
- Discuss user survey & usage exhibition on website
    - GitHub crawl
    - Popup on website
    - Have you ever raised an issue?
    - Company? Willing to fund development?
- Exhibition section on website: popular science writing style

-- BREAK --

- [Needs decision](https://github.com/scikit-image/scikit-image/labels/status%3A%20needs%20decision)

    - https://github.com/scikit-image/scikit-image/issues/3124
        - Allow recursive montages by accepting images of varying sizes in the montage function, doing padding as necessary
    - https://github.com/scikit-image/scikit-image/issues/3082
        - Removed label
        - [HackerNews thread](https://news.ycombinator.com/item?id=17165889)
    - https://github.com/scikit-image/scikit-image/pull/3055
        - Removed label
        - Refactor dtype range checking; will not include warning handling
    -  https://github.com/scikit-image/scikit-image/issues/3019
        -  Removed label
        -  Solution: sort peaks by amplitude, and suppress nearby neighbors.
    -  https://github.com/scikit-image/scikit-image/issues/3001
        -  Removed label
        -  This is a bug: fix Cython code not to segfault, error on such input.
    -  https://github.com/scikit-image/scikit-image/issues/2997
        - Should support non-integer radii
        - Line Hough TF should also be updated to have non-integer distances
        - First, the circle drawing in `skimage.draw` will need to be updated
    - https://github.com/scikit-image/scikit-image/issues/2995
        - Should be `log10`?  Stéfan emailed Cedric.
    - https://github.com/scikit-image/scikit-image/issues/2993
        - Removed label
        - Will probably not fix this soon, but leaving issue open
        - After we clarified our vision/mission, add an issue for **removing `viewer` from skimage**
    - https://github.com/scikit-image/scikit-image/issues/2989
        - Removed label
        - Asked user for example image
    - https://github.com/scikit-image/scikit-image/issues/2982
        - Removed label
        - Juan will test on his Raspberry Pi
    - https://github.com/scikit-image/scikit-image/issues/2956
        - Removed label
        - Punt
    - https://github.com/scikit-image/scikit-image/issues/2952
        - Removed label
        - Will investigate why Otsu is 3–4 times slower than other rank filters (probably just complexity)

- Discuss funding opportunities
- Outreach
- Consider advertised nightly builds for Windows users?
- Figure out how to include long form errors in code with appropriate formatting
- Add linking to objects & functions in gallery (maybe this can just go into an issue); should we provide mybinder links for documentation?
- Cross-language usage examples in documentation (skimage + ImageJ in microscopy, e.g.)?
- Paper v2: interest, necessity, process
- Release schedules
- Plan for a roadmap?

Tasks:

- Add section on website about mission/vision/values of the project
    - *Vision:* What is the future you are trying to achieve?
        - Focus: science & teaching
        - Targets multiple disciplines, modalities
    - *Mission:* What are the steps you are going to take?
        - Reusable library, ability to be deployed in various contexts (pipelines, GUIs, etc.)
        - Great documentation of all functions, plus gallery examples
        - Consistent API
    - *Values:* What are the things you will not do to get there.
        - We choose correctness above growth; maintainability counts
        - No part of skimage belongs to any individual
        - Low entry-barrier to development
        - Readability over performance
        - Respect people, welcome contributors
        - Not magic; we choose education over making decisions on the user's behalf. We use NumPy arrays instead of custom objects.
    - Examples: Stéfan will ask Karthik
        - https://datapractices.org/manifesto/
        - [ROpenSci](https://ropensci.org/about/)

- Set up core team mailing list

- Add one-screen contributing guide (with the rest under "more details")

- Add admin guide
    - Push changes more liberally
        - Don't request many small "petty" changes, except perhaps of another admin
        - For inactive PRs, don't wait for response from original author
    - Pull requests should not include new copyright information
    - Monthly review of abandoned issues and PRs
        - Last Friday of every month
    - Wait for consensus on closing PRs (1/2 other core devs)
        - When closing, express gratitude for effort made by contributor
    - Value elegance and readability [needs fleshing out]
    - Add examples of code we consider particularly clean, to give new admins a flavour of where new code should go. Similarly for bad code: what kind of stuff *don't* we want merged?
    - Technical decisions are made by consensus on mailing list and github issues. Discussions should happen in the open as much as possible. In the event of irrecoverable differences, (someone specific at that moment, determined over time by community consensus) will have the final say.

- Last Friday of each month video call
    - https://appear.in/skimage
    - Northern Hemisphere Summer:
        - Emmanuelle: 6:30
        - Juan: 14:30
        - Stéfan: 21:30
    - Northern Hemisphere Winter:
        - Emmanuelle: 22:00
        - Juan: 8:00
        - Stéfan: 13:00

- Improve errors from imread to point to possible alternative libraries

- Invite to core team
    - Jeremy Metz
    - Thomas Walter
    - Watch: Lars G.

- Add list of core team to website
- Discuss decision making with sklearn team
- Reach out to companies / groups for gallery entries

#### Goals for today

* Make Cython function that can receive/use a LowLevelCallable
* Get ASV working
    * Turns out that ASV is broken with conda > 4.3 because conda is no longer a binary
    * need to patch ASV to point it to the conda binary that is currently shadowed by the conda bash function
* benchmark ndimage transforms
    * @kne42 showed that map_coordinates can be ~4x faster (still needs investigating), but managed to match Cython, Numba, and cfunc performance

#### Summary

* Updated/updating conda-forge recipes for conda-build 3
  * https://github.com/conda-forge/scikit-image-feedstock/pull/21
  * https://github.com/conda-forge/scikit-image-feedstock/pull/21
* Made some doc fixes for `apply_parallel`
  * https://github.com/scikit-image/scikit-image/pull/3121

#### Done

-  Removed matplotlib dependency


### scikit-learn

* Update conda-forge recipe for conda-build 3
  * https://github.com/conda-forge/scikit-learn-feedstock/pull/66
* Sequential Distributed SGD: https://gist.github.com/TomAugspurger/a738871d92e5ef4d4a5b7c55ce52f47d
    * Some unexpected data transfer?
    * Lazy Distributed SGD: https://github.com/dask/dask-ml/pull/187
    * Added compute=False to fit method, need to use this in SearchCV systems
* Pull request to Scikit-Learn to unvendor Joblib with an environment variable
* Nested Joblib
    * Seeing a lot of memory issues because joblib is submitting too much work
    * We need to have a negotiation between joblib and backends
* SAGA 32 bit
    * Fighting with yaep, an extension profilers, trying to identify performance issues from yesterday
* Automatically scattering numpy arrays
    * Things work, but there is a reference cycle
    * This creates some version mismatch issues between joblib and dask/distributed
* Hyperband debugging finished. Next steps:
    * Testing
    * Looking at sklearn compatibility
    * Adding support for max_iters in partial_fit for various sklearn models (https://github.com/scikit-learn/scikit-learn/pull/11174)

### Other

* Hashing out possible options for malloc in threads with numpy arrays
* Identified issue with asv and the new version of conda, where `conda` is no longer a binary
* Some work done on dask-yarn API

## Day 4

### scikit-image

#### Goals for today

* New GPU-accelerated viewer (with Loic Royer)
* Finish ASV improvements
* extract `void* user_data` from LLCs in Cython

#### Summary of day

* Ran into difficulties passing numpy pointers as scipy LowLevelCallables and having that object cleaned up
* Numba also worked, but was somewhat slower
* 
* Benchmarked all scikit-image functions to compare when apply_parallel/map_overlap is effective
    * Wrote a check_parallel function to find the minimum depth that is effective for map_overlap
    * Looked at performance increase/decrease when using apply_parallel
* Cython prange giving decent speedups
* Looked at large images.  Map_overlap has some scheduling issues.  Also it would be nice to have a tiff reader that didn't require gdal.

### scikit-learn

* Grid-search over Partial* fitters
    * incremental learners can manage multiple passes over the data until convergence?
    * limit concurrent futures API
    * Do we care about OOC here?
        * Would like to load a block, do all the parameters, load next block, ...
    * How does this fit in a pipeline?
    * Going to prototype a pipeline with mix of stateless, incremental, and regular classifiers, see what breaks
    * Early stopping in SGD
    * ColumnTransformer w/ dask dataframe.
    * Build prototype on text dataset and heterogenous dataframe dataset
* Wrapped up SAGA 64b to 32b pull requests.
    * Sometimes huge difference between 32b and 64b in the benchmark, sometimes no differences at all.
    * Still working on the details on why sometimes we see speed up, sometimes we don't. Might be due to the fact I'm using my computer during benchmarks. 
* Joblib oversubscription issue.  Decided that we can just switch back to sequential mode if we detect that the backend is saturated
* Joblib dask backend automatically detects data that should be scattered.  Sample computation changes from 30s to 1.5s because now we get to avoid communication
* Comparing hyperparameter optimization.  Comparing hyperopt, skopt, random.  
* Incremental SAGA.  Something basic is working.  Fabian wrote an implementation that worked on a list of numpy arrays, then we used dask delayed.  Still needs tuning, but does seem to work.

### Other

*  ASV: fixed a bug where asv doesn't work with new conda
*  Dictionary learning on Neurofinder data with online dictionary learning with incremental learning
    *  currently only running on small data, but the infrastructure should scale out
    *  model moves to data on workers currently, which we may want to adjust

* SAGA solver and dask:
    * using dask.delayed's decorator on a 10 line implementation of SAGA automatically renders the evaluation lazy.

## Day 5

### scikit-image

* Morning meeting: go through all *needs-decision* issues and PRs. (Inaugural developer meeting? ;)
* Registration; Gaël thinks that these things will get you very far:
    * 3 losses: cross correlation, mutual information, blockwise cross correlation, [mutual information gradient approximation](http://elastix.isi.uu.nl/marius/downloads/2005_c_SPIEMIb.pdf) (S: I found this after a cursory search, but not sure it's the best paper)
    * 2 transforms: affine, affine + [cosine basis](http://eecs.vanderbilt.edu/courses/CS359/other_links/papers/1999_Ashburner_Human_Brain_Mapping_spatial_normalization.pdf)
    * [pypreprocess]( https://github.com/neurospin/pypreprocess )

* PR fixing asv with conda 4.4+ was merged https://github.com/airspeed-velocity/asv/pull/646 :tada:

* Demoed use of BigDataViewer and discussed future work relevant to this

#### Goals for today

* figure out how to run LLCs in Python
* update parallel processing tutorial with more expansive dask & joblib examples
* figure out [N-dimensional rotation formalisms](http://www.paginasprodigy.com/is104378/papers/N29.pdf)
* continue core meeting
* plan registration work

### scikit-learn
* Hyperband implementation finished. Scott will put in a PR on Monday.
* Worked on adding positivity constraints to dictionary learning.
    * ref: https://github.com/scikit-learn/scikit-learn/pull/6374
* Closed out PR adding elastic net regularization to dictionary learning.
    * ref: https://github.com/scikit-learn/scikit-learn/pull/6382
* Revisited PR to bump atom reseeding threshold.
    * ref: https://github.com/scikit-learn/scikit-learn/pull/6385

#### Goals for today
* get the SAGA PR (https://github.com/scikit-learn/scikit-learn/pull/11155) merged
* Do some matrix balancing benchmark with dask.
* Try using MODL with dask-ml


### Other
* Yuvi came down to discuss how people use computing power for their research, the pain points, and how he and the jupyter team can help solve those.
    * Notes were taken here: https://hackmd.io/IpdnVs9rSXK_IH1moja4Ew#
* John demonstrated the image viewer

# What have we learned

* Over subscription in joblib when doing parallelization of several loops inside one another.
    * Examples to can be ran on: https://github.com/joblib/joblib/pull/690
* The adaptive Hyperband implementation is not difficult: use a GridSearchCV instance for every set of adaptive jobs.

# Blogpost ideas

* **joblib/sklearn/**: Over subscription blogpost [Gael]
    * https://github.com/joblib/joblib/pull/690
* **sklearn**: 64bit versus 32bit SAGA [Nelle]
* **skimage**: ndtransforms [Kira, Juan]
* **skimage**: summary of the governance improvements [Stéfan, Juan]
* **skimage**: python 3 [Juan]
* **skimage**: release [Juan]
* **skimage**: strategies for optimization and speeds up [Emma, Kira, Juan]
    * numba
    * prange
    * bug
* **sklearn/dask**: benchmarks & auto scattering [Olivier, Tom]
* **sklearn/dask**: summary of the discussion on the white boards that Olivier took picture of… [Olivier, Tom]
    * "Scalable Pipelines"
        * Taxonomy of sklearn estimators (stateless, increment, full pass)
        * How data can / should flow through pipelines
    * "Model Packs"
        * Efficiently fitting multiple models simultaneously?
* **dask/sklearn**: Incremental learning stuff + SAGA's experiment of Fabian [Tom, Fabian]
* **sklearn/dask** adaptive hyper parameter optimization - benchmarks and lessons learned [Michael, Fabian]
* **sklearn/skimage/dask**: Incremental MODL on images [John]
* **global**: global summary of the sprint [Nelle, Matt]
* **global**: how yuvi is going to solve all of our computing problems [Yuvi, Nelle]



# List of people present at the reception

- Nelle Varoquaux
- Stéfan van der Walt
- Lauren van der Walt
- Fabian Pedregosa
- Michael Eickenberg
- Valentina Borghesani
- Jeremy Freeman
- James McBride
- Jarrod Millman
- Mark Harfouche
- Aamod Shanker
- Emmanuelle Gouillart
- Gael Varoquaux
- Olivier Grisel
- Juan Nunez-Iglesias
- Matthew Rocklin
- Kira Evans
- Tom Augspurger
- Jim Crist
- John Kirkham
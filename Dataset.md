# Dataset

The following instructions were adapted from [Kaihua's repository](https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch/blob/master/DATASET.md). While the original instructions are comprehensive, I encountered a few challenges during setup that require additional clarification to avoid potential errors.

To download and preprocess the data:

- Download the VG images [part1 (9 Gb)](https://cs.stanford.edu/people/rak248/VG_100K_2/images.zip) and [part2 (5 Gb)](https://cs.stanford.edu/people/rak248/VG_100K_2/images2.zip). Extract these images to the file datasets/vg/VG_100K. Note that the VG Images come in two separate folders which need to be merged, and have caution with duplicate files.  Also, the correct file paths need to be updated inside of /home/marc/Desktop/Scene-Graph-Benchmark.pytorch-master/maskrcnn_benchmark/config/paths_catalog.py

- Download the [scene graphs](https://1drv.ms/u/s!AmRLLNf6bzcir8xf9oC3eNWlVMTRDw?e=63t7Ed) and extract them to datasets/vg/VG-SGG-with-attri.h5.


Finally, note that the model expects exactly 108073 photos from the Visual Genome dataset meaning that you may encounter the error the following errror: AssertionError on "assert len(fns) == 108073. If you are using a custom dataset, you can bypass this issue by commenting out the assertion in: /home/marc/Desktop/Scene-Graph-Benchmark.pytorch-master/maskrcnn_benchmark/data/datasets/visual_genome.py. You can consult the [FAQ section in Kaihua's repository](https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch/blob/master/README.md) (at the bottom of the README) for more information.

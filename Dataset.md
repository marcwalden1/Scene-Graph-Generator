# Dataset

The following instructions were taken from [Kaihua's repository](https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch/blob/master/DATASET.md). However, there are important details mentioned that need to be fixed in order to avoid running into issues down the line that personally took a while to figure out. 

1. Download the VG images [part1 (9 Gb)](https://cs.stanford.edu/people/rak248/VG_100K_2/images.zip) and [part2 (5 Gb)](https://cs.stanford.edu/people/rak248/VG_100K_2/images2.zip). Extract these images to the file datasets/vg/VG_100K.
2. Download the [scene graphs](https://1drv.ms/u/s!AmRLLNf6bzcir8xf9oC3eNWlVMTRDw?e=63t7Ed) and extract them to datasets/vg/VG-SGG-with-attri.h5.
3.
4. Note that the VG Images come in two separate folders which need to be merged. You should keep in mind duplicates.
5. Secondly, the model expects exactly 108073 photos from the Visual Genome dataset meaning that you may encounter the error the following errror: AssertionError on "assert len(fns) == 108073. If you are working on your custom datasets, you can just comment out the assertions inside of /home/marc/Desktop/Scene-Graph-Benchmark.pytorch-master/maskrcnn_benchmark/data/datasets/visual_genome.py.

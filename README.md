# Scene Graph Generator

### Description
This project is a reimplementation of the Scene Graph Generation Benchmark based on [Kaihua Tang's Scene-Graph-Benchmark repository](https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch/blob/master/README.md). It is introduced in the paper [Unbiased Scene Graph Generation from Biased Training](https://openaccess.thecvf.com/content_CVPR_2020/papers/Tang_Unbiased_Scene_Graph_Generation_From_Biased_Training_CVPR_2020_paper.pdf). The outputs for three of my own custom images can be found in [output_visualizations.ipynb](output_visualizations.ipynb). Scene graph generation aims to predict objects and the relationships between them (subject-predicate-object) from an input image. It is a crucial task in computer vision, facilitating higher-level understanding of visual scenes and enabling downstream applications like image captioning, visual question answering, and semantic segmentation. In this project, I include the main difficulties that I faced with implementing this model on a NVIDIA GeForce GTX 780 Ti GPU with CUDA 10.1 and my own package versions.


### Objectives
- To implement a scene graph generator capable of:
    - Assigning labels to objects in images.
    - Drawing bounding boxes around detected objects.
    - Predicting relationships between object pairs (e.g., dog-chasing-ball).
- To gain research experience in computer vision and contribute to foundational work in scene understanding.
- To demonstrate the utility of the Scene Graph Generation model for visual scene analysis using a pre-trained architecture.
- To provide clear examples of input-output visualizations, making this repository a useful reference for academic and research purposes.
- To include some challenges I faced when programming this model using my versions of GPU, CUDA, and the necessary packages.


### Installation Requirements
Check [Installation Requirements](Installation_Requirements.md) for installation instructions.


### Dataset
Check [Dataset](Dataset.md) to preprocess the data. 

### Usage
In this section I demonstrate how to run the pretrained Scene Graph Detection model from [the original repository](https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch/blob/master/README.md) to generate scene graphs for custom images. 

Enure that your input images are in jpg format and that they are stored in the file path denoted by TEST.CUSTUM_PATH. Since we are running the pretrained SGDet model which detects Scene Graphs from scratch, it is important that MODEL.ROI_RELATION_HEAD.USE_GT_BOX and MODEL.ROI_RELATION_HEAD.USE_GT_OBJECT_LABEL are set to False. For the Predicate Classification (PredCls) and Scene Graph Classification (SGCls) models, check the original repository. Moreover, you can also consider switching your MODEL.ROI_RELATION_HEAD.CAUSAL.CONTEXT_LAYER to none if you run into a Cuda Out of Memory error or have other issues with your GPU. 

It is also important that each of the lines in the command below are understood to ensure correct implementation.

For my specific file paths, this was my command to run the model:

```bash
CUDA_VISIBLE_DEVICES=0 \
python -m torch.distributed.launch \
--master_port 10027 \
--nproc_per_node=1 \
tools/relation_test_net.py \
--config-file "configs/e2e_relation_X_101_32_8_FPN_1x.yaml" \
MODEL.ROI_RELATION_HEAD.USE_GT_BOX False \
MODEL.ROI_RELATION_HEAD.USE_GT_OBJECT_LABEL False \
MODEL.ROI_RELATION_HEAD.PREDICTOR CausalAnalysisPredictor \
MODEL.ROI_RELATION_HEAD.CAUSAL.EFFECT_TYPE TDE \
MODEL.ROI_RELATION_HEAD.CAUSAL.FUSION_TYPE sum \
MODEL.ROI_RELATION_HEAD.CAUSAL.CONTEXT_LAYER motifs \
TEST.IMS_PER_BATCH 1 \
DTYPE "float16" \
GLOVE_DIR /home/marc/glove \
MODEL.PRETRAINED_DETECTOR_CKPT /home/marc/Desktop/Scene-Graph-Benchmark.pytorch-master/Scene-Graph-Benchmark.pytorch/checkpoints/pretrained_faster_rcnn/model_final.pth \
OUTPUT_DIR /home/marc/Desktop/Scene-Graph-Benchmark.pytorch-master/Scene-Graph-Benchmark.pytorch/checkpoints/outputs \
TEST.CUSTUM_EVAL True \
TEST.CUSTUM_PATH /home/marc/Desktop/Scene-Graph-Benchmark.pytorch-master/Scene-Graph-Benchmark.pytorch/checkpoints/custom_images \
DETECTED_SGG_DIR /home/marc/Desktop/Scene-Graph-Benchmark.pytorch-master/Scene-Graph-Benchmark.pytorch/checkpoints/outputs
```

### Scene Graph Visualization
The [output_visualizations.ipynb](output_visualizations.ipynb) visualizes the Scene Graphs for three custom images of my choice. Some adjustments are made to the original repository jupyter notebook file for this visualization. The inputs of our visualization code are custom_data_info.json (including the possible objects ind_to_classes and relationships ind_to_predicates to detect), and custom_prediction.json (providing the specific information for each image). These are the files that will be created inside of DETECTED_SGG_DIR if you run the commands above wihtout error. Note that the file paths specified inside each of these files will need to work in order for the visualization to be successful. Note that you can adjust the number of objects and relationships displayed in the image. The model will pick the top k that have highest probabilty of being detected.

### Future Work


### References 
This is a reimplementation of [Kaihua Tang's Scene-Graph-Benchmark repository](https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch/blob/master/README.md).


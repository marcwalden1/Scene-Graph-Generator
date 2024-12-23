# Scene Graph Generator

### Description
This project is a reimplementation of the Scene Graph Generation Benchmark based on [Kaihua Tang's Scene-Graph-Benchmark repository](https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch/blob/master/README.md). It is introduced in the paper [Unbiased Scene Graph Generation from Biased Training](https://openaccess.thecvf.com/content_CVPR_2020/papers/Tang_Unbiased_Scene_Graph_Generation_From_Biased_Training_CVPR_2020_paper.pdf). Scene graph generation aims to predict objects and the relationships between them (subject-predicate-object) from an input image. It is a crucial task in computer vision, facilitating higher-level understanding of visual scenes and enabling downstream applications like image captioning, visual question answering, and semantic segmentation. 

In this reimplementation, I applied the pretrained SGDet model on custom images. The outputs for three of my own custom images can be found in [output_visualizations.ipynb](output_visualizations.ipynb). In this project, I include the main difficulties that I faced with implementing this model on a **NVIDIA GeForce GTX 780 Ti GPU** with CUDA 10.1 and my own package versions.


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

### The Model

To implement this model, I used this pretrained [Faster R-CNN model](https://1drv.ms/u/s!AmRLLNf6bzcir8xemVHbqPBrvjjtQg?e=hAhYCw) that is provided in [Kaihua's original repository](https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch/blob/master/README.md). All files from this folder are stored inside /home/marc/Desktop/Scene-Graph-Benchmark.pytorch-master/Scene-Graph-Benchmark.pytorch/checkpoints/pretrained_faster_rcnn.

### Running the Scene Graph Generator
In this section I demonstrate how to run the SGDet model for custom images, which detects Scene Graphs from scratch. For the Predicate Classification (PredCls) and Scene Graph Classification (SGCls) models, check the original repository. 

It is important that each of the lines in the command below are understood to ensure correct implementation.

**This was my command to run the model:**

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

**Key Considerations**
- Input images must be in .jpg format and stored at the path specified in TEST.CUSTUM_PATH.
- Ensure MODEL.ROI_RELATION_HEAD.USE_GT_BOX and MODEL.ROI_RELATION_HEAD.USE_GT_OBJECT_LABEL are set to False for SGDet.
- Set MODEL.ROI_RELATION_HEAD.CAUSAL.CONTEXT_LAYER to "none" if a CUDA out-of-memory error is encountered.
- Generated outputs (e.g., scene graphs) are saved in the DETECTED_SGG_DIR directory.


### Scene Graph Visualization
The [output_visualizations.ipynb](output_visualizations.ipynb) visualizes the Scene Graphs for three custom images of my choice. Some adjustments are made to the original repository jupyter notebook file for this visualization. 

**Visualization Requirements**

Input files:

    - custom_data_info.json: Contains possible class labels (ind_to_classes) and relationships (ind_to_predicates).
    
    - custom_prediction.json: Contains the detection results for the images.
    
These files are generated in the DETECTED_SGG_DIR directory after running the model.

Note that the file paths specified inside each of these files will need to be accurate in order for the visualization to be successful. Also, the number of objects and relationships displayed in the image can be adjusted. The model will pick the top k that have highest probabilty of being detected.

### Future Work
This project demonstrates the capability of scene graph generation for understanding visual relationships in static images. Looking forward, there are exciting opportunities to extend this work, including:

**1. Dynamic Scene Analysis:**
- Extending the scene graph generator to process sequences of images (e.g., video frames) for dynamic scene understanding. This would involve generating frame-by-frame scene graphs and analyzing changes over time to capture motion, interactions, and events.

**2. Natural Language Video Synopses:**
- Leveraging scene graph outputs as structured data inputs for large language models (LLMs) like GPT or similar architectures.
- The objective would be to produce coherent, natural language descriptions of videos by aggregating frame-level scene graphs and contextualizing relationships across frames.
- For example, a scene graph sequence describing "person-holding-camera" followed by "person-taking-photo-of-dog" could be summarized as: "A person takes a picture of a dog."

**3. Broader Applications:**
- Applying this framework to tasks like Visual Question Answering, video summarization, or autonomous robotics. 


### References 
This is a reimplementation of [Kaihua Tang's Scene-Graph-Benchmark repository](https://github.com/KaihuaTang/Scene-Graph-Benchmark.pytorch/blob/master/README.md).


# CD-COCO: Complex Distorted COCO database for Scene-Context-Aware computer vision

Here is an original strategy of image distortion generation applied to the MS-COCO dataset that combines some local and global distortions to reach a better realism. We have shown that training with this distorted dataset improves the robustness of models by 31.5% approximately through this full framework evaluation and training. This repository is summarized as follow:
- [Overview](https://github.com/Aymanbegh/CD-COCO#overview): brief presentation of this framework evaluation of robustness
- [Image Distortions](https://github.com/Aymanbegh/CD-COCO#image-distortions): presents the main concept about image distortion
- [Requirements](https://github.com/Aymanbegh/CD-COCO#requirements): gives global dependencies information and links
- [Distorted dataset generation](https://github.com/Aymanbegh/CD-COCO#distorted-dataset-generation): provides a tutorial about the distorted dataset generation
- [Training results](https://github.com/Aymanbegh/CD-COCO#training-results): explains how to perform a training through YOLOv4 method (YOLOv4 or YOLOv4-tiny) and how to evaluate the robustness of the trained model.
- [Citation](https://github.com/Aymanbegh/CD-COCO#citation): BibTeX to cite this repository and the corresponding paper




Overview
-----------------------------------
we built this new versatile database derived from the well-known MS-COCO database to which we applied local and global photo-realistic distortions.
These new local distortions are generated by considering the scene context of the images that guarantees a high level of photo-realism. 
Distortions are generated by exploiting the depth information of the objects in the scene as well as their semantics. 
This guarantees a high level of photo-realism and allows to explore real scenarios ignored in conventional databases dedicated to various CV applications.
Our versatile database offers an efficient solution to improve the robustness of various CV tasks such as Object Detection (OD), scene segmentation, and distortion-type classification methods.

Furthermore, we propose a full framework generation of complexe photo-realistic distorted images applied on MS-COCO dataset.
The main contributions are:
-  New distortions with improved realism are introduced, describing common phenomena in computer vision through complex local and atmospheric distortions (see **Distorted dataset generation** section).
- This paper proposes efficient algorithms to generate local and global photorealistic distortions that are not included in any existing database. (see **Evaluation** section).
- A novel dataset is built from the MS-COCO dataset, dedicated to the improvement of the robustness of the OD and object segmentation models against a broad type of distortion.
- The image database, the proposed database scene classification index, and distortion generation codes are publicly available.


Image Distortions
-----------------------------------

- **Global distortions**: affect the image as a whole and come from different sources related in general to the acquisition conditions. Some are directly dependent on the physical characteristics of the camera and are of photometric or geometric origin. Among the most common distortions that affect the quality of the signal are defocusing blur, photon noise, geometric or chromatic aberrations, and blur due to the movement of the camera or the movement of objects. The other types of degradation are related to the environment and more particularly the lighting in the case of outdoor scenes. Compression and image transmission artifacts are another source of degradation that is difficult to control. These common distortions have been already considered in benchmarking the performance of some models.
![global](https://github.com/Aymanbegh/CD-COCO/assets/80038451/ca397e8b-13e9-4f95-abe7-9c6d546e8389)

- **Local distortions**: are undesirable signals affecting one or more localized areas in the image (see figure 1). A typical case is the blurring due to the movement of an object of relatively high speed. Another photometric distortion is the appearance of a halo around the object contours due to the limited sensitivity of the sensors or backlight illumination (BI). The artistic blur affecting a particular part of the targeted scene, the object to be highlighted by the pro-shooter, is another type of local distortion. Thus, integrating the local distortions in the database increases its size and makes it richer and more representative of scenarios close to real applications which improves the relevance of trained models.
    - **Local motion blur**: motion blur applied to object
      
        ![motion](https://github.com/Aymanbegh/CD-COCO/assets/80038451/73ba9bc4-4d6c-494a-ab10-7370487b62ad)


    - **Local defocus blur**: defocus blur adapted to the scene depth
      
        ![defocus](https://github.com/Aymanbegh/CD-COCO/assets/80038451/2ac45c1b-9d86-4d59-b8b7-3a5171ef19e8)

    - **Local backlight illumination**: contrast changing to objects related to backlight illumination
 
        ![back](https://github.com/Aymanbegh/CD-COCO/assets/80038451/c19885be-fa85-485f-b14b-7739fa0e1b2a)


- **Atmospheric distortions**: correspond to image acquisition for environments with particular atmospheric conditions such as rain and fog. We propose a new approach that applies masks of these atmospheric phenomena in a photorealistic way, taking into account the context of the scene (depth, object position,etc.).
  
    - **Photorealistic rain**
      
![rain](https://github.com/Aymanbegh/CD-COCO/assets/80038451/80fa69bb-2051-4757-8b38-1c6782b236e8)

    - **Photorealistic haze**  

![haze](https://github.com/Aymanbegh/CD-COCO/assets/80038451/35ef6ce2-3776-462d-905e-804cd5a9d73d)


Requirements
-----------------------------------
- **MATLAB R2021a or higher**
- **MS-COCO dataset: validation and train sets (corresponding images and instances annotations)**
    - https://cocodataset.org/#download  
- **Instances annotations converted in a Matlab matrix to generate CD-COCO dataset:**
    - **(train set):** https://drive.google.com/file/d/1vduixQEHJxMvdU0kaJ8GtiLkeqPE7i2L/view?usp=sharing
    - **(validation set):** https://drive.google.com/file/d/1yqHBH7kJfBWh7uG_r40P507wYWXdnjOy/view?usp=sharing
- **Generated distorted datasets (if you dont want to generate them yourself):**
    - **(image train set):** https://drive.google.com/file/d/1icqFD7MoLghg8PliNW_THCNiTfwae70b/view?usp=sharing
    - **(annotation train set):** https://drive.google.com/file/d/1JrI2JML5D8koexqi2AnlB0_QFariFfbY/view?usp=sharing
    - **(validation set):** https://drive.google.com/file/d/1aY3kXbH_HRu7YK7JfmnK4Xho5ToSxnkQ/view?usp=sharing
    - **(annotation validation set):** https://drive.google.com/file/d/1ju3Td425OTyvyCDonQnWGHDdXmF8aumE/view?usp=sharing
    - **(Local server for all data, in particular test set images):** https://www.l2ti.univ-paris13.fr/VSQuad/CD-COCO_ICIP2023_Challenge/ 


Distorted dataset generation 
-----------------------------------
Distortions are applied to 2 sets from the MS-COCO 2017 dataset => train (95K images) and validation (5K images) sets. 23K images are dedicated for the test set.
**You generate yourself your CD-COCO distorted dataset for the train and evaluation sets thanks to the following functions. Otherwise, you can download directly download our distorted dataset: (train set: GB) and (validation set: GB)**
**Follow the following instructions:**
- **Download the main directory from this depository**
- **Create the following folders**:
    - train2017: it contains the training set images from the COCO dataset
    - val2017: it contains the validation set images from the COCO dataset
    - train2017_distorted: it will contain the generated training set images
    - val2017_distorted: it will contain the generated validation set images
- Place in the main directory the annotations of the instances downloaded in Matlab format (training and validation sets)
- **Download the depth images of the training and validation sets and extract them into the main depository**:
    - [https://drive.google.com/drive/folders/1PDlCD63W8myRbsaJTRjwYBVlt8X2uwaA?usp=sharing](https://drive.google.com/file/d/1iEMjZuk-T9-XHGCqgMNBuvHCYp0Tb0ZS/view?usp=sharing)    (training)
    - [https://drive.google.com/drive/folders/1PDlCD63W8myRbsaJTRjwYBVlt8X2uwaA?usp=sharing](https://drive.google.com/file/d/1hYL2Kkia3LItRurtV7Ks05sMtAk20IOK/view?usp=sharing)   (validation)  


- **Distortion generation**: run the following matlab functions
    - %% Matlab script to launch the distorted training set
    - Main_application_train.m
    - %% Matlab script to launch the distorted validation set
    - Main_application_validation.m
 

Evaluate your performance
-----------------------------------

Follow the instructions below to evaluate the performance of your model with the CD-COCO database:

(1) Create an account on CodaLab. This will allow you to participate in all CD-COCO challenges [https://codalab.lisn.upsaclay.fr/competitions/9981](https://codalab.lisn.upsaclay.fr/competitions/9981).

(2) Carefully review the guidelines for entering the CD-COCO challenges and using the test sets.

(3) Prepare a JSON file containing your results in the correct results format for the challenge you wish to enter. Please name your JSON file "predict" and use it for the "Final evaluation Phase".
 
         
Training results [Previous work](https://github.com/Aymanbegh/Benchmarking-performance#overview)
-----------------------------------

Training experiments are done with the YOLOv4-tiny model on GPU RTX 2080 SUPER. Find all dependencies to train your model with our distorted train set here:
- Copy and paste into the darknet directory our dependencies (cfg, img_dir, and data folders): https://drive.google.com/drive/folders/187RnbPSwFhEOH5k1E4LgrEDFuMOY4qEI?usp=sharing
- **coco2017_d**: files that contains annotation in txt format.
    - link: https://drive.google.com/file/d/1O3TtciCbmqeq0M5FpkaSjqP_boXB9eR7/view?usp=sharing
- **data folder**: folder that must contain information files to run training
    - link: https://drive.google.com/drive/folders/1Wd0wcNEoeNcKaJs1M6BMPZUtFPQve8hv?usp=sharing
- **pre-trained models**: instructions and pre-trained models to launch a costum training
    - link: https://github.com/AlexeyAB/darknet#how-to-train-to-detect-your-custom-objects

Protocol to launch a costum training:
- Copy and paste files from the coco2017_d folder previously downloaded into the coco2017 folder from the data folder present in the darknet directory:

      ```
      ├── darknet
          └── data
            └── coco2017
                └── paste here all files contain in the coco2017_d folder
            └── coco2017.data    
            ...
            └── val2017.txt
      ├── cfg 
          └── 9k.labels
          ...
          └── yolo-voc.cfg
      ├── img_dir
          └── train2017.txt
          ...
          └── val2017_rain10.txt   
         ```  
         
- Copy and paste python and shell files into the darknet directory : https://drive.google.com/drive/folders/187RnbPSwFhEOH5k1E4LgrEDFuMOY4qEI?usp=sharing
- **To train your model on our distorted MS-COCO dataset:**
    - Copy and paste images from our distorted MS-COCO 2017 training set (**see Requirements to download it**) into the coco2017 folder (in folder data)
    - Copy and paste the desired **pre-trained models** in the darknet directory
    - Modify the coco2017.data in the data folder as follow:

            classes= 80
            train  = /home/beghdadi/darknet/data/train2017_d.txt
            valid  = /home/beghdadi/darknet/data/val2017.txt
            #valid = data/coco_val_5k.list
            names = data/coco2017.names
            backup = /home/beghdadi/darknet/backup/
            eval=coco
        
    - Select the desired models to train into the script **launch_traind** and run it as follow:
    
            ./launch_traind.sh

- **To train your model on the original MS-COCO dataset:**
    - make the same process but copy paste the original MS-COCO 2017 training set instead of our distorted MS-COCO 2017 training set.

Here, evaluation results of our trained models robustness against each distortion type and level.
![image](https://user-images.githubusercontent.com/80038451/153755895-5503f06a-9465-4267-b3c9-e4df9f794dd7.png)
![image](https://user-images.githubusercontent.com/80038451/153755924-b7496789-4b34-46f8-92f9-a5e4379f28ab.png)

Previous graphics are summarized in the following table to highlight the impact of data augmentation on robustness for each specific distortions and for specified levels:

|Distortion Type| 2  | 4 | 6 | 8 | 10 |mAP Average per distortion| 
| ------ | :------: | :------: | :------: |  :------: | :------: | :------: |
| **Noise** | 9.58% | 22.3% |37.9% | 80.3% | 114% | 47.2% |
| **Contrast** | -0.54% | 1.8% | 2.47% | 3.23% | 5.71% | 1.99% |
| **Compression** | -2.36% | -0.48% |-0.5% | 4.84% | 35.4% | 4.26% |
| **Rain** | 12.7% | 40.4% | 71.4% | 101.8% | 114.3% | 61.7% |
| **Haze** | -0.49% | 3.14% |10.5% | 29.8% | 51.7% | 15.8% |
| **Motion-Blur** | 6.11% | 39.5% |95.3% | 151.4% | 183.3% | 86.3% |
| **Defoc-Blur** | 1.02% | 14.2% |22.4% | 26.3% | 26.7% | 16.5% |
| **Loc. MBlur** | 10.3% | 31.6% |46.9% | 59.3% | 67.0% | 39.8% |
| **Loc. Defoc.** | 3.08% | 16.0% |23.3% | 25.5% | 25.9% | 17.2% |
| **BAckLit** | 1.45% | 5.85% |19.1% | 40.3% | 76.8% | 24.1% |
| **Average per level** | 4.11% | 17.9% |32.9% | 52.3% | 70.1% | **31.5%** |


In our case the global average improvement reaches 31.5% 

    
    
Citation
-----------------------------------

Use this BibTeX to cite this repository:

Previous work


    @inproceedings{beghdadi2022benchmarking,
      title={Benchmarking performance of object detection under image distortions in an uncontrolled environment},
      author={Beghdadi, Ayman and Mallem, Malik and Beji, Lotfi},
      booktitle={2022 IEEE International Conference on Image Processing (ICIP)},
      pages={2071--2075},
      year={2022},
      organization={IEEE}
    }



Use this BibTeX to cite the corresponding paper:


    @INPROCEEDINGS{10323035,
      author={Beghdadi, Ayman and Beghdadi, Azeddine and Mallem, Malik and Beji, Lotfi and Cheikh, Faouzi Alaya},
      booktitle={2023 11th European Workshop on Visual Information Processing (EUVIP)}, 
      title={CD-COCO: A Versatile Complex Distorted COCO Database for Scene-Context-Aware Computer Vision}, 
      year={2023},
      volume={},
      number={},
      pages={1-6},
      doi={10.1109/EUVIP58404.2023.10323035}}

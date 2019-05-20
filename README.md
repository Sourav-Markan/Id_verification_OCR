# Id card Varification OCR

This repo basically takes the raw input image and return you the basic information that any Id card (in json format). In this basically, first text boxes are detected using the Connectionist Text Proposal Network that is giving the accuracy around 89%. Then comes the OCR Model for converting the image into the text, for this i am using pytesseract with is the open source of tesseract-OCR. One can directly use this library. At last is the Information Extraction part in which in am using the regex and other Natural Language Technique for extracting the required information. You can update the Inforamtion Extraction part as per your need.

## PIPELINE :
Pic Upload ---> Text Boxes detected using CTPN + OCR on each text block + Format and sorting according to dimensions(for getting clear text of id card + Information Extraction)
	Note: For Increasing the accuracy one need to only modify the get_info file in ocr folder. This we can improve while testing.

DATASET:
Used for training = ICDAR 2013 and 2015 

NEURAL NETWORK MODEL:
1. CTPN - Very deep VGG16 model implemented in Tensorflow.
2. Works reliably on multi-scale and multi- language text without further post-processing, departing from previous bottom-up methods requiring multi-step post-processing.
3. Computationally efficient with 0:14s/image.
4. Not only restricted to id cards, we can use it for any image where there is horizontal data present. Like Pan card, Adhaar card.

ACCURACY:
Measures for given test data = 0.88 and 0.61 F-measure

FEATURES USED:
1. Fast RCNN (For detecting the text boxes at very high rate)
2. Side Refinement

USAGE:
1. pip intall requirements.txt
2. chmod +x run.sh
3. ./run.sh

[IMPORTANT NOTE:] 
1. User image should be of extention user_id.jpg
2. Source Folder Contain the all source files.
3. Output Folders will contain results for images those are proccessed.
4. Algorithm will take care wheather image has been previously processed or not.


TRAINING:

1. cd lib/utils
2. chmod +x make.sh
3. ./make.sh

Prepare Data
--> Modify the path and gt_path in prepare_training_data/split_label.py according to your dataset. And run
1. cd lib/prepare_training_data
2. python split_label.py
--> This will generate the prepared data in current folder, and then run
3. python ToVoc.py
--> To convert the prepared training data into voc format. It will generate a folder named TEXTVOC. move this folder to data/ and then run
4. cd ../../data
5. ln -s TEXTVOC VOCdevkit2007

Train
1. python ./ctpn/train_net.py

	Note: You can modify some hyper parameters in ctpn/text.yml, or just used the parameters I set.

PARAMETERS USED:
1. USE_GPU_NMS # whether to use nms implemented in cuda or not
2. DETECT_MODE # H represents horizontal mode, O represents oriented mode, default is H
3. checkpoints_path # the model I provided is in checkpoints/, if you train the model by yourself,it will be saved in output/
Note: You may need to modify according to your requirement, you can find them in ctpn/text.yml

PRE-TRAINED Model
The model I am using checkpoints is trained on GTX1070 for 50k iters.

For getting the pre-trianed model- You can directly mail me(sourav.kumar@research.iiit.ac.in)
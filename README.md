# RPS-VS-AI-ROBOT
 In this game, players upload pictures of their hand gestures to the dataset, and the AI robot responds with a randomized output, simulating the classic rock-paper-scissors experience. The AI's unpredictable responses add an element of excitement and intrigue to the gameplay. 

# The Algorithm
This re-trained ResNet-18 model was created on Jetson Nano and trained on a dataset of Rock, Paper and Scissors images. It runs on an imagenet.py program that will classify your hand gesture and reply with a random output to determine if you drew, won or lost.

# Running This Project
1. Make sure that both the Jetson Inference library and Python3 are installed on your Jetson Nano.
2. Download the model_best.pth.tar. Links to the models and data:(https://drive.google.com/drive/folders/1FruGAXi81CQoS5BPv7WosuzJQ-KyAEKy?usp=sharing)
3. Then upload photos of your own hand in any one of the rock paper scissors position.
4. Download then run the code in Visual Studios Code.
5. Navigate to the jetson-inference directory and run the docker:
```
$ ./docker/run.sh
```
6. Navigate to the classification directory again:
```
   $ cd python/training/classification
```
7. Export the model to ONNX:
```
   $ python3 onnx_export.py --model-dir=models/rps
```
8. Exit the docker by doing:
```
   $ exit
```
9. Then make sure you go back to the proper folder:
```
   $ cd jetson-inference/python/training/classification
```
10. Set the net and data variables as shown below:
```
   $ NET=models/rps
   $ DATASET=data/rps
```
11. Adjust the following to the correct data set and picture name then run:
```
   $ ./imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/rock/DS.jpg rps.jpg ```
```
12. View video for explanation.
https://youtu.be/xfKSGxtsdvs

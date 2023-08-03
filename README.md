# RPS-VS-AI-ROBOT
 In this game, players upload pictures of their hand gestures to the dataset, and the AI robot responds with a randomized output, simulating the classic rock-paper-scissors experience. The AI's unpredictable responses add an element of excitement and intrigue to the gameplay. 

# The Algorithm
This re-trained ResNet-18 model was created on Jetson Nano and trained on a dataset of Rock, Paper and Scissors images. It runs on an imagenet.py program that will classify your hand gesture and reply with a random output to determine if you drew, won or lost.

# Running This Project
1. Make sure that both the Jetson Inference library and Python3 are installed on your Jetson Nano.
2. Run the code in Visual Studios Code.
3. Upload photos of your hand in any one of the rock paper scissors position.
4. Make sure your in the proper folder.
```
   $ cd jetson-inference/python/training/classification
```
6. Adjust the following to the correct data set and picture name and run.
```
   $ ./imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/rock/DS.jpg rps.jpg ```
```
7. View video for explanation. 

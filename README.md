# RPS-VS-AI-ROBOT
 In this game, players upload pictures of their hand gestures to the dataset, and the AI robot responds with a randomized output, simulating the classic rock-paper-scissors experience. The AI's unpredictable responses add an element of excitement and intrigue to the gameplay. 
import sys
import argparse
import random

from jetson_inference import imageNet
from jetson_utils import videoSource, videoOutput, cudaFont, Log

# parse the command line
parser = argparse.ArgumentParser(description="Classify a live camera stream using an image recognition DNN.", 
                                 formatter_class=argparse.RawTextHelpFormatter, 
                                 epilog=imageNet.Usage() + videoSource.Usage() + videoOutput.Usage() + Log.Usage())

parser.add_argument("input", type=str, default="", nargs='?', help="URI of the input stream")
parser.add_argument("output", type=str, default="", nargs='?', help="URI of the output stream")
parser.add_argument("--network", type=str, default="googlenet", help="pre-trained model to load (see below for options)")
parser.add_argument("--topK", type=int, default=1, help="show the topK number of class predictions (default: 1)")

try:
	args = parser.parse_known_args()[0]
except:
	print("")
	parser.print_help()
	sys.exit(0)


# load the recognition network
net = imageNet(args.network, sys.argv)

# note: to hard-code the paths to load a model, the following API can be used:
#
# net = imageNet(model="model/resnet18.onnx", labels="model/labels.txt", 
#                 input_blob="input_0", output_blob="output_0")

# create video sources & outputs
input = videoSource(args.input, argv=sys.argv)
output = videoOutput(args.output, argv=sys.argv)
font = cudaFont()

# process frames until EOS or the user exits
while True:
    # capture the next image
    img = input.Capture()

    if img is None: # timeout
        continue  

    # classify the image and get the topK predictions
    # if you only want the top class, you can simply run:
    #   class_id, confidence = net.Classify(img)
    predictions = net.Classify(img, topK=args.topK)

    # draw predicted class labels
    for n, (classID, confidence) in enumerate(predictions):
        classLabel = net.GetClassLabel(classID)
        confidence *= 100.0

        print(f"imagenet:  {confidence:05.2f}% class #{classID} ({classLabel})")

        font.OverlayText(img, text=f"{confidence:05.2f}% {classLabel}", 
                         x=5, y=5 + n * (font.GetSize() + 5),
                         color=font.White, background=font.Gray40)
                         
    # render the image
    output.Render(img)

    # update the title bar
    output.SetStatus("{:s} | Network {:.0f} FPS".format(net.GetNetworkName(), net.GetNetworkFPS()))

    # print out performance info
    net.PrintProfilerTimes()

    #Rps code
    win = """
    /$$      /$$ /$$$$$$ /$$   /$$ /$$   /$$ /$$$$$$$$ /$$$$$$$        
    | $$  /$ | $$|_  $$_/| $$$ | $$| $$$ | $$| $$_____/| $$__  $$       
    | $$ /$$$| $$  | $$  | $$$$| $$| $$$$| $$| $$      | $$  \ $$       
    | $$/$$ $$ $$  | $$  | $$ $$ $$| $$ $$ $$| $$$$$   | $$$$$$$/       
    | $$$$_  $$$$  | $$  | $$  $$$$| $$  $$$$| $$__/   | $$__  $$       
    | $$$/ \  $$$  | $$  | $$\  $$$| $$\  $$$| $$      | $$  \ $$       
    | $$/   \  $$ /$$$$$$| $$ \  $$| $$ \  $$| $$$$$$$$| $$  | $$       
    |__/     \__/|______/|__/  \__/|__/  \__/|________/|__/  |__/       
                                                                        
                                                                        
                                                                        
    /$$      /$$ /$$$$$$ /$$   /$$ /$$   /$$ /$$$$$$$$ /$$$$$$$        
    | $$  /$ | $$|_  $$_/| $$$ | $$| $$$ | $$| $$_____/| $$__  $$       
    | $$ /$$$| $$  | $$  | $$$$| $$| $$$$| $$| $$      | $$  \ $$       
    | $$/$$ $$ $$  | $$  | $$ $$ $$| $$ $$ $$| $$$$$   | $$$$$$$/       
    | $$$$_  $$$$  | $$  | $$  $$$$| $$  $$$$| $$__/   | $$__  $$       
    | $$$/ \  $$$  | $$  | $$\  $$$| $$\  $$$| $$      | $$  \ $$       
    | $$/   \  $$ /$$$$$$| $$ \  $$| $$ \  $$| $$$$$$$$| $$  | $$       
    |__/     \__/|______/|__/  \__/|__/  \__/|________/|__/  |__/       
                                                                        
                                                                        
                                                                        
    /$$$$$$  /$$   /$$ /$$$$$$  /$$$$$$  /$$   /$$ /$$$$$$$$ /$$   /$$
    /$$__  $$| $$  | $$|_  $$_/ /$$__  $$| $$  /$$/| $$_____/| $$$ | $$
    | $$  \__/| $$  | $$  | $$  | $$  \__/| $$ /$$/ | $$      | $$$$| $$
    | $$      | $$$$$$$$  | $$  | $$      | $$$$$/  | $$$$$   | $$ $$ $$
    | $$      | $$__  $$  | $$  | $$      | $$  $$  | $$__/   | $$  $$$$
    | $$    $$| $$  | $$  | $$  | $$    $$| $$\  $$ | $$      | $$\  $$$
    |  $$$$$$/| $$  | $$ /$$$$$$|  $$$$$$/| $$ \  $$| $$$$$$$$| $$ \  $$
    \______/ |__/  |__/|______/ \______/ |__/  \__/|________/|__/  \__/
                                                                        
                                                                        
                                                                        
    /$$$$$$$  /$$$$$$ /$$   /$$ /$$   /$$ /$$$$$$$$ /$$$$$$$           
    | $$__  $$|_  $$_/| $$$ | $$| $$$ | $$| $$_____/| $$__  $$          
    | $$  \ $$  | $$  | $$$$| $$| $$$$| $$| $$      | $$  \ $$          
    | $$  | $$  | $$  | $$ $$ $$| $$ $$ $$| $$$$$   | $$$$$$$/          
    | $$  | $$  | $$  | $$  $$$$| $$  $$$$| $$__/   | $$__  $$          
    | $$  | $$  | $$  | $$\  $$$| $$\  $$$| $$      | $$  \ $$          
    | $$$$$$$/ /$$$$$$| $$ \  $$| $$ \  $$| $$$$$$$$| $$  | $$          
    |_______/ |______/|__/  \__/|__/  \__/|________/|__/  |__/ """
    lose = """
    
  █     █░ ▒█████   ███▄ ▄███▓ ██▓███      █     █░ ▒█████   ███▄ ▄███▓ ██▓███     
▓█░ █ ░█░▒██▒  ██▒▓██▒▀█▀ ██▒▓██░  ██▒   ▓█░ █ ░█░▒██▒  ██▒▓██▒▀█▀ ██▒▓██░  ██▒   
▒█░ █ ░█ ▒██░  ██▒▓██    ▓██░▓██░ ██▓▒   ▒█░ █ ░█ ▒██░  ██▒▓██    ▓██░▓██░ ██▓▒   
░█░ █ ░█ ▒██   ██░▒██    ▒██ ▒██▄█▓▒ ▒   ░█░ █ ░█ ▒██   ██░▒██    ▒██ ▒██▄█▓▒ ▒   
░░██▒██▓ ░ ████▓▒░▒██▒   ░██▒▒██▒ ░  ░   ░░██▒██▓ ░ ████▓▒░▒██▒   ░██▒▒██▒ ░  ░   
░ ▓░▒ ▒  ░ ▒░▒░▒░ ░ ▒░   ░  ░▒▓▒░ ░  ░   ░ ▓░▒ ▒  ░ ▒░▒░▒░ ░ ▒░   ░  ░▒▓▒░ ░  ░   
  ▒ ░ ░    ░ ▒ ▒░ ░  ░      ░░▒ ░          ▒ ░ ░    ░ ▒ ▒░ ░  ░      ░░▒ ░        
  ░   ░  ░ ░ ░ ▒  ░      ░   ░░            ░   ░  ░ ░ ░ ▒  ░      ░   ░░          
    ░        ░ ░         ░                   ░        ░ ░         ░               
    """
    user_action = classLabel
    possible_actions = ["rock", "paper", "scissors"]
    computer_action = random.choice(possible_actions)
    print(f"Who Will Win!??!?!?")


    if user_action == "scissors":
        the_scissors = """
         _        ,/'
        (_).  ,/'
        __  ::
        (__)'  `\.
                   `\.
            """
        print("You chose " + classLabel)
        print(the_scissors)
            
    elif user_action == "paper":
        the_paper ="""
            _______
        ---'    ____)____
                   ______)
                 _______)
                _______)
        ---.__________)
    """
        print("You chose " + classLabel)
        print(the_paper)
    elif user_action == "rock":
        the_rock = """
        ⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⡿⠿⠛⠛⠛⠛⠿⣿⣿⣿⣿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⡿⠛⠉⠁⠀⠀⠀⠀⠀⠀⠀⠉⠻⣿⣿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⡟⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠘⢿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⣿⣿⣿⡟⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣾⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⣿⠋⠈⠀⠀⠀⠀⠐⠺⣖⢄⠀⠀⠀⠀⠀⠀⠀⠀⣿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⡏⢀⡆⠀⠀⠀⢋⣭⣽⡚⢮⣲⠆⠀⠀⠀⠀⠀⠀⢹⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⡇⡼⠀⠀⠀⠀⠈⠻⣅⣨⠇⠈⠀⠰⣀⣀⣀⡀⠀⢸⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⡇⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣟⢷⣶⠶⣃⢀⣿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⡅⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢿⠀⠈⠓⠚⢸⣿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⡇⠀⠀⠀⠀⢀⡠⠀⡄⣀⠀⠀⠀⢻⠀⠀⠀⣠⣿⣿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⡇⠀⠀⠀⠐⠉⠀⠀⠙⠉⠀⠠⡶⣸⠁⠀⣠⣿⣿⣿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⣿⣦⡆⠀⠐⠒⠢⢤⣀⡰⠁⠇⠈⠘⢶⣿⣿⣿⣿⣿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⣿⣿⡇⠀⠀⠀⠀⠠⣄⣉⣙⡉⠓⢀⣾⣿⣿⣿⣿⣿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⣿⣿⣷⣄⠀⠀⠀⠀⠀⠀⠀⠀⣰⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿
        ⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣷⣤⣀⣀⠀⣀⣠⣾⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿"""
        print("You chose " + classLabel)
        print(the_rock)


    if computer_action == "scissors":
        the_scissors = """
         _        ,/'
        (_).  ,/'
        __  ::
        (__)'  `\.
                   `\.
        """
        print("Computer chose " + computer_action)
        print(the_scissors)
    elif computer_action == "paper":
        the_paper ="""
            ---' ____)____
                    ______)
                    _______)
                    _______)
            ---.__________)
        """
        print("Computer chose " + computer_action)
        print(the_paper)
    elif computer_action == "rock":
        the_rock = """
            ⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⡿⠿⠛⠛⠛⠛⠿⣿⣿⣿⣿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⡿⠛⠉⠁⠀⠀⠀⠀⠀⠀⠀⠉⠻⣿⣿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⡟⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠘⢿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⣿⣿⣿⡟⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣾⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⣿⠋⠈⠀⠀⠀⠀⠐⠺⣖⢄⠀⠀⠀⠀⠀⠀⠀⠀⣿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⡏⢀⡆⠀⠀⠀⢋⣭⣽⡚⢮⣲⠆⠀⠀⠀⠀⠀⠀⢹⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⡇⡼⠀⠀⠀⠀⠈⠻⣅⣨⠇⠈⠀⠰⣀⣀⣀⡀⠀⢸⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⡇⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣟⢷⣶⠶⣃⢀⣿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⡅⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⢿⠀⠈⠓⠚⢸⣿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⡇⠀⠀⠀⠀⢀⡠⠀⡄⣀⠀⠀⠀⢻⠀⠀⠀⣠⣿⣿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⡇⠀⠀⠀⠐⠉⠀⠀⠙⠉⠀⠠⡶⣸⠁⠀⣠⣿⣿⣿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⣿⣦⡆⠀⠐⠒⠢⢤⣀⡰⠁⠇⠈⠘⢶⣿⣿⣿⣿⣿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⣿⣿⡇⠀⠀⠀⠀⠠⣄⣉⣙⡉⠓⢀⣾⣿⣿⣿⣿⣿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⣿⣿⣷⣄⠀⠀⠀⠀⠀⠀⠀⠀⣰⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿
            ⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣷⣤⣀⣀⠀⣀⣠⣾⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿
        """
        print("Computer chose " + computer_action)
        print(the_rock)


    if user_action == computer_action:
        print(f"Both players selected {user_action}. It's a tie!")
    elif user_action == "rock":
        if computer_action == "scissors":
            print("Rock smashes scissors! You win!")
            print(win)
        else:
            print("Paper covers rock! You lose.")
            print(lose)
    elif user_action == "paper":
        if computer_action == "rock":
            print("Paper covers rock! You win!")
            print(win)
        else:
            print("Scissors cuts paper! You lose.")
            print(lose)
    elif user_action == "scissors":
        if computer_action == "paper":
            print("Scissors cuts paper! You win!")
            print(win)
        else:
            print("Rock smashes scissors! You lose.")
            print(lose)
    #end


    # exit on input/output EOS
    if not input.IsStreaming() or not output.IsStreaming():
        break

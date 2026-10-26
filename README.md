PHASE 1: Install the Required Applications

Step 1. Install Python
Python runs your program.
1. Open the official website: Python for Windows
2. Download Python 3.11 (64-bit).
3. Open the downloaded installer.
4. Select Add python.exe to PATH.
5. Click Install Now.
6. Wait for installation to finish.
Check Python
Press Windows + R.

Type:
cmd

Press Enter.

Run:
python --version

Expected output:
Python 3.11.x

The final number may differ.

Then run:
python -m pip --version

If both commands work, continue.

If Python is already installed, you don't need to reinstall it immediately. Check the version first.

Step 2. Install Visual Studio Code

VS Code is where you'll write and execute your program.
1. Open Visual Studio Code download page.
2. Download the Windows installer.
3. Install the application.
4. Open VS Code.
5. Select the Extensions icon on the left.
6. Search for Python.
7. Install the official Python extension published by Microsoft.

PHASE 2: Create Your Project
Step 3. Create a project folder

Open File Explorer.

Create this folder:

C:\HandGestureControl

Your project will be stored in this location.
Open it in VS Code
1. Open VS Code.
2. Click File → Open Folder.
3. Select C:\HandGestureControl.
4. Click Select Folder.
Your folder should now appear in the Explorer panel.

Step 4. Open the VS Code terminal

In VS Code:
1. Click Terminal in the top menu.
2. Select New Terminal.
The terminal will appear at the bottom.

Run:
python --version

Confirm that Python works inside VS Code.

PHASE 3: Create a Virtual Environment

Step 5. Create the environment

A virtual environment keeps your project's Python libraries separate from other projects.

In the VS Code terminal, run:
python -m venv .venv

Wait for the command to finish.
Activate it
If your terminal is PowerShell, run:
.\.venv\Scripts\Activate.ps1

If successful, the terminal should look similar to:
(.venv) PS C:\HandGestureControl>

If PowerShell blocks activation, switch the VS Code terminal to Command Prompt and run:
.venv\Scripts\activate.bat

Select the Python interpreter
1. Press Ctrl + Shift + P.
2. Search for Python: Select Interpreter.
3. Choose the Python interpreter inside your project's .venv folder.
Important: Install your libraries after activating this environment.

PHASE 4: Install the Required Libraries

Your code imports these modules:
Library	Purpose
OpenCV	Opens the webcam and displays video
MediaPipe	Detects hand landmarks
pynput	Simulates keyboard presses
time	Provides the sleep() function


The time module is built into Python, so you do not need to install it separately.

Step 6. Install OpenCV

Run:
python -m pip install opencv-python

Wait for installation to complete.

Step 7. Install MediaPipe

Run:
python -m pip install mediapipe

MediaPipe provides the hand-tracking functionality used in your code.

If installation fails, don't randomly install other versions. Send me the complete error message so we can choose a compatible setup.

Step 8. Install pynput

Run:
python -m pip install pynput

This library lets your Python program send keyboard commands.

Step 9. Check all three libraries

Run:
python -c "import cv2, mediapipe, pynput; print('All libraries installed successfully!')"

Expected output:
All libraries installed successfully!

If you see an error, stop here and fix it before running your main program.

PHASE 5: Test Your Webcam

Step 10. Create a camera test program

Before running the complete hand gesture program, make sure your webcam works.

PHASE 6: Add Your Original Hand Gesture Code

Step 11. Create the main Python file

Inside your project folder, create:
main.py

Open your existing code and copy all of it into main.py.

Your project folder should now look like this:

C:\HandGestureControl
│
├── .venv
├── camera_test.py
└── main.py

Save the file using Ctrl + S.
Check your code before running
Your code already contains:
import cv2import mediapipe as mpfrom pynput.keyboard import Controller, Keyfrom time import sleep


These imports correspond to the libraries you installed.

The following line initializes the keyboard controller:
keyboard = Controller()


The following lines initialize MediaPipe hand tracking:
mp_hands = mp.solutions.handshands = mp_hands.Hands(    min_detection_confidence=0.8,    min_tracking_confidence=0.8)


And this line opens the default webcam:
cap = cv2.VideoCapture(0)


These are the core components of your program.

PHASE 7: Run the Program

Step 12. Execute main.py

First, ensure your virtual environment is activated.

In the terminal, run:
python main.py

What should happen?
1. The webcam opens.
2. A window named Hand Gesture Control appears.
3. MediaPipe attempts to detect your hand.
4. Landmarks appear over the detected hand.
5. You make a fist.
6. Your program determines which action to trigger.
7. The keyboard controller presses the corresponding arrow key.
8. Opening your hand releases the key.
9. Press Q to exit.
How to test it
Initially, test the program with the game closed.
1. Start the program.
2. Place your hand in front of the webcam.
3. Make a fist.
4. Observe the landmarks and the status text.
5. Open your hand.
6. Check whether the status text disappears.
Your program prints messages in the terminal when it recognizes an accelerator or brake gesture.

Important: The code does not display a hand-status message if no hand is detected. It also does not explicitly release the arrow keys when the hand disappears from the camera. We'll correct that in the improved version below.

PHASE 8: Important Corrections to Your Original Code

There are three issues worth fixing before using the program for game control.

Problem 1: The hand identification logic

Your code uses:
if landmarks.landmark[mp_hands.HandLandmark.THUMB_TIP].x > 0.5:


This checks whether the thumb is on one side of the image, not whether the detected hand is the right hand.

Because the camera image is flipped, the result can be confusing.

A better approach is to use MediaPipe's handedness classification, which attempts to identify whether a detected hand is left or right.

Problem 2: Keyboard keys can remain pressed

Your code releases the keys when a detected hand is open, but if the hand disappears completely from the frame, the release logic may not execute.

The improved version should release both keys whenever no hand is detected.
Problem 3: Multiple key presses can interfere
The original code maintains two separate variables:
is_gas_pressed = Falseis_brake_pressed = False

This can work, but using one variable to track the active action makes the key state easier to manage.

PHASE 9: Connect the Program to Hill Climbing

Step 13. Open the game

Once the hand detection works:
1. Start main.py.
2. Open your Hill Climbing game.
3. Click inside the game window so it receives keyboard input.
4. Make a closed right-hand gesture.
5. Verify that the vehicle accelerates.
6. Make a closed left-hand gesture.
7. Verify that the vehicle brakes.
8. Open your hand to release the key.
If the game doesn't respond, confirm which keyboard keys it uses.

Safety tip: Test with a simple game or an empty text editor first. The program sends actual keyboard input to whichever application has focus.

PHASE 10: Deploy and Run the Project Again Later

You do not need to reinstall Python or the libraries every time.

Whenever you want to run the project:

1. Open VS Code.

2. Open C:\HandGestureControl.

3. Open the terminal.

4. Activate your virtual environment:
.\.venv\Scripts\Activate.ps1

5. Run the project:
python main.py

6. Press Q in the webcam window when you want to stop.
Optional: Prepare for sharing or submitting your BCA project
Once everything works, you can create a requirements.txt file:
python -m pip freeze > requirements.txt

This records the installed package versions.

You can also create a README.md containing your project description, software requirements, installation steps, gesture mappings, and instructions for running the application.

For your first successful deployment, running main.py from VS Code is sufficient. Packaging it as an executable or hosting it online can be done later.

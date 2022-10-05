# StoryToolkitAI

## Description

This is meant as an editing extension for Davinci Resolve. 

Among other things, it transcribes timelines directly from Resolve by rendering to Audio Only (mp4/wav) and then passing
the rendered file to OpenAI Whisper, a state-of-the-art speech recognition model. 

After the audio files are processed, the tool saves the transcription into both a detailed JSON and an SRT file ready to
be imported into Davinci Resolve (or other software).

### Is it really completely free?
Yes, the tool runs locally and there's no need for any additional account.

### Results
The results we get with Whisper are significantly better than any other Speech-to-Text model (including Google, AWS
etc.) out there and the models are free to use.

### Speed
Our tests show that on a Macbook Pro M1 a 30-second timeline is transcribed on average in approx. 1 minute, but results
may vary.

On a Windows workstation with a GTX1070, the transcription time is around a quarter of the length of the audio file
(4-minute audio is transcribed in approx. 1 minute).

---

## Setup & Installation

Our installation is on MacOS 12.6 running on M1, but the scripts should run fine on other CPUs and GPUs. For both
production and development we're currently using Python 3.9.13. 

_Note: Whisper worked fine on Python 3.10.2, but we ran into problems when trying to install some packages which we're
planning to use for future developments._

**The tool only works on Resolve Studio 18.**

_Note: Unfortunately, only the Studio version of Resolve supports external scripting and Resolve versions earlier than
18 do not support Python 3.6+_

We recommend running the tool inside a virtual environment like virtualenv. This is not required, but it prevents
messing up your Python installation.


_**Important Disclaimer: you need to be comfortable with using the Terminal on Mac OS, or the Command Prompt on Windows.
You should be fine even with little to no experience, but keep in mind that you are installing stuff on your machine
and there is a (very) slight chance that you'll affect your Operating System's overall performance or even the performance of
your other apps. In an unlikely worst-case scenario, some stuff might not work anymore at all and you'll need pro help
to fix them back._

### Mac OS
In the Terminal:

#### 1. You'll need Homebrew
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

#### 2. You'll need Python 3.9, Python Tkinter, Git, and FFMPEG:

    brew install python@3.9
    brew install python-tk@3.9
    brew install git
    brew install ffmpeg

#### 3. Make sure you now have virtualenv:

    pip install virtualenv

#### 4. Download StoryToolkitAI:
First, go to the Folder you want to install StoryToolkit in via Finder. Right-click on it and select "New Terminal at Folder".
Once you get another terminal window open, run:

    git clone https://github.com/octimot/StoryToolkitAI.git

This should download the app in the folder that you chose.

#### 5. Set up a virtual environment
Now create a virtual environment (to prevent messing up with other python packages you may have installed on your OS for other stuff):

    virtualenv -p python3.9 venv

Right now, your installation folder should contain 2 other folders, and the tree should look like this:

    YOUR_INSTALLATION_FOLDER
    +- StoryToolkitAI
    +- venv

#### 6. Activate virtual environment
Now enable the virtual environment (this means that all the packages you'll install now via pip will be contained in the
virtual environment, meaning that for the tool to work you'll ALWAYS have to activate the virtual environment first
using the following command!)

    source venv/bin/activate

#### 7. Install OpenAI Whisper

    pip install git+https://github.com/openai/whisper.git 

For more info regarding Whisper installation, please check https://github.com/openai/whisper 

#### 8. Install all the stuff the tool requires:

    pip install -r StoryToolkitAI/requirements.txt

If you are running the tool on a machine with an NVIDIA CUDA GPU, make sure you install Torch with CUDA:

    pip uninstall torch
    pip cache purge
    install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu116

_Note: If Resolve is not turned on or not available, the transcription and translation functions will work on normal wav 
files too. Simply press the transcribe or translate buttons and follow the process._

#### That's it!
Inside the virtual environment, you should now be able to start the tool:

    python StoryToolkitAI/app.py

_Note: After restart of the machine or your terminal window, never forget to activate the virtual environment before
starting the app. In the folder where you created venv, run:_

    source venv/bin/activate
    
### Windows

Detailed Windows installation instructions coming soon. 

---

## How to transcribe timelines:

#### 1. Open Resolve and StoryToolkitAI
Open a project in Resolve. Then, go back to the folder where you installed the tool, activate the virtual environment 
(if you created one) and then start the tool:
    
    source venv/bin/activate
    python StoryToolkitAI/app.py

A simple GUI should appear on the screen:

![StoryToolkitAI GUI](help/StoryToolkitAI_GUI.png)

#### 2. Open the Timeline and Press Transcribe 

Go back to Resolve and open the Timeline that you want to transcribe, then click the "Transcribe Timeline" button and follow the process from there.

#### 3. Wait a bit

Once the process has started, it needs a bit of time to transcribe. As soon as it's done, it will save an SRT with the transcription and a JSON with the full result generated by Whisper.

#### 4. Import SRT into Resolve (optional)
When the transcription is ready, you can choose a Media Bin in Resolve where the app will automatically import the generated SRT file. 

---

## Direct Translations to English
The tool also supports direct translation to English by clicking the "Translate Timeline to English" button. However, it will not generate any original language transcription together with the translation.

---

# Known issues
There seems to be a problem with the Resolve libraries on some Windows 10 machines, but we're trying to find a fix.
If you're running the tool on a Windows 10 machine with Resolve 18 and it works, please get in touch.

To report any issues, please use the Issues tab here on Github: https://github.com/octimot/StoryToolkitAI/issues

# Contributions
This tool is developed and maintained by Octavian Mot (https://mots.us).

Feel free to get in touch or contribute.

The app is very raw and not polished at all!

Ideally, it should evolve by incorporating other machine learning models such as CLIP and GPT-3 to assist editors in their work.
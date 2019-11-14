# Interface for Running Speech Enhancement and Recognition.

## The interface has 3 section:
* Choosing multi-channel file
* Running Enhancements
* Running ASR

![GUI for Analysis](https://github.com/iitbdaplab/iitbtcsgui2/blob/master/analysis.png)

### Choosing multi-channel file
You can choose any multi-channel file of any format here. The multi-channel audio should be one file containing all the channels. After clicking the ’Browse’ button,a dialog box will open to choose the multi-channel file. Locate the multi-channel file you want to enhance and click 'Open'.

After choosing, it will display the name of the file along with the path and also how many channels are present in the file.
Also the GUI will allow you to play/ASR decode each channel. The single-channel audio is extracted and stored at ```single_channel/```. By deafult, it will open ```VLC``` media player to play and decoding will be done by the specified models (to setup own ASR model, steps are mentioned below) which will open a new window with the decoded hypothesis transcripts and the reference transcripts. 

![Section 1 of the GUI](https://github.com/iitbdaplab/iitbtcsgui2/blob/master/sec1.png)


### Running Enhancements

Now this section of the interface follows an order. It starts with single-channel de-noising,
single-channel dereverberation, DoA estimation and beamforming.
Starting with single-channel de-noising, the drop-down list has two techniques namely:
{Weiner and Spectral Subtraction}. You can  play and decode each single-
channel denoised audio, same as that of input multi-channel files which is mentioned before. It has a checkbox, so if not nneded, it can be disabled. 


![Section 2 of the GUI](https://github.com/iitbdaplab/iitbtcsgui2/blob/master/sec2.png)
This is succeeded by single-channel dereverberation with the similar options to play and decode each channel. For dereverberation, we have used {WPE and NMF} techniques. 

This stage is followed by localization with GCC-{PHAT,SCOT} available as options. Beamforming using the TDOA estimates obtained from the localization( expect for Beamformit Tool). Options available in Beamforming are: {DSB, GEV, MVDR, NN-GEV,
NN-MVDR}.

Finally, after beamforming, a single-channel enhanced file obtained can be played or decoded.

Note that the if selected, the order of single channel enhacements is : Denoising all channels > Dereverberating all channels > Beamforming. 


### Running ASR
This stage is still under development. We plan to use the DoA values to identify the
speaker. But one problem here is DoA is not reliable as they vary a lot due to varying
energy levels in speech and can give noisy DoA estimates in between the audio(frame
level). 

The button ’Show Diarization time stamps’ will show which speaker spoke and
their start and end time. The bottom-most button show transcripts will show which
speaker spoke with what in <speaker id: text >format.
  
  
The log of each stage is displayed in the ’log’ console window.  
  
## Setup, Directory Structure and Other requirements

Linux machine is required to run this task as most of the ASR models are being tested
run on Linux machine.

The following toolkits are required:
* Kaldi ```5.0``
* Octave-dev,Octave-lib

Other python based package requirements are specified in the requirements.txt file. Python3 is used for building the GUI, so install python3 and the dependencies to run the the GUI.

### Setting up the GUI

The folder from cloned https://github.com/iitbdaplab/iitbtcsgui2 is to be placed in ```<Your kaldi path >/egs``` folder or if the ```.zip``` file is used, extract the folder to ```<Your kaldi path >/egs``` .
  
Here are the steps:

1) Clone the repository from https://github.com/iitbdaplab/iitbtcsgui2 using

  ```
  git clone https://github.com/iitbdaplab/iitbtcsgui2                 (from github)
  
  Or 
  
  unzip <filename>.zip                                                (for zip file)
 ```

3) Copy the cloned/extarcted folder ```gui```  to <Your path >/kaldi/egs   # where 'Your path' is whbere kaldi folder resides
  ```
   cp -r gui <Your path>/kaldi/egs/.
  ```

2) Run the following command on your unix machine to install required dependencies.
  ```
   chmod +x dependencies.sh
   ./dependencies.sh
  ```


4) Change the directory to <Your path>/kaldi/egs/gui/s1
  ```
   cd <Your path>/kaldi/egs/gui/s1
  ```

## Run the Program
 ```
 ./gui.py    # Runs using python3
 
```
## Here is the placed by deafult where the files are stored
* After de-noising, the enhanced file is stored at ```single_denoising/<method>/<filename>_CH{1/2/3/4}.wav for method={nmf,wiener}```
* After dereverberation, the enhanced file is stored at ```single_dereverb/<method>/<filename>_CH{1/2/3/4}.wav for method={wpe,nmf}```
* After Beamforming, the enhanced file is stored at ```out_beamform/<filename>_<method>.wav for method={MVDR,DSB,GEV,MVDR_NN,MVDR_TA} ```

### You can change the acoustic and language model using the following steps below.
The acoustic model is located at ```exp/model_typ`` ..for e.g. exp/tri1```
The langauge model is located at ```data/lang```

First step is to make a graph. YOu can do this using the ```mkgraph``` kaldi function. For e.g. 
```utils/mkgraph.sh data/lang exp/tri1 exp/tri1/graph || exit 1;```
Now the last step is to change the graph directory in ``decode_gui.sh`` script by changing the ```graph_dir``` to ```exp/tri1/graph```

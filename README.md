# Stop Signal Task - EEG version
## Overview:
Welcome! This is Professor Neil McNaughton's Lab's Stop Signal Task (SST) 
experiment (see the below figure). This SST was created with the intention of 
being used in conjunction with an electroencephalogram (EEG). 
Information about where in the project different components are scripted 
can be found in [the wiki](https://github.com/Neil-McNaughton-Lab/EEG-StopSignalTask/wiki).

As is standard with SSTs this task can be broken into two types of trials: 
go trials and stop trials. A single go trial our SST consists of the 
following steps: a variable inter-trial interval (ITI), a 0.5 s attention 
stimulus, a 1 s directional stimulus, and a 0.5 s feedback stimulus. The ITI 
times are a set of values, between 0.5 and 4 s, which were sampled from a 
logarithmic distribution (*t(x) = -ln(x)/λ*). While these times will appear 
random to a participant engaging with the task, they are fixed across all 
trials. 

In the case of stop trials, all of the steps are the same with the exception
of the directional stimulus where a stop sound will play some time after the 
stimulus has been presented. It is the participant's job to refrain from 
entering a left or right key press when they hear the sound. The time between 
the onset of the stimuli and the stop sound, known as the stop signal delay 
(SSD), is drawn from one of three staircase designs.

![SST Flowchart](doc/figures/SST_flow_2021-07-20.png?raw=true "SST flowchart")
<p align="center" style="font-size:10px">
    Figure 1<br />  
    The experimental flow chart of the Stop Signal Task as the subject 
    experiences it. The red numbers indicate the corresponding parrallel port 
    output triggers.
</p>


## Installation:
The SST is has been created in [PsychoPy3](https://www.psychopy.org/) 
and requires [PsychoPy3 2021.1.3](https://github.com/psychopy/psychopy/releases) 
or newer to run (provided PsychoPy's "Use PsychoPy version:" option still works).

## Quick Set Up:
Once PsychoPy3 is installed you can open `SST.psyexp` and run the task. If you 
installed a newer version of PsychoPy3 you may need to change the PsychoPy 
version being used to run the task. This should be located under: 
__experiment settings__ (the gear icon) __-> Basic -> Use PsychoPy version__.

Additionally, the left and right response keys are mapped to `2` & `3` for 
right handed participants and `7` & `8` for left handed participants. 
<!---These can be changed in the code if necessary (see wiki [link]).--->

Ensure your conditions file has been filled out correctly. 
The conditions file is used to define the trial blocks and the type of trial 
presented to the participant. In particular trials labelled `0` under 
`TrialTypes` will be __go__ trials, while any other integer will be a __stop__ 
trials with each number representing a unique staircase. Each block is defined 
by a set of repeating integers under the `Block` header. For example, 
*1, 1, 1, 2, 2, 2, 3, 3, 3* indicates three blocks: 1, 2, and 3. These integers 
must be in ascending order. The `BlockType` column is used to indicate whether 
or not a block is a practise run or not. To indicated a block as a practise 
block simply insert the keyword `practice` under the `BlockType` column, 
next to the first integer in the block. Any other keyword will be treated as 
"not-a-practise." So I like to use `EEG`. Finally, the `L2R_ratio` column is 
used to indicate the ratio of left stimuli to right stimuli you would like to 
appear during each block. Leaving this column empty will default practise 
blocks to *0.5* (or a *50:50* split) and stop blocks to a random ratio.


### Choosing a parraellel port:
This script defaults to the port `0xD010`. If you need to change this you will
have to do so manually unfortunately. This is becuase Psychopy Builder 
doesn't allow for all parrael port calls to be changed simultaneously. 
*I'm working on a work around for this, but for now it works.*

To change the port you must go into the each `_port` item in the 'countdown',
'standby', 'go_stim', 'go_feedback', 'stop_stim' , 'stop_feedback', 'ITI', and 
'End' routines in the builder. From there you will find a list of available 
addresses under  __Port Address__ located within the Hardware tab. 

Please note that these aren't the addresses available to the computer but the 
addresses listed in Psychopy's system preferences (yes, it is this 
unnecessarily complicated). To add your address to the list go to 
__File__ -> __preferences__, see Figure 2. Then, once your in `Psychopy 
Preferences` go to the __Hardware__ tab and add your address to the list of
__parraell ports__. Click `Apply`, save the experiment, and then (becuase the 
ports don't update immediately) close and reopen it.

![Change parraellel port](doc/figures/change_parrallel_port.png?raw=true "Change parraellel port")
<p align="center" style="font-size:10px">
    Figure 2<br />  
    Menus you will need to navigate in order to add a new parrallel port to 
    PsychoPy's list of available ports.
</p>


### Disabling ESC and Fullscreen:
By default I have left the task with full screen disabled and the Esc key, 
which exits the task, available. This is because I assume you'd like to test 
the task before using it on participants. 
To enable fullscreen you will need to go into the `experiment settings` and 
untick the `Enable Escape key` tick box in under the __Basic__ tab (see 
Figure 3). Likewise, to disable the escape key you will need to go into the 
`experiment settings` and untick the `Full-screen window` tick box in under the 
__Screen__ tab (see Figure 3).

![DisableFSandESC](doc/figures/DisableFSandESC.png?raw=true "Disable Fullscreen and ESC key")
<p align="center" style="font-size:10px">
    Figure 3<br />  
    Menus to traverse in order to disable the Escape key and/or enable full 
    screen.
</p>


## Troubleshooting:
If you run into troubles the PsychoPy Runner screen displays the script Python 
outputs. This is useful for certain errors. Such as the 
```python
.
.
.
    block_port = parallel.ParallelPort(address='0xD010')
TypeError: 'NoneType' object is not callable
```` 
which comes up if you run this experiment of a PC that doesn't have a parallel 
port driver installed.




# Communication app activation during active embedded audio source or navigation

* Proposal: [SDL-NNNN]
* Author: [Melnyk Tetiana](https://github.com/TMelnyk)
* Status: Awaiting review
* Impacted Platforms: Core
[SDL-NNNN]: https://github.com/smartdevicelink/sdl_evolution/blob/master/proposals/NNNN-Communication-app-activation.md

## Introduction
This proposal is to handle communication app activation during active embedded audio and navigation sources.

## Motivation  
**Required for FORD.**  
**Required additional clarification from FORD.**  
Covered cases:
voice-com app activation during embedded audio source
voice-com app activation during embedded navigation
. MixingAudioSupported=true or false 

## Proposed solution

1. In case communication app in LIMITED and AUDIBLE due to active embedded navigation 
and SDL receives SDL.ActivateApp (appID_of_communication_app) from HMI
SDL must: 
respond SDL.ActivateApp (SUCCESS) to HMI
send OnHMIStatus (FULL, AUDIBLE) to mobile app (embedded navigation is still active) 

2. In case communication app in FULL and AUDIBLE
and SDL receives OnEventChanged (EMBEDDED_NAVI, isActive=true) from HMI
SDL must:
send OnHMIStatus (LIMITED, AUDIBLE) to mobile app  

3. Communication app activation is the trigger for HMI to switch off embedded audio source,
HMI switches off embedded audio source and sends OnEventChanged (AUDIO_SOURCE, isActive=false) to SDL. 


4. In case communication app in LIMITED and AUDIBLE due to active embedded navigation
and "MixingAudioSupported" = true at .ini file
and SDL receives TTS.Started from HMI
SDL must: 
send OnHMIStatus (LIMITED, ATTENUATED) to mobile app
send OnHMIStatus (LIMITED, AUDIBLE) to mobile app right after embedded navigation stops streaming (SDL receives TTS.Stopped from HMI) 

5. In case communication app in BACKGROUND and NOT_AUDIBLE due to active embedded audio source () 
and user activates this communication app
and SDL receives from HMI:
a) OnEventChanged (AUDIO_SOURCE, isActive=false) 
b) SDL.ActivateApp (appID_of_communication_app) 
SDL must:
respond SDL.ActivateApp (SUCCESS) to HMI
send OnHMIStatus (FULL, AUDIBLE) to mobile app
Information: 
a. Communication app activation is the trigger for HMI to switch off embedded audio source
b. HMI switches off embedded audio source and sends OnEventChanged (AUDIO_SOURCE, isActive=false) to SDL 

6. In case voice-com app in LIMITED and AUDIBLE due to active embedded navigation and "MixingAudioSupported" = false at .ini file
and SDL receives TTS.Started from HMI
SDL must:
send OnHMIStatus (LIMITED, NOT_AUDIBLE) to mobile app
send OnHMIStatus (LIMITED, AUDIBLE) to mobile app right after embedded navigation stops streaming (SDL receives TTS.Stopped from HMI) 

## Detailed design
TBD
## Impact on existing code
TBD
## Alternatives considered
TBD
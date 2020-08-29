# HomeAssistant Node-Red Alexa Timer Notification
This is a custom Node Red Flow for notifying via Text To Speech (TTS) on multiple Alexa Devices when a Timer goes off on any device.  This includes the label and takes into account cancelling, pausing and delaying timers.

![NodeRed Alexa Timer Flow](/images/HA_NodeRed_Alexa_Timer_Notification.png)

# How does it work?
 This flow creates a global variable in Node Red (alexaTimerArray) and maintains this as a list of all "Active" Timers or "Cancelled" Timers still running Node Red Flow.  Each Timer change is set to delay until the Timer set time.  Once the set time is achieved the flow evaluates the state to determine if the timer is still active.  If the timer is active it will generate a TTS message and send to all devices except the device where the timer is going off.  Voice notifications are time restricted so that they occur outside of set hours.
  
# How To Install
## Dependencies
  Requires the installation of the Alexa Media Player Integration.  Install through HACS or from <https://github.com/custom-components/alexa_media_player>
## Install Process  
  * Copy the JSON from Alexa_Timers_flow.nodered and import the Flow into Node Red.  
  * Add the device exclusion to groups.yaml in your Home Assistant Config directory
  
# Configuration
## Excluding Devices from TTS
  The flow will look for a Home Assistant group for devices to exclude from this process.
  
  services_exlude_from_alexa_timer_notifications:
  name: "Entities to exclude from Alexa Timer Notifications"
  entities:
    - notify.alexa_media_this_device
  
## Customizing the TTS Message
  Inside the flow is a Function Node labeled "Generate TTS Message".  In the top of the node is a variable called msgTTS.  Set the Text String to the message you wnat delivered on your Alexa device.  This excepts placedholders for variable data.  Possible Values are:
  * {DeviceName} : The Name given to the timer, omitted if null
  * {timerLabel}: The name of the device
  
## Customizing the voice restriction hours
  Inside the flow is a function Node labeled "Don't Alert at Night".  Use this node to update the times range you don't want messages delivered via TTS.  The defaults is 22:00 to 08:00.
  



Each EVAcharge board computes the CAN IDs it uses by adding CAN_ID_Base which is 0x0 by default. The base can be set to another value via the CAN message SetMappingForDevice.

# frame


id{SetMappingForDevice}

cob_id{33}

## data
### payload
#### int32
in{DeviceID}

out{DeviceID}



#### int16
in{CAN_ID_Base}

out{CAN_ID_Base}












Each EVAcharge board broadcasts a unique id every 1 sec.
# frame


id{PropagateDeviceID}

cob_id{34}

## data
### payload
#### int32
in{DeviceID}

out{DeviceID}











# frame


id{ResetAllMappings}

cob_id{34}





# __State Machine__  *__EVAChargeEVSEBoard__* 
__States__ :

__Initial__ , __PowerOn__ , __Ready__ 

__Transitions__ :

Initial -`➜`PowerOn

PowerOn -`➜`Ready

Ready   -`➜`CANInterfaceMapper   .    -`➜`DeviceBroadcast

## __State Machine__  *__EVAChargeEVSEBoard.DeviceBroadcast__* 
__Actions__ :

doStartTimer:


```javascript
start_periodic_timer(1,evTimer_BroadcastingDeviceID_Elapsed)
```
doSendDevicID:


```javascript
evPropagateDeviceID
```
__States__ :

__Initial__ , __GenerateUniqueDeviceID__ , __WaitForTimer__ , __BroadcastDeviceID__ 

__Transitions__ :

Initial                -/doStartTimer();`➜`GenerateUniqueDeviceID

BroadcastDeviceID      -`➜`WaitForTimer

GenerateUniqueDeviceID -`➜`WaitForTimer

WaitForTimer           -evTimer_BroadcastingDeviceID_Elapsed`➜`BroadcastDeviceID



## __State Machine__  *__EVAChargeEVSEBoard.CANInterfaceMapper__* 
__States__ :

__Initial__ , __WaitForSetMapping__ , __SetNewBase__ , __ResetBaseToOriginal__ 

__Transitions__ :

Initial             -`➜`WaitForSetMapping

ResetBaseToOriginal -`➜`WaitForSetMapping

SetNewBase          -`➜`WaitForSetMapping

WaitForSetMapping   -evResetAllMappings`➜`ResetBaseToOriginal         .          -evSetMappingForDevice`➜`SetNewBase






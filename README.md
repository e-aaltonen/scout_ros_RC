# RC state publisher mod for Scout ROS packages
by E. Aaltonen March 2024

This project includes additions and modifications necessary to publish RC data in Agilex Scout Mini (https://github.com/agilexrobotics/scout_ros (license: BSD). Original files (retrieved on 1 Mar 2024) are marked with the ".original" file name extension.

## New files:

* scout_msgs/msg/ScoutRCStatus.msg

## Modified files:

* scout_msgs/CMakeList.txt
* scout_base/include/scout_base/scout_messenger.hpp
* scout_base/src/scout_messenger.cpp

RC data is published in topic /scout_rc_status:

```
uint8 swa
uint8 swb
uint8 swc
uint8 swd
int8 stick_right_v
int8 stick_right_h
int8 stick_left_v
int8 stick_left_h
int8 var_a
```

## Changes:
```
diff scout_base/include/scout_base/scout_messenger.hpp.original scout_base/include/scout_base/scout_messenger.hpp:

8a9
>   // Modifications by EA Mar 4, 2024: added rc_publisher_
56a58
>     ros::Publisher rc_publisher_;   // EA


diff scout_base/src/scout_messenger.cpp.original scout_base/src/scout_messenger.cpp:

8a9,10
>  
>  // Modifications by EA Mar 4, 2024: added RC state publisher (rc_publisher_)
16a19
> #include "scout_msgs/ScoutRCState.h" // EA
32c35,36
< 
---
>     rc_publisher_ = nh_->advertise<scout_msgs::ScoutRCState>("/scout_rc_status", 10);   // EA
>     
210a215,228
>     
>     // publish Scout RC state message // EA
>     scout_msgs::ScoutRCStatus rc_msg;
>     
>     rc_msg.swa = robot_state.rc_state.swa;
>     rc_msg.swb = robot_state.rc_state.swb;
>     rc_msg.swc = robot_state.rc_state.swc;
>     rc_msg.swd = robot_state.rc_state.swd;
>     rc_msg.stick_right_v = robot_state.rc_state.stick_right_v;
>     rc_msg.stick_right_h = robot_state.rc_state.stick_right_h;
>     rc_msg.stick_left_v = robot_state.rc_state.stick_left_v;
>     rc_msg.stick_left_h = robot_state.rc_state.stick_left_h;
>     
>     rc_publisher_.publish(rc_msg);


diff scout_msgs/CMakeLists.txt.original scout_msgs/CMakeLists.txt:

55a56
>     ScoutRCState.msg ## EA
205d205
< 
```

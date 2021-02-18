# Aeroguide
Guide humans with MAVs.

## Simple Sim Testing:
You will need the following packages:
* `admittance_controller`
* `mav_external_force_plugin`
* `rotors_gazebo`
* `mav_nonlinear_mpc`


Start RotorS Sim:
```
roslaunch ag_integration ag_simple_sim.launch mav_name:=firefly
```

Takeoff and engange the mpc to take trajectories:
```
rosservice call /firefly/takeoff
rosservice call /firefly/back_to_position_hold
```
Engange the admc_controller:
```
rosservice call /firefly/admc_engage_controller "data: True"

```

Now we send desired force commands via node_manager to topic `/firefly/human_control_force`

If you wish to add external forces to act on the mav: Publish a force command to



## Indoor testing:
You will need the following packages:
* `vrpn_client`
* `mav_ros`
* `pose_sensor_vicon` (msf for vicon + imu fusion)
* `state_monitor`

### Startup State estimation and MPC (w.o. admittance):
For vicon and state estimation setup, perform the following steps in node_manger:
1. Engage a ros core on your Kiwi and your computers (top left wkindow)
2. Go to `mav_tools\mav_startup\launch\pixhawk` and load `kiwi.launch` (lower left window) -> IMPORTANT: set state_estimator parameter to `vicon`
3. Launch `vrpn_client`
4. Launch `mav_ros`
5. Launch `pose_sensor_vicon`
6. Initialize `msf` with scale 1.0 (`rosservice call initialize_msf_scale`)
7. Launch `mav_nonlinear_mpc`

Now your ready to operate the MPC.

### Startup State estimation and MPC (for admittance):
1. Engage a ros core on your Kiwi and your computers (top left wkindow)
2. Go to `ag_integration\launch\ac_simple.launch` and load `ac_simple.launch` (lower left window) -> IMPORTANT: set state_estimator parameter to `vicon` (if not set as default)
3. Launch `vrpn_client`
4. Launch `mav_ros`
5. Launch `pose_sensor_vicon`
6. Initialize `msf` with scale 1.0 (`rosservice call initialize_msf`)
7. Via state_monitor, check if msf is stable
8. Launch `mav_nonlinear_mpc`
9. Launch `admittance_controller`

Now all the tools are set up to fly:
1. Take off manually and switch to offboard ctrl
2. `rosservice call /firefly/back_to_position_hold`
3. `rosservice call /firefly/admc_engage_controller "data: True"`

Now the drone should react compliantly to external force inputs

### Set force setpoints
Now we send desired force commands via node_manager to topic `/firefly/human_control_force`

--> check notes on drive /RotaryWing/AeroGuide for

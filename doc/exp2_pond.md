# Second motor powered experiment

After the first experiment, lots of issue were found. Fixing them took quite a lot of time but finally all solved. .

So I decided to take another motor driven experiment to test the rudder effectiveness and the waypoint mission. 

Overall, the experiment went well. 

- rudder effectiveness improved by increasing the angle limitation and extending the length. 

- There still problem with LORA communication. 

- There is only one water leaking issue found on bow deck due to a unscrew bot. 

- Waypoint planning seems effective, the boat can drive itself toward to right direction. Although the turning radius is still quite large. 

- Camera exposure is now working correctly, however, there were some issues with connection. All images taken during the experiment are broken, cannot be fixed. It a pity. 

![exp2_boat.jpg](figures/exp2_boat.jpg)

![exp2_boat_on_water.jpg](figures/exp2_boat_on_water.jpg)

## Waypoint mission

Thanks for the navigation simulator, I was able to validate my navigation algorithms in the simulated world. See the [navigation development log](navigation.md) for details.

I use the default parameters for waypoint mission, which is very rough and conservative. however, it did work well. The PID parameters were set to 1, 0, 0. which means every metre of  *cross track error* turn the rudder for 1 degree. No integration and damping components. The pond seems a little bit small for testing the waypoint function but the result is still exciting. 

![exp2_waypoint.gif](figures/exp2_waypoint.gif)

While we solved the EMI issue, the GNSS can locate the boat pretty well now. 

![exp2_path.png](figures/exp2_path.png)

With only 20% of throttle, the boat sails peacefully. 

![exp2_waypoint_video.gif](figures/exp2_waypoint_video.gif)

## Unstable rolling motion

without hydro wings, keel and underwater ballast, the boat is still unstable at high throttle. 

I am not worrying too much about that because when the wing sail is ready, we must install the keel together with the ballast. 

## Minor water leaking

The gaps between the screw and polycarbonate glass are not seal correctly. When water come up to the deck, it is leaking into the boat. I bought a bag of O ring and new seal, hopefully can fixed this issue. 

The other water leaking source is the screws for the 3D printed solar panel mounts. The bot underneath became loose very easily when water leaking in. I would seal them with silicone glue for sea test.

![exp2_water_leaking.gif](D:\work\personal\DeepPlankter\DeepPlankter\doc\figures\exp2_water_leaking.gif)

## Batteries management issue

- One of the protector is broken. Looks like the IC is bricked at charge protection. I could not recover from the error state. I replaced the board .

- The motor drain majority of current from one of the battery. Later found this was due to a short under the ideal diode controller.  Fixed. 

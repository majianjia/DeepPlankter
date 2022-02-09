# Navigation

Navigation is a challenge for the small boat like DeepPlankter. 

- The ship might equip with a free-rotated wing. Head wind and tail wind will generate little-to-no force. The boat need to offset to the wind direction. 

- The propulsion (from wing or wave) is small and unstable. It can even be push back by wind or sea current. The navigation should be able to drive the boat back in this situation.   

- Ship speed is very low. 

## Simulator

> The source code is available in [navi-sim](https://github.com/majianjia/nav-sim)

To ensure the navigation algorithm works as expected, I actually build a 2-D simulator out of [Processing 3](https://processing.org/). Processing 3 is basically a java graphical lib with some customized interfaces. This simulator is extremely helpful for me to develop a navigation. 

Up until today, it has the features below:

- Ture earth coordination

- Dynamic wind simulation [direction, wind guest, wind speed]

- Dynamic sea current [direction, speed]

- Drag

- Interactive waypoint 

- time acceleration

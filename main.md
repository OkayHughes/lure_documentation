#Outline
* The Lure
* Pozyx tags
* Arduino/Motor Controller
* Ramps, stands, and pullies
* Batteries
* Entropy Code

##Recommending reading material:
You should be able to obtain from Dr. Moore or I the supplements of her paper on how entropy distributions can be used to explore evolutionary ideas. I’d recommend reading these in order to understand why we are doing this project, and to start to understand what the mechanical lure needs to replicate. 

#The general idea:
The objective of this project is to experimentally validate that the entropy of a rodent's trajectory predicts its ability to evade predators. This would provide an evolutionary explanation for the strange anatomy of Jerboas, a type of small rodent that Dr. Moore has studied.

In order to validate this, we needed to generate a mechanical lure that could create trajectories with certain properties. The goal is to recruit birds of prey from the local hawking community, train them to use our lure system, and test how easily the hawks learn to catch the lure when the trajectory has different properties.
The three trajectory properties that must be varied (simultaneously) are as follows:

* Verticality
		Either a trajectory remains close to the ground, or it mimics the way that a rodent jumps. This can be achieved using the ramps that I have assembled. I will talk about this later.
* Entropy
		In some sense, the differential entropy of a distribution is a measure of the information of the trajectory; alternately, it's possible to view it as "randomness" in the trajectory. I would recommend reading one of the links I listed above for the mathematical ideas behind why this corresponds to the entropy of a distribution. The entropy of a trajectory can likely be increased by putting the bait at the end of the line in a mass-spring system and driving the mechanical lure in short bursts (discussed below).
* Speed 
		Each of the previously listed variables must be able to be varied in a trajectory with a low average velocity, and a high average velocity (as measured by the sensor that is measuring the trajectory). 
	
#Simulation Results
In order to measure what kind of motion generates high-entropy distributions I wrote a simulator that tried varying the trajectory in ways that I thought could be mimicked with the mechanical lure to determine the best way to increase the entropy of a trajectory. The main result is that the large-scale features of the trajectory don't impact the entropy; what was important was rapid changes in direction of motion. For example, making the lure hop 6 inches side-to-side is a good way to increase the entropy. 

#The Lure:
The lure is created to achieve maximum speeds of 3 1 m/sec, with a maximum torque of ____.
It is designed to be run using an arduino (though any micro controller which is able to generate PWM would work). It's constructed of aluminum, but the bolts are steel. I have temporarily created a case around the lure, spool, battery, and motor controller using a plastic storage container. This case isn't water-tight, and this led to me shorting out the first motor controller I used in the system.  Replacing it with a more water-tight seal would be a good idea.

#The Motor Controller and Arduino #1:
The current motor controller that is included in the lure system is a Victor SPX, whose datasheets and informational materials can be found here:
https://www.vexrobotics.com/217-9191.html#Docs\_&\_Downloads.
At the moment, I control it using PWM input from one of the Arduino motor controllers. I don't have a significant amount of software written to control it, so you will need to write this yourself. The reason that I have included a motor controller at all is so that entropy and verticality can be varied in a relatively repeatable way. With a mass-spring system attached to the end of the lure, you can drive the motor in short bursts in order to cause the trajectory to become chaotic. You can also write a macro so that the motor will drive the lure faster just before ramps. Using this, you can vary verticality while keeping average velocity of the lure low. All of this is open-loop, so you will need to tune the signal you're sending to the motor to generate the right differential entropy. I'll talk below about why this may be slightly complicated, but not impossible. 

#The Pozyx Tags and Arduino #2:
These tags are from https://www.pozyx.io/ and provide centimeter-level positioning in 3 dimensional space at a rate of around 10-20 Hertz. These are used to get the location of the lure (and eventually the hawk) in real time. 

There are many gotchas in this system, but they can be circumvented with some effort.

For 3 dimensional positioning, 4 “anchors” are necessary. Anchors are tags which are not designed to interface with an Arduino. Currently I use a different Arduino than the one that controls the Motor Controller, given that I started developing the two in tandem. It may be possible to consolidate the two on one Arduino, but up to this point commanding the two separately hasn't been that much of a hassle.

I currently have 6 total sensors. Four of them are anchors, two are “tags” which have the headers to interface with an Arduino. The four anchors are easy to set up (bolt them to the waterproof enclosures that are attached to the stands, then plug them into the external battery pack). Using the stands (which I talk about below), elevate two anchors. It’s important to know the spatial locations of the sensors, to a reasonable degree of accuracy. At the moment, I’ve been determining them with a tape measure, and I've been unable to quantify how sensitive the data I get from the sensors is to precise localization of the sensors. 

The reason for having some of the sensors elevated is that many people have observed that larger vertical displacements in the sensor positions results in less noise in the data that comes from the sensor. 

Unfortunately, even when sensors have adequate vertical offset, there is noise in the positional data of the lure that has a magnitude of several centimeters. This is potentially a problem, because of how entropy is calculated. The entropy of a trajectory is calculated from a distribution that is fit to the distribution of velocities that occur over the course of a trajectory, calculated by finite-differences. The noise in the data may impact this in ways that I don't fully understand yet. When you are tuning the mechanical lure to create trajectories that have the right entropy, I would recommend keeping this noise in mind as a potential source of error. 

On a more concrete level, the Pozyx sensors are easy to use. After placing them on the stands, measure their relative positions and note the hex code printed on the outside of the anchor housing. I will provide the simple script that I used to report and gather data.Once you replace lines __ to __ with the data you measured, you can run _________ to start receiving data from the Pozyx board, and run ______ to log it to a file. It's not significantly different from the example scripts provided on the website. A modification that I have not done yet is to use remote-positioning, which allows me to measure the positional measurements of a Pozyx tag from a different tag, that's on my computer. This allows us to get sensor data without having to connect directly to the Pozyx sensor on the end of the lure line. 

#Ramps, stands, and pullies:
##Stands
The stands for sensors are simple. I haven’t tried them in high-wind environments yet, so that may be need to be dealt with.
All of the stands can pack flat, and the longest piece is no longer than __ feet. You should only need two crescent wrenches, one allen wrench, and a phillips-head screwdriver in order to assemble them. I have drilled holes in long pieces of wood to which the metal bases of the stands can be attached. These pieces of wood can be staked into the ground in order to provide a wider base of support. The bases are relatively short. 

##Pullies
The pullies are the small cylindrical objects that can be staked into the ground. These are designed to allow the lure line to make tight turns, and so that you can make large-scale changes in the trajectory. Currently, I keep these in place when I'm doing tests by placing them on pieces of plywood and staking both the pully and the plywood into the ground. This helps to prevent the lure from sinking into the dirt, and to keep the grass and plants from pushing the lure line upwards.
You need a different allen wrench to assemble the pullies. The allen wrench is used to tighten the shaft collars that hold the shaft and spool onto the bearing. If the cable continues to come off of the pullies, it would be possible to drill into the top of the spool and affix some metal rods that keep the lure line on the spool. You probably won’t need more than 5 of these spools, I can send you the CAD files, or you can iterate on my design. 

##Ramps
The ramps I've constructed are prototypes, but they've been relatively robust so far. The plywood can be staked into the ground to hold them in place. They're designed so that the lure line is fed between the two sets of posts. I'd recommend slowing the lure down when the lure case gets to the base of a ramp, and speeding up once it's aligned with the PVC tubing. I don't know how these ramps wear with time.

## Lure case
The lure case is made from 3d printed resin. The CAD file is included with these files. The case is designed to be held together with 440 hardware, and there are holes drilled in the sides of the case, one for an eye-hook that you can attach the lure line to, and the other for a bolt to hold the half tennis balls that encase the case. The half tennis balls are the best method I've found to reduce damage to the case. The large tennis ball is also the correct diameter to fit in the ramps, which has been helpful. The eye-hook can also be attached to a second tennis ball with a length of surgical tubing, in order to create a trajectory with higher entropy. 

The Pozyx tag has to be attached to the LiPo battery before being placed inside. At the moment, I put female headers over the pins on the bottom of the Pozyx board so it fits snugly in the casing. You could potentially glue the female headers to the side of the case and use that to affix the Pozyx board in place. My current approach is to ensure that the Pozyx board is in place, and then place a piece of non-conductive foam on top of the board and put the lid in place. This keeps the board snug in the case, but there are definitely improvements to be made.

Unfortunately, this design means that in order to change the battery the whole lid has to be removed (the two tennis ball halves can be turned inside-out), and the board removed. 

## Batteries
The mechanical lure comes with a lead acid battery and a charger. It was designed to run X number of tests. I've only charged it 3 or 4 times, so it should have a relatively long lifespan left. It's worth reviewing proper lead-acid battery maintenance. 

There are 4 cellphone recharging packs that can be used to power the Pozyx anchors for around an hour at peak usage. Unfortunately, temperature may have an impact in how long their charge lasts. These can be charged using normal outlets. 

The small LiPo batteries that are included are designed for use with the Pozyx tags, and one will fit in the Lure case. The smallest battery is intended to be used on the Pozyx tag on the hawk. The batteries have __ mm DC adapters soldered to them. The two USB-powered charging boards that I've included use a different plug, so I've soldered two adapter cables that connect the charging boards to the DC adapters on the batteries. It's likely that these batteries will need to be changed relatively often, so you'll probably need to buy more of these. 

One of the Pozyx tags has a DC adapter with a different diameter, so you may need to re-solder the adapters on the batteries. ]


#Entropy Code
The last thing you'll find in the digital supplement to this document is the code for calculating entropy. There's one file in particular that should provide an outline in how the rest of the code fits together, which is titled `moments/c4e/momentComputation/validation/aeplots_test.m`. I've included comments in this code that explain what the different parts do. In order to convert a trajectory that's a time-series of position data to the velocity data that is required to calculate an entropy distribution, you merely need to calculate the velocities by finite differences (i.e. in matlab, if you have positional data `pos` which is a Nx3 matrix, then velocity can be calculated as `pos(1:end-1, :) - pos(2:end, :)`. `aeplots_test.m` scales the velocity data so that the maximum entropy optimization converges easily, so you don't have to worry about doing that.

# GPS Tracker

After having to cancel vet appointments because my cats were gallivanting somewhere outside, I decided to build a GPS tracking collar. These already exist but I wanted to create a fully open sourced version. I'm an embedded software engineer so my PCB knowledge is lacking, I'm much more software orientated! The PCB has been designed on KiCad.

# Architecture

The device uses a U-BLOX SARA-N310 LTE module, U-BLOX ZOE-M8G GPS module, an embedded SIM card and an STM32L011 to control everything.

The ST uses two UART lines to communicate with the GPS device and the LTE device. The GPS device will output NMEA messages by default but both lines are connected in case there is a requirement to modify any of the default settings.

The LTE device is controlled with AT commands and will communicate with the SIM card by itself.

The LTE device will then send GPS data to an endpoint, the UI can then pick up this data and overlay it on a map.

The idea is to keep everything in a deep sleep mode as much as possible, only waking to send some data. This is to reduce power consumption as much as possible. 

I'm not decided on how often to update the GPS. Maybe it can be geo-fenced so that it will update at a low rate (once a minute?) if the device hasn't left the house. Otherwise, update every 10 seconds. This will need some discussion and testing. Power consumption will come into this decision too.

# UI

I haven't given the UI much thought or how it would even work. This is something that needs to be discussed. It should have a map overlay which will plot where the cat has been over a ime period that the user can select. It should also be able to plot the current location.

# Hardware

A big consideration for this project is power usage and footprint. The smaller and lighter the PCB, the better. A small device is less likely to get stuck in something, making the collar snap free. Also, some cats may find it annoying if it's a bit bulky. The GPS and ST have been chosen for their low current draw. 

I would like to make the whole thing waterproof is possible, or at least water resistant. Who knows what crazy situations cats get into! 

I have included lots of test points but these can be removed in the final design.

# Antennas

The GPS antenna will be a small puck antenna. The device will naturally sit under a cats neck, this is going to be a problem for GPS! I don't know if the antenna will be sensitive enough to pick up GPS data bouncing off of the ground, this is something that could be tested. It this isn't an option, perhaps the whole thing could be integrated into a collar as one unit, with the antenna running up inside the collar.

I haven't given the LTE antenna much thought, it would be really nice to have it designed into the PCB but I don't know enough about antenna design to do this. The current design is for an external antenna with a UFL connection. Hopefully there is an antenna small enough to fit!

# Software

I planned to use the HAL for the ST, the software shouldn't be that complicated and using the HAL will make it much easier to get this up and running. 

There needs to be a server somewhere to provide the front end and store some historic GPS data. This should be a full stack app that can be accessed from anywhere. The front end needs a map of the world, on to which the position can be overlaid. There should be a "live" mode which just reads an endpoint, waiting for GPS data to come in and then updating the point on the map. 

The server should also store all GPS data so the user can login and plot where the device has been over a user defined period of time. This will allow the user to see where the device has been on a given day, week or longer.


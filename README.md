Parking Garage System

Overview:

This program simulates the management of a parking garage that can accommodate different types of vehicles (Car, Bike, and Truck). 
The garage has a limited number of parking slots and a queue for waiting vehicles when the garage is full. 
Vehicles are identified by their license plates, and the system tracks the total time spent by each vehicle in the garage.

Classes:
Vehicle (Abstract Base Class)

Attributes:

licensePlate: A string representing the vehicle’s license plate.
entryTime: The time at which the vehicle enters the garage.

Methods:

getPlate(): Returns the vehicle's license plate.
getEntryTime(): Returns the entry time of the vehicle.
Car, Bike, and Truck (Derived Classes from Vehicle)
These classes inherit from the Vehicle class and represent specific vehicle types.
Each subclass calls the base class constructor to initialize licensePlate and entryTime.

ParkingSlot
Attributes:

isOccupied: A boolean flag to indicate whether the parking slot is occupied.
parkedVehicle: A pointer to the vehicle currently parked in the slot (if any).

Garage
Attributes:

slots: A vector of ParkingSlot objects that represent the parking slots in the garage.
wait: A queue that holds vehicles waiting for an available parking slot.
plates: A set that keeps track of the license plates of vehicles currently in the garage.
plates_time: A map to store the total time spent by each vehicle in the garage, keyed by their license plates.

Methods:

enterGarage(Vehicle* v): Handles the process of a vehicle entering the garage.
If a vehicle's license plate is already in the garage, it is not allowed to enter again.
If there is an available slot, the vehicle is parked there. If not, the vehicle is added to the waiting queue.
exitGarage(string plate, int exitTime): Handles the process of a vehicle exiting the garage.
If the vehicle is found in the garage, it calculates the time spent, removes the vehicle, and frees the slot.
If there are any vehicles in the waiting queue, the next vehicle is allowed to enter the garage.
showStats(): Displays the total time spent in the garage for each vehicle.

Program Flow:
The program starts by initializing a Garage object with 2 parking slots.
Three vehicles (a Car, a Bike, and a Truck) try to enter the garage.
The first two vehicles (Car and Bike) are successfully parked in the available slots.
The Truck, which arrives when both slots are filled, is added to the waiting queue.
The Car and Bike exit the garage, and the time spent by each vehicle is calculated and printed.
The total time spent by each vehicle in the garage is displayed.

Sample Output:

Parked: ABC123
Parked: XYZ456
Garage full. Added DEF789 to waiting queue.
Exited: ABC123 | Time spent: 10
Exited: XYZ456 | Time spent: 13
Plate: ABC123, Total Time: 10
Plate: XYZ456, Total Time: 13

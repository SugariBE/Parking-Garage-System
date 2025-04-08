# Parking-Garage-System

#include <iostream>
#include <vector>
#include <queue>
#include <set>
#include <map>
#include <string>

using namespace std;

class Vehicle {
protected:
    string licensePlate;
    int entryTime;

public:
    Vehicle(string plate, int time) : licensePlate(plate), entryTime(time) {}

    virtual ~Vehicle() {}

    string getPlate() const { return licensePlate; }
    int getEntryTime() const { return entryTime; }
};

class Car : public Vehicle {
public:
    Car(string plate, int time) : Vehicle(plate, time) {}
};

class Bike : public Vehicle {
public:
    Bike(string plate, int time) : Vehicle(plate, time) {}
};

class Truck : public Vehicle {
public:
    Truck(string plate, int time) : Vehicle(plate, time) {}
};

class ParkingSlot {
public:
    bool isOccupied = false;
    Vehicle* parkedVehicle = nullptr;
};

class Garage {
public:
    vector<ParkingSlot> slots;
    queue<Vehicle*> wait;
    set<string> plates;
    map<string, int> plates_time;

    Garage(int totalSlots) {
        slots.resize(totalSlots);
    }

    void enterGarage(Vehicle* v) {
        string plate = v->getPlate();
        if (plates.count(plate)) {
            cout << "Vehicle already in garage.\n";
            return;
        }

        for (auto& slot : slots) {
            if (!slot.isOccupied) {
                slot.isOccupied = true;
                slot.parkedVehicle = v;
                plates.insert(plate);
                cout << "Parked: " << plate << endl;
                return;
            }
        }

        wait.push(v);
        cout << "Garage full. Added " << plate << " to waiting queue.\n";
    }

    void exitGarage(string plate, int exitTime) {
        for (auto& slot : slots) {
            if (slot.isOccupied && slot.parkedVehicle->getPlate() == plate) {
                int timeSpent = exitTime - slot.parkedVehicle->getEntryTime();
                plates_time[plate] += timeSpent;
                plates.erase(plate);
                delete slot.parkedVehicle;
                slot.parkedVehicle = nullptr;
                slot.isOccupied = false;
                cout << "Exited: " << plate << " | Time spent: " << timeSpent << endl;

                if (!wait.empty()) {
                    Vehicle* next = wait.front();
                    wait.pop();
                    enterGarage(next);
                }
                return;
            }
        }
        cout << "Vehicle not found in garage.\n";
    }

    void showStats() {
        for (auto& [plate, time] : plates_time) {
            cout << "Plate: " << plate << ", Total Time: " << time << endl;
        }
    }
};

int main() {
    Garage g(2); // garage with 2 slots
    g.enterGarage(new Car("ABC123", 10));
    g.enterGarage(new Bike("XYZ456", 12));
    g.enterGarage(new Truck("DEF789", 15)); // goes to wait queue

    g.exitGarage("ABC123", 20);
    g.exitGarage("XYZ456", 25);

    g.showStats();
    return 0;
}

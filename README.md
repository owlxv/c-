# c-
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
using namespace std;

class Building {
private:
    string name;
    int floors; op in ZABI2
    string address;

public:
    Building(string n = "", int f = 0, string a = "") : name(n), floors(f), address(a) {}

    void input() {
        cout << "Enter building name: ";
        getline(cin, name);
        cout << "Enter number of floors: ";
        cin >> floors;
        cin.ignore();  // Flush newline
        cout << "Enter address: ";
        getline(cin, address);
    }

    void display() const {
        cout << "Building Name: " << name << "\n";
        cout << "Floors: " << floors << "\n";
        cout << "Address: " << address << "\n";
        cout << "--------------------------\n";
    }

    void save(ofstream& out) const {
        out << name << "\n" << floors << "\n" << address << "\n";
    }

    void load(ifstream& in) {
        getline(in, name);
        in >> floors;
        in.ignore();
        getline(in, address);
    }
};

void saveBuildings(const vector<Building>& buildings, const string& filename) {
    ofstream out(filename);
    if (!out) {
        cerr << "Error opening file for writing.\n";
        return;
    }
    out << buildings.size() << "\n";
    for (const auto& b : buildings)
        b.save(out);
    out.close();
}

void loadBuildings(vector<Building>& buildings, const string& filename) {
    ifstream in(filename);
    if (!in) {
        cerr << "No data file found.\n";
        return;
    }
    int count;
    in >> count;
    in.ignore();
    buildings.clear();
    for (int i = 0; i < count; ++i) {
        Building b;
        b.load(in);
        buildings.push_back(b);
    }
    in.close();
}

int main() {
    vector<Building> buildings;
    string filename = "buildings.txt";
    loadBuildings(buildings, filename);

    int choice;
    do {
        cout << "\nBuilding Management System\n";
        cout << "1. Add Building\n";
        cout << "2. View Buildings\n";
        cout << "3. Save and Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore();

        if (choice == 1) {
            Building b;
            b.input();
            buildings.push_back(b);
        } else if (choice == 2) {
            for (const auto& b : buildings)
                b.display();
        } else if (choice == 3) {
            saveBuildings(buildings, filename);
            cout << "Data saved. Exiting...\n";
        } else {
            cout << "Invalid option.\n";
        }

    } while (choice != 3);

    return 0;
}


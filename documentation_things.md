# THINGS: SENSOR TEMPERATURE DOCUMENTATION

## Flutter Project - Business B

### Table of Contents
- [Presentation](#presentation)
- [Architecture](#architecture)
- [Project Structure and Code Organization](#project-structure-and-code-organization)
- [Installation](#installation)
- [Usage](#usage)
- [Sensor Behavior](#sensor-behavior)
- [Specific Requirements](#specific-requirements)
- [Testing](#testing)
- [Licenses and Copyright](#licenses-and-copyright)
- [Contribution](#contribution)
- [Questions and Support](#questions-and-support)

## Presentation

This Flutter project enables real-time temperature and humidity monitoring, simulating an on-site sensor system with automatic and manual controls. The application supports real weather data integration, offering a valuable tool for technicians and operators working with temperature sensors.

### Technologies Used
- **Flutter** - Cross-platform development  
- **HTTP** - Communication protocol  
- **Shelf** - Server request handling  

## Architecture

This project consists of two main components:
- **Flutter Application (Client)** - Provides GUI and user interaction  
- **Server** - Receives telemetry data (temperature and humidity)  

### Data Transmission
- **Active Mode** - Sends data every 10 seconds  
- **Sleep Mode** - Sends data every 20 seconds  

WebSocket support is not available in this version.

### Project Structure
├── lib/ 
│ ├── main.dart # Entry point, loads HomeScreen 
│ ├── design.dart # UI Components and Layout elements 
│ ├── home_screen.dart # Main application logic and data handling 
│ ├── client.dart # Handles server communication

## Project Structure and Code Organization

To ensure **better maintainability, scalability, and readability**, the project code is split into multiple files rather than keeping everything inside a single file. This separation helps by **reducing complexity** and making the code easier to debug and modify.

### Why Separate the Code?  
1. **Improved Readability**  
   - Separating the UI components (`design.dart`) from logic (`home_screen.dart`) ensures the interface is clean and **does not mix with business logic**.  
   
2. **Modular Code**  
   - Each file serves a specific purpose, making it easier to **find and modify** sections without breaking the whole application.  

3. **Scalability**  
   - If additional features or pages need to be added, having separate files makes the project **easier to extend** without altering core components.  

4. **Better Debugging**  
   - When debugging, working on a well-structured project ensures **errors are isolated** rather than scattered across a single large file.  

## Installation

### Prerequisites
- Flutter SDK (≥ 3.x)
- IDE (VS Code, Android Studio...)

### Setup Instructions
```bash
git clone https://github.com/yourusername/temperature-sensor.git


## Dependencies in pubspec.yaml
'''
dependencies:
  flutter:
    sdk: flutter
  http: ^1.1.0
  shelf: ^1.4
'''

## Usage
Graphical User Interface (GUI)
Users can:

Adjust temperature and humidity manually using sliders
Switch between active and sleep mode
Send data to the server
Sensor Behavior

## Operating Modes

* Active Mode
Sensors continuously update
Real-time data transmission enabled
Weather API integration active

* Sleep Mode
Sensors stop updating
Lower resource usage
Maintains last known values

Sensor Simulation
'''
Timer.periodic(const Duration(seconds: 10), (timer) {
  _temperature = (_cityTemperature + (Random().nextBool() ? 2 : -2)).clamp(_cityTemperature - 2, _cityTemperature + 2);
});
The temperature varies only within ±2°C of actual city temperature.

'''


# Server Communication
Data Format is json. for exemple:
'''
{
  "temperature": 20.0,
  "humidity": 50.0
}
'''

## Sending Data
'''
Future<void> sendDataToServer(double temperature, double humidity) async {
  var url = Uri.parse("http://localhost:8080/data");
  var data = jsonEncode({"temperature": temperature, "humidity": humidity});
  await http.post(url, headers: {"Content-Type": "application/json"}, body: data);
}
'''
Data is transmitted via HTTP in JSON format.



# Specific Requirements
## Must Have
Send temperature data to the server
Frequency (10s / 20s based on mode)
Manual adjustment via GUI
Temperature values in Celsius
Display server-stored values

## Should Have
Future WebSocket integration

## Testing
Tests Conducted 

GUI Testing - Ensuring proper input & UI responsiveness
Data Transmission - Validating sensor data interval updates
Server Communication - Checking HTTP data exchange

## Tools Used
Flutter Testing Framework - UI & logic validation
Git/GitHub - Team collaboration

## Licenses and Copyright
This project is licensed under MIT License.
SPDX-License-Identifier: MIT  

## Contribution
1.Fork the repository
2.Create a feature branch
3.Submit well-documented Pull Requests

## Questions and Support
For inquiries or contributions, please refer to the GitHub repository or contact the development team.
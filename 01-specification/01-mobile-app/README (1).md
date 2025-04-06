# IoT Dashboard App

A Flutter application to visualize and interact with connected objects (Things), including real-time telemetry, unit management, theme switching, and historical data display.

---

## Features

### Mandatory (NMA)
- **NMA1**: Display of the list of devices (sensors).
- **NMA2**: Filtering by sensor type (Temperature, Humidity, Acceleration).
- **NMA3**: Access to a dedicated page for each sensor (`ThingDetailPage`).
- **NMA4**: Display of client attributes (read-only), integrated into the detailed pages.
- **NMA5**: Display and partial modification of server attributes.
- **NMA6**: Visualization of telemetry data (temperature working, humidity and acceleration partially integrated).
- **NMA7**: Ability to change the refresh rate.
- **NMA8**: Access to telemetry history in a dedicated page (`HistoricalPage`) with graph.
- **NMA9**: App displayed in French or English based on phone language (currently being integrated).
- **NMA10**: Light/Dark theme switching from the settings page (and from the user page for now).
- **NMA11**: "Groupe groupe" logo as app icon – to be added in assets.
- **NMA12**: Temperature displayed in °C or °F based on settings.
- **NMA13**: Temperature unit change from the settings page (saved as a server attribute – currently simulated).

### Improvements (NMA+)
- **NMA+1**: Graphical visualization of telemetry data in `HistoricalPage` (currently only temperature).

---

## File Structure

```
lib/
│
├── main.dart                # Application entry point
└── pages/
    ├── home_page.dart           # Main page with sensor list, filtering, and navigation
    ├── second_page.dart         # Settings page (theme, temperature unit)
    ├── user_page.dart           # User page (dark mode, language...)
    ├── thing_detail_page.dart   # Detailed view of a selected sensor
    ├── historicalPage.dart      # Historical telemetry and graph display
```

---

## Installation  
(*Disclaimer: the app is in the master branch because it refuses to submit pull requests to other branches*)

1. Clone the repository:
```bash
git clone https://github.com/Lidreyx/esaip-flutter-app.git
```

2. Install dependencies:
```bash
flutter pub get
```

3. Run the app:
```bash
flutter run
```

---

## Preview

- Filterable list of sensors  
- Sensor detail page with live value  
- Settings: temperature unit and theme  
- History page with a graph  

---

## Packages Used

- [`fl_chart`](https://pub.dev/packages/fl_chart) – for the graph in the history page  
- [`flutter_screenutil`](https://pub.dev/packages/flutter_screenutil) – for responsive layout  

---

## Upcoming Features

- Backend connection via WebSocket  
- Full support for client/server attributes  
- Server-side preference saving  
- Configurable data refresh rate  

# IoT-Based Fall Detection System

An IoT solution designed for automated fall detection and recovery analysis to assist elderly individuals. The system uses sensors and modules integrated with an Arduino Nano to detect falls, evaluate recovery status, and send location-based SOS alerts.

## Components
- **Arduino Nano**: Core microcontroller.
- **MPU6050**: Accelerometer and gyroscope for motion analysis.
- **GSM Sim800L**: Sends SMS alerts.
- **GPS Neo 6m**: Provides location coordinates.
- Power: 9V battery for Arduino; 3.7V batteries for GPS and GSM modules.

## Methodology
1. **Connections**:
   - MPU6050: SDA/SCL to A4/A5 on Arduino.
   - GPS: RX/TX to D3/D4 on Arduino.
   - GSM: RX/TX to D11/D10 on Arduino; VNET pin connected to antenna.
   - GND of all modules is common.
2. **Workflow**:
   - Collect motion data from MPU6050.
   - Detect falls using a 3-trigger algorithm based on acceleration and angle thresholds.
   - Use GPS to generate a Google Maps link with location data.
   - Send SMS alerts via GSM with messages: “FALL DETECTED” or “RECOVERED FROM FALL.”
   - Delay and reprocess data for recovery detection.

## Fall Detection Triggers Using MPU6050

The fall detection system processes accelerometer and gyroscope data from the MPU6050 sensor using a three-trigger mechanism to identify falls:

1. **Trigger 1: Lower Threshold Activation**
   - The accelerometer data is used to calculate the Root Mean Square (RMS) value (AM):
     \[
     AM = \sqrt{a_x^2 + a_y^2 + a_z^2}
     \]
   - If the RMS value exceeds the lower threshold of **0.4g**, Trigger 1 is activated.

2. **Trigger 2: Upper Threshold Activation**
   - If the RMS value (AM) continues to increase and exceeds the upper threshold of **3g** within a **6-second window**, Trigger 2 is activated. This deactivates Trigger 1 and transitions the system to the next stage.

3. **Trigger 3: Angle Change Detection**
   - The gyroscope data is processed to calculate the angle change:
     \[
     Angle\ Change = \sqrt{g_x^2 + g_y^2 + g_z^2}
     \]
   - If the calculated angle change surpasses a predefined threshold, Trigger 3 is activated, signaling a potential fall.

Once all three triggers are sequentially activated, the system confirms a fall, initiates recovery checks, and sends SOS messages with the detected location.

## Libraries Used
- `I2Cdev.h`: For I2C communication with the MPU6050 sensor.  
- `MPU6050.h`: For configuring and retrieving motion data from the MPU6050 sensor.  
- `TinyGPSPlus.h`: For parsing GPS data to extract latitude and longitude.  
- `SoftwareSerial.h`: For creating additional serial interfaces for GPS and GSM communication.  
- AT commands: For sending SMS messages through the GSM module.

## How to Use
1. Assemble the circuit as described in the methodology.
2. Upload the code to Arduino Nano.
3. Power the device and test by simulating fall scenarios.

## Future Enhancements
- Adaptive thresholds using machine learning.
- Voice alert for nearby assistance.
- Improved accuracy for neurological false positives.

## References
1. Lindemann, U. et al. (2005). *Evaluation of a fall detector based on accelerometers*.  
2. Al-dahan, Z. et al. (2016). *Design and Implementation of Fall Detection System Using MPU6050 Arduino*.

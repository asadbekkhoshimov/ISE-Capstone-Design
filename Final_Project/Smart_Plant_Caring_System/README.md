
# Smart Plant Caring System (Team_#6)
The Smart Plant Caring System is an advanced automation solution for indoor gardening and plant care, leveraging Raspberry Pi 3 and a variety of sensors.

## Overview
This system is designed to monitor and manage the environmental conditions essential for plant growth, such as moisture, humidity, and temperature. One of its smart features is the ability to intelligently irrigate your plants based on real-time moisture level data. Furthermore, it includes a webcam for people detection, distinguishing authorized from unauthorized individuals. All sensor data is stored in a JSON file, which can be uploaded to the cloud and accessed for further usage.

## Video:
https://youtu.be/Smel22U0VKo
## Images:
![image1](https://github.com/asadbekkhoshimov/ISE-Capstone-Design/assets/84382619/98283b64-5a0a-4ca2-ad7b-a59791420942)


![image2](https://github.com/asadbekkhoshimov/ISE-Capstone-Design/assets/84382619/7eb9b5d3-5693-432a-8098-f35a84072630)


![image3](https://github.com/asadbekkhoshimov/ISE-Capstone-Design/assets/84382619/11e5ba6b-749c-435a-b94f-fac611f4f979)

## Key Features
Smart Irrigation: The system continuously monitors soil moisture levels and activates the water pump to irrigate plants when the soil moisture level is low. This ensures that plants are watered efficiently and precisely when they need it.
Environmental Monitoring: In addition to soil moisture, the system also monitors and records the temperature and humidity in the environment.
People Detection: The webcam identifies authorized and unauthorized individuals, contributing to the security of the system.
Data Storing and Sharing: All collected data are stored in a JSON file and can be uploaded to the cloud for access from the web or other applications.

## Advantages Over Existing Systems
Intelligent Irrigation: Unlike traditional systems which irrigate on a set schedule, the Smart Plant Caring System adjusts irrigation based on real-time soil moisture levels, resulting in more efficient water usage and healthier plants.
Integrated Monitoring: The system monitors multiple factors (soil moisture, temperature, humidity), providing a comprehensive understanding of the plant's environment.
Security Feature: The inclusion of people detection adds a layer of security not commonly found in traditional plant caring systems.
Cloud Compatibility: The ability to upload data to the cloud makes it easy to monitor and analyze your plant's environment from anywhere, at any time.

## Who Can Use It?
This system is ideal for indoor garden enthusiasts, smart home hobbyists, and those looking to implement an automated plant care solution at home, in an office, or any indoor setting. It is also a great learning tool for students and researchers in the fields of IoT and automation.

## Why It Is Useful?
The Smart Plant Caring System simplifies and optimizes the process of indoor plant care. It provides real-time monitoring and smart control, relieving you from the routine of manual watering and enabling more efficient plant care. By detecting people, the system also adds an extra layer of security. The data storage and sharing capability offer valuable insights into your plant's growth pattern and environmental requirements.

## Hardware Used
Raspberry Pi 3
Webcam
Relay
Moisture, humidity, and temperature sensor
Water pump
9V power supply
Plants

## Codes
#### humidity_temp.py
``` 
import os
import json
import time
import board
import adafruit_dht

# Initialize the DHT device with data pin connected to GPIO pin 17 (change if necessary)
dhtDevice = adafruit_dht.DHT22(board.D17, use_pulseio=False)

# Create the folder to store JSON data if it doesn't exist
data_folder = "JSON_Humidity_Temperature_data"
os.makedirs(data_folder, exist_ok=True)

# File to store the sensor data
filename = f"{data_folder}/sensor_data.txt"

while True:
    try:
        # Read the humidity and temperature values from the sensor
        temperature_c = dhtDevice.temperature
        temperature_f = temperature_c * (9 / 5) + 32
        humidity = dhtDevice.humidity

        print(
            "Temp: {:.1f} F / {:.1f} C    Humidity: {}% ".format(
                temperature_f, temperature_c, humidity
            )
        )

        # Create a dictionary with the sensor data
        sensor_data = {
            "timestamp": str(time.time()),
            "temperature_c": temperature_c,
            "temperature_f": temperature_f,
            "humidity": humidity
        }
            # Append the new data to the file
        with open(filename, "a") as file:
            file.write(json.dumps(sensor_data) + "\n")

    except RuntimeError as error:
        # Errors happen fairly often, DHT sensors can be hard to read, just keep going
        print(error.args[0])
        time.sleep(2.0)
        continue

    time.sleep(2.0)
```
#### Moisture_sensor.py
``` 
# Create an empty list to store pump status data
pump_status_list = []

# Initialize the pump status
pump_status = "off"

while True:
    moisture_level = moisture_sensor.value * 100
    print(f"Moisture level: {moisture_level:.2f}%")

    if moisture_level < moisture_threshold:
        #print('Moisture level is low. Watering the plant...')
        print('Moisture level is sufficient.')
        water_pump.on()  # Turn on the water pump
        pump_status = "on"
        time.sleep(5)  # Run the water pump for 5 seconds
        water_pump.off()  # Turn off the water pump
        print('Watering complete.')
    else:
        #print('Moisture level is sufficient.')
        print('Moisture level is low. Watering the plant...')
        pump_status = "off"

    # Get the current timestamp
    current_time = str(datetime.now())

    # Create a dictionary with the pump status data
    pump_status_data = {
        "time": current_time,
        "status": pump_status
    }

    # Append the pump status data to the list
    pump_status_list.append(pump_status_data)

    # Generate a filename for the JSON file
    filename = f"{data_folder}/pump_status.json"

    # Save all pump status data to a JSON file in column and row format
    with open(filename, "w") as file:
        json.dump(pump_status_list, file, indent=4)

    time.sleep(2)
    
    ```
 



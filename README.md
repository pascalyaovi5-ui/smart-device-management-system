# smart-device-management-system
# ==============================================================================
# EL 162 / 234 OBJECT ORIENTED PROGRAMMING - MINI PROJECT
# Project Name: Smart Device Management System


class SmartDevice:
    """Parent Class representing a base Smart Device."""
    def init(self, name: str, device_id: str):
        if not device_id.strip():
            raise ValueError("Device ID cannot be empty.")
            
        self.name = name
        # Encapsulated Private Attributes
        self.__device_id = device_id
        self.__power_status = False  # False means Off, True means On

    # @property Decorators for Encapsulation (Getters and Setters)
    @property
    def device_id(self) -> str:
        """Getter for the private device ID."""
        return self.__device_id

    @property
    def power_status(self) -> str:
        """Getter converting boolean status to readable text."""
        return "ON" if self.__power_status else "OFF"

    @property
    def is_on(self) -> bool:
        """Internal helper helper to check raw power boolean."""
        return self.__power_status

    def turn_on(self):
        """Changes power status to True."""
        self.__power_status = True
        print(f"[{self.name}] has been powered ON.")

    def turn_off(self):
        """Changes power status to False."""
        self.__power_status = False
        print(f"[{self.name}] has been powered OFF.")

    def display_info(self):
        """Displays common device information."""
        print(f"Device Name  : {self.name}")
        print(f"Device ID    : {self.device_id}")
        print(f"Power Status : {self.power_status}")


class TemperatureSensor(SmartDevice):
    """Child Class representing a Temperature Sensor."""
    def init(self, name: str, device_id: str, initial_temp: float = 24.5):
        # Initializing inherited parent attributes using super()
        super().init(name, device_id)
        self.temperature = initial_temp

    def read_temperature(self):
        """Reads current ambient temperature if device is powered on."""
        if self.is_on:
            print(f"[{self.name}] Current Temperature: {self.temperature}°C")
        else:
            print(f"Operation failed. [{self.name}] is currently powered OFF.")


class SmartLight(SmartDevice):
    """Child Class representing a Smart Light."""
    def init(self, name: str, device_id: str, brightness: int = 50):
        super().init(name, device_id)
        # Private attribute validation rule: must be 0-100
        self.__brightness = max(0, min(brightness, 100))

    @property
    def brightness(self) -> int:
        """Getter for light brightness."""
        return self.__brightness

    @brightness.setter
    def brightness(self, value: int):
        """Setter to safely adjust brightness between 0 and 100."""
        if 0 <= value <= 100:
            self.__brightness = value
            print(f"[{self.name}] Brightness updated to {self.__brightness}%.")
        else:
            print("Error: Brightness level must be between 0 and 100.")

    def increase_brightness(self, amount: int = 10):
        """Increases brightness safely by a given step."""
        if self.is_on:
            self.brightness = self.__brightness + amount
        else:
            print(f"Operation failed. [{self.name}] is currently powered OFF.")

    def decrease_brightness(self, amount: int = 10):
        """Decreases brightness safely by a given step."""
        if self.is_on:
            self.brightness = self.__brightness - amount
           class SecurityCamera(SmartDevice):
    """Child Class representing a Security Camera."""
    def init(self, name: str, device_id: str):
        super().init(name, device_id)
        self.__recording_status = False

    @property
    def recording_status(self) -> str:
        """Getter for internal recording status conversion."""
        return "RECORDING..." if self.__recording_status else "STANDBY"

    def start_recording(self):
        """Starts stream capture if machine is active."""
        if self.is_on:
            self.__recording_status = True
            print(f"[{self.name}] Stream capture initialized. {self.recording_status}")
        else:
            print(f"Operation failed. [{self.name}] is currently powered OFF.")

    def stop_recording(self):
        """Stops live capture stream."""
        if self.__recording_status:
            self.__recording_status = False
            print(f"[{self.name}] Stream capture stopped safely.")
        else:
            print(f"[{self.name}] is not currently recording.")

    def display_info(self):
        """Overrides display_info to show recording profile updates."""
        super().display_info()
        print(f"Rec Status   : {self.recording_status}")


# ==============================================================================
# MENU-DRIVEN INTERFACE
# ==============================================================================
def main():

    living_room_temp = TemperatureSensor("Living Room Sensor", "TS-101", 26.0)
    kitchen_light = SmartLight("Kitchen Accent Light", "SL-202", 70)
    front_door_cam = SecurityCamera("Front Door Camera", "SC-303")

    devices = {
        "1": living_room_temp,
        "2": kitchen_light,
        "3": front_door_cam
    }

    while True:
        print("\n" + "="*45)
        print("     SMART DEVICE MANAGEMENT SYSTEM MENU     ")
        print("="*45)
        print("1. Display Device Information")
        print("2. Turn Device On")
        print("3. Turn Device Off")
        print("4. Read Temperature (Sensor Only)")
        print("5. Adjust Brightness (Smart Light Only)")
        print("6. Start Recording (Camera Only)")
        print("7. Exit")
        print("="*45)
        
        choice = input("Select an option (1-7): ").strip()

        if choice == "7":
            print("\nExiting System. Goodbye!")
            break

        if choice in ["1", "2", "3"]:
            print("\nAvailable Devices:")
            print("1. Temperature Sensor")
            print("2. Smart Light")
            print("3. Security Camera")
            dev_choice = input("Select Target Device (1-3): ").strip()
            
            device = devices.get(dev_choice)
            if not device:
                print("Invalid device selected.")
                continue

            if choice == "1":
                print("\n--- Device Status Details ---")
                device.display_info()
            elif choice == "2":
                device.turn_on()
            elif choice == "3":
                # Clean exit wrapper for recording state
                if isinstance(device, SecurityCamera):
                    device.stop_recording()
                device.turn_off()

        elif choice == "4":
            living_room_temp.read_temperature()

        elif choice == "5":
            if not kitchen_light.is_on:
                print(f"Operation failed. [{kitchen_light.name}] is powered OFF.")
                continue
            print(f"\nCurrent brightness is {kitchen_light.brightness}%.")
            action = input("Type '+' to increase or '-' to decrease: ").strip()
            if action == "+":
                kitchen_light.increase_brightness()
            elif action == "-":
                kitchen_light.decrease_brightness()
            else:
                print("Invalid modifier action syntax.") 
        else:
            print(f"Operation failed. [{self.name}] is currently powered OFF.")

    def display_info(self):
        """Overrides display_info to include brightness."""
        super().display_info()
        print(f"Brightness   : {self.brightness}%")
        elif choice == "6":
            action = input("Type 'start' to run recording or 'stop' to halt: ").strip().lower()
            if action == "start":
                front_door_cam.start_recording()
            elif action == "stop":
                front_door_cam.stop_recording()
            else:
                print("Invalid action input sequence choice.")
        else:
            print("Invalid global entry option. Please enter a valid number (1-7).")

if name == "main":
    main()
   ## How to Execute the Application

Ensure you have Python 3 installed on your workstation.
 

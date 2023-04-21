from ctypes import *


class UPLEDController:
    def __init__(self, dllPath="C:\Program Files\IVI Foundation\VISA\Win64\Bin\TLUP_64.dll"):
        self.lib = cdll.LoadLibrary(dllPath)
        self.device_count = c_uint32()
        self.upHandle = c_int(0)
        self.model_name = None

    def find_devices(self):
        self.lib.TLUP_findRsrc(0, byref(self.device_count))
        if self.device_count.value > 0:
            print("Number of upSeries devices found: " + str(self.device_count.value))
            return self.device_count.value
        else:
            print("No upSeries devices found.")
            return None

    def get_device_info(self, index):
        modelName = create_string_buffer(256)
        serialNumber = create_string_buffer(256)
        self.lib.TLUP_getRsrcInfo(0, index, modelName, serialNumber, 0, 0)
        return (modelName.value).decode(), (serialNumber.value).decode()

    def connect_device(self, index):
        upName = create_string_buffer(256)
        self.lib.TLUP_getRsrcName(0, index, upName)
        res = self.lib.TLUP_init(upName.value, 0, 0, byref(self.upHandle))
        self.model_name, _ = self.get_device_info(index)
        return res

    def get_led_info(self):
        LEDName = create_string_buffer(256)
        LEDSerialNumber = create_string_buffer(256)
        LEDCurrentLimit = c_double()
        LEDForwardVoltage = c_double()
        LEDWavelength = c_double()
        self.lib.TLUP_getLedInfo(self.upHandle, LEDName, LEDSerialNumber, byref(LEDCurrentLimit),
                                 byref(LEDForwardVoltage), byref(LEDWavelength))
        return LEDName.value.decode(), LEDSerialNumber.value.decode(), LEDCurrentLimit.value, LEDForwardVoltage.value, LEDWavelength.value

    def get_op_mode(self):
        operatingModeFlag = c_uint32()
        operatingModeDescription = create_string_buffer(256)
        self.lib.TLUP_getOpMode(self.upHandle, byref(operatingModeFlag), operatingModeDescription)
        return operatingModeFlag.value, operatingModeDescription.value.decode()

    def get_extended_op_modes(self):
        TLUP_OPERATION_MODE_ARRAY_LENGTH = 32  # Assuming the array length is 32, adjust if needed
        extendedOperationModes = (c_uint8 * TLUP_OPERATION_MODE_ARRAY_LENGTH)()
        self.lib.TLUP_getExtendedOperationModes(self.upHandle, extendedOperationModes)
        return list(extendedOperationModes)

    def is_upled(self):
        return self.model_name == "upLED"

    def measure_led_current(self):
        led_current = c_double()
        self.lib.TLUP_measureLedCurrent(self.upHandle, byref(led_current))
        return led_current.value

    def set_led_output_state(self, enableLEDOutput):
        status = self.lib.TLUP_switchLedOutput(self.upHandle, enableLEDOutput)
        return status

    def get_led_output_state(self):
        LEDOutputState = c_bool()
        status = self.lib.TLUP_getLedOutputState(self.upHandle, byref(LEDOutputState))
        return LEDOutputState.value

    def set_led_current_limit_user(self, LEDUserCurrentLimit):
        status = self.lib.TLUP_setLedCurrentLimitUser(self.upHandle, c_double(LEDUserCurrentLimit))
        return status

    def get_led_current_limit_user(self, attribute):
        LEDUserCurrentLimit = c_double()
        status = self.lib.TLUP_getLedCurrentLimitUser(self.upHandle, c_int16(attribute), byref(LEDUserCurrentLimit))
        return LEDUserCurrentLimit.value

    def set_led_current_setpoint_startup(self, LEDCurrentSetpointStartup):
        status = self.lib.TLUP_setLedCurrentSetpointStartup(self.upHandle, c_double(LEDCurrentSetpointStartup))
        return status

    def get_led_current_setpoint_startup(self, attribute):
        LEDCurrentSetpointStartup = c_double()
        status = self.lib.TLUP_getLedCurrentSetpointStartup(self.upHandle, c_int16(attribute),
                                                            byref(LEDCurrentSetpointStartup))
        return LEDCurrentSetpointStartup.value

    def set_led_current_setpoint(self, LEDCurrentSetpoint):
        status = self.lib.TLUP_setLedCurrentSetpoint(self.upHandle, c_double(LEDCurrentSetpoint))
        return status

    def get_led_current_setpoint(self, attribute):
        LEDCurrentSetpoint = c_double()
        status = self.lib.TLUP_getLedCurrentSetpoint(self.upHandle, c_int16(attribute), byref(LEDCurrentSetpoint))
        return LEDCurrentSetpoint.value

    def set_led_current_setpoint_source(self, LEDCurrentSetSource):
        status = self.lib.TLUP_setLedCurrentSetpointSource(self.upHandle, c_uint16(LEDCurrentSetSource))
        return status

    def get_led_current_setpoint_source(self):
        LEDCurrentSetSource = c_uint16()
        status = self.lib.TLUP_getLedCurrentSetpointSource(self.upHandle, byref(LEDCurrentSetSource))
        return LEDCurrentSetSource.value

    def set_led_switch_on_at_startup(self, LEDSwitchOnAtStartup):
        status = self.lib.TLUP_setLedSwitchOnAtStartup(self.upHandle, c_bool(LEDSwitchOnAtStartup))
        return status

    def get_led_switch_on_at_startup(self):
        LEDSwitchOnAtStartup = c_bool()
        status = self.lib.TLUP_getLedSwitchOnAtStartup(self.upHandle, byref(LEDSwitchOnAtStartup))
        return LEDSwitchOnAtStartup.value

    def set_led_switch_off_at_disconnect(self, LEDSwitchOffAtDisconnect):
        status = self.lib.TLUP_setLedSwitchOffAtDisconnect(self.upHandle, c_bool(LEDSwitchOffAtDisconnect))
        return status

    def get_led_switch_off_at_disconnect(self):
        LEDSwitchOffAtDisconnect = c_bool()
        status = self.lib.TLUP_getLedSwitchOffAtDisconnect(self.upHandle, byref(LEDSwitchOffAtDisconnect))
        return LEDSwitchOffAtDisconnect.value


upled_controller = UPLEDController()

if upled_controller.find_devices():
    upled_controller.connect_device(0)
    # upled_controller.set_led_switch_off_at_disconnect(1)
    # upled_controller.set_led_switch_on_at_startup(0)
    # upled_controller.set_led_current_limit_user(0.8)
    # upled_controller.set_led_current_setpoint_startup(0)

    print(upled_controller.get_led_switch_on_at_startup())
    print(upled_controller.get_led_switch_off_at_disconnect())

    print(upled_controller.get_led_output_state())
    upled_controller.set_led_output_state(0)
    led_current = upled_controller.measure_led_current()
    print("LED current:", led_current, "A")



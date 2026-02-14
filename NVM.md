# Non-volatile memory

OpenRTX needs persistent storage to store various data such as the radio settings or current VFO parameters.
This is implemented in the NVM subsystem.

The NVM subsystem is composed of different layers.

## Platform layer

The top-most layer is the platform layer (OpenRTX/platform/drivers/NVM/nvmem_*target*.c).
This file defines the non-volatile memory devices and layout used for the device. It also implements the functions to store and read the VFO state and radio settings.

The main entry point is a list of nvmDescriptors. 
Each nvmDescriptor contains a name, a pointer to an nvmDevice, a base address used within the nvmDevice, a size of the device, a number of partitions and a list of partitions.

It also implements the following functions defined in interfaces/nvmem.h
 * the nvm_init, nvm_terminate that initialize or terminate the nvm subsystem. It initializes all the NVM devices used by the platform.
 * the nvm_getDesc that returns an nvmDescriptor given a device number
 * the nvm_readCalib that populates a given buffer with the cailbration data
 * the nvm_readHwInfo that populates a hwInfo_t structure using data stored in nvm devices
 * int nvm_readVFOChannelData that reads the last saved VFO state
 * nvm_readSettings/nvm_writeSettings that reads/writes the device settings (callsign, accessibility settings, ...)
 * nvm_writeSettingsAndVfo that writes the current vfo states as well as the device settings

These functions use the functions nvm_read/nvm_write/nvm_erase defined in nvmem_access.c/.h

## NVM Device layer

This layer implements the driver for a particular non-volatile memory device.

An NVM device is represented by an nvmDevice structure. This structure contains pointers to an information structure and an ops structure. It also contains a priv pointer that is differently by each driver.

### ops structure

The ops structure is defined in interfaces/nvmem.h as nvmOps and contains pointers to the function implemented by the driver for read/write/erase/sync. If a driver does not implement some of these functions, the pointer should be set to NULL.

### info structure

The info structure is defined in interfaces/nvmem.h as nvmInfo and contains:
 * write_size: the alignment size of a write operation, meaning a write operation should be an integer multiple of write_size
 * erase_size: the alignment size of an erase operation, meaning an erase operation should be an integer multiple of erase_size. For example, in the case of flash memory, this is the sector/page size.
 * erase_cycles: the typical number of write/erase cycles the memory supports. This can be used by higher level drivers to adapt the behavior when using low-endurance memory
 * device_info: OR'd flags defining standardized device parameters such as the type of memory (EEPROM/Flash/Emulated-EEPROM/File) and some capabilities or requirements (read-only or read-write, requirement to erase the memory before writing again, ...)

## Low-level driver

This layer implements the low-level drivers required to use the memory device. Those drivers are often not exclusive to the NVM sybsystem. This can be an SPI driver, I2C driver, ... . Those should be initialized in the init function of the memory device.
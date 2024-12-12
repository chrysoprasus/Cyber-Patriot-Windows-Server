# Packet Tacer

## Modes
As a security feature, the Cisco IOS software separates management access into the following two command modes:

  - **User EXEC Mode** - This mode has limited capabilities but is useful for basic operations. It allows only a limited number of basic monitoring commands but does not allow the execution of any commands that might change the configuration of the device. The user EXEC mode is identified by the CLI prompt that ends with the > symbol.
  - **Privileged EXEC Mode** - To execute configuration commands, a network administrator must access privileged EXEC mode. Higher configuration modes, like global configuration mode, can only be reached from privileged EXEC mode. The privileged EXEC mode can be identified by the prompt ending with the # symbol.
![image](https://github.com/chrysoprasus/Cyber-Patriot-Windows-Server/blob/main/images/modes.JPG)
  - To enter Privileged EXEC Mode: type enable while in EXEC mode:
    ```
    Switch>enable
    Switch# //now in Privileged EXEC Mode
    ``` 

To configure the device, **the user must enter global configuration mode**, which is commonly called global config mode.
1. **Global Configuration Mode**:
  - To enter Global config mode:
      ```
      Switch#configure terminal
      Switch(config)# // should look like this
      ```
  - Used to configure the device as a whole.
  - Prompt format: `DeviceName(config)#` (e.g., `Switch(config)#`).
  - Acts as the entry point for other specific configuration modes.
2. **Subconfiguration Modes (accessed from global config mode)**:
  - Line Configuration Mode
    - Configures console, SSH, Telnet, or AUX access.
    - To enter line config mode:
        ```
        Switch(config)#line console 0
        Switch(config-line)# // result
        ```
    - Prompt format: `DeviceName(config-line)#` (e.g., `Switch(config-line)#`).
  - Interface Configuration Mode:
    - Configures a switch port or router network interface.
    - To enter interface config mode:
        ```
        Switch(config)#interface <specific interface type and number>
        Switch(config-if)# // result
        ```
        - Examples:
          - For a specific Ethernet port (e.g., FastEthernet 0/1):
            `
            Switch(config)#interface FastEthernet0/1
            `
          - For a Gigabit Ethernet port (e.g., GigabitEthernet 0/1):
            `Switch(config)#interface GigabitEthernet0/1`
          - For VLAN interfaces (e.g., VLAN 1):
            `Switch(config)#interface vlan 1`
          - For a range of interfaces (e.g., FastEthernet 0/1 to 0/4):
            `Switch(config)#interface range FastEthernet0/1 - 0/4`
    - Prompt format: `DeviceName(config-if)# `(e.g., `Switch(config-if)#`).
3. **Prompt Identification:**
  - The command-line prompt indicates the current configuration mode.
  - Prompts begin with the device name, followed by a unique identifier for the mode.

PCF8574T I2c LCD Library.

# How To Use

1. Defines I2C write function and delay function.
2. Call PCF8574T_Init by specifying the above function as an argument.
3. Put string




# Reference
## PCF8574T_Init

initialize LCD.

```cpp
void PCF8574T_Init(uint8_t i2c_addr, Delay_Func delay_func, I2C_Write_Func i2c_func);
```

|param|description|
|-|-|
|i2c_addr|PCF8574T address. normally 0x27|
|delay_func|delay function ptr|
|i2c_func|i2c write function ptr|

```cpp
// I2c Write Function
typedef void (*I2C_Write_Func)(uint8_t addr, uint8_t *send_data, uint8_t send_data_len);
// Delay Function
typedef void (*Delay_Func)(uint16_t delay_time_ms);
```

## PCF8574T_displayString

put string to display.  
The string must be ended by \0 (null).

```cpp
void PCF8574T_displayString(char *string, uint8_t line);
```

|param|description|
|-|-|
|string|string to display|
|line|line number (1 or 2)|

## PCF8574T_sendCommand8bit

send custom command.

```cpp
void PCF8574T_sendCommand8bit(uint8_t command, bool is_control_data);
```

|param|description|
|-|-|
|command|command to send|
|is_control_data|true:command is control data. / false: comman is diaplay data|




# Example (stm32 HAL)

```cpp
#include "PCF8574T_LCD_Driver.h"

void stm_i2c_write(uint8_t addr, uint8_t *send_data, uint8_t send_data_len)
{
    HAL_I2C_Master_Transmit(&hi2c1, addr<<1, send_data, send_data_len, 1000);
}
void stm_delay(uint16_t delay_ms)
{
    HAL_Delay(delay_ms);
}

int main(void)
{
    PCF8574T_Init(0x27, stm_delay, stm_i2c_write);
    char outString[] = "sample string";
    PCF8574T_displayString(outString, 1);
}

```

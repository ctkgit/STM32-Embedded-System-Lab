# STM32-Embedded-System-Lab
This repository was created because the course sucks.

## Getting Started

You have to download STM32CubeIDE First. Then, select board STM32F4DISCOVERY and select debugger as J-LINK (OR ST-Link).

[See Getting Started in Lab1](https://github.com/tongplw/STM32-Embedded-System-Lab/blob/master/Lab1%20Basic%20MCU%20development%20on%20STM32/Lab1-Main.pdf)

## TIM timer characteristics

Each TIM uses different clock source frequency according to the table provided below.

| APB1 Domain | APB2 domain |
| ------------- | ------------- |
| TIM2  | TIM1 	|
| TIM3  | TIM8 	|
| TIM4  | TIM9 	|
| TIM5  | TIM10 |
| TIM6  | TIM11 |
| TIM7  |	|
| TIM12 |	|
| TIM13 |	|
| TIM14 |	|

## GPIOx PinCode

### LED Light Pin

| LED | GPIOD_PD |
| ------------- | ------------- |
| Green (LD4)  |  PD12 	|
| Orange (LD3)  |  PD13	|
| Red (LD5)  | 	PD14 	|
| Blue (LD6)  | PD15 |

### User Pin

| Button | GPIOA_PA |
| ------------- | ------------- |
| User button (blue button)  |  PA0 |


### Useful function in HAL library for GPIOs

x in GPIOx for A, B, C, D eg. "GPIOA", "GPIOD"

```C
// Read pin
// HAL_GPIO_ReadPin(GPIOx, GPIO_PIN_xx);
HAL_GPIO_ReadPin(GPIOD, GPIO_PIN_12); // read from PD12
HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0); // read from user button

// Write pin
// HAL_GPIO_WritePin(GPIOx, GPIO_PIN_xx, GPIO_PIN_SET/GPIO_PIN_RESET);
HAL_GPIO_WritePin(GPIOD, GPIO_PIN_12, GPIO_PIN_SET); //turn on light

// Toggle on/off pin
// HAL_GPIO_TogglePin(GPIOx, GPIO_PIN_xx);
HAL_GPIO_TogglePin(GPIOD, GPIO_PIN_12);	// toggle pin 12
```

## Interrupt

### Button interrupt
```c
// External interrupt/event controller (EXTI)
HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pinx);
```

## Useful Tips & Tricks


### UART (Universal Asynchronous Receiver and Transmitter)

#### Set up your ioc

* Connectivity >> USART2 >> Mode (Asynchronous)
* Connect the wire from RX (UART) to TX (Board) and from TX (UART) to RX (Board)
* Connect the ET-MINI USE-TTL Board to Computer
* Open terminal (e.g. Tera Term)
* [Download Tera Term](https://osdn.net/projects/ttssh2/releases/)
* ***Very fricking important:*** set serial port to **115200** in Tera Term

#### Useful statement to use with UART:

**Receive and print char one by one to terminal:**
```c
// Private variable
UART_HandleTypeDef huart2;	// program generated

// User variable
char buffer[1];

// User code
while(1){

	if (HAL_UART_Receive(&huart2, buffer, sizeof(buffer), HAL_MAX_DELAY) == HAL_OK){ 					
		HAL_UART_Transmit(&huart2, buffer, sizeof(buffer), HAL_MAX_DELAY);
	}
}
```


**Print whole string at terminal:**

```C
// Private variable
UART_HandleTypeDef huart2;	// program generated

// User variable
int myValue;
char buffer[20];

while(1){
	
	// sprintf(char *, char format, arguments...)
	sprintf(buffer, "%d \r\n", myValue);	

	HAL_UART_Transmit(&huart2, &buffer, strlen(buffer), HAL_MAX_DELAY); // Transmit
}
```

### PWM (Pulse Width Modulation)

* Set Clock Source > Internal Clock
* Set Channel > PWM Generation CHX

```C
// Private variable
TIM_HandleTypeDef htim4; // program generated


HAL_TIM_PWM_Start(&htim4, TIM_CHANNEL_3);
while (1) {
	TIM4->CCR3 = myPulse;
}
```

### ADC (Analog to Digital Converter)

* Select your prefered Pin (e.g. PC3, PC2) and set mode to ADCxIN_x (e.g. ADC1_IN13)
* Go to Analog >> ADCx >> check INx



```C
// Private variable
ADC_HandleTypeDef hadc1; // program generated

int adcValue;

while (1) {
	HAL_ADC_Start(&hadc1);
	if (HAL_ADC_PollForConversion(&hadc1, 1000000) == HAL_OK) {
		adcValue = HAL_ADC_GetValue(&hadc1);
    	}
}
```

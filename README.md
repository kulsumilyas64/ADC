## ADC- Code for Potentiometer

```
#include"mains.h"
#include<string.h>         // For string libraries
#include<stdio.h>         // For I/O 

/* Created By STM 
ADC_HandleTypeDef hadc1;
UART_HandleTypeDef huart1;
*/

int main(void)
{
uint16_t raw;              // A var created to store 12 bit ADC Reading
char msg[10];             //  10 Character Buffer created to store the data to be transmitted over uart 

while(1)
{
/* Set GPIO Pin High to start the timing*/
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_SET);

/*Get ADC Value*/
HAL_ADC_Start(&hadc1);                               //Start the ADC Conversion by passing the addr of ADC Handle
HAL_ADC_PollConversion(&hadc1, HAL_MAX_DELAY);      // It pauses the processor so that conversion can be completed
raw = HAL_ADC_GetValue(&hadc1);                    // Stores the output in the var

/*Set GPIO Pin Low to stop the timing*/
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_RESET);

/*Convert the output stored in raw var to string and print*/
sprintf(msg, "%%h0%d\r\n", raw);                                            //%% is used to print everything and %d is used for the integers to print
HAL_UART_Transmit(&huart1, (uint8_t *)msg, strlen(msg), HAL_MAX_DELAY);    // Transmission through UART 
HAL_Delay(500)                                                            // Prints after every half second

}

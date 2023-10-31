# EXPERIMENT 05 INTERFACING A 4X4 MATRIX KEYPAD AND DISPLAY THE OUTPUT ON LCD

## Aim: 
To Interface a 4X4 matrix keypad and show the output on 16X2 LCD display to ARM controller , and simulate it in Proteus
## Components required: 
STM32 CUBE IDE, Proteus 8 simulator .
## Theory:

<img src="https://github.com/vasanthkumarch/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/36288975/2a4a795e-1674-4329-ae07-3f5e8d5073e2" width=450 height=450>

4×4 Keypad Module Pin Diagram
 
4x4 Keypad module Pin Diagram
4×4 Keypad module Pin Diagram
Pin Number	Pin Name	Description
1	R1	Taken out from 1st  ROW
2	R2	Taken out from 2nd  ROW
3	R3	Taken out from 3rd  ROW
4	R4	Taken out from 4th  ROW
5	C1	Taken out from 1st  COLUMN
6	C2	Taken out from 2nd COLUMN
7	C3	Taken out from 3rd  COLUMN
8	C4	Taken out from 4th  COLUMN
4×4 Matrix Keypad Module Hardware Overview
These Keypad modules are made of thin, flexible membrane material. The 4 x4 keypad module consists of 16 keys, these Keys are organized in a matrix of rows and columns. All these switches are connected to each other with a conductive trace. Normally there is no connection between rows and columns. When we will press a key, then a row and a column make contact.

 ## LCD 16X2 
   16×2 LCD is named so because; it has 16 Columns and 2 Rows. There are a lot of combinations available like,
   8×1, 8×2, 10×2, 16×1, etc. But the most used one is the 16*2 LCD, hence we are using it here.

All the above mentioned LCD display will have 16 Pins and the programming approach is also the same and hence the choice is left to you. 
Below is the Pinout and Pin Description of 16x2 LCD Module:

<img src="https://user-images.githubusercontent.com/36288975/233858086-7b1a88a2-f941-475c-86c2-b3bae68bdf7e.png" width=450 height=450>
<img src="https://user-images.githubusercontent.com/36288975/233857710-541ac1c2-786c-4dfc-b7b5-e3a4868a9cb6.png" width=450 height=450>
<img src="https://user-images.githubusercontent.com/36288975/233857733-05df5dbf-1a1e-479e-85bb-8014a39ad878.png" width=450 height=450>

4-bit and 8-bit Mode of LCD:

The LCD can work in two different modes, namely the 4-bit mode and the 8-bit mode. In 4 bit mode we send the data nibble by nibble, first upper nibble and then lower nibble. For those of you who don’t know what a nibble is: a nibble is a group of four bits, so the lower four bits (D0-D3) of a byte form the lower nibble while the upper four bits (D4-D7) of a byte form the higher nibble. This enables us to send 8 bit data.

Whereas in 8 bit mode we can send the 8-bit data directly in one stroke since we use all the 8 data lines.

 8-bit mode is faster and flawless than 4-bit mode. But the major drawback is that it needs 8 data lines connected to the microcontroller. This will make us run out of I/O pins on our MCU, so 4-bit mode is widely used. No control pins are used to set these modes. 
 LCD Commands:

There are some preset commands instructions in LCD, which we need to send to LCD through some microcontroller. Some important command instructions are given below:

Hex Code

Clear display screen - 01

Return home - 02

Decrement cursor (shift cursor to left) - 04

Increment cursor (shift cursor to right) - 06

Shift display right - 05

Shift display left -07

Display ON, cursor blinking - 0E

Force cursor to beginning of first line - 80

Force cursor to beginning of second line - C0

2 lines and 5×7 matrix - 38

Cursor line 1 position 3 - 0,2

Display OFF, cursor OFF - 08

Jump to second line, position 1 - C1

Display ON, cursor OFF - 0C

Jump to second line, position 2 - C2
 
## Procedure:

1.Select a new STM32 Project.

2.Select GPIO Ports
```
  PC0 , PC1 , PC2 , PC3 , PA0 , PA1 , PA2 , PA3 , PB0 , PB1  -> Output

  PC4 , PC5 , PC7 , PC8  -> Input
```
3.Configure the Input Ports at Pull up Mode followed by generating the code.

4.Build Debug and Create 'hex.file'.

5.Open a new Proteus Project.

6.Select STM32F401RB, LCD 16*2 and Keypad.

7.Connect PA0 to D7 , PA1 to D6 , PA2 to D5 , PA3 to D4 , PB0 to RS , PB1 to E , PC0 to r1 , PC1 to r2 , PC2 to r3 , PC3 to r4 , PC4 to c1 , PC5 to c2 , PC6 to c3 and PC7 to c4.

8.Check the execution of the output using Run Option.

## STM 32 CUBE PROGRAM :

```
#include "main.h"
#include <stdbool.h>
#include "lcd.h"

bool col1,col2,col3,col4;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
void key();

void key()
{
	Lcd_PortType ports[] = {GPIOA,GPIOA,GPIOA,GPIOA};
	Lcd_PinType pins[] ={GPIO_PIN_3,GPIO_PIN_2,GPIO_PIN_1,GPIO_PIN_0};
	Lcd_HandleTypeDef lcd;
	lcd = Lcd_create(ports,pins,GPIOB,GPIO_PIN_0,GPIOB,GPIO_PIN_1,LCD_4_BIT_MODE);

	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_RESET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_SET);

	col1 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
	col2 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
	col3 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
	col4 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);
	if(!col1)
	{
		Lcd_cursor(&lcd,0,1);
		Lcd_string(&lcd,"key 7\n");
		HAL_Delay(500);
	}
	else if(!col2)
	{
		Lcd_cursor(&lcd,0,1);
		Lcd_string(&lcd,"key 8\n");
		HAL_Delay(500);
	}
	else if(!col3)
	{
		Lcd_cursor(&lcd,0,1);
		Lcd_string(&lcd,"key 9\n");
		HAL_Delay(500);
	}
	else if(!col4)
	{
		Lcd_cursor(&lcd,0,1);
		Lcd_string(&lcd,"key %\n");
		HAL_Delay(500);
	}


		HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_SET);
		HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_RESET);
		HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_SET);
		HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_SET);

		col1 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
		col2 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
		col3 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
		col4 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);
		if(!col1)
		{
			Lcd_cursor(&lcd,0,1);
			Lcd_string(&lcd,"key 4\n");
			HAL_Delay(500);
		}
		else if(!col2)
		{
			Lcd_cursor(&lcd,0,1);
			Lcd_string(&lcd,"key 5\n");
			HAL_Delay(500);
		}
		else if(!col3)
		{
			Lcd_cursor(&lcd,0,1);
			Lcd_string(&lcd,"key 6\n");
			HAL_Delay(500);
		}
		else if(!col4)
		{
			Lcd_cursor(&lcd,0,1);
			Lcd_string(&lcd,"key *\n");
			HAL_Delay(500);
		}


			HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_SET);
			HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_SET);
			HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_RESET);
			HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_SET);

			col1 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
			col2 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
			col3 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
			col4 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);
			if(!col1)
			{
				Lcd_cursor(&lcd,0,1);
				Lcd_string(&lcd,"key 1\n");
				HAL_Delay(500);
			}
			else if(!col2)
			{
				Lcd_cursor(&lcd,0,1);
				Lcd_string(&lcd,"key 2\n");
				HAL_Delay(500);
			}
			else if(!col3)
			{
				Lcd_cursor(&lcd,0,1);
				Lcd_string(&lcd,"key 3\n");
				HAL_Delay(500);
			}
			else if(!col4)
			{
				Lcd_cursor(&lcd,0,1);
				Lcd_string(&lcd,"key -\n");
				HAL_Delay(500);
			}


				HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_SET);
				HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_SET);
				HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_SET);
				HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_RESET);

				col1 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
				col2 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
				col3 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
				col4 = HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);
				if(!col1)
				{
					Lcd_cursor(&lcd,0,1);
					Lcd_string(&lcd,"key ON/C\n");
					HAL_Delay(500);
				}
				else if(!col2)
				{
					Lcd_cursor(&lcd,0,1);
					Lcd_string(&lcd,"key 0\n");
					HAL_Delay(500);
				}
				else if(!col3)
				{
					Lcd_cursor(&lcd,0,1);
					Lcd_string(&lcd,"key =\n");
					HAL_Delay(500);
				}
				else if(!col4)
				{
					Lcd_cursor(&lcd,0,1);
					Lcd_string(&lcd,"key +\n");
					HAL_Delay(500);
				}
}

int main(void)
{

  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();

  while (1)
  {
      key();
  }

}


void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};


  __HAL_RCC_PWR_CLK_ENABLE();
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE2);

  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }


  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}


static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};

  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();


  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0|GPIO_PIN_1, GPIO_PIN_RESET);

  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);

  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1|GPIO_PIN_2|GPIO_PIN_3;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

  GPIO_InitStruct.Pin = GPIO_PIN_4|GPIO_PIN_5|GPIO_PIN_6|GPIO_PIN_7;
  GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
  GPIO_InitStruct.Pull = GPIO_PULLUP;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);

  GPIO_InitStruct.Pin = GPIO_PIN_0|GPIO_PIN_1;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOB, &GPIO_InitStruct);

}


void Error_Handler(void)
{

  __disable_irq();
  while (1)
  {
  }

}

#ifdef  USE_FULL_ASSERT

void assert_failed(uint8_t *file, uint32_t line)
{

}
#endif


```

## Output screen shots of proteus  :<br>
## BUTTON ON <br>
 <img src="https://github.com/VIJAYKUMAR22007124/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/119657657/358c707c-af31-4233-a3fa-38a6f88680ca" width =450 height=450><br>
## BUTTON OFF
 <img src="https://github.com/VIJAYKUMAR22007124/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/119657657/f12e9f22-e643-4dce-80d2-b3ab36da2d37" width =450 height=450><br>
## CIRCUIT DIAGRAM (EXPORT THE GRAPHICS TO PDF AND ADD THE SCREEN SHOT HERE): 
 <img src="https://github.com/VIJAYKUMAR22007124/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/119657657/a2fb11ab-ad52-401e-b221-bb9cd894a229" width=450 height=450><br>
 
## Result :<br>
Interfacing a 4x4 keypad with ARM microcontroller are simulated in proteus and the results are verified.

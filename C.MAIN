/****************************************************************************/
/*  Filename: Smart Aquarium                                                */
/*  Date:     20/11/2023                                                    */
/*  Authors:  Lovedeep Singh                                                */
/*  Purpose:  Create an embedded control system                             */
/*********************CONFIGURATION******************************************/
#pragma config OSC    = HS          // External High Speed crystal selected */
#pragma config WDT    = OFF         // Watch-Dog Timer off                  */
#pragma config PBADEN = OFF         // Analog I/O port B dissabled          */
#pragma config MCLRE  = ON          // Master Clear enabled                 */
#pragma config LVP    = OFF         // Low-Voltage Programming is OFF       */
//====================LIBRARIES=============================================*/
#include <p18f4520.h>               // Library for PIC18F4520               */
#include <delays.h>                 // Library for C18 delays               */
#include <timers.h> 	            // Libary for timers                    */
#include <adc.h>	            // Library for ADC                      */
#include "lcd.h"                    // Library for LCD diplay               */                    
//                                                                          */
//==================VARIABLES===============================================*/
int password = 1234;                // Default password                     */
int four_digit = 1000;              // Count digited numbers                */
int userpass = 0;                   // Store digited password               */
int sub_menu = 1;                   // Secondary Menu                       */
int pin_shifter = 8;                // Value to Toggle Keypad's rows        */ 
int x = 1;                          // Support for single input             */
int function = 0;                   // Function selection                   */
int menu_guide = 0;                 // Informative Menu                     */
long realtemperature = 0;           // Temperature measured by LM sensor    */ 
char tempmsg[5] = {0};              // Temperature msg                      */
int tap = 1;                           
//=========================MAIN=============================================*/
void main()                          
 {   
TRISA = 0b00000011;                 // Sets port A as 00000011 
TRISB = 0x0F;                       // Sets port B as input 
TRISC = 0x00;                       // Sets port B as output
TRISD = 0X00;                       // Sets port D as output
LCD_CreateC(LCD_ADDR1, bat_1bar);   // Store character data into ADDR1                                
LCD_CreateC(LCD_ADDR2, bat_2bar);   // Store character data into ADDR2    
LCD_CreateC(LCD_ADDR3, bat_3bar);   // Store character data into ADDR3                                
LCD_CreateC(LCD_ADDR4, bat_4bar);   // Store character data into ADDR4    
#define LDR LATAbits.LATA1          // Define port LDR
#define LEDyellow LATCbits.LATC0    // Define port for LEDyellow
#define LEDred LATCbits.LATC1       // Define port for LEDred
#define Pump LATCbits.LATC2         // Define port for Pump
#define LEDgreen LATCbits.LATC3     // Define port for LEDgreen
#define BLUEgreen LATCbits.LATC4    // Define port for LEDblue
#define Feeder LATCbits.LATC5       // Define port for Feeder
#define Alarm LATDbits.LATD7        // Define port for Alarm
#define row1 LATBbits.LATB4         // Define port for row1
#define row2 LATBbits.LATB5         // Define port for row2
#define row3 LATBbits.LATB6         // Define port for row3
#define row4 LATBbits.LATB7         // Define port for row4
#define col1 PORTBbits.RB1          // Define port for columm1
#define col2 PORTBbits.RB2          // Define port for columm2
#define col3 PORTBbits.RB3          // Define port for columm3
LCD_Init();                         // LCD ON                              
LCD_Cmd(CUR_OFF);                   // Cursor OFF
LCD_PrintRS("****************");    // Print msg to Line1                 
LCD_Cmd(LINE2_LCD);                 // Move cursor to Line2                
LCD_PrintRS("****************");    // Print msg to Line2                 
LCD_Cmd(LINE3_LCD);                 // Move cursor to Line3               
LCD_PrintRS("****************");    // Print msg to Line3                 
LCD_Cmd(LINE4_LCD);                 // Move cursor to Line4                
LCD_PrintRS("****************");    // Print msg to Line4                 
Delay10KTCYx(0);                    // Maximum delay
LCD_Cmd(CLR_LCD);                   // Clear diplay
LCD_PrintRS("****************");    // Print msg to Line1                 
LCD_Cmd(LINE2_LCD);                 // Move cursor to Line2                
LCD_PrintRS("****************");    // Print msg to Line2                  
LCD_Cmd(LINE3_LCD);                 // Move cursor to Line3               
LCD_PrintRS("****************");    // Print msg to Line3                  
Delay10KTCYx(0);                    // Maximum delay
LCD_Cmd(CLR_LCD);                   // Clear diplay
LCD_PrintRS("****************");    // Print msg to Line1                
LCD_Cmd(LINE2_LCD);                 // Move cursor to Line2              
LCD_PrintRS("****************");    // Print msg to Line1                  
LCD_Cmd(LINE3_LCD);                 // Move cursor to Line3               
LCD_PrintRS("Password:");           // Print msg to Line3                  
Delay10KTCYx(0);                    // Maximum delay
LCD_Cmd(CLR_LCD);                   // Clear diplay
LCD_PrintRS("****************");    // Print msg to Line1                  
LCD_Cmd(LINE2_LCD);                 // Move cursor to Line2                
LCD_PrintRS("    Aquarium");        // Print msg to Line2                  
LCD_Cmd(LINE3_LCD);                 // Move cursor to Line3                
LCD_PrintRS("Password:");           // Print msg to Line3                  
Delay10KTCYx(0);                    // Maximum delay
LCD_Cmd(CLR_LCD);                   // Clear diplay
LCD_PrintRS("Welcome to Smart");    // Print msg to Line1                      
LCD_Cmd(LINE2_LCD);                 // Move cursor to Line2               
LCD_PrintRS("    Aquarium");        // Print msg to Line2                  
LCD_Cmd(LINE3_LCD);                 // Move cursor to Line3               
LCD_PrintRS("Password:");           // Print msg to Line3                 
LCD_Cmd(LINE4_LCD);                 // Move cursor to Line4               
LATAbits.LATA1 =1;                  // LDR ON
while (1)                           // Loop 
{
 Delay1KTCYx(250);                  //Delay for keyboard
 if(pin_shifter == 128)             //If the loop reaches RC7 (the last keypad row)
  {
  pin_shifter = 16;                 //Reset row to 16, correponding to row 1 (RB4)
  }
  else                              //If value doesnt reach RC7
  {
  pin_shifter = pin_shifter*2;      //Row value is doubled (corresponding to the next row RB[n+1])
  }
  LATB = pin_shifter;               //Set LATC to the pin_shifter value 
  if(col1|col2|col3|x == 2)         //If a keyboad's switch is pressed
   {
    if(tap == 1 | x == 2);          //No mutiples inputs
	 {                          //1 touch equal 1 didgit
   userpass = userpass + (col1 + 2*col2 + 3*col3 + 3*row2 + 6*row3) * four_digit * (1-row4); //equation for the password
   switch(sub_menu)                 //Switch to sub_menu
    {
    case 1:                         //Deafult case (input password)
    LCD_PrintRS("*");
    if(four_digit == 1)             //If 4 numbers are pressed
     {
     if(userpass == password)       //If password is correct
      {
      LATCbits.LATC0 = 1;           //Led yellow ON 
      LCD_Cmd(CLR_LCD);             //Clear display
      LCD_PrintRS("Loading");       //Print msg to Line1
      LCD_Cmd(LINE2_LCD+7);         //Move cursor to Line2 + shift right by 7               
      LCD_PrintC(LCD_ADDR1);        //Print custom character stored in LCD_ADDR1
      LCD_Cmd(LINE3_LCD+8);         //Move cursor to Line3 + shift right by 8 
      LCD_PrintC(LCD_ADDR2);        //Print custom character stored in LCD_ADDR2
      LCD_Cmd(LINE4_LCD+9);         //Move cursor to Line4 + shift right by 9
      LCD_PrintC(LCD_ADDR3);        //Print custom character stored in LCD_ADDR3
      Delay10KTCYx(0);              //Maximum delay
      LCD_Cmd(CLR_LCD);             //Clear display
      LCD_PrintRS("Loading.");      //Print msg to Line1
      LCD_Cmd(LINE2_LCD+8);         //Move cursor to Line2 + shift right by 8
      LCD_PrintC(LCD_ADDR2);        //Print custom character stored in LCD_ADDR2
      LCD_Cmd(LINE3_LCD+9);         //Move cursor to Line3 + shift right by 9
      LCD_PrintC(LCD_ADDR3);        //Print custom character stored in LCD_ADDR3
      Delay10KTCYx(0);              //Maximum delay
      LCD_Cmd(CLR_LCD);             //Clear display
      LCD_PrintRS("Loading..");     //Print msg to Line1
      LCD_Cmd(LINE2_LCD+9);         //Move cursor to Line2 + shift right by 9
      LCD_PrintC(LCD_ADDR3);        //Print custom character stored in LCD_ADDR3
      Delay10KTCYx(0);              //Maximum delay
      LCD_Cmd(CLR_LCD);             //Clear display
      LCD_PrintRS("Loading...");    //Print msg to Line1
      Delay10KTCYx(0);              //Maximum delay              
      LATCbits.LATC0 = 0;           //Led yellow OFF
      sub_menu = 0;                 //Submenu off
        LCD_Cmd(CLR_LCD);           //Clear display
	LCD_PrintRS("Access Granted");  //Print msg to Line1    
	LATCbits.LATC3 = 1;             //LEDgreen ON
	Delay10KTCYx(250);              //Maximum delay 
	Delay10KTCYx(250);              //Maximum delay
	LATCbits.LATC3 = 0;             //LEDgreen OFF
	four_digit = 1000;              //Reset the variable for 4 digits
	userpass = 0;                   //Reset the variable for digited pass 
	}
	else
	{
	LATCbits.LATC0 = 1;             //Led yellow ON
      LCD_Cmd(CLR_LCD);             //Clear display
      LCD_PrintRS("Loading");       //Print msg to Line1
      LCD_Cmd(LINE2_LCD+7);         //Move cursor to Line2 + shift right by 7               
      LCD_PrintC(LCD_ADDR1);        //Print custom character stored in LCD_ADDR1
      LCD_Cmd(LINE3_LCD+8);         //Move cursor to Line3 + shift right by 8 
      LCD_PrintC(LCD_ADDR2);        //Print custom character stored in LCD_ADDR2
      LCD_Cmd(LINE4_LCD+9);         //Move cursor to Line4 + shift right by 9
      LCD_PrintC(LCD_ADDR3);        //Print custom character stored in LCD_ADDR3
      Delay10KTCYx(0);              //Maximum delay
      LCD_Cmd(CLR_LCD);             //Clear display
      LCD_PrintRS("Loading.");      //Print msg to Line1
      LCD_Cmd(LINE2_LCD+8);         //Move cursor to Line2 + shift right by 8
      LCD_PrintC(LCD_ADDR2);        //Print custom character stored in LCD_ADDR2
      LCD_Cmd(LINE3_LCD+9);         //Move cursor to Line3 + shift right by 9
      LCD_PrintC(LCD_ADDR3);        //Print custom character stored in LCD_ADDR3
      Delay10KTCYx(0);              //Maximum delay
      LCD_Cmd(CLR_LCD);             //Clear display
      LCD_PrintRS("Loading..");     //Print msg to Line1
      LCD_Cmd(LINE2_LCD+9);         //Move cursor to Line2 + shift right by 9
      LCD_PrintC(LCD_ADDR3);        //Print custom character stored in LCD_ADDR3
      Delay10KTCYx(0);              //Maximum delay
      LCD_Cmd(CLR_LCD);             //Clear display
      LCD_PrintRS("Loading...");    //Print msg to Line1
      Delay10KTCYx(0);              //Maximum delay               
        LATCbits.LATC0 = 0;            //Led yellow OFF
	userpass = 0;                  //Reset the digited password
	LCD_Cmd(CLR_LCD);              //Clear diplay
	LCD_PrintRS("Access Denied");  //Print msg to Line1
	LCD_Cmd(LINE2_LCD);            //Move cursor to Line2
	LATCbits.LATC1 = 1;            //LEDred ON
	LATDbits.LATD7= 1;             //Buzzer ON
	Delay10KTCYx(0);               //Maximum delay
	LATDbits.LATD7 = 0;            //LEDred OFF
	LATCbits.LATC1 = 0;            //Buzzer OFF
	Delay10KTCYx(0);               //Maximum delay
        LATCbits.LATC1 = 1;            //LEDred ON
	LATDbits.LATD7= 1;             //Buzzer ON
	Delay10KTCYx(0);               //Maximum delay
	LATDbits.LATD7 = 0;            //LEDred OFF
	LATCbits.LATC1 = 0;            //Buzzer OFF
	Delay10KTCYx(0);               //Maximum delay
	LATCbits.LATC1 = 1;            //LEDred ON
	LATDbits.LATD7= 1;             //Buzzer ON
	Delay10KTCYx(0);               //Maximum delay
	LATDbits.LATD7 = 0;            //LEDred OFF
        LATCbits.LATC1 = 0;            //Buzzer OFF
	four_digit = 1000;             //Reset the variable for 4 digits
	}}
        else                           //If digited numbers < 4
	{
	four_digit = four_digit/10;    //Check the next digit of userpass
	}
	break;                        //Break the case
	case 2:                       //Case for create a new password
	LCD_PrintRS("*");             //Print msg to Line1
	if(four_digit == 1)           //If 4 numbers are pressed 
	{
	password = sub_menu;          //Set new password
	sub_menu = 0;                 //Returns to sub_menu 0
	userpass = 0;                 //After setting a password resets userpass 
	four_digit = 1000;            //Reset the variable for 4 digits
	LCD_Cmd(CLR_LCD);             //Clear diplay
	LCD_PrintRS("Digit new password:");  //Print msg to Line1
	}
	else 
	{
	four_digit = four_digit/10;   //Check the next digit of userpass
	}
        break;                        //Break the case
	case 0:                       //Case 0 Unlocked mode
	{
	switch(userpass)              //Switch between these cases
	{
	case 1000:                   //If 1 is pressed
	if(x == 1)                   //Adjastment
	{
	LCD_Cmd(CLR_LCD);               //Clear diplay
	LCD_PrintRS("  MENU LEGEND ");  //Print msg to Line1                  
	LCD_Cmd(LINE2_LCD);             //Move cursor to Line2               
        LCD_PrintRS("*:Main Set. ");    //Print msg to Line2                 
	LCD_Cmd(LINE3_LCD);             //Move cursor to Line3                
        LCD_PrintRS("0:Lights Set. ");  //Print msg to Line3                
	LCD_Cmd(LINE4_LCD);             //Move cursor to Line4               
        LCD_PrintRS("#:Pass Set. ");    //Print msg to Line4                 
	function = 1000;                //Select Function
	} 
	else
	{
	switch(menu_guide)              //Small menu for guide
	{
	case 0:                         //Case0 if * is pressed
	menu_guide = 1;                 //Set variable to 1 (unique case)
	{LCD_Cmd(CLR_LCD);              //Clear LCD
	LCD_PrintRS("2:Diplay Temp");   //Print msg to Line1                  
	LCD_Cmd(LINE2_LCD);             //Move cursor to Line2               
        LCD_PrintRS("3:Feeder ON ");    //Print msg to Line2                 
	LCD_Cmd(LINE3_LCD);             //Move cursor to Line3                
        LCD_PrintRS("4:Pump ON ");      //Print msg to Line3                   
	LCD_Cmd(LINE4_LCD);             //Move cursor to Line4                
        LCD_PrintRS("5:Heater ON ");    //Print msg to Line4                  
	}
	break;                          //Break the case
	case 1:                         //Case0 if 0 is pressed
	menu_guide = 2;                 //Set variable to 2 (unique case)
	{LCD_Cmd(CLR_LCD);              //Clear LCD
	LCD_PrintRS("6:RGB lights");    //Print msg to Line1
	LCD_Cmd(LINE2_LCD);             //Move cursor to Line2                
        LCD_PrintRS("7:Bluetooth"); }   //Print msg to Line2                  			       
	break;                          //Break the case
	case 2:                         //Case0 if # is pressed
	menu_guide = 0;                 //Set variable to 0 (unique case)
	{LCD_Cmd(CLR_LCD);              //Clear diplay
	LCD_PrintRS("8:Set new Pass");  //Print msg to Line1
	LCD_Cmd(LINE2_LCD);             //Move cursor to Line2               
        LCD_PrintRS("9:Lock system ");  //Print msg to Line2                  
        }
	break;                          //Break the case
	}
	if(x == 2)                      //Set X to 2 to temporary access
	{
	x = 1;                          //Set X to 1 to direct access
	}}                           
	break;                          //Break the case
	case 2000:                      //If 2 is pressed
	if(x == 1)                      //Sets X to 1 to direct access
	{
	LCD_Cmd(CLR_LCD);               //Clear display
	LCD_PrintRS("**TEMPERATURE**");     //Print msg to Line1                  
	LCD_Cmd(LINE2_LCD);             //Move cursor to Line2               
        LCD_PrintRS("0: to display");   //Print msg to Line2                 
	LCD_Cmd(LINE3_LCD);             //Move cursor to Line3                
        LCD_PrintRS("temperature");     //Print msg to Line3                  
	function = 2000;                //Select Function
	}
	else
	{
	//Code for Aula
	OpenADC(ADC_FOSC_32 & ADC_RIGHT_JUST & ADC_20_TAD,
	ADC_CH0 & ADC_INT_OFF & ADC_VREFPLUS_VDD & ADC_VREFMINUS_VSS,
	0b1011); 
	ConvertADC();                   //Start A/D conversion
	while(BusyADC());               //Wait for completion   
	realtemperature = ReadADC();    // Read ADC works 
	CloseADC();                     //Close ADC
	/*
	The TMP36 sensor changes 100mV for 1 degree Celsius. 
	For temperature 0 celsius Voltage equal 750mV
	So if 5V is equivalent to 1773 ADC output,
	Then 0.01V change (i.e. 1 degree change) is equivalent to 3.546 ADC output change.
	Therefore if ADC output is divided by 2.046, Thats how to obtaine the degree.
	*/
	realtemperature = realtemperature * 488;            //Convert Vin to degrees Celsius data sheet LMP36
	realtemperature = (realtemperature - 50000) /1000;  //Celsius Convertion 
	realtemperature = (realtemperature +1);             //Small adjastment
	LCD_Cmd(CLR_LCD);                                   //Clear display
	itoa(realtemperature, tempmsg);                     //Inter to ASCII
	LCD_PrintS(tempmsg);                                //Print the value to display
	LCD_PrintRS(" Celsius");                            //Print msg to Line1
	}
	{
	x = 1;                          //Check button pressed (Adjustment) 
	}
	break;                          //Break the case
	case 3000:                      //If 3 is pressed
	if(x == 1)                      //Sets X to 1 to direct access
	{LCD_Cmd(CLR_LCD);              //Clear display
	LCD_PrintRS("Feeder ON");       //Print msg to Line1
	LATCbits.LATC5 = 1;             //FEEDER ON
	LATCbits.LATC0 = 1;             //LEDyellow ON
	Delay10KTCYx(250);              //Delay
	Delay10KTCYx(250);              //Delay
	Delay10KTCYx(250);              //Delay
	Delay10KTCYx(250);              //Delay
	LATCbits.LATC0 = 0;             //LEDyellow OFF
	LATCbits.LATC5 = 0;             //FEEDER OFF
	}
	break;                          //Break the case
	case 4000:                      //if 4 is pressed
	if(x == 1)                      //Set X to 1 to direct access
	{LCD_Cmd(CLR_LCD);              //Clear display
	LCD_PrintRS("Pump ON");         //Print msg to Line1
	LATCbits.LATC2 = 1;             //PUMP ON
	LATCbits.LATC1 = 1;             //LEDred ON
	Delay10KTCYx(250);              //Delay
	Delay10KTCYx(250);              //Delay
	Delay10KTCYx(250);              //Delay
	LATCbits.LATC1 = 0;             //LEDred OFF
	LATCbits.LATC2 = 0;             //PUMP OFF
	}
	break;                          //Break the case
	case 5000:                      //If 5 is pressed
	if(x == 1)                      //Sets X to 1 to direct access
	{
	LCD_Cmd(CLR_LCD);               //Clear display
	LCD_PrintRS("Heater ON");       //Print msg to Line1
	LATCbits.LATC4 = 1;             //HEATER ON 
	Delay10KTCYx(250);              //Delay
	Delay10KTCYx(250);              //Delay
	Delay10KTCYx(250);              //Delay
	Delay10KTCYx(250);;             //Delay
	LATCbits.LATC4 = 0;             //HEATER OFF
	} 	
	break;                          //Break the case
	case 6000:                      //If 6 is pressed
	if(x == 1)                      //Sets X to 1 to direct access
	{
	LCD_Cmd(CLR_LCD);               //Clear display
	LCD_PrintRS("RGB settings");    //Print msg to Line1
	function = 5000;                //Select Function
	}
	else      
	{
	LCD_PrintRS("");                //Print msg to Line1
        sub_menu = 4;                   // (unique case)
	if(x == 2)                      //Set X to 2 to temporary access
	{
	x = 1;                          //Check button pressed (Adjustment)
	}}
	break;                          //Break the case
	case 7000:                      //If 7 is pressed
	{LCD_Cmd(CLR_LCD);              //Clear display
	LCD_PrintRS("Bluetooth");       //Print msg to Line1
	}
	break;                          //Break the case
	case 8000:                      //If 8 is pressed
	if(x == 1)                      //Sets X to 1 to direct access
	{LCD_Cmd(CLR_LCD);              //Clear display
     LCD_PrintRS("Set new password");   //Print msg to Line1
	function = 8000;                //Select Function
	}
	else
	{LCD_Cmd(CLR_LCD);              //Clear display
      LCD_PrintRS("Digit new pass:");   //Print msg to Line1
	LCD_Cmd(LINE2_LCD);             //Move cursor to Line2
	sub_menu = 2;                   //Set sub_menu to 2 (input new password)
	four_digit = 1000;              //Reset the variable for 4 digits
	}
	if(x == 2)                      //Set X to 2 to temporary access
	{
	x = 1;                          //Check button pressed (Adjustment)
	}
	break;                          //Break the case
	case 9000:                      //If 9 is pressed
	if(x == 1)                      //Sets X to 1 to direct access
	{LCD_Cmd(CLR_LCD);              //Clear display
	LCD_PrintRS("**LOCK SYSTEM**"); //Print msg to Line1
	LCD_Cmd(LINE2_LCD);             //Move cursor to Line2
	LCD_PrintRS("Press O to ");     //Print msg to Line2
	LCD_Cmd(LINE3_LCD);             //Move cursor to Line3
	LCD_PrintRS("Confirm");         //Print msg to Line3 
	function = 9000;                //Select Function
	}
	else
	{LCD_Cmd(CLR_LCD);              //Clear display
	LCD_PrintRS("System locked");   //Print msg to Line1
	LCD_Cmd(LINE2_LCD);             //Move cursor to Line2
	LCD_PrintRS("Password:");       //Print msg to Line2
	LCD_Cmd(LINE3_LCD);             //Move cursor to Line2
	sub_menu = 1;                   //Set sub_menu to 1, lock the menu
	}
	if(x == 2)                      //Set X to 2 to temporary access
	{
	x = 1;                          //Check button pressed (Adjustment)
	}
	break;                          //Break the case
	}
	if(userpass == 0000 && x == 1)  //Reset the userpassword and Sets X to 1 to direct access
	{
	x = 2;                          //Set X  to 2, to temporarily access 
	userpass = function;            //Userpass is equal to function
	}
	else
	{
	userpass = 0;                   //Reset userpass after picking a function
	}
	break;                          //Break the case
	}}
	tap = 0;                        //prevent multiple inputs 
	 LATC = 0;                      //sets the C port to 0 for a moment, to prevent extra inputs
	 Delay10KTCYx(250);             //Delay
	 tap = 1;                       //Resets tap to 1 after a delay to prevent double inputs
	 }}}}

  

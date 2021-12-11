#include "mbed.h"
#include "ctype.h"

#define SPI_MOSI D11
#define SPI_SCLK D13
#define SPI_CS D10

#define TRIGGER_PIN D3                                          //Connect your Ultra Sonic device to these pins
#define ECHO_PIN    D2                                          //Modify for you platform used 'Arduino' Header on F401RE NUCLEO
 
DigitalOut Trigger(TRIGGER_PIN);                                //Instance of the DigitalOut class called 'Trigger'
DigitalIn  Echo(ECHO_PIN);  

DigitalOut red_led(D6);
DigitalOut green_led(D7);
DigitalOut blue_led(D9);  

PwmOut speaker(D4);                                  //Instance of the DigitalIn class called 'Echo'
 
Timer pulse;                                                    //Instance of the Timer class called 'pulse'
 
float GetDistance();                                            //Function Prototype
 
int main() {

     speaker.period(0.001);
     float Distance;                                            //Assign a local variable for the main function
     pulse.start();                                             //Start the instance of the class timer        
     
     while(true)                                                //Continuous loop always true
     {
        red_led = 0;
        green_led = 0;
        blue_led = 0;

        Distance=GetDistance();                                 //Get the value returned from the function GetDistance()
        printf("Distance %d\n\r",int(Distance));     
        
        if(Distance <= 200)
        {
            speaker = 0.9f;
        }
        else
        {
            speaker = 0.0f;
        }

        if(Distance <= 200 && Distance > 100) //20 cm
        {
            red_led = 1;
            green_led = 0;
            blue_led = 0;
        }
        else if(Distance <= 100 && Distance > 50) //10 cm
        {
            red_led = 0;
            green_led = 1;
            blue_led = 0;
        }          
        else if(Distance <= 50) // 5 cm
        {
            red_led = 0;
            green_led = 0;
            blue_led = 1;
        }
        wait_us(250000);                                           //Wait for 250ms
     }    
}
 
float GetDistance(){                                            //Function Name to be called
     int EchoPulseWidth=0,EchoStart=0,EchoEnd=0;                //Assign and set to zero the local variables for this function
     Trigger=1;                                                 //Signal goes High i.e. 3V3
     wait_us(100);                                              //Wait 100us to give a pulse width
     Trigger=0;                                                 //Signal goes Low i.e. 0V
     pulse.reset();                                             //Rest the instance of the Timer Class
     while(Echo==0&&EchoStart<25000){                           //wait for Echo to go high
            EchoStart=pulse.read_us();                          //AND with timeout to prevent blocking!      
     }
     while(Echo==1&&(EchoEnd-EchoStart<25000)){                 //wait for echo to return low
        EchoEnd=pulse.read_us();                                //or'd with timeout to prevent blocking!   
     }
     EchoPulseWidth=EchoEnd-EchoStart;                          //time period in us
     return (float)EchoPulseWidth/5.8f;                         //calculate distance in mm and return the value as a float
}

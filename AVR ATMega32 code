#include <avr/io.h>
#include <util/delay.h>
#include <avr/interrupt.h>
#define bit_is_high(reg,bit) (reg & (1<<bit))
#define F_CPU 1000000
 
int i,n,sum;
int x,y;
 
float savg;
int sensor[7];
uint8_t cases;
 
void str()
{
    PORTD |= (1<<PD2);
    PORTD |= (1<<PD0);
    PORTD&=~(1<<PD1);
    PORTD&=~(1<<PD3);
    
}
 
void sl()
{
    PORTD |= (1<<PD2);
    PORTD&=~(1<<PD0);
    PORTD |= (1<<PD1);
    PORTD&=~(1<<PD3);
}
 
void sr()
{
    PORTD |= (1<<PD0);
    PORTD&=~(1<<PD1);
    PORTD |= (1<<PD3);
    PORTD&=~(1<<PD2);
 
}
 
void stop()
{
    PORTD &=~(1<<PD2);
    PORTD &=~ (1<<PD0);
    PORTD&=~(1<<PD1);
    PORTD&=~(1<<PD3);
    
}
void crossroads()
{
    str();
    _delay_ms(80);
    sr();
    _delay_ms(300);
    
    
}
 
 
 
 
void deadend()
{    str();
    _delay_ms(900);
    sl(n);
    _delay_ms(850);
 
}
 
int main(void)
{
    
    DDRD=0XFF;//output
    DDRC=0X00;//input
    PORTC=0XFF;
    DDRA=0XFF;
    
    while (1)
    {
        
        PORTA=0X00;
        i=0;
        n=0;
        sum=0;
        uint8_t SENSORA = PINC & 0b01111111;
        cases = SENSORA;
        while(SENSORA != 0)
        {
            sensor[i]=SENSORA%2;
            SENSORA/=2;
            i++;
            if(sensor[i])
            {
                sensor[i]=i-3;
                n++;
                sum+=sensor[i];
            }
            
        }
        
        savg = sum/n;
        
        if(cases == 0b00000000)
        {
            crossroads();
            
        }
        
        else if(cases == 0b01111111)
        {
            deadend();
        }
        
        
        else if (((bit_is_high(PINC,0))) && ((bit_is_high(PINC,6))) && (!(bit_is_high(PINC,3))))
        {
            break;
        }
        
        
        
        else
        {
            if(savg>0.7)  //value to be changed to 1 for steep left turn
            sl();
            
            else if(savg<-1)
            sr();
            
            else
            str();
        }
    }
    
    str();
    _delay_ms(500);
 
    while(1)
    {
        PORTA=0X00;
        i=0;
        n=0;
        sum=0;
        uint8_t SENSORA = PINC & 0b01111111;
        cases = SENSORA;
        while(SENSORA != 0)
        {
            sensor[i]=SENSORA%2;
            SENSORA/=2;
            i++;
            if(sensor[i])
            {
                sensor[i]=i-3;
                n++;
                sum+=sensor[i];
            }
            
        }
        
        savg = -sum/n;
        
        if(cases == 0b01111111)
        {
            deadend();
            
            
        }
        
        else if(cases == 0b00000000)
        {
            crossroads();
        }
        
        
        else
        {
            if(savg>0.7)  //value to be changed to 1 for steep left turn
            sl();
            
            else if(savg<-1)
            sr();
            
            else
            str();
        }
    }
    
    
    
}

/*	Author: lab
 *  Partner(s) Name: Kenny
 *	Lab Section:
 *	Assignment: Lab #  Exercise #
 *	Exercise Description: [optional - include for your own benefit]
 *
 *	I acknowledge all content contained herein, excluding template or example
 *	code, is my own original work.
 */
#include <avr/io.h>
#ifdef _SIMULATE_
#include "simAVRHeader.h"
#include<avr/interrupt.h>
#endif

volatile unsigned char TimerFlag = 0;

unsigned long _avr_timer_M = 1;
unsigned long _avr_timer_cntcurr = 0;
unsigned char tmpB = 0x00;

void TimerOn(){
	TCCR1B = 0x0B;
	OCR1A = 125;
	TIMSK1 = 0x02;
	TCNT1 = 0;
	_avr_timer_cntcurr = _avr_timer_M;
	SREG |= 0x80;
}
void TimerOff(){
	TCCR1B = 0x00;
}
void TimerISR(){
	TimerFlag = 1;
}
ISR(TIMER1_COMPA_vect){
	_avr_timer_cntcurr--;
	if(_avr_timer_cntcurr == 0){
		TimerISR();
		_avr_timer_cntcurr = _avr_timer_M;
	}
}
void TimerSet(unsigned long M){
	_avr_timer_M = M;
	_avr_timer_cntcurr = _avr_timer_M;	
}

typedef struct task{
	int state;
	unsigned long period;
	unsigned long elapsedTime;
	int(*TickFct)(int);
}task;
task tasks[3];
const unsigned short tasksNum = 3;
unsigned char i = 0;
unsigned char j = 0;
unsigned char tl;
unsigned char bl;

enum TL_States{tl_start} TL_state;
void ThreeLED_Tick(){
	switch(TL_state){
		case tl_start: 
			if(i == 0){tl = 0x01; i++;}
			else if(i == 1){tl = 0x02; i++;}
			else if(i == 2){tl = 0x04; i = 0;} break;
		default: TL_state = tl_start; break;
	}
        switch(TL_state){
                default: break;
        }	
}

enum BL_States{bl_start} BL_state;
void BlinkLED_Tick(){
        switch(BL_state){
                case bl_start: 
			if(j == 0){bl = 0x08; j++;}
			else if(j == 1){bl = 0x00; j = 0;}
	  		break;
                default: BL_state = bl_start; break;
	}
	switch(BL_state){
		default: break;
	}
}

enum CL_States{cl_start} CL_state;
void CombLED_Tick(){
        switch(CL_state){
                case cl_start: PORTB = tl + bl; CL_state = cl_start; break;
                default: CL_state = cl_start; break;
        }
	switch(CL_state){
                default: break;
        }
}

int main(void) {
	unsigned long TL_elapsedTime = 1000;
	unsigned long BL_elapsedTime = 1000;
	unsigned long CL_elapsedTime = 1000;
	const unsigned long timerPeriod = 100;
	DDRB = 0xFF; PORTB = 0x00;
	TimerSet(100);
	TimerOn();
	TL_state = tl_start;
	BL_state = bl_start;
	CL_state = cl_start;
    while (1) {
	if(TL_elapsedTime >= 1000){ThreeLED_Tick(); TL_elapsedTime = 0;}
	if(BL_elapsedTime >= 1000){BlinkLED_Tick(); BL_elapsedTime = 0;}
	if(CL_elapsedTime >= 1000){CombLED_Tick(); CL_elapsedTime = 0;}
	while(!TimerFlag){}
		TimerFlag=0;
		TL_elapsedTime += timerPeriod; 
		BL_elapsedTime += timerPeriod;
		CL_elapsedTime += timerPeriod;
    }
    return 1;
}






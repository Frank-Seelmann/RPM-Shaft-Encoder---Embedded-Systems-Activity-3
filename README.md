# RPM-Shaft-Encoder---Embedded-Systems-Activity-3

Introduction

For this activity, students were tasked with designing a system to measure the revolutions
per minute (RPM) of a hobby motor. The system is meant to make use of the analog to
digital converter on the microcontroller to take a varying voltage as input, then output to
a motor using a DAC methodology, then determine the RPM.

Design

In the previous activity it was determined that, due to the limited current output of the
NUCLEO board, a good way to drive the motor is using a ULN2003 Darlington
Transistor Array which allows a battery actually power the motor, but the microcontroller
can determine how fast it should go. Also in the previous lab, it was determined that the
best digital to analog converter for this setup is pulse width modulation (PWM).
For the PWM portion, the program configures TIM8. For this setup, the frequency is
determined by TIM8 ARR, and the duty cycle by TIM8 CCR1. Since it is known that the
range for the duty cycle is 0 to 26667 and the range of the result of the ADC is 4096, the
ADC result can be mapped to the PWM by multiplying by 26667/4096 and passing it to TIM8
CCR1, which outputs to PA5.

For input capture from the shaft encoder, TIM3 is driven by a 1kHz, and as such can
count the milliseconds between rising edges of the input signal (from PA6). With this the
time period between input captures can be determined by subtracting the previous
captured time from the current captured time. Dividing 1000 by the period yields the
frequency in Hertz (1000 milliseconds in 1 second), i.e. revolutions per second.
Multiplying this by 60 yields the revolutions per minute. It is important to note that this
assumes the input capture is occurring once per revolution.

Results

As shown in the verification video, https://www.youtube.com/watch?v=Z_WYD4dSx3A&t=1s, the 
RPM shaft encoder works very well, though it maxes out at about 3000 RPM. After this point 
it, presumably, begins missing some inputs and displays the wrong RPM values. In this regard, 
choosing to have the input signal come only once per revolution is extremely important, since 
when it is taken at both ends of the appended popsicle stick it roughly halves the max readable RPM.
While it is difficult to verify the precision, a check was performed in the verification
video by generating an input by hand every second, in sync with a watch, which yielded
an expected frequency of 1Hz and RPM of 60.

Conclusion

The system works as anticipated, achieving the goals set, though only to a threshold of
about 3000 RPM. One way to increase the maximum captured RPM would be to increase
the clock of TIM3; however, the issue with this is another bottleneck arises, the LCD.
Since the LCD is updated every time an input is captured, it can become unreadable as
the frequency of captured inputs increases. This was already a problem at current max
captured RPM, as shown in the verification video.
This system for this project will be used again in the next, which will add “cruise control”
functionality

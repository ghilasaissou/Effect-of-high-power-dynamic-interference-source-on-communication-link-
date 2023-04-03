#import the libraries needed 
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import math
from numpy import linspace, meshgrid
from numpy import exp,arange
from pylab import meshgrid,cm,imshow,contour,clabel,colorbar,axis,title,show
from matplotlib.pyplot import colorbar, pcolor, show
# Identify the constants
c=3e8
pi=3.141592653589793
fc =int()
# The carrier frequency
fc = float(input("Enter the value of the carrier frequency in Hz"))
K=(4*pi*fc)/c

#α is the path-lossexponent
# d is the distance betwen RX and TX
#μLoS and μNLoS are excessive attenuation factors for the LoS and the NLoS links
d=float(input("Enter the distance between the UAS and transmitter (GCS or other UAS)"))
a=2
#Distance of interfernce source from the UAS
d2=1000.01
d3=1500.01
uLos=3 #in dB
NLos=23 #in dB

##Calculate the pathloss
Pathloss= 20*math.log10(K*d) + uLos
Pathloss_interferer= 20*math.log10(K*d)+ uLos # at same distance
if d2 != d:
    Pathloss_interferer2= 20*math.log10(K*d2)+ uLos
    him2=1/Pathloss_interferer2
if d3 != d:    
    Pathloss_interferer3= 20*math.log10(K*d3)+ uLos
    him3=1/Pathloss_interferer3

# Channnel Model
hij=1/Pathloss
him=1/Pathloss_interferer


#Ask the user to inter the transmitting power 
input_power = input("Enter the transmitting power in dBm:")
Pi=float(input_power)
# ask  the user to enter what he wants to plot

options = ['SIR as a function of Interference power', 'SIR as a function of interference distance', 'Interference power power effect on bandwidth utilization']
a=0
while a<4:
    a+=1
    user_input = ''

    input_message = "Pick an option:\n"

    for index, item in enumerate(options):
        input_message += f'{index+1}) {item}\n'

    input_message += 'Your choice: '


    user_input = float(input(input_message))

    if user_input == 1: 
        #Initialization
        SIR=[]
        SIR2=[]
        SIR3=[]
        i=[]
        # convert into watt
        Pi=pow(10,(Pi-30)/10)

        for Pj in range(-10,30):
            Pint=pow(10,(Pj-30)/10)
            SIR.append(Pi*hij/(Pint*him))
            SIR2.append(Pi*hij/(Pint*him2))
            SIR3.append(Pi*hij/(Pint*him3))
            i.append(Pj)
        plt.figure(figsize=(10,6))
        plt.plot(i,SIR,label='Interferer at 500m')
        plt.plot(i,SIR2,label='Interferer at 1000m')
        plt.plot(i,SIR3,label='Interferer at 1500m')
        plt.title('SIR versus Interferer Power with '+ input_power +' dBm UAS transmitting power')
        plt.xlabel('Interferer Power')
        plt.ylabel('SIR')
        #show plot to user
        plt.legend()
        plt.show()
    elif user_input == 2: 
        SIR=[]
        SIR2=[]
        SIR3=[]
        i=[]
        for j in range(1,800):
            dim=j
            Pathloss_interferer= 20*math.log10(K*dim)+ uLos
            hij=1/(20*math.log10(K*500) + uLos)
            him=1/Pathloss_interferer
            Pi=pow(10,(Pi-30)/10)
            SIR.append((Pi*hij)/(pow(10,(-5-30)/10)*him))
            SIR2.append((Pi*hij)/(pow(10,(5-30)/10)*him))
            SIR3.append((Pi*hij)/(pow(10,(20-30)/10)*him))
            i.append(j)
        plt.figure(figsize=(10,6))
        plt.plot(i,SIR,label='Interferer with -5 dbm')
        plt.plot(i,SIR2,label='Interferer with 5 dBm')
        plt.plot(i,SIR3,label='Interferer with 20 dBm')
        plt.title('SIR versus Interferer distance')
        plt.xlabel('Distance (m)')
        plt.ylabel('SIR (db)')
        #show plot to user
        plt.legend()
        plt.show()
    elif user_input == 3:
        B1=[]
        B2=[]
        B3=[]
        i=[]
        for Pj in range(-10,30):
            hij=1/(20*np.log10(K*500) + uLos)
            him=1/(20*np.log10(K*500)+ uLos)
            #hij=1/ (pow(K*500,2)* uLos)
            #him=1/(pow(K*400,2)*uLos)
            SIR= 10*math.log10((pow(10,(20-30)/10))*hij/((pow(10,(Pj-30)/10))*him))
            C1=3400
            C2=2.5e6
            C3=5e6
            B1.append(C2/(math.log2(1+pow(10,SIR/10))*1000))
            i.append(Pj)
        plt.figure(figsize=(10,6))
        plt.plot(i,B1, label='3.4Kbps RTCA DO-377 minimum Data rate')
        #plt.plot(i,B2, label='2.5Mbps Recommended data rate for 720p video')
        #plt.plot(i,B3, label='5Mbps Recommended data rate for 1080p video ')
        plt.title(' Bandwidth as a function of interference power with 3.4Kbps RTCA DO-377 minimum Data rate')
        plt.xlabel('Interference Power in dBm')
        plt.ylabel('Bandwidth (KHz)')
        plt.show()

options_3D = ['Yes', 'No']
user_input = ''

input_message = "Would you like to plot the 3D plot as a function of interfernce power and distance?:\n"

for index, item in enumerate(options_3D):
    input_message += f'{index+1}) {item}\n'

input_message += 'Your choice: '


user_input = float(input(input_message))

if user_input == 1: 
    Pj, y = linspace(-10, 30, 100), linspace(500, 5000, 200)
    X, Y = meshgrid(Pj, y)
    B1=[]
    B2=[]
    B3=[]
    i=[]
    def f(Pj, y):  
        hij=1/(20*np.log10(K*1000) + uLos)
        him=1/(20*np.log10(K*y)+ uLos)
        #hij=1/ (pow(K*500,2)* uLos)
        #him=1/(pow(K*400,2)*uLos)
        #SIR= (pow(10,(10-30)/10))*hij/((pow(10,(Pj-30)/10))*him)
        C1=3400
        C2=2.5e6
        C3=5e6
        return 10*np.log10((pow(10,(20-30)/10))*hij/((pow(10,(Pj-30)/10))*him))
    Z = f(X, Y)
    plt.figure(figsize=(10,6))
    plt.contourf(X,Y,Z,30)
    cbar= plt.colorbar()
    cbar.set_label('SIR in dB')
    plt.xlabel('Interference Power in dBm')
    plt.ylabel('Interference Source distance from the receiver in m')
    plt.show()

    #Bandwidth plot
    from numpy import linspace, meshgrid
    Pj, y = linspace(-10, 30, 100), linspace(500, 5000, 200)
    X, Y = meshgrid(Pj, y)
    B1=[]
    B2=[]
    B3=[]
    i=[]
    import math
    def f(Pj, y):  
        hij=1/(20*np.log10(K*1000) + uLos)
        him=1/(20*np.log10(K*y)+ uLos)
        #hij=1/ (pow(K*500,2)* uLos)
        #him=1/(pow(K*400,2)*uLos)
        SIR= 10*np.log10((pow(10,(20-30)/10))*hij/((pow(10,(Pj-30)/10))*him))
        C1=3400
        C2=2.5e6
        C3=5e6
        #B1.append(C1/(math.log2(1+SIR)*1000))
        return C1/(np.log2(1+pow(10,SIR/10)*1000))
    plt.figure(figsize=(10,6))
    Z = f(X, Y)
    plt.contourf(X,Y,Z,30)
    cbar= plt.colorbar()
    cbar.set_label('Used bandwidth in Khz')
    plt.xlabel('Interference Power in dBm')
    plt.ylabel('Interference Source distance from the receiver in m')

    plt.show()
    #pcolor(X, Y, Z)
    #colorbar()

     

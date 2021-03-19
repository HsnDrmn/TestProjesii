# TestProjesii
Proje öğrenmesi
import sqlite3 as lite
from vpython import*
import time as tm
import math
import numpy as np
import os

win=650
dt=0
yaw=0
pitch=0
roll=0
torad=0.01745
todeg=57.29578
time1=0.5
time2=0.13
time3=0.1
c=20
cnvs=canvas(width=win,height=win,color=color.black,align='left')
cnvs.range=200

##Curve listelerin olusturulması
lists1=[]
lists2=[]
lists3=[]
lists4=[]
lists5=[]

##Curvelerin pozisyonlarının atanması
for i in range(0,197,1):
    lists1.append(vector(196,i,0))
for i in range(196,-196,-1):
    lists2.append(vector(i,196,0))
for i in range(196,-196,-1):   
    lists3.append(vector(-196,i,0))
for i in range(-196,196,1):
    lists4.append(vector(i,-196,0))
for i in range(-196,0,1):
    lists5.append(vector(196,i,0))

##Giris yazısının olusturulması
T0=text(text='_',visible=False)
T0.length=c
T0.height=c
T1=text(text='C',color=color.red,visible=False)
T1.length=c
T1.height=c
T2=text(text='A',color=color.green,visible=False)
T2.length=c
T2.height=c
T3=text(text='C',color=color.red,visible=False)
T3.length=c
T3.height=c
T4=text(text='A',color=color.green,visible=False)
T4.length=c
T4.height=c
T5=text(text='-',color=color.red,visible=False)
T5.length=c
T5.height=c
T6=text(text='B',color=color.green,visible=False)
T6.length=c
T6.height=c
T7=text(text='E',color=color.red,visible=False)
T7.length=c
T7.height=c
T8=text(text='Y',color=color.green,visible=False)
T8.length=c
T8.height=c
T9=text(text='R',color=color.red,visible=False)
T9.length=c
T9.height=c
T10=text(text='O',color=color.green,visible=False)
T10.length=c
T10.height=c
T11=text(text='K',color=color.red,visible=False)
T11.length=c
T11.height=c
T12=text(text='E',color=color.green,visible=False)
T12.length=c
T12.height=c
T13=text(text='T',color=color.red,visible=False)
T13.length=c
T13.height=c
T14=text(text='T',color=color.green,visible=False)
T14.length=c
T14.height=c
T15=text(text='A',color=color.red,visible=False)
T15.length=c
T15.height=c
T16=text(text='K',color=color.green,visible=False)
T16.length=c
T16.height=c
T17=text(text='I',color=color.red,visible=False)
T17.length=5
T17.height=c
T18=text(text='M',color=color.green,visible=False)
T18.length=c
T18.height=c
T19=text(text='I',color=color.red,visible=False)
T19.length=5
T19.height=c
#Yazının konumlandrılması
T0.pos=vector(-110,0,0)
T1.pos=vector(-85,0,0)
T2.pos=vector(-60,0,0)
T3.pos=vector(-35,0,0)
T4.pos=vector(-10,0,0)
T5.pos=vector(15,0,0)
T6.pos=vector(40,0,0)
T7.pos=vector(65,0,0)
T8.pos=vector(90,0,0)
T9.pos=vector(-125,-25,0)
T10.pos=vector(-100,-25,0)
T11.pos=vector(-75,-25,0)
T12.pos=vector(-50,-25,0)
T13.pos=vector(-25,-25,0)

T14.pos=vector(25,-25,0)
T15.pos=vector(50,-25,0)
T16.pos=vector(75,-25,0)
T17.pos=vector(100,-25,0)
T18.pos=vector(110,-25,0)
T19.pos=vector(135,-25,0)

##Roketin olusturulması
rocketbody=cylinder(radius=11,pos=vector(0,-127,0),axis=vector(0,254,0),visible=False)
rocketup=cone(radius=11,pos=vector(0,127,0),axis=vector(0,23,0),visible=False)
wing1=box(width=0.3,length=10,height=16.5,pos=vector(16,-127+16.5/2,0),visible=False)
wing2=box(width=0.3,length=10,height=16.5,pos=vector(-16,-127+16.5/2,0),visible=False)
wing3=box(width=10,length=0.3,height=16.5,pos=vector(0,-127+16.5/2,16),visible=False)
wing4=box(width=10,length=0.3,height=16.5,pos=vector(0,-127+16.5/2,-16),visible=False)

rocket=compound([rocketbody,rocketup,wing1,wing2,wing3,wing4],visible=False)
rocket.texture=textures.metal

##Eksenlerin Olusturulması
frontArrow=arrow(length=4,shaftwidth=.1,color=color.purple,axis=vector(1,0,0),visible=False)
sideArrow=arrow(length=4,shaftwidth=.1,color=color.magenta,axis=vector(0,0,1),visible=False)
upArrow=arrow(length=4,shaftwidth=.1,color=color.orange,axis=vector(0,1,0),visible=False)

##Sağ alt Eksenlerin olusturulması
xarrow=arrow(length=50,shaftwidth=6,color=color.red,axis=vector(1,0,0),pos=vector(125,-152,0))
yarrow=arrow(length=50,shaftwidth=6,color=color.blue,axis=vector(0,1,0),pos=vector(125,-152,0))
zarrow=arrow(length=50,shaftwidth=6,color=color.green,axis=vector(0,0,1),pos=vector(125,-152,0))

arrows=compound([xarrow,yarrow,zarrow],billboard=True,emissive=True,visible=False)
arrows.rotate(angle=pi/5,axis=vec(0,-1,0))
arrows.visible=True

##PLot grafiklerinin oluşturulması
g1=graph(width=win*0.8,height=win/(2.5),align='left',title='Yalpalama(yaw)-Zaman Grafiği',xtitle='Saniye(s)',ytitle='yalpalama (d/s)',fast=False,background=color.black,foreground=color.white)
g2=graph(width=win*0.8,height=win/(2.5),align='left',title='Yunuslama(pitch)-Zaman Grafiği',xtitle='Saniye(s)',ytitle='yunuslama (d/s)',fast=False,background=color.black,foreground=color.white)
g3=graph(width=win*0.8,height=win/(2.5),align='left',title='Dönü-Zaman(roll) Grafiği',xtitle='Saniye(s)',ytitle='dönü (d/s)',fast=False,background=color.black,foreground=color.white)
##grafiklerin oluşturulması
f1=gcurve(graph=g1,color=color.red,markers=False,marker_color=color.yellow,radius=0.6,label="Yalpalama (d/s)")
f2=gcurve(graph=g2,color=color.blue,markers=False,marker_color=color.yellow,radius=0.6,label="Yunuslama (d/s)")
f3=gcurve(graph=g3,color=color.green,markers=False,marker_color=color.yellow,radius=0.6,label="Dönü (d/s)")

##Caca Remake in olusturulması
Text=text(text='CACA - R',color=color.green,align='center',visible=False,depth=1,billboard=True,emissive=True)
Text.height=16
Text.length=145
Text.pos=vector(0,170,0)

#################Curvelerin gösterilmesi#######################
c1=curve(lists1,color=color.red)
tm.sleep(time3)
c2=curve(lists2,color=color.blue)
tm.sleep(time3)
c3=curve(lists3,color=color.green)
tm.sleep(time3)
c4=curve(lists4,color=color.white)
tm.sleep(time3)
c5=curve(lists5,color=color.red)

#############grafiğin gösterilmesi##################
f1.plot(pos=(0,0))
f2.plot(pos=(0,0))
f3.plot(pos=(0,0))
##################Yazıların gösterilemesi#######################
T0.visible=True
tm.sleep(0.5)
T0.visible=False
tm.sleep(0.5)
T0.visible=True
tm.sleep(time1)
T0.visible=False
tm.sleep(time1)
T0.visible=True
tm.sleep(time1)
T0.visible=False
tm.sleep(time1)
T0.visible=True


T1.visible=True
tm.sleep(time2)
T2.visible=True
tm.sleep(time2)
T3.visible=True
tm.sleep(time2)
T4.visible=True
tm.sleep(time2)

T5.visible=True
tm.sleep(time1)
T5.visible=False
tm.sleep(time1)
T5.visible=True
tm.sleep(time1)
T5.visible=False
tm.sleep(time1)
T5.visible=True


T6.visible=True
tm.sleep(time2)
T7.visible=True
tm.sleep(time2)
T8.visible=True
tm.sleep(time2)
tm.sleep(0.5)
T9.visible=True
tm.sleep(time2)
T10.visible=True
tm.sleep(time2)
T11.visible=True
tm.sleep(time2)
T12.visible=True
tm.sleep(time2)
T13.visible=True
tm.sleep(time2)
T14.visible=True
tm.sleep(time2)
T15.visible=True
tm.sleep(time2)
T16.visible=True
tm.sleep(time2)
T17.visible=True
tm.sleep(time2)
T18.visible=True
tm.sleep(time2)
T19.visible=True
###############Yazıların sönükleştirilmesi##############
TT=compound([T0,T1,T2,T3,T4,T5,T6,T7,T8,T9,T10,T11,T12,T13,T14,T15,T16,T17,T18,T19])
for i in arange(1,0,-0.03):
    TT.opacity=i
    tm.sleep(0.09)

#############Caca Remakein gösterilmesi#############
Text.visible=True
#############Roketin gösterilmesi###################
rocket.visible=True


i=0
t=0
Kontrol=True
def goster():
    db=lite.connect('C:/Users/hdura/OneDrive/Masaüstü/esdeneme.db')
    im=db.cursor()
    global i
    global t
    i=i+1
    t=t+1
    im.execute("""SELECT * FROM hasan_Table""")
    rows=im.fetchall()

    if i<=(len(rows)-1):
        yaw=(float((rows[i][0]).replace(',','.'))*torad)
        pitch=(float((rows[i][1]).replace(',','.'))*torad)
        roll=(float((rows[i][2]).replace(',','.'))*torad)

        k=vector(cos(yaw)*cos(pitch),sin(pitch),sin(yaw)*cos(pitch))
        y=vector(0,1,0)
        s=cross(k,y)
        v=cross(s,k)

        vrot=v*cos(roll)+cross(k,v)*sin(roll)
        frontArrow.axis=k
        sideArrow.axis=cross(k,vrot)
        upArrow.axis=vrot

        rocket.axis=k
        rocket.up=vrot
        
        f1.plot(pos=(t,yaw))
        f2.plot(pos=(t,pitch))
        f3.plot(pos=(t,roll))

    if i > (len(rows)-1):
        i=i-1
        yaw=rows[i][0]
        pitch=rows[i][1]
        roll=rows[i][2]
        
while True:
    veritabanı='esdeneme.db'
    dosya_mevcut=os.path.exists(veritabanı)
    if dosya_mevcut:
        try:
            tm.sleep(0.04)
            goster()
        except IndexError:
            goster()
    else:
        print("data base cannot found")

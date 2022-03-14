from tkinter import *
import time

window = Tk()  # creates window
window.geometry("650x500")
window.title("Accumulator Volume Calculator")

icon = PhotoImage(file='logo.png')

window.iconphoto(True, icon)
window.config(background="white")


def count():
    hour = time.strftime("%I")
    minute = time.strftime("%M")
    second = time.strftime("%S")
    timelabel = Label(window, fg='green', font=('Arial', 10), text="New Text")
    timelabel.place(x=10, y=410)
    timelabel.config(text=hour + ":" + minute + ":" + second)
    timelabel.after(100, count)


count()

T_global = DoubleVar()


def count2():
    hour = time.strftime("%I")
    minute = time.strftime("%M")
    second = time.strftime("%S")
    milliseconds = (time.time() * 1000)
    millisecondsint = int(milliseconds)
    hourfloat = float(hour)
    minutefloat = float(minute)
    secondfloat = float(second)
    milliseconds2 = milliseconds - millisecondsint
    floattime3 = ((hourfloat * 3600) + (minutefloat * 60) + secondfloat + milliseconds2).__round__(3)
    timelabel3 = Label(window, fg='green', font=('Arial', 10), text=floattime3)
    timelabel3.place(x=250, y=550)
    timelabel3.after(100, count2)
    T_global.set(floattime3)


count2()

runtime = DoubleVar()

T_start = DoubleVar()

T_elapse = DoubleVar()

running = IntVar()


def timer():
    timertime = ((T_global.get() - T_start.get()).__round__(3))
    timelabel3 = Label(window, fg='green', font=('Arial', 10), text=timertime)
    timelabel3.place(x=250, y=560)
    timelabel3.after(100, timer)
    if running.get() == 1:
        T_elapse.set(timertime)


timer()

P1 = IntVar()

c = Checkbutton(window, text="P1 Precharge Pressure (psi)", variable=P1, onvalue=1, offvalue=0)
c.place(x=10, y=10)

P2 = IntVar()

c = Checkbutton(window, text="P2 System Pressure (psi)", variable=P2, onvalue=1, offvalue=0)
c.place(x=10, y=40)

V1 = IntVar()

c = Checkbutton(window, text="V1 Accumulator Volume (L)", variable=V1, onvalue=1, offvalue=0)
c.place(x=10, y=70)

V2 = IntVar()

c = Checkbutton(window, text="V2 Pressurised Volume (L)", variable=V2, onvalue=1, offvalue=0)
c.place(x=10, y=100)


def IfZero():
    if (P1.get() + P2.get() + V1.get() + V2.get()) == 0:
        emptylabel.config(text='You have no inputs : Please input at least 3 knowns')


def IfOne():
    if (P1.get() + P2.get() + V1.get() + V2.get()) == 1:
        emptylabel.config(text='You have one input : Enter, then please add 2 more knowns')


def IfTwo():
    if (P1.get() + P2.get() + V1.get() + V2.get()) == 2:
        emptylabel.config(text='You have two inputs : Enter, then please add 1 more known')


def IfThree():
    if (P1.get() + P2.get() + V1.get() + V2.get()) == 3:
        emptylabel.config(text='You have three inputs : The missing element may be calculated :)')


def IfFour():
    if (P1.get() + P2.get() + V1.get() + V2.get()) == 4:
        emptylabel.config(text='If you have four inputs, uncheck one to recalculate uncertain value')


emptylabel = Label(window, fg='green', font=('Arial', 10))
emptylabel.place(x=10, y=170)


def ActivateP1():
    if P1.get() == 1:
        entry1.config(state=NORMAL)


def ActivateP2():
    if P2.get() == 1:
        entry2.config(state=NORMAL)


def ActivateV1():
    if V1.get() == 1:
        entry3.config(state=NORMAL)


def ActivateV2():
    if V2.get() == 1:
        entry4.config(state=NORMAL)


def DeactivateP1():
    if P1.get() == 0:
        entry1.config(state=DISABLED)


def DeactivateP2():
    if P2.get() == 0:
        entry2.config(state=DISABLED)


def DeactivateV1():
    if V1.get() == 0:
        entry3.config(state=DISABLED)


def DeactivateV2():
    if V2.get() == 0:
        entry4.config(state=DISABLED)


check = Button(window, command=lambda: [IfZero(), IfOne(), IfTwo(), IfThree(), IfFour(), DeactivateP1(), DeactivateP2(),
                                        DeactivateV1(), DeactivateV2(), ActivateP1(), ActivateP2(), ActivateV1(),
                                        ActivateV2()], text='Check Known Inputs')
check.place(x=10, y=130)

Entry1 = DoubleVar()

Entry2 = DoubleVar()

Entry3 = DoubleVar()

Entry4 = DoubleVar()

Entry5 = DoubleVar()

entry1 = Entry(window, textvariable=Entry1)
entry1.insert(0, '0')
entry1.config(state=DISABLED)
entry1.place(x=400, y=10)

psi1 = Label(window, text="P1 psi")
psi1.place(x=530, y=10)

entry2 = Entry(window, textvariable=Entry2)
entry2.insert(0, '0')
entry2.config(state=DISABLED)
entry2.place(x=400, y=40)

psi2 = Label(window, text="P2 psi")
psi2.place(x=530, y=40)

entry3 = Entry(window, textvariable=Entry3)
entry3.insert(0, '0')
entry3.config(state=DISABLED)
entry3.place(x=400, y=70)

vol1 = Label(window, text="V1 Litres")
vol1.place(x=530, y=70)

entry4 = Entry(window, textvariable=Entry4)
entry4.insert(0, '0')
entry4.config(state=DISABLED)
entry4.place(x=400, y=100)

vol2 = Label(window, text="V2 Litres")
vol2.place(x=530, y=100)

entry5 = Entry(window, textvariable=Entry5)
entry5.insert(0, "Enter 0.7 - 1.4 ")
entry5.config()
entry5.place(x=400, y=200)

Factor = Label(window, text="Gas Factor (not functioning yet)")
Factor.place(x=400, y=230)


def CalcP1():
    if P1.get() == 0:
        if P2.get() == 1:
            if V1.get() == 1:
                if V2.get() == 1:
                    result = (Entry2.get() * Entry4.get()) / Entry3.get()
                    Entry1.set(result.__round__(2))
                    emptylabel2.config(text=result.__round__(2))
                    emptylabel3.config(text='psi P1 Precharge Pressure')


def CalcP2():
    if P1.get() == 1:
        if P2.get() == 0:
            if V1.get() == 1:
                if V2.get() == 1:
                    result = (Entry1.get() * Entry3.get()) / Entry4.get()
                    Entry2.set(result.__round__(2))
                    emptylabel2.config(text=result.__round__(2))
                    emptylabel3.config(text='psi P2 System Pressure')


def CalcV1():
    if P1.get() == 1:
        if P2.get() == 1:
            if V1.get() == 0:
                if V2.get() == 1:
                    result = (Entry2.get() * Entry4.get()) / Entry1.get()
                    Entry3.set(result.__round__(2))
                    emptylabel2.config(text=result.__round__(2))
                    emptylabel3.config(text='Litres V1 Precharge Gas Volume')


def CalcV2():
    if P1.get() == 1:
        if P2.get() == 1:
            if V1.get() == 1:
                if V2.get() == 0:
                    result = (Entry1.get() * Entry3.get()) / Entry2.get()
                    Entry4.set(result.__round__(2))
                    emptylabel2.config(text=result.__round__(2))
                    emptylabel3.config(text='Litres V2 Compressed Gas Volume')


def CalcVw():
    result2 = (Entry3.get() - Entry4.get())
    emptylabel4.config(text=result2.__round__(2))
    emptylabel5.config(text='Litres Vw Delta Fluid Volume')


emptylabel2 = Label(window, fg='red', font=('Arial', 10))
emptylabel2.place(x=10, y=200)

emptylabel3 = Label(window, fg='red', font=('Arial', 10))
emptylabel3.place(x=60, y=200)

emptylabel4 = Label(window, fg='red', font=('Arial', 10))
emptylabel4.place(x=10, y=230)

emptylabel5 = Label(window, fg='red', font=('Arial', 10))
emptylabel5.place(x=60, y=230)

CalcButton = Button(window, command=lambda: [CalcP1(), CalcP2(), CalcV1(), CalcV2(), CalcVw()], text='Calculate')
CalcButton.place(x=400, y=125)

Flow1 = DoubleVar()

Flow2 = DoubleVar()

flow1 = Entry(window, textvariable=Flow1)
flow1.insert(0, "0")
flow1.config(state=DISABLED)
flow1.place(x=400, y=350)

Sec1 = Label(window, text="Flow rate LPM")
Sec1.place(x=530, y=350)

flow2 = Entry(window, textvariable=Flow2)
flow2.insert(0, "0")
flow2.config(state=DISABLED)
flow2.place(x=400, y=420)

Sec2 = Label(window, text="Flow rate LPM")
Sec2.place(x=530, y=420)

PrechargeButton = Button(window, command=lambda: [Precharge(), timer()], text='Precharge')
PrechargeButton.config(state=DISABLED)
PrechargeButton.place(x=400, y=310)

ExpandButton = Button(window, command=lambda: [Discharge(), timer()], text='Expand/Discharge')
ExpandButton.config(state=DISABLED)
ExpandButton.place(x=400, y=445)

CompressButton = Button(window, command=lambda: [Compress(), timer()], text='Compress/Fill')
CompressButton.config(state=DISABLED)
CompressButton.place(x=400, y=375)

run1 = Checkbutton(window, text='Accu sim armed', variable=running, onvalue=1, offvalue=0)
run1.place(x=10, y=340)

SimButton = Button(window, command=lambda: [SimAct(), SimOff()], text='Ready')
SimButton.place(x=10, y=370)


def SimAct():
    if running.get() == 1:
        flow1.config(state=NORMAL)
        flow2.config(state=NORMAL)
        PrechargeButton.config(state=NORMAL)
        Armed.config(text="Sim ready", fg='green')


def SimOff():
    if running.get() == 0:
        flow1.config(state=DISABLED)
        flow2.config(state=DISABLED)
        CompressButton.config(state=DISABLED)
        ExpandButton.config(state=DISABLED)
        PrechargeButton.config(state=DISABLED)
        Armed.config(text="Sim off", fg='red')


canvas = Canvas(window, bg="White", highlightthickness=1, highlightbackground="black", height=60, width=375)
canvas.config(bg="light blue")
canvas.place(x=10, y=270)

CanvasWidth = 375
PistonWidth = 20

Piston = canvas.create_rectangle(20, 1, 0, 60, fill='green')
Fluid = canvas.create_rectangle(20, 1, 400, 60, fill='violet')

timeref = Label(window, text="Clock")
timeref.place(x=100, y=410)

Clock2 = Entry(window, textvariable=T_start)
Clock2.insert(0, '0')
Clock2.config(state=DISABLED)
Clock2.place(x=10, y=440)

Timestart = Label(window, text="Sim Start time")
Timestart.place(x=100, y=440)

T_stop = DoubleVar()

Clock3 = Entry(window, textvariable=T_stop)
Clock3.insert(0, '0')
Clock3.config(state=DISABLED)
Clock3.place(x=10, y=470)

Timestop = Label(window, text="Sim Stop time")
Timestop.place(x=100, y=470)

Clock4 = Entry(window, textvariable=T_elapse)
Clock4.insert(0, '0')
Clock4.config(state=DISABLED)
Clock4.place(x=200, y=440)

Timeelapse = Label(window, text="Sim run time")
Timeelapse.place(x=280, y=440)

Clock5 = Entry(window, textvariable=T_global)
Clock5.insert(0, '0')
Clock5.config(state=DISABLED)
Clock5.place(x=200, y=470)

Globalrun = Label(window, text="Global ref time")
Globalrun.place(x=280, y=470)

Armed = Label(window, text="")
Armed.place(x=280, y=340)


def Stop():
    running.set(0)
    Armed.config(text="Sim off", fg='red')


def Start_time():
    hour = time.strftime("%I")
    minute = time.strftime("%M")
    second = time.strftime("%S")
    milliseconds = (time.time() * 1000)
    millisecondsint = int(milliseconds)
    hourfloat = float(hour)
    minutefloat = float(minute)
    secondfloat = float(second)
    milliseconds2 = milliseconds - millisecondsint
    floattime = ((hourfloat * 3600) + (minutefloat * 60) + secondfloat + milliseconds2).__round__(3)
    T_start.set(floattime)


def Stop_time():
    hour = time.strftime("%I")
    minute = time.strftime("%M")
    second = time.strftime("%S")
    milliseconds = (time.time() * 1000)
    millisecondsint = int(milliseconds)
    hourfloat = float(hour)
    minutefloat = float(minute)
    secondfloat = float(second)
    milliseconds2 = milliseconds - millisecondsint
    floattime2 = ((hourfloat * 3600) + (minutefloat * 60) + secondfloat + milliseconds2).__round__(3)
    T_stop.set(floattime2)


def Precharge():
    CompressButton.config(state=NORMAL)
    Start_time()
    while running.get() == 1:
        Armed.config(text="Sim running", fg='orange')
        coordinates = canvas.coords(Piston)
        if coordinates[0] >= 355:
            Stop()
            Stop_time()
        canvas.move(Piston, 0.1, 0)
        canvas.move(Fluid, 0.1, 0)
        window.update()


def Compress():
    ExpandButton.config(state=NORMAL)
    Start_time()
    Timetomove_Compress = ((Entry3.get() - Entry4.get()) / Flow1.get() * 60)
    print(Timetomove_Compress)
    while running.get() == 1:
        Armed.config(text="Sim running", fg='orange')
        coordinates = canvas.coords(Piston)
        PISTONLIMIT = (Entry4.get() / Entry3.get()) * CanvasWidth
        if coordinates[0] <= PISTONLIMIT:
            Stop()
            Stop_time()
        canvas.move(Piston, [(Flow1.get() * (-1)) / 220], 0)
        canvas.move(Fluid, [(Flow1.get() * (-1)) / 220], 0)
        window.update()


def Discharge():
    Start_time()
    Timetomove_Expand = ((Entry3.get() - Entry4.get()) / Flow2.get() * 60)
    print(Timetomove_Expand)
    while running.get() == 1:
        Armed.config(text="Sim running", fg='orange')
        coordinates = canvas.coords(Piston)
        if coordinates[0] >= 355:
            Stop()
            Stop_time()
        canvas.move(Piston, [Flow2.get() / 220], 0)
        canvas.move(Fluid, [Flow2.get() / 220], 0)
        window.update()

Exit = Button(window, command=exit, text='Shut down')
Exit.place(x=570, y=470)


window.mainloop()

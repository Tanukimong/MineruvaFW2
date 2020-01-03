# Mineruva FW

개인 소장하고 있는 개조 FFF 프린터, 미네르바를 위한 각종 설정 모음

## 펌웨어
해당 펌웨어는 SKR MINI E3 v1.2와 TMC2209기반으로 구성되어 있음.
이는 smart reprap controller를 지원하지 않아 mks mini 12864 controller를 별도 구매해야할듯 함.

## 슬라이서 설정
### 큐라 설정
#### Machine setting
##### Printer setting
- X : 200
- Y : 177
- Z : 185
- Build plate shape = Rect.
- Heat bed : yes
- Heated build volume : no
- Origin at center : no
- G-code flavor : Marlin

##### Head setting
안 재봄

##### Start G-code
~~~
;Sliced at: {day} {date} {time}
;Basic settings: Layer height: {layer_height} Walls: {wall_thickness} Fill: {fill_density}
;Print time: {print_time}
;Filament used: {filament_amount}m {filament_weight}g
;Filament cost: {filament_cost}
;M190 S{print_bed_temperature} ;Uncomment to add your own bed temperature line
;M109 S{print_temperature} ;Uncomment to add your own temperature line
;흘러내리지 않도록 내려간 필라멘트를 당깁니다.
G21         ;metric values
G90         ;absolute positioning
M82         ;set extruder to absolute mode
M107        ;start with the fan off

M117 Leveling...

G28	    ;Home Axis
G29	    ;Auto Leveling

M117 Cleaning...

G12 P1 S1 T3; Nozzle cleaning

M117 Optimizing...

G1 X0 Y0 Z0.2 ;원점이동

G1 Z10 F200 E5 ;move the platform down 5mm

G92 E0      ;zero the extruded length

G1 F{travel_speed}

;Put printing message on LCD screen

M117 Printing...
~~~
##### End gcode
~~~
;End GCode
M104 S0         ;heater off
M140 S0         ;bed heater off
M107            ;fan off
G92 E0
G1 E-3 F500     ;retract the filament a bit
G92 E0
G1 Y200 F5500   ;move Y to 200
M84             ;steppers off
G90             ;absolute positioning
;{profile_string}
M117 Finished.
~~~

##### Nozzle Settings
- Nozzle size : 0.4mm
- Compatible material diameter : 1.75mm
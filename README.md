# Mineruva FW2

개인 소장하고 있는 개조 FFF 프린터, 미네르바를 위한 각종 설정 모음

## 개발자 연락처 및 정보
[신현호](https://tanukimong.github.io/online-cv)

## HW info
- 사용 메인보드 : [BIGTREETECH SKR-mini-E3](https://github.com/bigtreetech/BIGTREETECH-SKR-mini-E3)
- 사용 드라이버 : TMC2209 Uart

## 참고자료
- Marlin Firmware [2.0.x](https://github.com/MarlinFirmware/Marlin) + [Bugfix-2.0.x](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x)

## 사용법
해당 저장소를 Download한 후에, VScode에 내장된 PlamidIO를 이용하여 펌웨어를 업로드하시면됩니다.
업로드 방법은 단순히 생성된 bin파일을 보드 내에 내장된 MicroSD slot에 업로드하여 보드에 전원을 재인가하면 됩니다.
이 때, 핀의 위치를 바꾸어 전원 공급 방법을 External -> USB로 바꾸어 주어야, USB를 이용하여 **안전하게** 확인할 수 있습니다.

더 구체적인 방법은 [여기](https://youtu.be/oaXfXkPYHpw?t=144)에 있습니다.

## Cura 설정
### Printer setting
- X : 200
- Y : 177
- Z : 185
- Build plate shape = Rect.
- Heat bed : yes
- Heated build volume : no
- Origin at center : no
- G-code flavor : Marlin

### Head setting
- 버니어 캘리퍼스로 재보면 됨.

### Start Gcode
~~~
G1 E-5 F600; retract filament slightly

M117    Homing...
G28	    ;Home Axis

M117    Leveling...
G29	    ;Auto Leveling

M117 Cleaning...

G21 ;metric values
G90 ;absolute positioning

G1 Z15.0 F6000 ; Move up 15mm at 6000mm/min 
G92 E0 ; Reset extruder length to zero

G1 X0.0 Y0.0 F1000.0 ; go to edge of print area
G1 Z0.200 F1000.0 ; Go to Start Z position
G1 X20.0 E9.0 F1000.0 ; intro line
G1 X70.0 E21.5 F1000.0 ; intro line

G1 E-3 F600; retract filament slightly

G92 E0.0 ; reset extruder distance position

G92 E0      ;zero the extruded length

M117 Print start!
~~~

### End Gcode
~~~
G91
G1 F1800 E-3
G1 F3000 Z10
G90
G1 X150 Y250 ;Move to back

M140 S0; Turn off the bed heater
M104 S0; Turn off the nozzle heater
M106 S0; Turn off the cooling fan
M84; Turn off th motors

M117 Finished.
~~~



## Bug reports
- First layer가 뜨거나 베드를 긁는 경우 -> Z-offset값을 조정
    - Z-offset 감소 : Z=0일 때, 베드와 노즐이 더 가까워짐
    - Z-offset 증가 : Z=0일 때, 더 멀어짐

## 적용 내역
- S-curve acceleration
- Babystepping
- LCD 업그레이드
- Noctua FAN을 이용한 팬 소음 감소
- TMC2209기반으로 업그레이드
  - Coolstep
  - StallGuard
  - Dual z-axis 보정
  - BLtouch based auto leveling
  - Hybrid Mode(Stealth/SpreadChop)기능 추가
  - Crash detection 기반 HOMING 구현

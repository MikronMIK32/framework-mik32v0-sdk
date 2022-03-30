# framework-mik32v0-sdk

Репозиторий относится к микроконтроллеру MIK32 V0.

Содержание папок:

* openocd/share/openocd/scripts/ - скрипты отладчика OpenOCD
  * include_eeprom.tcl - скрипт прошивки встроенного EEPROM (передать название hex файла в функцию eeprom_write_file)
  * interface/ftdi/ - скрипты настройки эмуляторов JTAG, выбрать соответствующий Вашему отладчику
  * terget/ - скрипты настройки цели отладки (используйте mik32.cfg)
* shared - различные заголовочные файлы и код, относящийся к MIK32
  * include/ - заголовочные файлы ядра контроллера
    * mcu32_memory_map.h - карта памяти, маски тактирования шин, линии прерываний и DMA
  * ldscripts/ - скрипты линковщика
    * eeprom.ld - загрузка из ПЗУ
    * ram.ld - загрузка из ОЗУ
  * libs/ - библиотеки периферий
  * periphery/ - заголовочные файлы регистров периферий
  * runtime/crt0.S - код инициализации


Для загрузки программы по интерфейсу JTAG требуется OpenOCD. Скачать сборку под windows можно, например, отсюда: https://github.com/openocd-org/openocd/releases/tag/v0.11.0 . 
Далее достаточно скопировать содержимое openocd/share/openocd/scripts/ в соответствующую папку программы либо прописать путь в аргументе -s, который отвечает за установку пути папки скриптов openocd.

* Загрузка программы в ПЗУ:
  -s ../share/openocd/scripts -f interface/ftdi/m-link.cfg -f target/mcu32.cfg -f include_eeprom.tcl -c halt -c "eeprom_write_file filename.hex" -c "resume 0x0" -c "exit"
* Загрузка программы в ОЗУ:
  -s ../share/openocd/scripts -f interface/ftdi/m-link.cfg -f target/mcu32.cfg -c halt -c "load_image filename.elf 0x0 elf" -c "resume 0x0" -c "exit"
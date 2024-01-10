CFLAGS = -V -mmcs51 --model-large --xram-size 0x1800 --xram-loc 0x0000 \
	--code-size 0xec00 --stack-auto --Werror -Ichlib/src --opt-code-speed
CC     = sdcc
TARGET = moonsalt
OBJS  = main.rel cdc_device.rel command.rel controller.rel settings.rel \
  ch559.rel flash.rel gpio.rel hid.rel hid_dualshock3.rel \
	hid_guncon3.rel hid_keyboard.rel hid_mouse.rel hid_switch.rel hid_xbox.rel \
	led.rel serial.rel timer3.rel uart1.rel usb_device.rel usb_host.rel

.PHONY: all program run data prod clean build prod

all: build $(TARGET).bin

program: $(TARGET).bin
	ch559flasher -e -w $<

run: program
	ch559flasher -b

clean:
	rm -rf build $(TARGET).bin

.SILENT:
build:
	mkdir -p build

build/%.rel: %.c *.h chlib/src/*.h chlib/src/usb/*.h chlib/src/usb/hid/*.h
	$(CC) -c $(CFLAGS) -o $@ $<

build/%.rel: chlib/src/%.c chlib/src/*.h chlib/src/usb/*.h chlib/src/usb/hid/*.h
	$(CC) -c $(CFLAGS) -o $@ $<

build/%.rel: chlib/src/usb/%.c chlib/src/*.h chlib/src/usb/*.h chlib/src/usb/hid/*.h
	$(CC) -c $(CFLAGS) -o $@ $<

build/%.rel: chlib/src/usb/hid/%.c chlib/src/*.h chlib/src/usb/*.h chlib/src/usb/hid/*.h
	$(CC) -c $(CFLAGS) -o $@ $<

build/$(TARGET).ihx: $(addprefix build/,$(OBJS))
	$(CC) $(CFLAGS) $^ -o $@

%.bin: build/%.ihx
	sdobjcopy -I ihex -O binary $< $@

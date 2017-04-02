## Terminal output truncated to 80 symbols

For windows change the terminal shell path (File->Settings->Tools->Terminal) from cmd.exe to:

`cmd.exe "/K mode con:cols=120 lines=9999&cmd.exe"`


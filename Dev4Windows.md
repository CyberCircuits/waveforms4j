## What to Install on a Fresh System

1. Github Desktop
1. Pull Waveforms4j
1. Java 8 SDK
1. Mingw-w64
1. Waveforms 2015
1. IntelliJ

## First Sanity Check - Waveforms 2015

After downloading and installing Waveforms 2015, open it up and make sure it connects to the AD2 device and operates as expected.

## Second Sanity Check - Compile and Run SDK Example

Change permissions on `cd C:\Program Files\Digilent\WaveFormsSDK\samples` via the GUI.
Open up MinGW.

    cd C:\Program Files\Digilent\WaveFormsSDK\samples\c
    // gcc digitalout_pins.cpp -o digitalout_pins.out -L"C:\Windows\System32" -ldwf
    gcc digitalout_pins.cpp -o digitalout_pins.out -L"C:\Program Files\Digilent\WaveFormsSDK\lib\x64" -ldwf
    digitalout_pins.out

## Build Waveforms4J DLL for JNI

	cd C:\Users\Tim\Documents\GitHub\waveforms4j
	g++ -static -static-libgcc -static-libstdc++ -m64 -Wall -D_JNI_IMPLEMENTATION_ -I"C:\Program Files\Java\jdk1.8.0_121\include" -I"C:\Program Files\Java\jdk1.8.0_121\include\win32" -shared -o waveforms4j.dll ./c/org_knowm_waveforms4j_DWF.cpp -L"C:\Program Files\Digilent\WaveFormsSDK\lib\x64" -ldwf
    move ./waveforms4j.dll ./src/main/resources

Note: the key to solving the UnsatifiedLinkError was here: <http://stackoverflow.com/questions/18138635/mingw-exe-requires-a-few-gcc-dlls-regardless-of-the-code>.

I used the program Dependency Walker to figure out that the waveforms4j dll depended on `libstdc++-6.dll` and it could not link it during run time. The `-static -static-libgcc -static-libstdc++` options link it statically during compilation.
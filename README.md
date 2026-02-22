# Raspberry Pi 5
This repo contains C# software for Raspberry Pi 5. My variant has 8 GB RAM.

![Photo](images/PI5.png)

### Board Components

![Photo](images/Pi5-components.jpg)

### GPIO Pinout

![Photo](images/Pi5-gpio-pinout.png)

## Operating System

The Raspberry Pi 5 can run several OSs. The most popular are Raspian and Ubuntu. \
The software projects in this repo are written for Ubuntu.

## Login

The hostname of the Raspberry Pi 5 is **raspberrypi**. \
User: bcs \
Password: *The usual one*

You can login using ssh.

## Installing .NET SDK

Install the latest .NET SDK with apt install like this

    sudo apt install dotnet-sdk-10.0

Verify the .NET installation with the following command:

    dotnet --version

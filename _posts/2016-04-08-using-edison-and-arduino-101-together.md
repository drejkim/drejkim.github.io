---
layout: post
title: Using Edison and Arduino 101 together
categories: [embedded, iot, web]
---

## Introduction

I recently got my hands on [Arduino 101](https://www-ssl.intel.com/content/www/us/en/do-it-yourself/arduino-101.html), which combines Intel's compute capabilities with Arduino's ease-of-use. The board contains the Intel® Curie™ module, a button-sized computer that has a 6-axis IMU (interial measurement unit, i.e. accelerometer and gyroscope) and Bluetooth Low Energy (BLE).

For my first project with Arduino 101, I wanted to combine it with Edison. The two boards complement each other very well. Arduino 101 is a low power sensor module&mdash;it comes equipped to measure accelerometer / gyroscope data, but you can easily add other sensors to measure whatever your application needs. Meanwhile, Edison has built-in BLE and Wi-Fi capabilities, as well as enough compute power to gather data from multiple sensor modules, do some basic processing, and send to the cloud. The cloud is then responsible for intensive calculations, analytics, etc.

Hopefully, this sounds vaguely familiar&mdash;what I've described is the Internet of Things (IoT).

<figure class="figure">
  <img class="figure-img img-fluid border rounded" src="https://media.githubusercontent.com/media/estherjk/estherjk.github.io/master/assets/img/edison/edison-arduino101-iot-conceptual.png" alt="Conceptual IoT diagram with Edison and Arduino 101">
  <figcaption class="figure-caption text-center">Conceptual IoT diagram with Edison and Arduino 101</figcaption>
</figure>

## How the demo works

My demo illustrates a simple IoT example by displaying an Arduino 101's IMU (accelerometer / gyroscope) data on a web page. Edison is used to receive the data from the Arduino 101 via BLE (Bluetooth Low Energy), then sends it to a web server via WebSockets. The demo uses a single Arduino 101, but it is extensible to a collection of Arduino 101s, as I described above. The web server is also responsible for hosting a web page that displays the IMU data; any connected browser can then view this information.

Here's a picture that illustrates the demo's archiecture:

<div class="text-center">
  <figure class="figure">
    <img class="figure-img img-fluid border rounded" src="https://raw.githubusercontent.com/estherjk/edison-arduino101-iot/master/images/edison-arduino101-iot.png" alt="Demo archiecture diagram">
    <figcaption class="figure-caption text-center">Demo archiecture diagram; see <a class="text-dark" href="https://github.com/estherjk/edison-arduino101-iot">GitHub</a> for the source code</figcaption>
  </figure>
</div>

### BLE on Arduino 101

The first step involves broadcasting the IMU data on Arduino 101 using the BLE specification. The [CurieBLE library](https://www.arduino.cc/en/Reference/CurieBLE) has a great introduction on how to do this.

### Receiving and sending data on Edison

To read the data on Edison from Arduino 101, I used [noble](https://github.com/sandeepmistry/noble), a Node.js BLE central module. Noble makes it really easy to discover and access data from BLE peripherals. The data is then passed to the web server with [socket.io-client](https://github.com/socketio/socket.io-client). In this case, Edison is acting as a client to the web server, which is why socket.io-client is used.

### Displaying the data on a web page

The web server&mdash;which also uses Node.js and [socket.io](http://socket.io/)&mdash;is responsible for receiving data from Edison. From here, the web server can do advanced processing and analytics... but for this demo, it hosts a web page that displays the IMU data. Note that the web page is also a socket.io client&mdash;like Edison&mdash;but instead of sending data to the web server, its receiving and displaying the data.

<div class="text-center">
  <figure class="figure">
    <img class="figure-img img-fluid border rounded w-50" src="https://media.githubusercontent.com/media/estherjk/estherjk.github.io/master/assets/img/edison/edison-arduino101-iot-web-client.png" alt="Demo archiecture diagram">
    <figcaption class="figure-caption text-center">Screenshot of the web client</figcaption>
  </figure>
</div>

### Putting it all together

To see how it all works, check out the [source code](https://github.com/estherjk/edison-arduino101-iot) and video:

<div class="mb-3">
  <div class="embed-responsive embed-responsive-16by9">
    <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/NRZZRsqJQWY" frameborder="0" allowfullscreen=""></iframe>
  </div>
</div>

## Conclusion

Using Arduino 101 in combination with Edison can yield all sorts of interesting IoT applications. Hopefully, this project will help you get started in building your own!

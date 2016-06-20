# How to use "BLE Discovery (BLUETOOTH LOW ENERGY)"

----------

##  1. Overview

- BLE stands for Bluetooth low energy. 
- BLE is the mechanism to discover TV's using Bluetooth.

## 2. Android Guide 

### Start BLE Discovery
To start BLE discovery we need to Search object to discover TV's using startUsingBle() api of Smartview SDK. 

     Search search = com.samsung.multiscreen.Service.search(this);
     search.startUsingBle();

### Register Listener for getting TV Device list

To get the discovered devices we need to register search object with setOnBleFoundListener listener. If BLE discovers any device, it notify through onFound() method of OnBleFoundListener.

    search.setOnBleFoundListener(new Search.OnBleFoundListener() {
        @Override
        public void onFoundOnlyBLE(String tvname) {
            bleTVList.add(tvname);
            Log.i(TAG,"TV found isActivityRunning(getApplicationContext()"+isActivityRunning(getApplicationContext())));
            showNotification(tvname);
        	onTimerTick(tvname);
        }
    });

### Stop BLE Discovery

To stop BLE discovery, call stopUsingBLE() api of SmartView SDK.

    search.stopUsingBle(); 


## 3. iOS Guide

### Create search object 

    search = Service.search();

### Start discovery

This API start searching for TV using BLE search.

    search.startUsingBLE();

### OnFound Callback:

This callback method is invoked when the TV is using BLE search.

    func onFoundUsingBle(service : Service)



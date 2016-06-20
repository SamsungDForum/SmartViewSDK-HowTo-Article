# How to use "WoW (Wake on Wireless)"

----------

## 1. Overview  

With the help of MAC Address of the TV, the application on the phone is able to switch ON the TV which is in STAND BY Mode  The SDK provides an API through which this functionality can be used.

Above Mobile Library Android 2.3 and iOS 2.2.2  

 Mobile App (Android and iOS)    

Requires the MAC Address of the TV which needs to be powered ON

 

Check Smart View SDK UX guideline for implementing WoW API.

## 2. Android Guide

### WoW API:

	WakeOnWirelessLan(String macAddr)  
	
	where: macAddr Mac Address of TV
	
	WakeOnWirelessAndConnect(String macAddr, final Uri uri, final Result<Service> connectCallback)
	
	where:
	
	macAddr - mac address of TV            
	
	uri  - uri of service           
	
	connectCallback - callback to invoke with the result of the connect request
	
	WakeOnWirelessAndConnect(String macAddr, final Uri uri, int timeout, final Result<Service> connectCallback)
	
	where:
	
	macAddr - mac address of TV             
	
	uri  - uri of service            
	
	connectCallback - callback to invoke with the result of the connect request            
	
	timeout - timeout for service connect request    

### 1)  How to get MAC address:

Service service.getDeviceInfo(new Result<Device>() {    

    @Override 

    public void onSuccess(Device device) {                           

        if (device != null)                                                                       

           String wifiMac = device.getWifiMac();  

          SharedPreferences.Editor editor = getSharedPreferences("WoWMacs", 0).edit();                           

          editor.putString(name, wifiMac);                           

          editor.commit();                      

        }    

     @Override 

     public void onError(com.samsung.multiscreen.Error error) {

     } });

### 2) Wake up TV :

	 String mac = getSharedPreferences("WoWMacs", 0).getString(service.getName(), 0);
	
	 Service.WakeOnWirelessLan(mac);

 

### 3) Wake up TV and connect :

	Service.DEFAULT_WOW_TIMEOUT_VALUE = 60000; //default timeout of connection
	
	String mac = getSharedPreferences("WoWMacs", 0).getString(service.getName(), 0);
	
	String uri = getSharedPreferences("WoWIps", 0).getString(mac, 0);
	
	Service.WakeOnWirelessAndConnect(mac, uri, new Result<Service>() {    

		@Override 

		public void onSuccess(Service service) {
			if (service != null) {
				Application application = service.createApplication(http://dev-multiscreen-examples.s3-website-us-west-1.amazonaws.com/examples/helloworld/tv/,  "com.samsung.multiscreen.msf20");
			} 

        }     
		
		@Override  
		public void onError(com.samsung.multiscreen.Error error) {  
			// error logger
		} 
	});

	int timeout = 120000;	//Connect using custom timeout

	Service.WakeOnWirelessAndConnect(mac, uri, timeout, new Result<Service>()

        @Override

        public void onSuccess(Service service) {

			if (service != null) {

                Application application = service.createApplication(http://dev-multiscreen-examples.s3-website-us-west-1.amazonaws.com/examples/helloworld/tv/,

                "com.samsung.multiscreen.msf20");

			}

		}

        @Override

        public void onError(com.samsung.multiscreen.Error error) {
			// error logger
        } 
	}); 

### 4) Make WoW TV list:

	ListView wowList = (ListView)findViewById(R.id.wowListView); // create Wow TV list

	standByListAdapter = new StandByModeTVListAdapter(this, R.layout.listview_item); wowList.setAdapter(standByListAdapter);  

	ListView tvList = (ListView)findViewById(R.id.tvListView); // create available TV list

	tvListAdapter = new TVListAdapter(this, R.layout.listview_item); tvList.setAdapter(tvListAdapter);

	SharedPreferences WoWMacs  = getSharedPreferences("WoWMacs", 0); // stored names and mac addresses of connected before TVs 

	SharedPreferences WoWIps  = getSharedPreferences("WoWIps", 0); // stored names and uri of connected before TVs

	SharedPreferences WoWIds  = getSharedPreferences("WoWIds", 0); // stored mac addresses and network's ids of connected before TVs to show available TVs only

	if (WoWMacs!=null && WoWIps!=null && WoWIds!=null) {

        Map<String, String> macsList = (Map<String, String>) WoWMacs.getAll(); // get all stored TV's macs

        Set<String> names = macsList.keySet(); // get all stored TV's names

        for (String name : names) {

            String mac = WoWMacs.getString(name, null);  // get TV mac according to name

            String uri = WoWIps.getString(mac, null); // get TV uri according to mac

            int wifiId = WoWIds.getInt(mac, 0); // get network id according to mac

            if (mac != null && null != uri  && !tvListAdapter.contains(uri) // check whether TV is on or off

                && wifiId == wifiManager.getConnectionInfo().getNetworkId() // check whether TV is in network or not) {  

                if (!ListWoW.contains(name + " (standby) : " + mac)) {  // check regarding duplicates

                      ListWoW.add(name + " (standby) : " + mac);

                      standByListAdapter.add(name + " (standby) : " + mac);

                      standByListAdapter.notifyDataSetChanged();

				}       
			}    
		}
	}

## 3. iOS Guide

### WoW API:

	// Send a packet for WakeOnWirelessLan. func WakeOnWirelessLan(macAddr:String)         

	where: macAddr - MAC address of TV

	// Send a packet via WakeOnWirelessLan and create and connect to particular appilcation

	func WakeOnWirelessAndConnect(macAddr:String, uri:String, timeOut:NSTimeInterval, completionHandler: (service:Service? , error: NSError? ) -> Void) -> Void

	where:

	macAddr - MAC address of TV
	uri     - URI of service
	completionHandler - callback to invoke with the result of connect request

	// get the service of wake up TV

	func WakeUpAndConnect(uri:String, completionHandler: (service:Service?, error:NSError?) -> Void)      

      where:
      uri - URI of service
      completionHandler - callback to invoke with the result of connect request                  

### 1. How to get MAC Address:

	service!.getDeviceInfo(5, completionHandler:
	{
	
		(deviceInfo, error) -> Void in        
	
		let device = deviceInfo!["device"]
	
		let wifi = device!["wifiMac"]
	
	})
          

### 2. Wake up TV:

	Service.WakeOnWirelessLan(macAddr) 

### 3. Wake up TV and connect:

	Default timeout for connection is used Service.DEFAULT_WOW_TIMEOUT_VALUE:NSTimeInterval = 6

	Service.WakeOnWirelessAndConnect(macAddr, uri, completionHandler: {(service, error) -> Void in

		if(service != nil) {

			let url = NSURL(string: "http://dev-multiscreen-examples.s3-website-us-west-1.amazonaws.com/examples/helloworld/tv/")

            let app = service?.createApplication(url!, channelURI: "com.samsung.multiscreen.msf20", args: nil)

            app?.delegate = self

            app?.connect()

		}         
	})

	//Connect using custom timeout

	//let timeout:NSTimeInterval = 12

	Service.WakeOnWirelessAndConnect(macAddr, uri, timeout, completionHandler: {(service, error) -> Void in

		.....    
		.....           
	})

   

 

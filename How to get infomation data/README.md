# How to get information data

----------

This information data make base on 2016 TV.

You can get useful data each step.



##1.Service

:  get step 1.1) : all of TV's Service info, this is used to display TV's list usually.

:  get step 2) : get selected TV's Service info

	Service (
	
	    id=F8:04:2E:E9:0F:7D,
	
	    version=2.0.25,
	
	    name=[TV] Homemj,
	
	    type=Samsung SmartTV,
	
	    uri=http://192.168.0.75:8001/api/v2/
	
	)

 

 

##2.Device

 :  mService.getDeviceInfo()  

 :  get any time after step 2) 

	Device
	
	(    
	
	    duid=uuid:acdfc511-b3c4-40c0-a892-b40e7acbc48e,
	
	    model=16_JAZZM_UHD,     

	    description=Samsung DTV RCR,
	
	    networkType=wireless, 

	    ssid=90:9f:33:a4:0b:bc,

	    ip=192.168.0.75,
	
	    firmwareVersion=Unknown,
	
	    name=[TV] Homemj,
	
	    id=F8:04:2E:E9:0F:7D,     

	    udn=uuid:acdfc511-b3c4-40c0-a892-b40e7acbc48e,
	
	    resolution=1920x1080, 

	    countryCode=KR
	
	)

 

 

##3.ApplicationInfo

: Application.getInfo() 
: get any time after step 4), running value is depend on application.connect() API call.  

: TV app infomation in config.xml and running state.

	ApplicationInfo
	
	(    
	
	    id=0rLFmRVi9d.youtubetest,
	
	    running=false,
	
	    name=Youtube MSF Test,
	
	    version=0.2.1
	
	 )


##4.sequential code flow

	//1. MSF Search

    mSearch = Service.search(mContext);

    mSearch.start();

        mSearch.setOnServiceFoundListener(
                new Search.OnServiceFoundListener() {
                    @Override
                    public void onFound(Service service) {
                        //1.1 add device
                    }
                }
        );

        mSearch.setOnServiceLostListener(
                new Search.OnServiceLostListener() {
                    @Override
                    public void onLost(Service service) {
                        //1.2 remove device
                    }
                }
        );
 

	//2. Select TV in TV List and use "Service"
    mService =mTVListAdapter.getItem(which);

	//3. Get DeviceInfo if you need
    mService.getDeviceInfo(new Result<Device>() {
        @Override
        public void onSuccess(Device device) {
            Log.d(TAG, "getDeviceInfo " + device.toString());
        }
    });

 
	//4. Create Appication
    mApplication = mService.createApplication(mApplicationId, mChannelId);
 

	//5. Get AppicationInfo if you need
    mApplication.getInfo(new Result<ApplicationInfo>() {
        @Override
        public void onSuccess(ApplicationInfo applicationInfo) {
            Log.d(TAG, "getInfo " + applicationInfo.toString());
        }

        @Override
        public void onError(com.samsung.multiscreen.Error error) {
            if (error.getCode() == 404) {
                //Install the application on the TV. 
                //Note: This will only bring up the installation page on the TV. 
                //The user will still have to acknowledge by selecting "install" using the TV remote.
                application.install(new Result<Boolean>() {
                    @Override
                    public void onSuccess(Boolean result) {
                        Log.d(LOGTAG, "Application.install onSuccess() " + result.toString());
                    }
                });  
            }
        });
    });
 

	//6. Launch installed app or cloud app
    mApplication.connect(new Result<Client>() {
        @Override
        public void onSuccess(Client client) {
            Log.d(TAG, "application.connect onSuccess " + client.toString());
        }

        @Override
        public void onError(com.samsung.multiscreen.Error error) {
            if (error.getCode() == 404) {
                //Install the application on the TV. 
                //Note: This will only bring up the installation page on the TV. 
                //The user will still have to acknowledge by selecting "install" using the TV remote.
                application.install(new Result<Boolean>() {
                    @Override
                    public void onSuccess(Boolean result) {
                        Log.d(LOGTAG, "Application.install onSuccess() " + result.toString());
                    }
                });     
            }
        });
    } 

	//7. communication with tv app  

	//7.1 send
    mApplication.publish("event", messageData.toString());

	//7.2 receive
    mApplication.addOnMessageListener("event", new Application.OnMessageListener() {
        @Override
        public void onMessage(Message message) {
            Log.d(TAG, "addOnMessageListener event control :  " + message.toString());
        }
    });


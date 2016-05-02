# How to use install() API 
----------

Mobile app can make TV to show of webapp installation page when TV webapp is not exist.

Install() API is possible to make this scenario.

Also, you have 2 times chance to call install().




##1.getinfo() 

:  You call getinfo() then  you receviced onError code '404' that means your TV webapp not exist.

 

 

##2.connect()

 :  You call connect()  then you receviced onError code '404' that means your TV webapp not exist.

 

 

##3.code flow

	//1. Create Appication
	mApplication = mService.createApplication(mApplicationId, mChannelId);
	
	//2. Get AppicationInfo if you need
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
	
	
	//3. Launch installed app or cloud app
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

# How to use "StartArg"

----------

## 1. OverView
> 
> #### *"Authentication should ONLY be passed with secure connection"(coming soon)* ####
> 

- StartArg can be 'login user info','deeplink','AuthToken' etc..

- Above Mobile Library Android 2.0.18 and iOS 2.2.2

- Mobile App(Android) 

    ***MUST write "id" startArgs map's key.***

    ex) simple string ("id","aaa");

    ex) json ,("id",data{"id":"XX","pw":"1234","pairing_code":"XX@1234"}

## 2. Android Guide

- String Type

		Map<String,Object> startArgs1 = new HashMap<String, Object>();
		Object payload = "aaa";
		startArgs1.put(Message.PROPERTY_MESSAGE_ID,payload);
		mApplication = mService.createApplication(mApplicationId, mChannelId,startArgs1);
 
 
- JSONObject Type

		JSONObject test2 = new JSONObject();
		try {
		    test2.put("userID","XX");
		    test2.put("userPW","1234");
		    test2.put("pairCode","XX@1234");
		} catch (JSONException e) {
		    e.printtackTrace();
		}
		Map<String,Object> startArgs2 = new HashMap<String, Object>();
		Object  payload = test2.toString;
		startArgs2.put(Message.PROPERTY_MESSAGE_ID,payload);
		mApplication = mService.createApplication(mApplicationId, mChannelId,startArgs2); 
 

## 3. iOS Guide

- Swift

		let  dic:Dictionary  = ["userID":"XX", "userPW":"1234", "pairCode":"XX@1234"]
		
		app = service.createApplication(appURL,channelURI: channelId, args: dic)


- Object-C

		NSMutableDictionary* dic = [NSMutableDictionary dictionary];
		[dic setValue:@"XX" forKey:@"userID"];
		[dic setValue:@"1234" forKey:@"userPW"];
		[dic setValue:@"XX@1234" forKey:@"pairCode"];
		
		
		self.app = [service createApplication:self.appId channelURI:self.channelId args:dic];
  
 

## 4. TV Web App

- ***To received "Start Args", MUST write "PAYLOAD" to get Data***

		 msf.local(function(err, service){
		  var reqAppControl = tizen.application.getCurrentApplication().getRequestedAppControl();
		  log("reqAppControl =" + reqAppControl + ", appControl = "+reqAppControl.appControl);
		  if (reqAppControl && reqAppControl.appControl) {
		      var data = reqAppControl.appControl.data;
		      
		      for (var i = 0; i < data.length; i++) {
		       log("WRT param #" + i + " key : " + data[i].key);
		          for (var j = 0; j < data[i].value.length; j++) {
		              console.log("WRT param #" + i + " #" + j + " value : " + data[i].value[j]);
		          }
		          if (data[i].key == "PAYLOAD") {
		           log("payload =" + data[i].value[0]);
		           var payload = data[i].value[0];
		          }
		      }
		  }
		 

 

 

 

 


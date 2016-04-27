# How to use "StartArg"

----------

## 1. OverView
> 
> #### *"Authentication should ONLY be passed with secure connection"(coming soon)* ####
> 

- StartArg can be 'login user info','deeplink','AuthToken' etc..

- Above Mobile Library Android 2.0.18 and iOS 2.2.1

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
		Object  payload = test2.toString();
		startArgs2.put(Message.PROPERTY_MESSAGE_ID,payload);
		mApplication = mService.createApplication(mApplicationId, mChannelId,startArgs2); 
 

## 3. iOS Guide

- JSONObject type

        var app: Application?
        let  dic:NSDictionary  = ["userID":"XX", "userPW":"1234", "pairCode":"XX@1234"]
        do {
            let data =  try NSJSONSerialization.dataWithJSONObject(dic, options: NSJSONWritingOptions(rawValue: 0))
            
            let dataString = NSString( data: data, encoding: NSUTF8StringEncoding )
            
            let startArgs:[String:AnyObject] = ["id":dataString!]
            
            app = service.createApplication(NSURL(string: appURL)!, channelURI: channelId, args: startArgs)
            app!.delegate = self
            app?.connect()
        }
        catch
        {
            print("error")
        }
 

  
 

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
		 

 

 

 

 


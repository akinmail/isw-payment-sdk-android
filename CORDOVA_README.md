# Table of Contents
1. [Cordova Payment Plugin for Android](#CordovaPayment)
  * [Download the latest Plugin](#DownloadPlugin)
  * [Creating a Project and Adding Plugin](#CreateProject)
  * [Using The Plugin in Sandbox Mode](#SandBoxMode)
  
2. [Using the Plugin with UI (In PCI-DSS Scope: No )](#SDKWithUI)   
   * [Pay with Card/Wallet](#Pay)
   * [Pay with Card](#PayWithCard)
   * [Pay with Wallet](#PayWithWallet)
   * [Validate Card](#ValidateCard)
   * [Pay With Token](#PayWithToken)
   
3. [Using the Plugin without UI (In PCI-DSS Scope: Yes)](#SDKWithOutUI)
   * [Pay with Card/Token](#PayWithCardToken)
   * [Pay with Wallet](#PayWithWalletNoUI)
   * [Validate Card and Get Token](#ValidateCardNoUI)
   * [Authorize Transaction With OTP](#AuthorizeOTP)
   * [Checking Payment Status](#PaymentStatus)
  
## <a name='CordovaPayment'></a> Cordova Payment Plugin for Android
Interswitch payment SDK allows you to accept payments from customers within your mobile application.
**Please Note: *The current supported currency is naira (NGN), support for other currencies would be added later***
The first step to ​using the plugin is to register as a merchant. This is described [here] (merchantxuat.interswitchng.com)

### <a name='DownloadPlugin'></a> Download the latest plugin using CLI (Command Line Interface)

### <a name='CordovaProject'></a> Create a new Cordova Project using CLI (Command Line Interface)
1. To create a new project in cordova refer to the documentation [here](https://cordova.apache.org/docs/en/latest/guide/cli/index.html)
2. Add the cordova-payment-plugin from CLI, using this command **cordova plugin add path/to/cordova-payment-plugin**
**Please Note: *Ensure your cordova.js file is the first js file in your index.html***


### <a name='SandBoxMode'></a> Using The Plugin in Sandbox Mode

During development of your app, you should use the Plugin in sandbox mode to enable testing. Different Client Id and Client Secret are provided for Production and Sandbox mode. The procedure to use the Plugin on sandbox mode is just as easy:

* Use Sandbox Client Id and Client Secret got from the Sandbox Tab of the Developer Console after signup (usually you have to wait for 5 minutes after signup for you to see the Sandbox details) everywhere you are required to supply Client Id and Client Secret in the remainder of this documentation              
* In your code, override the api base as follows
```javascript
    function init(){
    	var userDetails = {
    	    clientId :"IKIAF8F70479A6902D4BFF4E443EBF15D1D6CB19E232",
    	    clientSecret : "ugsmiXPXOOvks9MR7+IFHSQSdk8ZzvwQMGvd0GJva30=",
    	    paymentApi : "https://sandbox.interswitchng.com",
    	    passportApi : "https://sandbox.interswitchng.com/passport"
    	}
    	var initial = PaymentPlugin.init(userDetails);				 
    	initial.done(function(response){    			
    	    alert(response); // success response if the initialization was successful
    	});
    	initial.fail(function (response) {    			
    		alert(response); // error response if the initialization failed
    	});
    }
```

* Follow the remaining steps in the documentation.
* call the init function inside the onDeviceReady function of your cordova app
* NOTE: When going into Production mode, use the Client Id and the Client Secret got from the Production Tab of Developer Console instead.

## <a name='SDKWithUI'></a>Using the Plugin with UI (In PCI-DSS Scope: No )

**NOTE: *To use the inapp change your manifest theme to android:theme="@style/Theme.AppCompat.Light"***

### <a name='Pay'>Pay with Card/Wallet

* To allow for Payment with Card or Wallet
* Create a Pay button
* In the onclick event of the Pay button, use this code
1. Set up payment request like this: 
```javascript
    var payRequest = {			
        amount : 100, // Amount in Naira
        customerId : 1234567890, // Optional email, mobile no, BVN etc to uniquely identify the customer.
        currency : "NGN", // ISO Currency code
        description : "Purchase Phone" // Description of product to purchase
    }
```
2. Create a button to make payment and use this code in the onclick event of the button
```javascript
    var pay = PaymentPlugin.pay(payRequest);
    pay.done(function(response){
        alert(response); // transaction success reponse
    });
    pay.fail(function (response) {           
        alert(response); // transaction failure reponse
    });
```

### <a name='PayWithCard'>Pay with Card

* To allow for Payment with Card only
* Create a Pay button and set the payment request
*Set up payment request like this: 
```javascript
    var payWithCardRequest = {			
        amount : 100, // Amount in Naira
        customerId : 1234567890, // Optional email, mobile no, BVN etc to uniquely identify the customer.
        currency : "NGN", // ISO Currency code
        description : "Purchase Phone" // Description of product to purchase
    }
```
* In the onclick event of the Pay button, use this code.
```javascript
    var payWithCard = PaymentPlugin.payWithCard(payWithCardRequest);
    payWithCard.done(function(response){
        alert(response); // transaction success reponse
    });
    payWithCard.fail(function (response) {        
        alert(response); // transaction failure reponse
    });
```

### <a name='PayWithWallet'>Pay With Wallet

* To allow for Payment with Wallet only
* Create a Pay button and set the payment request
* Set up payment request like this: 
```javascript
    var payWithWalletRequest = {			
        amount : 100, // Amount in Naira
        customerId : 1234567890, // Optional email, mobile no, BVN etc to uniquely identify the customer.
        currency : "NGN", // ISO Currency code
        description : "Purchase Phone" // Description of product to purchase
    }
```
* In the onclick event of the Pay button, use this code.
```javascript
    var payWithWallet = PaymentPlugin.payWithWallet(payWithWalletRequest);				 
    payWithWallet.done(function(response){				 
        alert(response); // transaction success reponse
    });
    payWithWallet.fail(function (response) {        			
        alert(response); // transaction failure reponse
    });
```

### <a name='ValidateCard'></a>Validate Card

* Validate card is used to check if a card is a valid card, it returns the card balance and token
* Set up payment request like this: 
```javascript
    var validateCardRequest = {
        customerId : 1234567890 // Optional email, mobile no, BVN etc to uniquely identify the customer
    }
```
* To call validate card, use this code.
```javascript
    var validateCard = PaymentPlugin.validatePaymentCard(validateCardRequest);
    validateCard.done(function(response){         
        alert(response); // transaction success reponse
    });
    validateCard.fail(function (response) {        
        alert(response); // transaction failure reponse
    });
```

### <a name='PayWithToken'></a> Pay with Token

* To allow for Payment with Token only
* Create a Pay button
* Set up payment request like this: 
```javascript
    var payWithTokenRequest = {
        pan : 5123459987670669364, //Token
        amount : 100, // Amount in Naira
        currency : "NGN", // ISO Currency code		
        cardtype : "Verve", // Card Type	
        expiryDate : 2004, // Card or Token expiry date in YYMM format
        customerId : 1234567890,	// Optional email, mobile no, BVN etc to uniquely identify the customer.	
        panLast4Digits : 7499,		//Last 4digit of the pan card
        description : "Pay for gown"
    }
```
* In the onclick event of the Pay button, use this code.
```javascript
    var payment =PaymentPlugin.payWithToken(payWithTokenRequest);	
	payment.done(function(response){
		alert(response); // transaction success reponse
	});
	payment.fail(function (response) {		
		alert(response); // transaction failure reponse
	});
```

## <a name='SDKWithOutUI'></a>Using the Plugin without UI (In PCI-DSS Scope: Yes)

### <a name='PayWithCardToken'></a>Pay with Card/Token

* To allow for Payment with Card or Token
* Create a UI to collect amount and card details
* Create a Pay button
* Set up payment request like this:

```javascript
    var purchaseRequest = {
        pan:5060990580000217499,  //Card No or Token
        amount : 100, // Amount in Naira
        cvv : 111, // Card CVV
        pin : 1111, // Optional Card PIN for card payment
        currency : "NGN", // ISO Currency code
        expiryDate : 2004, // Card or Token expiry date in YYMM format
        customerId : 1234567890 // Optional email, mobile no, BVN etc to uniquely identify the customer.
    }
```


```javascript    
    var payment = PaymentPlugin.makePayment(purchaseRequest);
    payment.done(function(response){    
    var obj = JSON.parse(response);
        //the response object here contains amount, message, transactionIdentifier and transactionRef			        
        alert(response);				
    });
    payment.fail(function (response) {			              
        alert(response); // transaction failure reponse
    });
```

### <a name='PayWithWalletNoUI'></a>Pay with Wallet
* To allow for Payment with Wallet only
* Create a UI to collect amount, CVV, expiry date and PIN and to display user's Payment Method(s). Use the code below to load the Payment Method(s)
```javascript    
    var paymentItems=PaymentPlugin.loadWallet();
    paymentItems.done(function(response){    
    alert("Wallet loaded successfully");
    var responseObject = JSON.parse(response); 
    // The response object here contains cardProduct, panLast4Digits and token, the token is used for making payment
    // Load the cardProduct on a dropdown list and use the token in making payment with wallet 
        for(var i=0; i<response.length; i++){
            console.log(responseObject.paymentMethods[i].token);
            console.log(responseObject.paymentMethods[i].panLast4Digits);
            console.log(responseObject.paymentMethods[i].cardProduct);
        } 
    });
    paymentItems.fail(function (response) {           			
        alert(response);	// transaction failure reponse	
    });
```

* Create a Pay button
* Set up payment request like this:

```javascript
    var walletRequest = {
        pan:ADA4C1FFE6DE40C584ABD3CBAFDA0D08,  //Token from the wallet
        amount : 100, // Amount in Naira
        cvv : 111, // Card CVV
        pin : 1111, // Optional Card PIN for card payment
        currency : "NGN", // ISO Currency code
        expiryDate : 2004, // Card or Token expiry date in YYMM format
        requestorId : 1234567890,
        customerId : 1234567890 // Optional email, mobile no, BVN etc to uniquely identify the customer.
    }
```
* In the onclick event of the Pay button, use this code.
```javascript
    var payment = PaymentPlugin.payWithWalletSDK(walletRequest);				 
    payment.done(function(response){                 
         alert(response);  // transaction success reponse
    });
    payment.fail(function (response) {               
        alert(response);   // transaction failure reponse
    });
```


### <a name='ValidateCardNoUI'></a> Validate Card and Get Token
* To check if a card is valid and get a token
* Create a UI to collect card details
* Create a Validate/Add Card button
* Set up validate card request using this code

```javascript
    var validateCardRequest = {
        pan : 5060990580000217499,  //Token from the wallet				
        cvv : 111, // Card CVV
        pin : 1111, // Optional Card PIN for card payment
        expiryDate : 2004, // Card or Token expiry date in YYMM format
        customerId : 1234567890 // Optional email, mobile no, BVN etc to uniquely identify the customer.
    }
```

* In the onclick event of the Validate/Add Card button, use this code.

```javascript
    var validateCard = PaymentPlugin.validateCard(validateCardRequest);
    validateCard.done(function(response){								              
        alert(response);  // transaction failure reponse
    });
    validateCard.fail(function (response) {        			        
        alert(response);
        // The response object contains fields transactionIdentifier, 
        // message,token, tokenExpiryDate, panLast4Digits, otpTransactionIdentifier
        // transactionRef and cardType. 
        // Save the token, tokenExpiryDate, cardType and panLast4Digits 
        // in order to pay with the token in the future.
    });
```

## <a name='AuthorizeOTP'></a>Authorize Transaction With OTP

* To authorize transaction with OTP
* Create a UI to collect OTP
* Create authorize otp button
* Set up otp request using this code
```javascript
    var authorizeOtpRequest = {		
        otp : 123456, // Accept OTP from user
        otpTransactionIdentifier: "2121324", // Set the OTP identifier for the request
        transactionRef: "13324444"	// Set the unique transaction reference.	
    }
```

* In the onclick event of the authorize otp button, use this code.

```javascript
    var status = PaymentPlugin.authorizeOtp(authorizeOtpRequest);				 
    status.done(function(response){
         alert(response);  // transaction success reponse    			         
    });
    status.fail(function (response) {
         alert(response);  // transaction failure reponse        
    });
```

### <a name='PaymentStatus'></a>Checking Payment Status
* To check payment status
* Create a UI to collect transaction identifier
* Create payment status button
* Set up payment status request using this code

```javascript
    var paymentStatusRequest = {
        transactionRef : 117499114589, // The transaction unique reference.
        amount : 100 //The transaction amount
    }
```

* To check the status of a payment made, use the code below
```javascript
    var paymentStatus = PaymentPlugin.paymentStatus(paymentStatusRequest);
    paymentStatus.done(function(response){                  
        alert(response); // transaction failure reponse
    });
    paymentStatus.fail(function (response) {              
        alert(response);  // transaction failure reponse
    });
```
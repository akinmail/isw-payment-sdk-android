## Payment SDK for Android

Interswitch payment SDK allows you to accept payments from customers within your mobile application.
The first step to ​using the ​Android SDK is to register as a merchant. This is described [here] (http://merchantxuat.interswitchng.com/paymentgateway/getting-started/overview/sign-up-as-a-merchant)


### Download the latest SDK


Download the latest SDK from the link below

https://github.com/techquest/isw-payment-sdk-android/releases

It consists of ​5 libraries:

1. *core.jar*
2. *payment.jar*
3. *payment-android-release.aar*
4. *gson-2.​3.1.jar*
5. *deviceprint-release-2.2.0.aar*
6. *appcompat folder*
7. *design folder*


**Use Android Studio’s dependency management tool (Gradle) to add the libraries to your project.**


### Creating the project and adding libraries using Android Studio

1. Download **Android Studio 1.2.2** or later
2. Create a new project
3. Add **core.jar, payment.jar, payment-android-release.aar**, **deviceprint-release-2.2.0.aar** and **gson-2.​3.1.jar** to your project by putting them in the libs folder of the app.
4. To add the **payment-android-release.aar** library, navigate to **File -> New -> New Module -> Import .JAR/.AAR Package** option in Android Studio.
5. Select the **payment-android-release.aar**
6. Repeat step 4 to step 5 to add **deviceprint-release-2.2.0.aar** to your project. Then select **deviceprint-release-2.2.0.aar** where applicable.
7. If you do not already have it, add the *appcompat* folder to your android sdk folder inside extras\android\support\v7
8. If you do not already have it, add the *design* folder to your android sdk folder inside android\support
9. To add the jar files, edit the build.gradle file of your app and add

```java
    compile files('libs/core.jar')
    compile files('libs/gson-2.3.1.jar')
    compile files('libs/payment.jar')
    compile 'com.android.support:appcompat-v7:23.1.1'
    compile 'com.android.support:design:23.1.1'
```

10 Finally, rebuild the project


### USING THE SDK IN SANDBOX MODE 

During development of your app, you should use the SDK in sandbox mode to enable testing. Different client id and client secret are provided for Production and Sandbox mode. The procedure to use the SDK on sandbox mode is just as easy, 

* Use sandbox client id and sandbox client secret got from the sandbox tab developer console after signup(usually you have to wait for 5 minutes after signup for you to see the sandbox details) everywhere you are required to supply client id and client secret in the remainder of this documentation              
* In your code, override the api base as follows
```java
    Passport.overrideApiBase("https://sandbox.interswitchng.com/passport"); 
    Payment.overrideApiBase("https://sandbox.interswitchng.com"); 
```
* Follow the remaining steps in the documentation. NOTE: When going into production mode, use the client id and the client secret got from the production tab of developer console instead



## Using the SDK with UI (IN PCI-DSS SCOPE : NO )

### Pay

* This option consists of both Pay with Card and Pay with Wallet
* Create a Pay button
* In the onClick listener of the Paybutton, add this code.

  Please note, supply your client id and client secret you got after registering as a merchant
  
```java
    RequestOptions options = RequestOptions.builder().setClientId("IKIA335B188FDC3527EDB1E9300D35F6C51826DFC8A5").setClientSecret("4HOFYiMJitFQeHYUCH/pvTF6jpiIaZqzVKB/pheK4Cs=").build();
    Pay pay = new Pay(activity, customerId, paymentDescription, amount, currency, options, new IswCallback<PurchaseResponse>()  {
        @Override
        public void onError(Exception error) {
            //Handle error
        }
    
        @Override
        public void onSuccess(PurchaseResponse response) {
        /*Handle success, user successfully paid, The purchase response object contains fields such as transactionIdentifier, message, amount, token, tokenExpiryDate, panLast4Digits, otpTransactionIdentifier, transactionRef and cardType which have getter methods to get them. Save the token, tokenExpiryDate, cardType and panLast4Digits if you want to pay with token in future
     */                     
       }
    });
    pay.start();
```


### Pay with card (UI)
    
* Create a Pay button
* In the onClick listener of the Paybutton, add this code.

  Please note, supply your client id and client secret you got after registering as a merchant
  
```java
    RequestOptions options = RequestOptions.builder().setClientId("IKIA14BAEA0842CE16CA7F9FED619D3ED62A54239276").setClientSecret("Z3HnVfCEadBLZ8SYuFvIQG52E472V3BQLh4XDKmgM2A=").build();
    PayWithCard payWithCard = new PayWithCard(activity, customerId, paymentDescription, amount, currency, options, new IswCallback<PurchaseResponse>() {
    
        @Override
        public void onError(Exception error) {
            //Handle error, user was unable to make successful payment
    
        }
    
        @Override
        public void onSuccess(final PurchaseResponse response) {
            /*Handle success, user successfully paid, The purchase response object contains fields such as transactionIdentifier, message, amount, token, tokenExpiryDate, panLast4Digits, otpTransactionIdentifier, transactionRef and cardType which have getter methods to get them. Save the token, tokenExpiryDate, cardType and panLast4Digits if you want to pay with token in future
        */                        
        }
    });
    payWithCard.start();
```


### Pay With Wallet (UI)

* Create a Pay button
* In the onClick listener of the Paybutton, add this code.

  Please note, supply your client id and client secret you got after registering as a merchant
  
```java
    RequestOptions options = RequestOptions.builder().setClientId("IKIA14BAEA0842CE16CA7F9FED619D3ED62A54239276").setClientSecret("Z3HnVfCEadBLZ8SYuFvIQG52E472V3BQLh4XDKmgM2A=").build();
    PayWithWallet payWithWallet = new PayWithWallet(activity, customerId, paymentDescription, amount, currency, options, new IswCallback<PurchaseResponse>() {
        @Override
        public void onError(Exception error) {
            //Handle error, user was unable to make successful payment
        }
    
        @Override
        public void onSuccess(PurchaseResponse response) {
            /*Handle success, user successfully paid, 
            The purchase response object contains fields such as transactionIdentifier, message, amount, token, tokenExpiryDate, panLast4Digits, otpTransactionIdentifier, transactionRef and cardType which have getter methods to get them. Save the token, tokenExpiryDate, cardType and panLast4Digits if you want to pay with token in future
            */              
        }
    });
    payWithWallet.start();
```


### Validate Card

* Validate card is used to check if a card is a valid card, it returns the card balance and token
* To call validate card, use this code.

  Please note, supply your client id and client secret you got after registering as a merchant
  
```java
    RequestOptions options = RequestOptions.builder().setClientId("IKIAD6DC1B942D95035FBCC5A4449C893D36536B5D54").setClientSecret("X1u1M6UNyASzslufiyxZnLb3u78TYODVnbRi7OxLNew=").build();
    ValidateCard validateCard = new ValidateCard(activity, customerId, currency, options, new IswCallback<ValidateCardResponse>() {
    
        @Override
        public void onError(Exception error) {
            //Handle error, user was unable to make successful payment

        }
    
        @Override
        public void onSuccess(final ValidateCardResponse response) {
            /*Handle success, user successfully paid, returns a validate card response object that contains several fields about the successful payment just made. 
            The purchase response object contains fields such as amount, token, tokenExpiryDate, panLast4Digits, otpTransactionIdentifier, transactionRef and cardType which have getter methods to get them. Save the token, tokenExpiryDate, cardType and panLast4Digits if you want to pay with token in future
                                             */
        }
    });
    validateCard.start();
```


### Pay with Token (UI)

* Interswitch Payment SDK allows you to generate a token in place of a user’s card details so that you can use it to debit the user at a later date without asking them for their card details again.
* Anytime you pay with card, token is returned as part of the purchase response object    
* The purchase response object contains fields such as transactionIdentifier, message, amount, token, tokenExpiryDate, panLast4Digits, otpTransactionIdentifier, transactionRef and cardType which have getter methods to get them. Save the token, tokenExpiryDate, cardType and panLast4Digits if you want to pay with token

Please note, supply your client id and client secret you got after registering as a merchant

```java
    RequestOptions options = RequestOptions.builder().setClientId("IKIAD6DC1B942D95035FBCC5A4449C893D36536B5D54").setClientSecret("X1u1M6UNyASzslufiyxZnLb3u78TYODVnbRi7OxLNew=").build();
    PayWithToken payWithToken = new PayWithToken(activity, customerId, amount, token, expiryDate, currency, cardType panLast4Digits, paymentDescription, options, new IswCallback<PurchaseResponse>() {
    
        @Override
        public void onError(Exception error) {
            //Handle error, user was unable to make successful payment

        }

        @Override
        public void onSuccess(final PurchaseResponse response) {
            /*Handle success, user successfully paid, returns a purchase response object that contains several fields about the successful payment just made. 
             The purchase response object contains fields such as transactionIdentifier, message, amount, token, tokenExpiryDate, panLast4Digits, otpTransactionIdentifier, transactionRef and cardType which have getter methods to get them. Save the token, tokenExpiryDate, cardType and panLast4Digits if you want to pay with token in future
                                             */   
        }
    });
    payWithToken.start();
```






## Using the SDK without UI (IN PCI-DSS SCOPE : YES)


### Pay with Card/Token (NO UI)

* Ask the user for card details
* In the onClick method of the button that asks the user to pay, add this code.

Please note, supply your client id and client secret you got after registering as a merchant

```java
	//Setup client id and secret
    RequestOptions options = RequestOptions.builder()
	.setClientId("IKIA3E267D5C80A52167A581BBA04980CA64E7B2E70E")
	.setClientSecret("SagfgnYsmvAdmFuR24sKzMg7HWPmeh67phDNIiZxpIY=").build(); 
```
```java
	//Setup request parameters
	final PurchaseRequest request = new PurchaseRequest();
    //Optional email, mobile no, BVN etc to uniquely identify the customer
	request.setCustomerId(“1234567890"); 
    request.setAmount(“100"); //Amount in Naira
	request.setCurrency("NGN");
    request.setPan(“5060100000000000012"); //Card No or Token
    request.setPinData("1111"); //Optional Card PIN for card payment
    request.setExpiryDate("2004"); // Card or Token expiry date in YYMM format
    request.setTransactionRef(RandomString.numeric(12)); //unique transaction reference
    Context context = this; // reference to your Android Activity
```
```java	
    //Send payment
    new PaymentSDK(context, options).purchase(request, new IswCallback<PurchaseResponse>() {
    @Override
    public void onError(Exception error) {
        // Handle and notify user of error
    }
    @Override
    public void onSuccess(PurchaseResponse response) {
        //Check if OTP is required
        if (StringUtils.hasText(response.getOtpTransactionIdentifier())) {
           //OTP required
           //Ask user for OTP and authorize transaction using the otpTransactionIdentifier
        } 
		else { 
         //OTP not required
         //Handle and notify user of successful transaction. A token for the card details is returned in the response
        }
    }
    });
	
```

## Pay With Wallet (NO UI)

First set your client id and client secret
```java
	//Setup client id and secret
    RequestOptions options = RequestOptions.builder()
	.setClientId("IKIA3E267D5C80A52167A581BBA04980CA64E7B2E70E")
	.setClientSecret("SagfgnYsmvAdmFuR24sKzMg7HWPmeh67phDNIiZxpIY=").build();
```

To load Verve wallet, add this code, use the resulting payment methods array to populate a spinner
```java
    //Load Wallet
    final WalletRequest request = new WalletRequest();
    request.setTransactionRef(RandomString.numeric(12));
    Context context = this; // reference to your Android Activity
    new WalletSDK(context, options).getPaymentMethods(request, new IswCallback<WalletResponse>() {
    @Override
    public void onError(Exception error) {
        // Handle and notify user of error
    }
 
    @Override
    public void onSuccess(WalletResponse response) {
		PaymentMethod[] paymentMethods = response.getPaymentMethods(); 
		//Display payment methods in a Spinner
    }
	});
​
```
After populating the spinner, when the user selects an item and then clicks pay, use this code
```java
	//Setup request parameters using wallet item
    final PurchaseRequest request = new PurchaseRequest();
    //Optional email, mobile no, BVN etc to uniquely identify the customer
	request.setCustomerId(“1234567890");
	//Amount in Naira
    request.setAmount("100"); 
	request.setCurrency("NGN");
    if (paymethodSpinner.getSelectedItem() == null) {
		return;
    }
    request.setPan(((PaymentMethod) paymethodSpinner.getSelectedItem()).getToken());
    request.setPinData(pin.getText().toString());
    request.setTransactionRef(RandomString.numeric(12));
```
```java
	//Send payment
    new WalletSDK(context, options).purchase(request, new IswCallback<PurchaseResponse>() {
    @Override
    public void onError(Exception error) {
        // Handle and notify user of error
    }
 
    @Override
    public void onSuccess(PurchaseResponse response) {
        if (StringUtils.hasText(response.getOtpTransactionIdentifier())) {       
			//OTP required
			//Ask user for OTP and authorize transaction using the otp Transaction Identifier
        } else { //OTP not required
           //Handle and notify user of successful transaction
        }
    }
	);
```

## Checking status of payment

To check the status of a payment made, use the code below

```java
	//pass the transactionRef and the amount as the parameters to getPaymentStatus()
	new WalletSDK(context, options).getPaymentStatus("117499114589", "100", new IswCallback<PaymentStatusResponse>() {
    @Override
    public void onError(Exception error) {
        // Handle and notify user of error
    }

    @Override
    public void onSuccess(PaymentStatusResponse response) {
        //print response message
    }
    });
```





### Using android sdk to create Blackberry App
To create a Blackberry app using the **runtime for Android** 

1. Create an android app as above using SDK provided for android
2. Convert the app according to the instructions stated on Blackberry's website [here] (http://developer.blackberry.com/android/) and [here] (http://developer.blackberry.com/android/documentation/bb_android_studio_plugin_tool.html)
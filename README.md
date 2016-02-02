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

**Use Android Studio’s dependency management tool (Gradle) to add the libraries to your project.**


### Creating the project and adding libraries using Android Studio

1. Download **Android Studio 1.2.2** or later
2. Create a new project
3. Add **core.jar, payment.jar, payment-android-release.aar**, **deviceprint-release-2.2.0.aar** and **gson-2.​3.1.jar** to your project by putting them in the libs folder of the app.
4. To add the **payment-android-release.aar** library, navigate to **File -> New -> New Module -> Import .JAR/.AAR Package** option in Android Studio.
5. Select the **payment-android-release.aar**
6. Repeat step 4 to step 5 to add **deviceprint-release-2.2.0.aar** to your project. Then select **deviceprint-release-2.2.0.aar** where apllicable.
7. Finally, rebuild the project


### USING THE SDK IN SANDBOX MODE 

The procedure to use the SDK on sandbox mode is just as easy, 

* Use sandbox client id and secret got from the developer console after signup(usually you have to wait for 5 minutes for you to see the sandbox details)              
* Override the api base as follows 
```java
    Passport.overrideApiBase("https://sandbox.interswitchng.com/passport"); 
    Payment.overrideApiBase("https://sandbox.interswitchng.com"); 
```
* Follow the remaining steps in the documentation 



### Accepting Payments with Card / Token

Ask the user for card details

In the onClick method of the button that asks the user to pay, add this code
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
        if (StringUtils.hasText(response.getOtpTransactionIdentifier())) {
           //OTP required
           //Ask user for OTP and authorize transaction using the otpTransactionIdentifier
        } 
		else { 
         //OTP not required
         //Handle and notify user of successful transaction
        }
    }
    });
	
```

## Accepting Payment with Wallet

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

##Checking status of payment

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

###Using android sdk to create Blackberry App
To create a Blackberry app using the **runtime for Android** 

1. Create an android app as above using SDK provided for android
2. Convert the app according to the instructions stated on Blackberry's website [here] (http://developer.blackberry.com/android/) and [here] (http://developer.blackberry.com/android/documentation/bb_android_studio_plugin_tool.html)
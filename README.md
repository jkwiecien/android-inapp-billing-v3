Android In-App Billing v3 Library
=======================

This is a simple, straight-forward implementation of the Android v3 In-app billing API.

It supports: In-App Product Purchases (both non-consumable and consumable) and Subscriptions.

Getting Started
===============

* You project should build against Android 2.2 SDK at least.

* Add this *Android In-App Billing v3 Library* to your Eclipse project. If you guys are using Android Studio and Gradle, add this to you build.gradle file:
```groovy
    repositories {
        mavenCentral()
    }
    dependencies {
       compile 'com.anjlab.android.iab.v3:library:1.0.+@aar'
    }
```

* Open the *AndroidManifest.xml* of your application and add this permission:
```xml
  <uses-permission android:name="com.android.vending.BILLING" />
```
* Create instance of BillingProcessor class in your Activity source code:
```java
  BillingProcessor bp;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		bp = new BillingProcessor(this);
	}
```

* Implement IBillingHandler Interface to handle purchase results and errors:
```java
	bp.setBillingHandler(this);
```

* override Activity's onActivityResult method:
```java
  @Override
	protected void onActivityResult(int requestCode, int resultCode, Intent data) {
		if (!bp.handleActivityResult(requestCode, resultCode, data))
			super.onActivityResult(requestCode, resultCode, data);
	}
```

* Call `purchase` method for a BillingProcessor instance to initiate purchase or `subscribe` to initiate a subscription:
```java
bp.purchase("YOUR PRODUCT ID FROM GOOGLE PLAY CONSOLE HERE");
bp.subscribe("YOUR SUBSCRIPTION ID FROM GOOGLE PLAY CONSOLE HERE");
```
* **That's it! A super small and fast in-app library ever!**

* **And dont forget**
 to release your BillingProcessor instance! 
```java
	@Override
	public void onDestroy() {
		if (bp != null) 
			bp.release();
		
		super.onDestroy();
	}
```

IBillingHandler Callback Interface
-----------------------------------
has 4 methods:

```java
  void onProductPurchased(String productId);
```
  called then requested PRODUCT ID was successfully purchased
```java
	void onPurchaseHistoryRestored();
```
  called then purchase history was restored and the list of all owned PRODUCT IDs was loaded from Google Play
```java
	void onBillingError(int errorCode, Throwable error);
```
  called then some error occured. See Constants class for more details
```java
	void onBillingInitialized();
```
  called then BillingProcessor was initialized and its ready to purchase 


Validate Purchases Against Your Merchant Key
-----------------------------------
This option is turned off by default. If you do want to validate:
```java
	bp.verifyPurchasesWithLicenseKey("YOUR LICENSE KEY FROM GOOGLE PLAY CONSOLE HERE");
```

Consume Purchased Products
--------------------------
You can always consume made purchase and allow to buy same product multiple times. To do this you need:
```java
	bp.consumePurchase("YOUR PRODUCT ID FROM GOOGLE PLAY CONSOLE HERE");
```

Restore Purchases & Subscriptions
--------------------------
```java
	bp.restorePurchases();
	bp.restoreSubscriptions();
```

Notice On Canceled/Expired Subscriptions
--------------------------
Since Google's v3 API doesn't provide any callbacks to handle canceled and/or expired subscriptions you have to handle it on your own.
The easiest way to do this - call periodically `bp.restoreSubscriptions()` method.

## License

Copyright 2014 AnjLab

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

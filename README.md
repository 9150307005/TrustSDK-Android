# TrustSDK-Android
[![](https://jitpack.io/v/TrustWallet/TrustSdk-android.svg)](https://jitpack.io/#TrustWallet/TrustSdk-android)

## Getting started

The TrustSDK lets you sign Ethereum transactions and messages so that you can bulid a native DApp without having to worry about keys or wallets. Follow these instructions to integrate TrustSDK in your native DApp.

## Demo

## Add dependency

1. Add jitpack to your root gradle file at the end of repositories:
```
allprojects {
    repositories {
	...
        maven { url 'https://jitpack.io'}
    }
}
```

2. Add dependency to your module:
```
dependencies {
    implementation 'com.github.TrustWallet:TrustSDK-Android:0.01.2'
}
```

## Register a scheme for your app in the AndroidManifest.xml

```
<activity android:name=".MyUriActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="myapp" android:host="myapp" />
    </intent-filter>
</activity>
```

## Handle Trust callbacks

In your signing activity `Trust`.

```
import Trust
```

Override `onActivityResult` to obtain the signing result. Handle the response data and pass onSuccessListener and onFailureListener.

```
    @SuppressLint("ShowToast")
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        Trust.onActivityResult(requestCode, resultCode, data).subscribe({
            request, signHex -> Toast.makeText(this, "Success: $signHex", Toast.LENGTH_LONG).show()
        }, {
            request, error -> Toast.makeText(this, "Fail: $error", Toast.LENGTH_LONG).show()
        })
        messageCall?.let {
            it.onActivityResult(requestCode, resultCode, data).subscribe { request, signHex ->

            }
        }
    }
```

### Sign a transaction

To sign a transaction use this code:

```
Trust.signTransaction()
    .recipient(Address("0x3637a62430C67Fe822f7136D2d9D74bDDd7A26C1"))
    .gasPrice(BigInteger.valueOf(16))
    .gasLimit(21000)
    .value(BigDecimal.valueOf(0.3).multiply(BigDecimal.TEN.pow(18)).toBigInteger())
    .nonce(0)
    .payload("0x")
    .call(this)
```

### Sign a message

To sign a message use this code:

```
Trust.signMessage()
    .message("message to be signed")
    .isPersonal(false)
    .call(this)
```

### Sign a personal message

To sign a personal message use this code:

```
Trust.signMessage()
    .message("private message to be signed")
    .isPersonal(true)
    .call(this)
```

## Example

Trust SDK includes an example project with the above code. To run the example project clone the repo and build the project with Android Studio. Run the app on your emulator or device. Make sure that you have Trust Wallet installed on the device or simulator to test the full callback flow.

## Authors

 * Maxim Rasputin
 * Marat Subkhankulov

License

TrustSDK is available under the MIT license. See the LICENSE file for more info.

Let
====

Annotation based simple API flavoured with AOP to handle new Android runtime permission model.

If you check [Google's Samples] (https://github.com/googlesamples/android-RuntimePermissions/blob/master/Application/src/main/java/com/example/android/system/runtimepermissions/MainActivity.java) 
about the new permission model, you'll see a lot of boiler plate code.

Let will minimize the boiler plate code you have to write for requesting and handling permissions and 
keep your code more readable.  
  
Usage
====

Add `@AskPermission` to your methods that require permissions to be granted at runtime.
 
```java
@AskPermission(ACCESS_FINE_LOCATION)
private void getUserLocationAndDoSomething() {
    Toast.makeText(
        SampleActivity.this, 
        "Now that I have the permission, I can get your location!", 
        Toast.LENGTH_SHORT
    ).show();
    ...
}
``` 
Let will check these annotated methods and execute them unless the permissions required are granted;
otherwise Let will request these permissions at runtime, examine the result and execute the method 
only if the permissions are granted by the user.
  
Let will inform about the rationales to be shown before making any permission request
and also tell about the permissions denied (with or without 'Never Ask Again' checked)  
 
Just make sure to override the `onRequestPermissionsResult` in your Activity or Fragment.

```java
@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
    Let.handle(requestCode, permissions, grantResults);
}
```

Make sure your Activity or Fragment implements `RuntimePermissionListener` in order to get notified 
about denied permissions and rationales to be shown, 

```java
public class SampleActivity extends AppCompatActivity implements RuntimePermissionListener {
....
@Override
public void onShowPermissionRationale(List<String> permissions, final RuntimePermissionRequest request) {
    /**
    * show permission rationales in a dialog, wait for user confirmation and retry the permission 
    * request by calling request.retry()    
    */        
    ....
}   

@Override
public void onPermissionDenied(List<DeniedPermissionRequest> results) {
    /**
    * Do whatever you need to do about denied permissions e.g. update UI and prompt a dialog to 
    * tell user to go to the app settings screen in order to grant again the permission denied with 
    * 'Never Ask Again' 
    */      
    ...
}
```

Annotate your methods with `@AskPermission` and let Let handle the rest.

```java
@AskPermission({
            Manifest.permission.READ_CONTACTS,
            Manifest.permission.WRITE_CONTACTS          
})
private void showContacts() {

    Toast.makeText(SampleActivity.this, "Calling..", Toast.LENGTH_SHORT).show();

    final Intent intent = new Intent(Intent.ACTION_CALL).setData(Uri.parse("tel:00123124234234"));
    startActivity(intent);
}
```

Add it to your project today!

```groovy
buildscript {
  repositories {
    mavenCentral()
  }

  dependencies {
    classpath 'com.canelmas.let:let-plugin:0.1.3-SNAPSHOT'
  }
}

apply plugin: 'com.android.application'
apply plugin: 'let'
```

License
====

    Copyright 2015 Can Elmas

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

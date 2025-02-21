# Permission ⚠️

## Android permissions are divided into two categories: *normal* and *dangerous*. 

### Normal permissions are granted by the system automatically.

example:

  INTERNET: Allows the app to access the internet

  VIBRATE: Allows the app to vibrate the device

  WAKE_LOCK: Allows the app to prevent the device from sleeping

 

### Dangerous permissions require explicit user approval.

##### They are used to access sensitive features that can potentially compromise user privacy or system security.

example:

  READ_CONTACTS: Allows the app to read the user's contact list

  WRITE_EXTERNAL_STORAGE: Allows the app to write data to the device's external storage

  CAMERA: Allows the app to access the device's camera

  

  ## Requesting Permissions

  ```
   <uses-permission android:name="android.permission.READ_CONTACTS" />
  ```

  When the user installs your app, they will be prompted to grant the requested permissions. If the user grants the permission, your app can access the corresponding feature or data. If the user denies the permission, your app will not be able to access the feature or data.
  

  

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
  

  ## Runtime Permissions

  To request runtime permissions, you need to use the *requestPermissions()* method, which takes an array of permissions as an argument.

  ```
  val requestPermissionLauncher =
        registerForActivityResult(
            ActivityResultContracts.RequestPermission()
        ) { isGranted: Boolean ->
            if (isGranted) {
                Toast.makeText(this, "Permission Granted", Toast.LENGTH_SHORT).show()
            } else {
                Toast.makeText(this, "Permission Denied", Toast.LENGTH_SHORT).show()
            }
        }           


 if (ContextCompat.checkSelfPermission(
                    this,
                    Manifest.permission.READ_CONTACTS  // Manifest.permission.Required_Runtime_Permission 
                ) == PackageManager.PERMISSION_GRANTED
            ) {
                Toast.makeText(this, "Permission Already Present", Toast.LENGTH_SHORT).show()
            } else {
                if (ActivityCompat.shouldShowRequestPermissionRationale(
                        this, Manifest.permission.READ_CONTACTS  //Manifest.permission.Required_Runtime_Permission
                    )
                ) {
                    Toast.makeText(this, "Rationale Required", Toast.LENGTH_SHORT).show()
                    AlertDialog.Builder(this).setMessage("permission required because...")
                        .setPositiveButton(
                            "Ok"
                        ) { p0, p1 ->
                            requestPermissionLauncher.launch(
                               Manifest.permission.READ_CONTACTS  //Manifest.permission.Required_Runtime_Permission)
                          }
                        .setNegativeButton(
                            "Deny"
                        ) { p0, p1 -> p0.cancel() }.show()
                } else {
                    requestPermissionLauncher.launch(
                              Manifest.permission.READ_CONTACTS  //Manifest.permission.Required_Runtime_Permission)
                        )
                }
            }
        }
  ```

![new ui](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSjk3T-KzefdzcI0IX8fKLPHME4IQ5hFiz1YyNve0m-qdLc-woNfibzdI7axOub06cYIYw&usqp=CAU)

![old ui](https://s3.amazonaws.com/assets.df.soti.net/default_assets/images/ca864387-a409-4939-a68a-ad4e00b0de49/8fe8868d-0093-437a-833d-6adbc9bdb00f/screencapture.PNG)

## Security Best Practices

1.Only request the permissions that are necessary for your app's functionality(user mistrust).

2.Group related permissions together to reduce the number of permission requests(CAMERA and RECORD_AUDIO).





## Reference

[first](https://30dayscoding.com/blog/android-permissions-and-security-guide)

[must read later](https://www.droidcon.com/2024/01/19/android-permissions-unveiled-a-developers-insight/)

[android 11](https://mikethecitadev.medium.com/permission-updates-in-android-11-e4a6aa8aff9)

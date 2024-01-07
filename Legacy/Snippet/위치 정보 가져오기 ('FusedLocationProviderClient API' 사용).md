---
boostnote:
  createdAt: '2018-10-26T07:00:47.666Z'
  updatedAt: '2019-03-06T01:33:39.105Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 위치 정보 가져오기 ('FusedLocationProviderClient API' 사용)
  tags:
    - Android
    - Java
    - Location
  isStarred: false
  linesHighlighted: []
  key: 38bfd720-ff56-4f83-ad9a-064b95b87161
  storage: d112f8ec1e85e056a09d
---

위치 정보 가져오기 ('FusedLocationProviderClient API' 사용)
---
MainActivity.java

```java
public class MainActivity extends AppCompatActivity implements MyLocationManager.OnLocationResultListener {

    private Context mContext;

    private TextView mTextView;

    private MyLocationManager myLocationManager;
    private boolean mRequestingLocationUpdates;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mContext = this;
        mTextView = findViewById(R.id.text_location);

        myLocationManager = new MyLocationManager(mContext, this);
        myLocationManager.startLocationUpdates();
    }

    @Override
    protected void onResume() {
        super.onResume();

        if (mRequestingLocationUpdates) {
            myLocationManager.startLocationUpdates();
        }
    }

    @Override
    public void onLocationResult(LocationResult locationResult) {
        if (locationResult == null) {
            Log.e("!!! ERROR", "# No location data.");
            return;
        }

        double latitude = locationResult.getLastLocation().getLatitude();
        double longitude = locationResult.getLastLocation().getLongitude();

        mTextView.setText(String.format("%s, %s", latitude, longitude));
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        switch (requestCode) {
            case MyLocationManager.LOCATION_REQUEST_CODE: {
                if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                    mRequestingLocationUpdates = true;
                    myLocationManager.startLocationUpdates();
                } else {
                    mRequestingLocationUpdates = false;
                    Toast.makeText(mContext, "Permission Denied", Toast.LENGTH_LONG).show();
                }
            }
        }
    }
}
```

MyLocationManager.java

```java
public class MyLocationManager extends LocationCallback {
    protected static final int LOCATION_REQUEST_CODE = 1000;

    private Context context;
    private FusedLocationProviderClient fusedLocationProviderClient;
    private OnLocationResultListener mListener;

    public interface OnLocationResultListener {
        void onLocationResult(LocationResult locationResult);
    }

    public MyLocationManager(Context context, OnLocationResultListener mListener) {
        this.context = context;
        this.mListener = mListener;
        this.fusedLocationProviderClient = LocationServices.getFusedLocationProviderClient(context);
    }

    @Override
    public void onLocationResult(LocationResult locationResult) {
        super.onLocationResult(locationResult);
        mListener.onLocationResult(locationResult);
    }

    public void startLocationUpdates() {
        if (ActivityCompat.checkSelfPermission(context, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED
                && ActivityCompat.checkSelfPermission(context, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            Log.d("!!! LocationManager", "Permission required.");
            ActivityCompat.requestPermissions((Activity) context, new String[]{Manifest.permission.ACCESS_FINE_LOCATION,}, LOCATION_REQUEST_CODE);
            return;
        }

        if (!isGPSEnabled()) {
            showLocationSettingDialog();
            return;
        }

        LocationRequest request = new LocationRequest();
        request.setInterval(5 * 1000); // 5s
        request.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);

        fusedLocationProviderClient.requestLocationUpdates(request, this, null);
    }

    public void stopLocationUpdates() {
        fusedLocationProviderClient.removeLocationUpdates(this);
    }

    private Boolean isGPSEnabled() {
        android.location.LocationManager locationManager = (android.location.LocationManager) context.getSystemService(Context.LOCATION_SERVICE);
        return locationManager.isProviderEnabled(android.location.LocationManager.GPS_PROVIDER);
    }

    private void showLocationSettingDialog() {
        new android.app.AlertDialog.Builder(context)
                .setMessage("Required location service")
                .setPositiveButton("OK", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        Intent intent = new Intent(Settings.ACTION_LOCATION_SOURCE_SETTINGS);
                        context.startActivity(intent);
                    }
                })
                .setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        //NOP
                    }
                })
                .create()
                .show();
    }
}
```
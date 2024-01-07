---
boostnote:
  type: MARKDOWN_NOTE
  folder: 73cb6e331607e9cab7be
  title: Bluetooth Low Energy UUID & Gatt Attributes
  tags: []
  isStarred: false
  createdAt: '2017-10-10T08:43:50.664Z'
  updatedAt: '2017-10-10T08:45:48.916Z'
  key: 260c52a1eb410091bbe9
  storage: d112f8ec1e85e056a09d
---

Bluetooth Low Energy UUID & Gatt Attributes
---
[GATT Services](https://www.bluetooth.com/specifications/gatt/services)
[GATT Characteristics](https://www.bluetooth.com/specifications/gatt/characteristics)
[GATT Descriptors](https://www.bluetooth.com/specifications/gatt/descriptors)


public static final UUID UUID_SERIAL_PORT_SERVICE_CLASS =
        UUID.fromString("00001101-0000-1000-8000-00805f9b34fb");

public static final UUID UUID_GAP =
        UUID.fromString("00001800-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_GATT =
        UUID.fromString("00001801-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_DEVICE_INFORMATION =
        UUID.fromString("0000180a-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_GATT_HEART_RATE =
        UUID.fromString("0000180d-0000-1000-8000-00805f9b34fb");

public static final UUID UUID_BATTERY_SERVICE =
        UUID.fromString("0000180F-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_GATT_USER_DATA =
        UUID.fromString("0000181c-0000-1000-8000-00805f9b34fb");


public static final UUID UUID_GAP_DEVICE_NAME =
        UUID.fromString("00002a00-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_GAP_APPEARANCE =
        UUID.fromString("00002a01-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_GAP_PERIPHERAL_PREFERRED =
        UUID.fromString("00002a04-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_GATT_SERVICE_CHANGED =
        UUID.fromString("00002a05-0000-1000-8000-00805f9b34fb");

public static final UUID UUID_BATTERY_LEVEL =
        UUID.fromString("00002a19-0000-1000-8000-00805f9b34fb");

public static final UUID UUID_DEVICE_INFORMATION_SYSTEM_ID =
        UUID.fromString("00002a23-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_DEVICE_INFORMATION_MODEL_NUMBER =
        UUID.fromString("00002a24-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_DEVICE_INFORMATION_SERIAL_NUMBER =
        UUID.fromString("00002a25-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_DEVICE_INFORMATION_FIRMWARE_REVISION =
        UUID.fromString("00002a26-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_DEVICE_INFORMATION_HARDWARE_REVISION =
        UUID.fromString("00002a27-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_DEVICE_INFORMATION_SOFTWARE_REVISION =
        UUID.fromString("00002a28-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_DEVICE_INFORMATION_MANUFACTURER_NAME_STRING =
        UUID.fromString("00002a29-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_DEVICE_INFORMATION_CERT =
        UUID.fromString("00002a2a-0000-1000-8000-00805f9b34fb");

public static final UUID UUID_DEVICE_INFORMATION_PNP_ID =
        UUID.fromString("00002a50-0000-1000-8000-00805f9b34fb");

public static final UUID UUID_CLIENT_CHARACTERISTIC_CONFIG =
        UUID.fromString("00002902-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_HEART_RATE_MEASUREMENT =
        UUID.fromString("00002a37-0000-1000-8000-00805f9b34fb");

public static final UUID UUID_SIMPLE_KEYS =
        UUID.fromString("0000ffe0-0000-1000-8000-00805f9b34fb");
public static final UUID UUID_SIMPLE_KEYS_DATA =
        UUID.fromString("0000ffe1-0000-1000-8000-00805f9b34fb");

public static final UUID UUID_IR_TEMPERATURE =
        UUID.fromString("f000aa00-0451-4000-b000-000000000000");
public static final UUID UUID_IR_TEMPERATURE_DATA =
        UUID.fromString("f000aa01-0451-4000-b000-000000000000");
public static final UUID UUID_IR_TEMPERATURE_CONFIG =
        UUID.fromString("f000aa02-0451-4000-b000-000000000000");
public static final UUID UUID_IR_TEMPERATURE_PERIOD =
        UUID.fromString("f000aa03-0451-4000-b000-000000000000");

public static final UUID UUID_HUMIDITY =
        UUID.fromString("f000aa20-0451-4000-b000-000000000000");
public static final UUID UUID_HUMIDITY_DATA =
        UUID.fromString("f000aa21-0451-4000-b000-000000000000");
public static final UUID UUID_HUMIDITY_CONFIG =
        UUID.fromString("f000aa22-0451-4000-b000-000000000000");
public static final UUID UUID_HUMIDITY_PERIOD =
        UUID.fromString("f000aa23-0451-4000-b000-000000000000");

public static final UUID UUID_BAROMETER =
        UUID.fromString("f000aa40-0451-4000-b000-000000000000");
public static final UUID UUID_BAROMETER_DATA =
        UUID.fromString("f000aa41-0451-4000-b000-000000000000");
public static final UUID UUID_BAROMETER_CONFIG =
        UUID.fromString("f000aa42-0451-4000-b000-000000000000");
public static final UUID UUID_BAROMETER_PERIOD =
        UUID.fromString("f000aa44-0451-4000-b000-000000000000");

public static final UUID UUID_IO =
        UUID.fromString("f000aa64-0451-4000-b000-000000000000");
public static final UUID UUID_IO_DATA =
        UUID.fromString("f000aa65-0451-4000-b000-000000000000");
public static final UUID UUID_IO_CONFIGURATION =
        UUID.fromString("f000aa66-0451-4000-b000-000000000000");

public static final UUID UUID_OPTICAL_SENSOR =
        UUID.fromString("f000aa70-0451-4000-b000-000000000000");
public static final UUID UUID_OPTICAL_SENSOR_DATA =
        UUID.fromString("f000aa71-0451-4000-b000-000000000000");
public static final UUID UUID_OPTICAL_SENSOR_CONFIGURATION =
        UUID.fromString("f000aa72-0451-4000-b000-000000000000");
public static final UUID UUID_OPTICAL_SENSOR_PERIOD =
        UUID.fromString("f000aa73-0451-4000-b000-000000000000");

public static final UUID UUID_MOVEMENT =
        UUID.fromString("f000aa80-0451-4000-b000-000000000000");
public static final UUID UUID_MOVEMENT_DATA =
        UUID.fromString("f000aa81-0451-4000-b000-000000000000");
public static final UUID UUID_MOVEMENT_CONFIGURATION =
        UUID.fromString("f000aa82-0451-4000-b000-000000000000");
public static final UUID UUID_MOVEMENT_PERIOD =
        UUID.fromString("f000aa83-0451-4000-b000-000000000000");

public static final UUID UUID_REGISTER =
        UUID.fromString("f000ac00-0451-4000-b000-000000000000");
public static final UUID UUID_REGISTER_DATA =
        UUID.fromString("f000ac01-0451-4000-b000-000000000000");
public static final UUID UUID_REGISTER_ADDRESS =
        UUID.fromString("f000ac02-0451-4000-b000-000000000000");
public static final UUID UUID_REGISTER_DEVICE_ID =
        UUID.fromString("f000ac03-0451-4000-b000-000000000000");

public static final UUID UUID_CONNECTION_CONTROL =
        UUID.fromString("f000ccc0-0451-4000-b000-000000000000");
public static final UUID UUID_CONNECTION_PARAMETERS =
        UUID.fromString("f000ccc1-0451-4000-b000-000000000000");
public static final UUID UUID_REQUEST_CONNECTION_PARAMETERS =
        UUID.fromString("f000ccc2-0451-4000-b000-000000000000");
public static final UUID UUID_REQUEST_DISCONNECT =
        UUID.fromString("f000ccc3-0451-4000-b000-000000000000");

public static final UUID UUID_OAD =
        UUID.fromString("f000ffc0-0451-4000-b000-000000000000");
public static final UUID UUID_OAD_IMAGE_IDENTIFY =
        UUID.fromString("f000ffc1-0451-4000-b000-000000000000");
public static final UUID UUID_OAD_IMAGE_BLOCK =
        UUID.fromString("f000ffc2-0451-4000-b000-000000000000");

public static final UUID UUID_SERVICE_ACCELEROMETER =
        UUID.fromString("f000aa10-0451-4000-b000-000000000000");
public static final UUID UUID_SERVICE_ACCELEROMETER_DATA =
        UUID.fromString("f000aa11-0451-4000-b000-000000000000");
public static final UUID UUID_SERVICE_ACCELEROMETER_CONFIG =
        UUID.fromString("f000aa12-0451-4000-b000-000000000000");
public static final UUID UUID_SERVICE_ACCELEROMETER_PERIOD =
        UUID.fromString("f000aa13-0451-4000-b000-000000000000");

public static final UUID UUID_SERVICE_MAGNETOMETER =
        UUID.fromString("f000aa30-0451-4000-b000-000000000000");
public static final UUID UUID_SERVICE_MAGNETOMETER_DATA =
        UUID.fromString("f000aa31-0451-4000-b000-000000000000");
public static final UUID UUID_SERVICE_MAGNETOMETER_CONFIG =
        UUID.fromString("f000aa32-0451-4000-b000-000000000000");
public static final UUID UUID_SERVICE_MAGNETOMETER_PERIOD =
        UUID.fromString("f000aa33-0451-4000-b000-000000000000");

public static final UUID UUID_SERVICE_GYROSCOPE =
        UUID.fromString("f000aa50-0451-4000-b000-000000000000");
public static final UUID UUID_SERVICE_GYROSCOPE_DATA =
        UUID.fromString("f000aa51-0451-4000-b000-000000000000");
public static final UUID UUID_SERVICE_GYROSCOPE_CONFIG =
        UUID.fromString("f000aa52-0451-4000-b000-000000000000");


/*****************************************************************************
**  Constants
*****************************************************************************/
/* Success code and error codes */
#define  GATT_SUCCESS                        0x00
#define  GATT_INVALID_HANDLE                 0x01
#define  GATT_READ_NOT_PERMIT                0x02
#define  GATT_WRITE_NOT_PERMIT               0x03
#define  GATT_INVALID_PDU                    0x04
#define  GATT_INSUF_AUTHENTICATION           0x05
#define  GATT_REQ_NOT_SUPPORTED              0x06
#define  GATT_INVALID_OFFSET                 0x07
#define  GATT_INSUF_AUTHORIZATION            0x08
#define  GATT_PREPARE_Q_FULL                 0x09
#define  GATT_NOT_FOUND                      0x0a
#define  GATT_NOT_LONG                       0x0b
#define  GATT_INSUF_KEY_SIZE                 0x0c
#define  GATT_INVALID_ATTR_LEN               0x0d
#define  GATT_ERR_UNLIKELY                   0x0e
#define  GATT_INSUF_ENCRYPTION               0x0f
#define  GATT_UNSUPPORT_GRP_TYPE             0x10
#define  GATT_INSUF_RESOURCE                 0x11
#define  GATT_ILLEGAL_PARAMETER              0x87
#define  GATT_NO_RESOURCES                   0x80
#define  GATT_INTERNAL_ERROR                 0x81
#define  GATT_WRONG_STATE                    0x82
#define  GATT_DB_FULL                        0x83
#define  GATT_BUSY                           0x84
#define  GATT_ERROR                          0x85
#define  GATT_CMD_STARTED                    0x86
#define  GATT_PENDING                        0x88
#define  GATT_AUTH_FAIL                      0x89
#define  GATT_MORE                           0x8a
#define  GATT_INVALID_CFG                    0x8b
#define  GATT_SERVICE_STARTED                0x8c
#define  GATT_ENCRYPED_MITM                  GATT_SUCCESS
#define  GATT_ENCRYPED_NO_MITM               0x8d
#define  GATT_NOT_ENCRYPTED                  0x8e
#define  GATT_CONGESTED                      0x8f
                                             /* 0xE0 ~ 0xFC reserved for future use */
#define  GATT_CCC_CFG_ERR                    0xFD /* Client Characteristic Configuration Descriptor Improperly Configured */
#define  GATT_PRC_IN_PROGRESS                0xFE /* Procedure Already in progress */
#define  GATT_OUT_OF_RANGE                   0xFF /* Attribute value out of range */
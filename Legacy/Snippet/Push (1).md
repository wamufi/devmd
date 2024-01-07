---
boostnote:
  createdAt: '2019-02-20T06:06:39.485Z'
  updatedAt: '2019-08-26T05:14:00.105Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Push
  tags:
    - Android
    - Java
  linesHighlighted: []
  isStarred: false
  key: c5dd7ad5-3aa2-42c3-ac22-a23fe056c81d
  storage: d112f8ec1e85e056a09d
---

Push
---
[[안드로이드/Android]8.0 오레오 알림채널(Notification Channel) 대응하기 - 박상권의 삽질블로그](https://gun0912.tistory.com/77)
[[번역] Android 8.0 - 알림 채널, 뱃지](https://bonoogi.herokuapp.com/android-oreo-notification-channel-badge/)


```java
private void createNotificationChannel(Bundle data) {
    NotificationManager mNotificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        NotificationChannel mChannel = new NotificationChannel("PaulBassett", "Paul Bassett", NotificationManager.IMPORTANCE_DEFAULT);
        mChannel.setDescription(null);
        mChannel.setLightColor(ContextCompat.getColor(getApplicationContext(), R.color.colorPrimary));
        mChannel.enableLights(true);
        mChannel.enableVibration(true);
        mChannel.setVibrationPattern(new long[]{100, 200, 300, 400, 500, 400, 300, 200, 400});
        mChannel.setShowBadge(true);
        mNotificationManager.createNotificationChannel(mChannel);
    }

    Intent intent = new Intent(IntentService.this, MainActivity.class);
    intent.putExtra("fromNotification", true);
    intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK | Intent.FLAG_ACTIVITY_CLEAR_TOP | Intent.FLAG_ACTIVITY_SINGLE_TOP);

    PendingIntent pi = PendingIntent.getActivity(IntentService.this, (int) System.currentTimeMillis(), intent, PendingIntent.FLAG_CANCEL_CURRENT);

    Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
    NotificationCompat.Builder mBuilder = null;
    try {
        mBuilder = new NotificationCompat.Builder(this, "PaulBassett")
                .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher))
                .setSmallIcon(R.mipmap.ic_stat_notify)
                .setContentTitle(data.getString("data.title"))
                .setContentText(URLDecoder.decode(data.getString("data.message"), "UTF-8"))
                .setNumber(Integer.parseInt(data.getString("data.badge")))
                .setAutoCancel(true)
                .setSound(defaultSoundUri);
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    mBuilder.setContentIntent(pi);

    mNotificationManager.notify((int) System.currentTimeMillis(), mBuilder.build());

    Preferences.putHaveNotification(getApplicationContext(), true);
}
```
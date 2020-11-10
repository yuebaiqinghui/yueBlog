---
date: 2020/8/4 11:32:44
updated: 2020/9/4 13:49:15
categories:
- Android
tags:
- Android
---
* 图片文字垂直居中对齐

  ```xml
<LinearLayout
android:layout_width="70dp"
android:layout_height="match_parent"
android:paddingLeft="10dp"
android:paddingTop="10dp">

  		<TextView
  		android:layout_width="match_parent"
  		android:layout_height="match_parent"
  		android:gravity="center"
  		android:drawableTop="@drawable/icon_detail_jincheng"
  		android:text="@string/text_ywdw"
  		android:textColor="@color/color_363636" />
</LinearLayout>
  ```

  **drawableTop**属性表示在文字上方放图
  
* HTTP请求封装

  使用okHTTP

  ```java
  package com.example.myapplication.utils;
  
  import android.util.Log;
  
  import org.json.JSONObject;
  
  import java.io.IOException;
  import java.util.Map;
  import java.util.concurrent.Callable;
  import java.util.concurrent.ExecutionException;
  import java.util.concurrent.ExecutorService;
  import java.util.concurrent.Executors;
  import java.util.concurrent.Future;
  
  import okhttp3.FormBody;
  import okhttp3.MediaType;
  import okhttp3.OkHttpClient;
  import okhttp3.Request;
  import okhttp3.RequestBody;
  import okhttp3.Response;
  
  public class ConnectionUtil {
      static ExecutorService executor = Executors.newSingleThreadExecutor();
  
      public static Object synchro(final String url,final Map<String,String> param) {
          Callable callable = new Callable() {
              @Override
              public Object call() throws Exception {
                  FormBody.Builder body = null;
                  for (String key : param.keySet()){
                      body = new FormBody.Builder()
                              .add(key,param.get(key));
                  }
  
                  Object jsonObject = null;
                  OkHttpClient okHttpClient = new OkHttpClient();//创建单例
                  Request request = new Request.Builder().post(body.build())//创建请求
                          .url(url)
                          .build();
                  try {
                      Response response = okHttpClient.newCall(request).execute();//执行请求
                      String mContent = response.body().string();//得到返回响应，注意response.body().string() 只能调用一次！
                      jsonObject = mContent;
  
                  } catch (IOException e) {
                      e.printStackTrace();
                      Log.e("OkHttpActivity", e.toString());
                  }
                  return jsonObject;
              }
          };
          Future future = executor.submit(callable);
          Object o = null;
          try {
              o = future.get();
          } catch (ExecutionException e) {
              e.printStackTrace();
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          return o;
      }
  
  
  
      public static Object synchroJson(final String url,final Map<String,String> param) {
          Callable callable = new Callable() {
              @Override
              public Object call() throws Exception {
                  JSONObject json = new JSONObject(param);
                  Object jsonObject = null;
                  RequestBody body = RequestBody.create(MediaType.parse("application/json; charset=utf-8"),json.toString());
                  Request request = new Request.Builder()
                          .url(url)
                          .post(body)
                          .build();
                  OkHttpClient okHttpClient = new OkHttpClient();//创建单例
                  try {
                      Response response = okHttpClient.newCall(request).execute();//执行请求
                      String mContent = response.body().string();//得到返回响应，注意response.body().string() 只能调用一次！
                      jsonObject = mContent;
  
                  } catch (IOException e) {
                      e.printStackTrace();
                      Log.e("OkHttpActivity", e.toString());
                  }
                  return jsonObject;
              }
          };
          Future future = executor.submit(callable);
          Object o = null;
          try {
              o = future.get();
          } catch (ExecutionException e) {
              e.printStackTrace();
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          return o;
      }
  }
  
  ```

* 判断是否在模拟器使用

  ```java
  package com.example.myapplication.utils;
  
  import android.bluetooth.BluetoothAdapter;
  import android.content.Context;
  import android.hardware.Sensor;
  import android.hardware.SensorManager;
  import android.os.Build;
  import android.text.TextUtils;
  import android.util.Log;
  
  import java.io.BufferedReader;
  import java.io.File;
  import java.io.IOException;
  import java.io.InputStreamReader;
  
  public class CheckRoot {
  
      public static boolean notHasBlueTooth() {
          BluetoothAdapter ba = BluetoothAdapter.getDefaultAdapter();
          if (ba == null) {
              return true;
          } else {
              // 如果有蓝牙不一定是有效的。获取蓝牙名称，若为null 则默认为模拟器
              String name = ba.getName();
              if (TextUtils.isEmpty(name)) {
                  return true;
              } else {
                  return false;
              }
          }
      }
  
      /*
       *用途:依据是否存在光传感器来判断是否为模拟器
       *返回:true 为模拟器
       */
      public static Boolean notHasLightSensorManager(Context context) {
          SensorManager sensorManager = (SensorManager) context.getSystemService(context.SENSOR_SERVICE);
          Sensor sensor8 = sensorManager.getDefaultSensor(Sensor.TYPE_LIGHT); //光
          if (null == sensor8) {
              return true;
          } else {
              return false;
          }
      }
  
      /*
       *用途:根据部分特征参数设备信息来判断是否为模拟器
       *返回:true 为模拟器
       */
      public static boolean isFeatures() {
          return Build.FINGERPRINT.startsWith("generic")
                  || Build.FINGERPRINT.toLowerCase().contains("vbox")
                  || Build.FINGERPRINT.toLowerCase().contains("test-keys")
                  || Build.MODEL.contains("google_sdk")
                  || Build.MODEL.contains("Emulator")
                  || Build.MODEL.contains("Android SDK built for x86")
                  || Build.MANUFACTURER.contains("Genymotion")
                  || (Build.BRAND.startsWith("generic") && Build.DEVICE.startsWith("generic"))
                  || "google_sdk".equals(Build.PRODUCT);
      }
  
      /*
       *用途:根据CPU是否为电脑来判断是否为模拟器
       *返回:true 为模拟器
       */
      public static boolean checkIsNotRealPhone() {
          String cpuInfo = readCpuInfo();
          if ((cpuInfo.contains("intel") || cpuInfo.contains("amd"))) {
              return true;
          }
          return false;
      }
      /*
       *用途:根据CPU是否为电脑来判断是否为模拟器(子方法)
       *返回:String
       */
      public static String readCpuInfo() {
          String result = "";
          try {
              String[] args = {"/system/bin/cat", "/proc/cpuinfo"};
              ProcessBuilder cmd = new ProcessBuilder(args);
  
              Process process = cmd.start();
              StringBuffer sb = new StringBuffer();
              String readLine = "";
              BufferedReader responseReader = new BufferedReader(new InputStreamReader(process.getInputStream(), "utf-8"));
              while ((readLine = responseReader.readLine()) != null) {
                  sb.append(readLine);
              }
              responseReader.close();
              result = sb.toString().toLowerCase();
          } catch (IOException ex) {
          }
          return result;
      }
  
      /*
       *用途:检测模拟器的特有文件
       *返回:true 为模拟器
       */
      private static String[] known_pipes = {"/dev/socket/qemud", "/dev/qemu_pipe"};
      public static boolean checkPipes() {
          for (int i = 0; i < known_pipes.length; i++) {
              String pipes = known_pipes[i];
              File qemu_socket = new File(pipes);
              if (qemu_socket.exists()) {
                  Log.v("Result:", "Find pipes!");
                  return true;
              }
          }
          Log.i("Result:", "Not Find pipes!");
          return false;
      }
  }
  
  ```
  
* 检测手机是否开启root权限：

  ```java
  package com.example.myapplication.utils;
  
  import android.util.Log;
  
  import java.io.BufferedReader;
  import java.io.File;
  import java.io.IOException;
  import java.io.InputStreamReader;
  
  public class RootCheck {
  
      private final static String TAG = "RootUtil";
  
      public static boolean isRoot() {
          String binPath = "/system/bin/su";
          String xBinPath = "/system/xbin/su";
          if (new File(binPath).exists() && isExecutable(binPath))
              return true;
          if (new File(xBinPath).exists() && isExecutable(xBinPath))
              return true;
          return false;
      }
  
      private static boolean isExecutable(String filePath) {
          Process p = null;
          try {
              p = Runtime.getRuntime().exec("ls -l " + filePath);
              // 获取返回内容
              BufferedReader in = new BufferedReader(new InputStreamReader(p.getInputStream()));
              String str = in.readLine();
              Log.i(TAG, str);
              if (str != null && str.length() >= 4) {
                  char flag = str.charAt(3);
                  if (flag == 's' || flag == 'x')
                      return true;
              }
          } catch (IOException e) {
              e.printStackTrace();
          } finally {
              if (p != null) {
                  p.destroy();
              }
          }
          return false;
      }
  }
  
  ```

  
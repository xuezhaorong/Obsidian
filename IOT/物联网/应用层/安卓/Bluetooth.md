> 项目链接：[xuezhaorong/MyBlueTooth (github.com)](https://github.com/xuezhaorong/MyBlueTooth)
## 权限声明
### 静态权限声明
在`AndroidMainfest.xml`中添加相关权限声明
```xml
<uses-permission android:name="android.permission.BLUETOOTH"/>  
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>  
<!--Android12 的蓝牙权限 如果您的应用与已配对的蓝牙设备通信或者获取当前手机蓝牙是否打开-->  
<uses-permission android:name="android.permission.BLUETOOTH_CONNECT"/>  
<!--Android12 的蓝牙权限 如果您的应用查找蓝牙设备（如蓝牙低功耗 (BLE) 外围设备）-->  
<uses-permission android:name="android.permission.BLUETOOTH_SCAN"  
    android:usesPermissionFlags="neverForLocation"  
    />  
<!--Android12 的蓝牙权限 如果您的应用使当前设备可被其他蓝牙设备检测到-->  
<uses-permission android:name="android.permission.BLUETOOTH_ADVERTISE"/>  
<uses-feature android:name="android.hardware.bluetooth" android:required="true"/>
```

### 动态权限声明
定义一个权限声明类，提供动态声明
```kotlin
class PermissionFragment : Fragment() {  
    private val requestPermissionLauncher =  
        registerForActivityResult(  
            ActivityResultContracts.RequestPermission()  
        ) { isGranted: Boolean ->  
            if (isGranted) {  
                // 授权成功 
            } else {  
                // 授权失败
            }  
        }  

    private fun PermissionGranted(){ // 权限申请  
  
        when (PackageManager.PERMISSION_GRANTED) {  
            ContextCompat.checkSelfPermission(  
                requireContext(),  
                Manifest.permission.BLUETOOTH_CONNECT  
            ) -> {  
                // 授权成功
  
            }  
            else -> {  
                requestPermissionLauncher.launch(  
                    Manifest.permission.BLUETOOTH_CONNECT  
                )  
            }  
        }  
  
    }
```

## 特征值
特征值是蓝牙各种功能的UUID，根据不同芯片而定
```kotlin
//蓝牙的特征值，发送  
val SERVICE_EIGENVALUE_SEND = "0000ffe1-0000-1000-8000-00805f9b34fb"  
  
//蓝牙的特征值，接收  
val SERVICE_EIGENVALUE_READ = "00002902-0000-1000-8000-00805f9b34fb"  
  
/**  
 * 服务 UUID */
 val SERVICE_UUID = "0000ff01-0000-1000-8000-00805f9b34fb"  
  
/**  
 * 特性写入 UUID */
 val CHARACTERISTIC_WRITE_UUID = "0000ff02-0000-1000-8000-00805f9b34fb"  
  
/**  
 * 特性读取 UUID */
 val CHARACTERISTIC_READ_UUID = "0000ff10-0000-1000-8000-00805f9b34fb"  
  
/**  
 * 描述 UUID */
 val DESCRIPTOR_UUID = "00002902-0000-1000-8000-00805f9b34fb"  
  
/**  
 * 电池服务 UUID */
 val BATTERY_SERVICE_UUID = "0000180f-0000-1000-8000-00805f9b34fb"  
  
/**  
 * 电池特征（特性）读取 UUID */
 val BATTERY_CHARACTERISTIC_READ_UUID = "00002a19-0000-1000-8000-00805f9b34fb"  
  
/**  
 * OTA服务 UUID */
 val OTA_SERVICE_UUID = "5833ff01-9b8b-5191-6142-22a4536ef123"  
  
/**  
 * OTA特征（特性）写入 UUID */
 val OTA_CHARACTERISTIC_WRITE_UUID = "5833ff02-9b8b-5191-6142-22a4536ef123"  
  
/**  
 * OTA特征（特性）表示 UUID */
 val OTA_CHARACTERISTIC_INDICATE_UUID = "5833ff03-9b8b-5191-6142-22a4536ef123"  
  
/**  
 * OTA数据特征（特性）写入 UUID */
 val OTA_DATA_CHARACTERISTIC_WRITE_UUID = "5833ff04-9b8b-5191-6142-22a4536ef123"
```

## 回调函数
为蓝牙的连接状态，发现服务，特征写入和数据接收等定义对应的回调函数

```kotlin
class BleCallback : BluetoothGattCallback() {    
    /**  
     * 连接状态回调  
     */  
    @SuppressLint("MissingPermission")  
    override fun onConnectionStateChange(gatt: BluetoothGatt, status: Int, newState: Int) {  
        if (newState != BluetoothGatt.STATE_CONNECTED) {  
            Log.d("TAG", "连接失败 $status")  
            return  
        } else { // GATT成功  
            thread {  
                gatt.discoverServices() // 扫描服务  
                Thread.sleep(3000)  
  
            } // 开启发现服务线程  
        }  
    }  
  
    // 发现服务回调函数  
    @SuppressLint("MissingPermission", "NewApi")  
    override fun onServicesDiscovered(gatt: BluetoothGatt?, status: Int) {  
        val characteristicsList = gatt?.services // 所有服务的列表  
        if (gatt != null) {  
            BluetoothHelper.mgatt = gatt  
            Log.d("TAG", "获取Gatt成功")  
        }  
        characteristicsList?.forEach { bluetoothGattService -> // 遍历所有服务  
            bluetoothGattService.characteristics.forEach { // 遍历所有特  
                if (it.uuid.toString() == BluetoothHelper.SERVICE_EIGENVALUE_SEND) { // 获取写入特征  
                    Log.d("TAG", "已获取写入特征")  
                    BluetoothHelper.mwriteCharacteristic = it // 存储写入特征  
                    BluetoothHelper.mgatt.setCharacteristicNotification(it, true) // 打开通知通道  
                    thread {  
                        Thread.sleep(200)  
                        val clientConfig = BluetoothHelper.mwriteCharacteristic.getDescriptor(  
                            UUID.fromString(BluetoothHelper.SERVICE_EIGENVALUE_READ)  
                        )  
                        if (clientConfig != null) {  
                            BluetoothHelper.mgatt.writeDescriptor(  
                                clientConfig,  
                                BluetoothGattDescriptor.ENABLE_NOTIFICATION_VALUE  
                            )// 设置监听模块  
                            Log.d("TAG", "设置监听成功")  
                            Log.d("TAG", "连接成功")  
                            BluetoothHelper.connectFlag = true  
                        }  
  
                        Thread.sleep(200)  
                    }  
                }  
            }  
  
        }  
  
    }  
  
    override fun onCharacteristicWrite(  
        gatt: BluetoothGatt?,  
        characteristic: BluetoothGattCharacteristic?,  
        status: Int  
    ) {  
        super.onCharacteristicWrite(gatt, characteristic, status)  
        if (status == BluetoothGatt.GATT_SUCCESS){  
            Log.d("TAG","发送数据成功")  
        }else{  
            Log.d("TAG","发送数据失败")  
        }  
    }  
  
	// 数据接收回调
    override fun onCharacteristicChanged(  
        gatt: BluetoothGatt,  
        characteristic: BluetoothGattCharacteristic,  
        value: ByteArray  
    ) {  
        super.onCharacteristicChanged(gatt, characteristic, value)  
         
        
    }  
}
```

## 扫描
1. 在授予权限后，需要获取蓝牙适配器
```kotlin
val manager = getSystemService(BluetoothManager::class.java)
val adapter = manager.adapter
```
2. 检查蓝牙是否打开
```kotlin
//请求BLUETOOTH_SCAN权限意图  
private val requestBluetoothScan =  
    registerForActivityResult(ActivityResultContracts.RequestPermission()) {  
        if (it) {  
            
        } else {  
            
        }  
    }

val enableBluetooth = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) {  
    when(it.resultCode){  
        Activity.RESULT_OK->  
            
        else->  
            
  
    }  
}

// 蓝牙未开启，开启蓝牙  
if (adapter.isEnabled == false){  
    enableBluetooth.launch(Intent(adapter.ACTION_REQUEST_ENABLE))  
}

```
3. 开始扫描
扫描结果回调函数
```kotlin
//扫描结果回调  
private val scanCallback = object : ScanCallback() {  
	@SuppressLint("MissingPermission")  
	override fun onScanResult(callbackType: Int, result: ScanResult) {  
		val device = result.device  
		if (device !in BluetoothList && device.name != null){ // 添加进列表中  
			BluetoothList.add(device)  
			binding.recyclerView.adapter?.notifyDataSetChanged()  
		}  
	}  
}
```


```kotlin

// 获得扫描器
val scanner = adapter.bluetoothLeScanner

private fun startScan() {  
    if (!isScanning) {  
        scanner.startScan(scanCallback)  
        isScanning = true  
    }  
}  
  

private fun stopScan() {  
    if (isScanning) {  
        scanner.stopScan(scanCallback)  
        isScanning = false  
    }  
}


// 开始扫描
when (PackageManager.PERMISSION_GRANTED) {  
    ContextCompat.checkSelfPermission(  
        requireContext(),  
        Manifest.permission.BLUETOOTH_SCAN  
    ) -> {  
        startScan()  
    }  
    else -> {  
        requestBluetoothScan.launch(  
            Manifest.permission.BLUETOOTH_SCAN  
        )  
    }  
}
```
扫描需要及时停止，否则会消耗大量电量
```kotlin
override fun onPause() {  
    super.onPause()  
  
    // 暂停  
    stopScan()  
  
}
```

## 连接
可以选择是否自动连接
```kotlin
@SuppressLint("MissingPermission")  
override fun onBindViewHolder(holder: ViewHolder, position: Int) {  
    val device = bluetoothList[position]  
    holder.binding.bluetoothName.text = device.name.toString()  
    holder.binding.bluetoothMacId?.text = device.address.toString()  
    holder.itemView.setOnClickListener {// 开始配对  
        stopScan() // 停止搜索  
        device.connectGatt(mainActivity,false,bleCallback)  
    }  
}
```

## 数据接收
在先前的回调函数中可以设置
```kotlin
	// 数据接收回调
    override fun onCharacteristicChanged(  
        gatt: BluetoothGatt,  
        characteristic: BluetoothGattCharacteristic,  
        value: ByteArray  
    ) {  
        super.onCharacteristicChanged(gatt, characteristic, value)  
         // 
        var result = ""  
		value.forEach {  
		    result += it.toInt().toChar().toString()  
		}
    }  
```

## 数据发送
需要在子线程中执行
```kotlin
@RequiresApi(Build.VERSION_CODES.TIRAMISU)  
@SuppressLint("MissingPermission")  
fun sendData(Data: String) {  

    // 发送数据线程  
    thread {  
        val sendData = getSendDataByte(Data)  // 选择将String字符串分包为List byte数组 一组20个字符
        sendData.forEach {  
            Thread.sleep(100) // 延时100ms  
            // 发送数据  
            sendDataSign =  
                mgatt.writeCharacteristic(mwriteCharacteristic,it,BluetoothGattCharacteristic.WRITE_TYPE_NO_RESPONSE)  
            if (sendDataSign == BluetoothStatusCodes.SUCCESS) {  
                // 发送数据成功
            } else {  
                Log.d("TAG", "发送数据失败")  
            }  
        }  
    }}

//将数据分包  
private fun dataSeparate(len: Int): IntArray {  
    val lens = IntArray(2)  
    lens[0] = len / 20  
    lens[1] = len % 20  
    return lens  
}  
  
//将String字符串分包为List byte数组  
private fun getSendDataByte(buff: String): List<ByteArray> {  
    val listSendData: MutableList<ByteArray> = mutableListOf<ByteArray>()  
    val sendDataLength: IntArray = dataSeparate(buff.length)  
    if (sendDataLength[0] == 0) { // 不足20  
        val dataLess20 = ByteArray(sendDataLength[1])  
        for (i in 0 until sendDataLength[1]) {  
            dataLess20[i] = buff[i].code.toByte()  
        }  
        listSendData.add(dataLess20)  
  
  
    } else { // 超过20  
        for (i in 0 until sendDataLength[0]) {  
            val dataFor20 = ByteArray(20)  
            for (j in 0 until 20) {  
                dataFor20[j] = buff[i * 20 + j].code.toByte()  
            }  
            listSendData.add(dataFor20)  
        }  
        if (sendDataLength[1] > 0) {  
            val dataLess20 = ByteArray(sendDataLength[1])  
            for (i in 0 until sendDataLength[1]) {  
                dataLess20[i] = buff[20 * sendDataLength[0] + i].code.toByte()  
            }  
            listSendData.add(dataLess20)  
        }  
  
    }  
  
    return listSendData  
}
```
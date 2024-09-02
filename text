import android.bluetooth.BluetoothAdapter;
import android.bluetooth.BluetoothDevice;
import android.bluetooth.BluetoothSocket;
import android.content.Context;
import android.util.Log;

import java.io.IOException;
import java.io.OutputStream;
import java.util.UUID;

public class BluetoothHeartRateSender {

    private static final String TAG = "BluetoothHeartRateSender";
    private BluetoothAdapter mBluetoothAdapter;
    private BluetoothDevice mDevice;
    private BluetoothSocket mSocket;
    private OutputStream mOutputStream;

    public BluetoothHeartRateSender(Context context) {
        mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
    }

    public boolean connectDevice(BluetoothDevice device) {
        mDevice = device;
        try {
            // 使用心率监测设备的UUID
            UUID uuid = UUID.fromString("0000180d-0000-1000-8000-00805f9b34fb");
            mSocket = mDevice.createRfcommSocketToServiceRecord(uuid);
            mSocket.connect();
            mOutputStream = mSocket.getOutputStream();
            return true;
        } catch (IOException e) {
            Log.e(TAG, "Error connecting device", e);
            try {
                if (mSocket != null && mSocket.isConnected()) {
                    mSocket.close();
                }
            } catch (IOException e1) {
                Log.e(TAG, "Error closing socket", e1);
            }
            return false;
        }
    }

    public void sendHeartRate(int heartRate) {
        if (mOutputStream != null) {
            try {
                // 将心率数据转换为字节
                byte[] heartRateBytes = intToByteArray(heartRate);
                mOutputStream.write(heartRateBytes);
            } catch (IOException e) {
                Log.e(TAG, "Error sending heart rate", e);
            }
        }
    }

    private byte[] intToByteArray(int value) {
        return new byte[]{
                (byte) ((value >> 24) & 0xFF),
                (byte) ((value >> 16) & 0xFF),
                (byte) ((value >> 8) & 0xFF),
                (byte) (value & 0xFF)
        };
    }

    public void closeConnection() {
        try {
            if (mOutputStream != null) {
                mOutputStream.close();
            }
            if (mSocket != null) {
                mSocket.close();
            }
        } catch (IOException e) {
            Log.e(TAG, "Error closing connection", e);
        }
    }
}

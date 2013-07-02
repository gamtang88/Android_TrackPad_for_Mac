package com.example.mackinpad;

import android.app.Activity;
import android.app.AlertDialog;
import android.bluetooth.BluetoothAdapter;
import android.content.Context;
import android.content.DialogInterface;
import android.content.Intent;
import android.content.res.Configuration;
import android.os.Bundle;
import android.util.Log;
import android.view.KeyEvent;
import android.view.Menu;
import android.view.View;
import android.view.Window;
import android.view.WindowManager;
import android.widget.Toast;

public class StartActivity extends Activity {

	private BluetoothAdapter mBluetoothAdapter = null;
	final static String TAG = "MacKinPad";

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		requestWindowFeature(Window.FEATURE_NO_TITLE);
		getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,
				WindowManager.LayoutParams.FLAG_FULLSCREEN);
		if (isTablet(this))
			setContentView(R.layout.activity_start_tab);
		else
			setContentView(R.layout.activity_start);

		getWindow().addFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON);
		mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter();

		if (mBluetoothAdapter == null) {
			Toast.makeText(this, "Bluetooth is not available",
					Toast.LENGTH_SHORT).show();
			finish();
		}
		if (!mBluetoothAdapter.isEnabled())
			startActivity(new Intent(
					"android.bluetooth.adapter.action.REQUEST_ENABLE"));
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.start, menu);
		return true;
	}

	static boolean isTablet(Context context) {
		// TODO: This hacky stuff goes away when we allow users to target
		// devices
		int xlargeBit = 4; // Configuration.SCREENLAYOUT_SIZE_XLARGE; // upgrade
							// to HC SDK to get this
		Configuration config = context.getResources().getConfiguration();
		return (config.screenLayout & xlargeBit) == xlargeBit;
	}

	public void OnClick(View v) {
		Intent intent = null;
		switch (v.getId()) {
		case R.id.connect:

			Log.i("TAG", "start");
			intent = new Intent(StartActivity.this, DeviceListActivity.class);
			startActivityForResult(intent, 1);
			break;
		case R.id.help:
			intent = new Intent(StartActivity.this, HelpActivity.class);
			startActivity(intent);
			break;
		}
	}

	@Override
	public boolean onKeyDown(int keyCode, KeyEvent event) {

		if (event.getKeyCode() == KeyEvent.KEYCODE_BACK) {
			new AlertDialog.Builder(this)
					.setIcon(R.drawable.ic_launcher)
					.setTitle("Do you want to exit from MacKinPad ?")
					.setPositiveButton("Yes",
							new DialogInterface.OnClickListener() {

								@Override
								public void onClick(DialogInterface arg0,
										int arg1) {
									moveTaskToBack(true);
									finish();
									android.os.Process
											.killProcess(android.os.Process
													.myPid());
								}
							}).setNegativeButton("No", null).create().show();
			return false;
		}

		return super.onKeyDown(keyCode, event);
	}

	@Override
	public void onDestroy() {
		super.onDestroy();

	}

	public void onActivityResult(int requestCode, int resultCode,
			Intent paramIntent) {
		Log.d("TAG", "onActivityResult " + resultCode);// get paired device &
														// will pairing device
														// list
		if (paramIntent == null)
			return;
		String str = paramIntent.getExtras().getString(
				DeviceListActivity.EXTRA_DEVICE_ADDRESS);
		Log.i(TAG, "device address : " + str);
		Intent intent = new Intent(StartActivity.this, TouchActivity.class);
		intent.putExtra("address", str);
		startActivity(intent);
		return;
	}
}

---
layout: post
title:  "讨论Activity的生命周期"
date:   2015-11-15 15:56:51 +0800
categories: jekyll update
---
Activity的作为android重要单元之一（其他元件包括：Service,BroadcastReceiver和ContentProvider）其重要性自然不必多说， 今天就来Activity的生命周期来讨论一下：


通过这张图，我们可以看到整个Activity的生命流程，总结几点：

 1. 每一个Activity被创建的时候，都要初始化，也就是回调Oncrea()函数。此方法在整个应用进程被终止或者整个activity被销毁之前都不会被再次调用。
 2. 二，当Oncreat()函数被回调以后，接着进入OnStrat(),接着有一个很特殊的状态OnResume().然后才进入running状态，这个时候整个anctivity才会被显示到前台。
 3. 三，当Activity在running状态时，跳转到新的Activity时，当前的Activity会先调用OnPause()，然后第二个Activity进入创建状态，先OnCreate(),再OnResume().意味这第二个Activity进入了运行状态了。
    此时第一个Activity将调用OnStop();进入停止状态。意味这进入后台状态，同理当手机点击home键使得程序进入后台时，当前Activity也最终执行Onpause()===>OnStop()流程。
 4. 四，当应用程序彻底关闭，整个进程断开时次Acitivityj将彻底销毁OnDestory();这种情况也发生在程序进入后台程序以后，（执行OnStop()被其他的程序占用了内存，被系统杀掉，此时整个Activity也将被OnDestory（）;
 5. 五，此时被Activity已经被销毁掉后，再次进入时，将重新新进行OnCcreate()-====>>OnStart()======>onResume创建流程。

为了测试，我做了一个Activity切换的demo
MainActivity.java(这是第一个Activity页面)；第二个页面代码类似。

```
public class MainActivity extends ActionBarActivity {
	
	final String TAG ="------hxTest--------";
	Button finish,starSecondActivity;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Log.d(TAG, "-------onCreate---firstActivity-------------");
        
        starSecondActivity=(Button)findViewById(R.id.newButton);
        starSecondActivity.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View arg0) {
				Intent intent = new Intent(MainActivity.this,SecondActivity.class);
				MainActivity.this.startActivity(intent);
			}
		});
        finish=(Button)findViewById(R.id.finish);
        
        finish.setOnClickListener(new OnClickListener() {
			
			public void onClick(View arg0) {
				MainActivity.this.finish();
			}
		});
    }
    public void onStart(){
    	super.onStart();
    	Log.d(TAG, "-------onCreate--firstActivity------------");
    	
    }
    public void onRestart(){
    	super.onRestart();
    	Log.d(TAG, "-------onRestart----firstActivity----------");
    	
    }
    public void onResume(){
    	super.onResume();
    	Log.d(TAG, "-------onResume---firstActivity-----------");
    	
    }
    public void onPause(){
    	super.onPause();
    	Log.d(TAG, "-------onPause---firstActivity-----------");
    	
    }
    public void onStop(){
    	super.onStop();
    	Log.d(TAG, "-------onStop---firstActivity-----------");
    	
    }
    
    public void onDestroy(){
    	super.onDestroy();
    	Log.d(TAG, "-------onDestroy---firstActivity-----------");
    }


    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();
        if (id == R.id.action_settings) {
            return true;
        }
        return super.onOptionsItemSelected(item);
    }
}
```


小demo的代码下载地址：
http://download.csdn.net/detail/u010178833/7923245
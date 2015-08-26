# Speed Test library for Java / Android  #

http://akinaru.github.io/speed-test-lib/

<i>Last update 26/05/2015</i>

Speed Test library featuring :

* speed test download with transfer rate output
* speed test upload with transfer rate output
* download and upload progress monitoring
* speed test server / port / uri can be configured easily

Library is available in release folder, buildable with build.xml ant file.

* For download process, library will download file from given speed test server parameters and calculate transfer rate for downloading this file from server
* For upload process, library will generate a random file with a given size and will upload this file to a server calculating transfer rate for uploading this file to server

<hr/>

<h4>How to use ?</h4>

Instanciate SpeedTest class :

```
SpeedTestSocket speedTestSocket = new SpeedTestSocket();

```

Add a listener to monitor :

* download process result with ``onDownloadPacketsReceived(int packetSize, float transferRateBitPerSeconds, float transferRateOctetPerSeconds)`` callback
* upload process result with ``onUploadPacketsReceived(int packetSize, float transferRateBitPerSeconds, float transferRateOctetPerSeconds)`` callback
* download progress with ``onDownloadProgress(int percent)`` callback
* upload progress with ``onUploadProgress(int percent)`` callback
* download error catch with ``onDownloadError(int errorCode, String message)``
* upload error catch with ``onUploadError(int errorCode, String message)``

```
speedTestSocket.addSpeedTestListener(new ISpeedTestListener() {

	@Override
	public void onDownloadPacketsReceived(int packetSize, float transferRateBitPerSeconds, float transferRateOctetPerSeconds) {
		System.out.println("download transfer rate  : " + transferRateBitPerSeconds + " bps");
		System.out.println("download transfer rate  : " + transferRateOctetPerSeconds + "Bps");
	}

	@Override
	public void onDownloadError(int errorCode, String message) {
		System.out.println("Download error " + errorCode + " occured with message : " + message);
	}

	@Override
	public void onUploadPacketsReceived(int packetSize, float transferRateBitPerSeconds, float transferRateOctetPerSeconds) {
		System.out.println("download transfer rate  : " + transferRateBitPerSeconds + " bps");
		System.out.println("download transfer rate  : " + transferRateOctetPerSeconds + "Bps");
	}

	@Override
	public void onUploadError(int errorCode, String message) {
		System.out.println("Upload error " + errorCode + " occured with message : " + message);
	}

	@Override
	public void onDownloadProgress(int percent) {
	}

	@Override
	public void onUploadProgress(int percent) {
	}

});

```

<b>Start Download speed test</b>

``void startDownload(String hostname, int port, String uri)``

* `hostname` : server hostname
* `port` : server port
* `uri` : uri to fetch your file from server

```
speedTestSocket.startDownload("ipv4.intuxication.testdebit.info", 80,"/fichiers/10Mo.dat");

```
You can wait for test completion with ``closeSocketJoinRead()`` which is prefered to ``closeSocket()`` since it join reading thread before resuming application.

```
// socket will be closed and reading thread will die if it exists
speedTestSocket.closeSocketJoinRead();
```

<b>Start Upload speed test</b>

```
void startUpload(String hostname, int port, String uri, int fileSizeOctet)
```

* `hostname` : server hostname
* `port` : server port
* `uri` : uri to fetch your file from server
* `fileSizeOctet` : the file size to be uploaded to server (file will be generated randomly and sent to speed test server)

Here is an example for a file of 10Moctet :
```
/* start speed test upload on favorite server */
speedTestSocket.startUpload("1.testdebit.info", 80, "/", 10000000);
```

<hr/>

<b>Build HTTP response</b>


<b>Quick test command line syntax</b> 

``java -jar speed-test-lib-1.0.jar``

in folder ./http-endec-java/release

<hr/>

* Project is JRE 1.7 compliant
* You can build it with ant => build.xml
* Development on Eclipse 

<b>Tested with</b>

* https://testdebit.info/

<h4>TODO</h4>

manage unexpected socket disconnection for downloading and uploading
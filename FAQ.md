## Frequently Asked Questions (FAQ) WebRTC C SDK

### Information gathering
1. 4. I would like to know to the contents of offer and answer SDP. How do I get it?

A. Run `export DEBUG_LOG_SDP=TRUE` and it will be available in the logs.

2. How do I save logs to files? 

A. Run `export AWS_ENABLE_FILE_LOGGING=TRUE` to enable file logging. The default log file size and log rotation index can be set in https://github.com/awslabs/amazon-kinesis-video-streams-webrtc-sdk-c/blob/959d259acff08c8b913faab5a3653f08cf0e409e/samples/Samples.h#L32

### ICE/Network 

1. ICE connection establishes successfully, but I do not see media being transferred. What can I do?

A. Make sure the actual MTU is higher than the MTU setting in:
https://github.com/awslabs/amazon-kinesis-video-streams-webrtc-sdk-c/blob/master/src/source/PeerConnection/Rtp.h#L12

2. How do I understand how my NAT behaves to debug network related issues?

A. You can run the `discoverNatBehavior` with network interface name and STUN host name. The application can be
found here: https://github.com/awslabs/amazon-kinesis-video-streams-webrtc-sdk-c/tree/master/samples

3. How long do selected candidate pairs persist throughout the session?

A. Right now when a candidate pair is selected, it will remain selected throughout the lifetime of current ice session. 

### Signaling
1. I am compiling LWS with -DLWS_WITHOUT_SERVER=0, but signaling does not seem to connect.

A. For more in depth detail, please refer to https://github.com/awslabs/amazon-kinesis-video-streams-webrtc-sdk-c/issues/585

2. connectSignalingChannelLws() fails with status code 0x0000000f. What does this mean? 

A. The status code indicates that the connection timed out. A few things you can do is to check if the device is in a bad network. To get more information, run debug version of libwebsockets by running cmake with `-DCMAKE_BUILD_TYPE=DEBUG` option. Please add these logs to the github issue and we can take a look!

### Miscellaneous
1. The GStreamer Sample does not work as expected when I use my own pipeline.

A. GStreamer pipeline related questions are best directed to GStreamer forums. Regardless, first thing you can do try to run the pipeline using `gst-launch` command.

2. I would like to stream frame files just like the samples do. How do I convert a file into h264 file for every frame? 

A. You can use `ffmpeg` or `gstreamer` to do this.

3. How can I know that the master detected the termination of the peer? 

A. Once the peer is closed, the master detects a DTLS close_notify alert. The logs should contain the following line: "Detected DTLS close_notify alert".



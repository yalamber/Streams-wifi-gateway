# I2T Streams Gateway

## Preparation
Install rust if you don't have it already, find the instructions here https://www.rust-lang.org/tools/install

Make sure you also have the build dependencies installed, if not run:  
`sudo apt install build-essential`  
`sudo apt install pkg-config`  
`sudo apt install libssl-dev`  
`sudo apt update`  

## Installing the streams-gateway

Download the Repository:  

`git clone https://github.com/iot2tangle/Streams-wifi-gateway.git`
  
Configure the streams-gateway:  

`nano config.json`  
 
Set the *device_name* to the value specified in the configuration file of the XDK110.  
Change *port, node, mwm, local_pow* if needed 

  
## Runnig the Examples:  
  
Run the streams-gateway:  

`cargo run --release`  

This starts the server which will forward messages from the devices to the Tangle  
  
The Output will be something like this:  

`>> Starting.... `  
`>> Channel root: "ab3de895ec41c88bd917e8a47d54f76d52794d61ff4c4eb3569c31f619ee623d0000000000000000"`  
`>> To Start the Subscriber run: `  
  
`>> cargo run --release --example subscriber "ab3de895ec41c88bd917e8a47d54f76d52794d61ff4c4eb3569c31f619ee623d0000000000000000" `  
  
`>> Listening on http://0.0.0.0:8080`  

In a separate window start a subscriber using the Channle Root printed by the Gateway (see example above):  

`cargo run --release --example subscriber <your_channel_root> `  

To send data to the server you can use Postman, or like in this case cURL, make sure the port is the same as in the config.json file:  
`  
curl --location --request POST '127.0.0.1:8080/sensor_data'   
--header 'Content-Type: application/json'   
--data-raw '{
    "iot2tangle": [
        {
            "sensor": "Gyroscope",
            "data": [
                {
                    "x": "4514"
                },
                {
                    "y": "244"
                },
                {
                    "z": "-1830"
                }
            ]
        },
        {
            "sensor": "Acoustic",
            "data": [
                {
                    "mp": "1"
                }
            ]
        }
    ],  
    "device": "XDK_HTTP",  
    "timestamp": ""  
}'  
`  
Note: Thetimestamp will be set the moment the transaction is sent to the Tangle 

         
IMPORTANT: The device will be authenticated through the "device" field in the request (in this example XDK_HTTP), this has to match what was set as device_name in the config.json on the Gateway (see Configuration section above)!  

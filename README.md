docker compose down -v && docker compose up --build

seperate shell:
docker exec -it acme-simulator sh
node cli.js register-devices "car=2,truck=2"
notice the UI updated with 4 devices in Registered Devices table
click on the device at the top of the Registered Devices table and notice that there are currently no readings

node cli.js simulate-readings-for-devices 1,2
notice that the selected device has readings now under the second table, Sensor Data for car #..
notice that clicking on the 2nd top most entry also has readings coming in now

ctrl/cmd + c to end the readings command

delete a device and notice it gets removed from the UI
node cli.js delete-device 1
node cli.js update-sensor -i 1 -u "px"

in browser, open:
http://localhost:5173/

open dev tools / inspect to observe websocket comms in console and Network for API calls






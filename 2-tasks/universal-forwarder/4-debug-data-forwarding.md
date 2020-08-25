- https://community.splunk.com/t5/Archive/Does-the-forwarder-send-data-in-chunks-of-64-KBs/td-p/411662
- In my local environment, setting `useAck=1` in `outputs.conf` on the forwarder did _not_ solve my issue where approximately every 8.1 kB an event gets broken up incorrectly
  as it passes through Cribl
  - This issue was the event breaker setting in Cribl!